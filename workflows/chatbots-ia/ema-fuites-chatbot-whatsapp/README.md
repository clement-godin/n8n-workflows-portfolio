# EMA Fuites - Chatbot WhatsApp

## Description
Chatbot conversationnel WhatsApp Business qui qualifie automatiquement les leads entrants pour EMA Fuites. Il reçoit les messages via l'API Cloud de Meta, gère la vérification du webhook (challenge GET), maintient un état de conversation par numéro de téléphone, et utilise l'API DeepSeek pour poser les questions de qualification (type de fuite, ville, urgence) et détecter les demandes de handover humain. Chaque échange est miroité dans Chatwoot (création de contact/conversation, log des messages entrants et sortants) pour donner une vue humaine sur la conversation bot. Une fois le lead qualifié, il est enregistré dans Google Sheets et une notification Telegram est envoyée à l'équipe ; en cas de demande explicite d'un humain, le bot bascule en mode "handover" et alerte l'équipe via Telegram tout en réassignant la conversation Chatwoot.

## Services / APIs utilisés
- WhatsApp Cloud API (Meta Graph API) — réception et envoi de messages
- DeepSeek API (deepseek-chat) — moteur conversationnel de qualification des leads
- Chatwoot API (self-hosted, chat.emafuites.fr) — miroir humain des conversations, gestion contacts/handover
- Google Sheets — enregistrement des leads qualifiés
- Telegram Bot API — notifications d'équipe (nouveau lead, handover humain)

## Prérequis
- Un compte WhatsApp Business avec l'API Cloud Meta configurée (token d'accès + phone_number_id)
- Un token de vérification de webhook Meta (hub.verify_token)
- Une clé API DeepSeek
- Une instance Chatwoot avec un compte (account_id=1) et un inbox WhatsApp dédié (inbox_id=2)
- Une feuille Google Sheets avec un onglet `leads_whatsapp`
- Un bot Telegram et son chat ID de destination
- Credentials n8n configurés : DeepSeek API, Meta WhatsApp Access Token (header auth), Chatwoot API (header auth), Google Sheets OAuth2, Telegram API

## Variables d'environnement
| Variable | Description |
|---|---|
| YOUR_WEBHOOK_VERIFY_TOKEN | Token de vérification utilisé lors du handshake GET du webhook Meta |
| VOTRE_SPREADSHEET_ID_EMA_FUITES | ID du Google Sheet où sont enregistrés les leads qualifiés |
| YOUR_TELEGRAM_CHAT_ID | Identifiant du chat/groupe Telegram recevant les notifications |

## Schéma du flux
```
[Webhook GET - Vérification Meta] --> [Vérifie token] --> [Répond challenge 200 / 403]

[Webhook POST - Reçoit messages WhatsApp] --> [Parse message entrant] --> [Chatwoot - Prépare entrant]
   --> [Chatwoot - Recherche/Créer contact] --> [Chatwoot - Liste/Créer conversation] --> [Chatwoot - Log message entrant]
   --> [Si - Message valide ?] --> [Charge état conversation] --> [Si - Skip IA (handover actif) ?]
   --> [Prépare prompt DeepSeek] --> [API DeepSeek] --> [Parse réponse DeepSeek]
   --> [Si - Handover humain ?]
        --> (oui) [Chatwoot - Lien conversation] --> [Telegram - Alerte handover] --> [Chatwoot - Assigne/Ouvre conversation]
        --> (non) [API Meta - Envoie réponse WhatsApp] --> [Chatwoot - Log réponse bot]
   --> [Si - Lead qualifié ?] --> [Google Sheets - Sauvegarde lead] --> [Telegram - Notif nouveau lead]
```
