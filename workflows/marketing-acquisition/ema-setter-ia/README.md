# EMA - Setter IA

## Description
Ce workflow automatise la qualification des réponses de prospects B2B reçues en cours de séquence de prospection (email, LinkedIn, etc.). Un webhook reçoit la réponse brute du prospect avec son historique de conversation, un prompt structuré est envoyé à Claude (Anthropic) pour classifier l'intention (intérêt fort, curiosité, politesse froide, objection, refus) et générer une réponse commerciale adaptée au ton et à la longueur du message reçu. Le résultat met à jour le CRM (Google Sheets) avec l'intention détectée et une note de suivi, puis notifie l'équipe commerciale sur Telegram avec la réponse suggérée. Si le prospect est écarté, la fiche est automatiquement marquée comme telle dans le tableau de suivi.

## Services / APIs utilisés
- Webhook n8n (authentification par header secret)
- Anthropic API (Claude) — analyse d'intention et génération de réponse
- Google Sheets — CRM de suivi des prospects
- Telegram — notification à l'équipe commerciale

## Prérequis
- Une instance n8n avec un node Webhook exposé et sécurisé par header auth
- Un compte Anthropic API avec une clé valide
- Une feuille Google Sheets "Prospects" avec les colonnes ID, Intention_réponse, Notes, Statut
- Un bot Telegram et l'ID du chat/canal de notification

## Variables d'environnement
| Variable | Description |
|---|---|
| YOUR_ANTHROPIC_API_KEY | Clé API Anthropic (credential n8n `anthropicApi`) |
| YOUR_GOOGLE_SHEET_ID | ID de la feuille Google Sheets utilisée comme CRM |
| YOUR_TELEGRAM_CHAT_ID | ID du chat Telegram recevant les notifications commerciales |
| YOUR_TELEGRAM_BOT_TOKEN | Token du bot Telegram (credential n8n `telegramApi`) |
| YOUR_GOOGLE_OAUTH_CREDENTIALS | Credentials OAuth2 Google Sheets |

## Schéma du flux
```
[Webhook - Setter IA]
        --> [Code - Préparer Setter] (construit le prompt d'analyse)
        --> [Claude - Setter Analyse] (appel API Anthropic)
        --> [Code - Parser Setter] (parse la réponse JSON de Claude)
        --> [IF - Row ID fourni]
              --oui--> [GSheets - Update CRM] --> [Telegram - Réponse à traiter]
                              --> [IF - Action ÉCARTÉ] --oui--> [GSheets - Marquer ÉCARTÉ]
              --non-> [Telegram - Réponse (sans CRM)]
```
