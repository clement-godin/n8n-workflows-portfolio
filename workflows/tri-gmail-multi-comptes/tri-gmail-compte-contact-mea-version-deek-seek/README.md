# TRI GMAIL COMPTE CONTACT MEA version deek seek

## Description
Ce workflow trie automatiquement la boîte de réception Gmail du compte "contact" de MEA en appliquant les labels métier appropriés à chaque email. Il récupère les emails de l'INBOX, vérifie si un thread est déjà classé (pour éviter de re-traiter), puis envoie chaque email non classé à DeepSeek avec un prompt de classification listant l'arborescence exacte des dossiers Gmail (juridique, devis, factures, partenaires, leads entrants, etc.). Le modèle renvoie l'ID du label à appliquer, le workflow applique le label puis retire l'email de l'INBOX. Un mécanisme de "chauffe de cache" DeepSeek est intégré : si le volume d'emails à traiter dépasse 50, un appel "ping" préalable est envoyé pour forcer la mise en cache du prompt système (économie de coûts sur les tokens répétés). Chaque classification est journalisée dans une feuille Google Sheets avec le détail des tokens consommés et le coût estimé.

## Services / APIs utilisés
- Gmail (labels, threads, messages)
- DeepSeek API (classification via chat completions)
- Google Sheets (journalisation des classifications)

## Prérequis
- Un compte Gmail avec une arborescence de labels déjà définie (dossiers métier)
- Une clé API DeepSeek configurée en credential n8n (`deepSeekApi`)
- Une feuille Google Sheets pour la journalisation
- Accès OAuth2 Gmail et Google Sheets configurés en credentials n8n

## Variables d'environnement
| Variable | Description |
|---|---|
| YOUR_DEEPSEEK_API_KEY | Clé API DeepSeek (gérée via credential n8n `deepSeekApi`) |

## Schéma du flux
```
[Trigger planifié]
  --> [Get many labels] --> [Filtre labels] --> [Get many messages] --> [Trie par date] --> [Limite 150 emails]
  --> [Get a thread] --> [Détecte dossiers déjà appliqués]
  --> [Si email déjà classé ?]
        OUI --> [Add label] --> [Remove label INBOX]
        NON --> [Évalue nb emails] --> [Si nb > 50 ?]
                  OUI --> [Chauffe cache DeepSeek] --> [Vérifie cache chaud] --> [Si cache chaud ?]
                            NON --> [Attente 2s] --> (retry chauffe)
                  --> [Nettoie email] --> [Prépare prompt classif] --> [API DeepSeek - Classifie]
                  --> [Parse réponse JSON] --> [Si skip ?]
                            NON --> [Add label] --> [Remove label INBOX] --> [Append row Google Sheets]
```
