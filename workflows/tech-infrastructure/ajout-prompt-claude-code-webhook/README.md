# AJOUT PROMPT CLAUDE CODE (WEBHOOK)

## Description
Ce workflow expose un webhook sécurisé permettant d'ajouter à distance (par exemple depuis un site web ou un formulaire) un nouveau prompt destiné à être traité plus tard par Claude Code. Il valide les champs obligatoires (titre, répertoire du projet, prompt), normalise le modèle demandé (sonnet/opus/haiku) et la priorité, génère un identifiant unique horodaté, puis enregistre la ligne dans un Google Sheet qui sert de file d'attente. Une notification Telegram confirme l'ajout, et une réponse JSON (200 ou 400) est renvoyée à l'appelant. Un autre workflow planifié vient ensuite lire cette file pour exécuter les prompts en attente.

## Services / APIs utilisés
- n8n Webhook (authentification par header secret)
- Google Sheets (file d'attente `claude_code_prompts`)
- Telegram Bot API (notification de confirmation)

## Prérequis
- Un Google Sheet dédié avec un onglet `claude_code_prompts` et les colonnes attendues (id, titre, repertoire_projet, statut, priorite, date_creation, date_lancement, date_fin, resume, erreur, prompt, modele).
- Un bot Telegram et l'identifiant de chat où envoyer les notifications.
- Un secret partagé (header HTTP) pour authentifier les appels entrants au webhook.
- Un workflow d'erreur configuré (`errorWorkflow`) pour capter les échecs d'exécution.

## Variables d'environnement
| Variable | Description |
|---|---|
| YOUR_GOOGLE_SHEET_ID | ID du Google Sheet servant de file d'attente des prompts |
| YOUR_TELEGRAM_CHAT_ID | ID du chat Telegram recevant les notifications de confirmation |

## Schéma du flux
```
[Webhook] --> [Validation (Code)] --> [Valide ?]
                                        ├─ non --> [Réponse 400]
                                        └─ oui --> [Ajoute ligne (Google Sheets)] --> [Telegram] --> [Réponse 200]
```
