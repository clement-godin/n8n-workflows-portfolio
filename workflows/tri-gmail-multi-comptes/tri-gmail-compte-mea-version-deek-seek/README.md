# Tri Gmail compte Clément MEA (version DeepSeek)

## Description
Ce workflow automatise le tri de la boîte Gmail professionnelle de Clément pour la société MEA. Il liste les labels Gmail existants, récupère les emails de plus d'un mois présents en boîte de réception, puis pour chaque email consulte le fil de discussion complet afin de vérifier s'il est déjà rangé dans un dossier. Si ce n'est pas le cas, un prompt métier détaillé (règles de classement juridique et professionnel propres à MEA) est envoyé à l'API DeepSeek pour déterminer le label le plus pertinent. Le workflow inclut un mécanisme de "chauffe du cache" DeepSeek (prompt caching) lorsqu'un grand nombre d'emails doit être traité, afin de réduire la latence et le coût. Chaque email traité est enfin archivé (retiré de la boîte de réception) avec le bon label, et une ligne de suivi (sujet, expéditeur, résumé, tokens consommés) est ajoutée dans un Google Sheets de traçabilité.

## Services / APIs utilisés
- Gmail (lecture des threads/labels, ajout/suppression de labels)
- API DeepSeek (classification des emails, `deepseek-chat`)
- Google Sheets (journal des emails classés)

## Prérequis
- Un compte Gmail avec des labels personnalisés déjà créés pour chaque catégorie de classement
- Une clé API DeepSeek valide
- Une feuille Google Sheets de suivi avec les colonnes Sujet, Expéditeur, Nom du Dossier, Résumé, Tokens Input/Output/Cache, Date

## Variables d'environnement
| Variable | Description |
|---|---|
| YOUR_DEEPSEEK_API_KEY | Clé API DeepSeek utilisée pour la classification des emails (référencée via credential n8n) |
| YOUR_GOOGLE_SHEET_ID_TRI_MEA | ID de la feuille Google Sheets de suivi des emails triés |

## Schéma du flux
```
[Trigger] --> [Get many labels] --> [Code JS - Filtre labels] --> [Get many messages]
  --> [Trie - Par date] --> [Get a thread] --> [Code JS - Détecte dossiers]
  --> [Si - Email déjà classé]
      --> (oui) [Add label to message] --> [Remove label from message1]
      --> (non) [Code JS - Évalue nb emails] --> [Si - Seuil nb emails] --> [Si - Chauffe nécessaire]
          --> [Code JS - Chauffe cache] --> [API DeepSeek - Chauffe] --> [Code JS - Vérifie cache]
          --> [Si - Cache chaud] --> [Code JS - Nettoie email] --> [Code JS - Prépare classif]
          --> [API DeepSeek - Classifie] --> [Code JS - Parse classif] --> [Add label to message1]
          --> [Remove label from message] --> [Append row in sheet1]
```
