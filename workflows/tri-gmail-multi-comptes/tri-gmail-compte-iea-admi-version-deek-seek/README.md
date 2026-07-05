# Tri Gmail automatique par IA (compte IEA ADMI / MEA) — version DeepSeek

## Description
Ce workflow trie automatiquement les emails entrants d'une boîte Gmail professionnelle en les classant dans la bonne arborescence de labels, à l'aide du modèle DeepSeek (LLM). Il récupère les labels existants, les emails non lus de la boîte de réception, détecte si un email a déjà été classé manuellement dans un thread, puis pour les emails restants, envoie le contenu (expéditeur, sujet, extrait) à DeepSeek avec un prompt système décrivant l'arborescence complète des dossiers et les règles métier de classement. Une astuce de "chauffe du cache" DeepSeek est utilisée avant les gros lots pour réduire les coûts (cache de prompt). Chaque classification est appliquée comme label Gmail, l'email est retiré de la boîte de réception, et le résultat (avec coût estimé en tokens) est journalisé dans une feuille Google Sheets.

## Services / APIs utilisés
- Gmail (API native n8n, OAuth2)
- DeepSeek API (chat completions, avec cache de prompt)
- Google Sheets (journal des classifications)

## Prérequis
- Un compte Gmail avec une arborescence de labels personnalisés
- Une clé API DeepSeek
- Une feuille Google Sheets de suivi avec les colonnes attendues (Sujet, Expéditeur, Nom du Dossier, Résumé, Tokens, Coût, Date, etc.)

## Variables d'environnement
| Variable | Description |
|---|---|
| YOUR_DEEPSEEK_API_KEY | Clé API DeepSeek (configurée via le système de credentials n8n) |
| REPLACE_WITH_YOUR_GOOGLE_SHEET_ID | ID de la feuille Google Sheets de journalisation |

## Note sur l'anonymisation
Le prompt système original contient une liste de correspondances entre noms de clients/dossiers juridiques réels et IDs de labels Gmail. Ces noms réels (clients, contentieux juridiques, partenaires) ont été remplacés par des identifiants génériques (`CLIENT_PRO_A`, `AFFAIRE_JURIDIQUE_A`, etc.) avant publication, tout en conservant la logique de classement et les IDs de labels bruts.

## Schéma du flux
```
[Schedule Trigger] --> [Get many labels] --> [Filtre labels utilisateur]
   --> [Get many messages INBOX] --> [Trie par date] --> [Limite 150 emails]
   --> [Get a thread] --> [Détecte dossiers déjà classés]
   --> [Si déjà classé ?] --(oui)--> [Add label] --> [Remove INBOX]
                          --(non)--> [Évalue nb emails] --> [Chauffe cache DeepSeek si besoin]
                              --> [Nettoie email] --> [Prépare prompt classif] --> [API DeepSeek]
                              --> [Parse réponse JSON] --> [Add label] --> [Remove INBOX] --> [Log Google Sheets]
```
