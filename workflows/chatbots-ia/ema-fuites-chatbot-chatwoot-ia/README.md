# EMA Fuites - Chatbot Chatwoot IA

## Description
Chatbot conversationnel branché sur Chatwoot (webhook `message_created`) qui qualifie automatiquement les leads entrants sur les canaux gérés par Chatwoot (site web, réseaux, etc.). Il ignore les messages sortants/système, maintient un état de conversation par `conversation_id`, et utilise l'API DeepSeek pour poser les questions de qualification (type de fuite, ville dans les Hauts-de-France, urgence) et détecter une demande de handover humain. La réponse de l'IA est renvoyée directement dans Chatwoot. En cas de demande explicite d'un humain, la conversation est réassignée (assignee_id: null) et l'équipe est alertée via Telegram avec un lien direct vers la conversation. Une fois le lead qualifié, il est enregistré dans Google Sheets et notifié par Telegram.

## Services / APIs utilisés
- Chatwoot API (self-hosted, chat.emafuites.fr) — réception des messages via webhook, envoi de réponses, gestion des assignations
- DeepSeek API (deepseek-chat) — moteur conversationnel de qualification des leads
- Google Sheets — enregistrement des leads qualifiés
- Telegram Bot API — notifications d'équipe (nouveau lead, handover humain)

## Prérequis
- Une instance Chatwoot avec un webhook configuré sur l'événement `message_created` pointant vers ce workflow
- Un jeton d'accès API Chatwoot (api_access_token) avec droits d'écriture sur les conversations
- Une clé API DeepSeek
- Une feuille Google Sheets avec un onglet `leads_chatwoot`
- Un bot Telegram et son chat ID de destination
- Credentials n8n configurés : DeepSeek API, Google Sheets OAuth2, Telegram API

## Variables d'environnement
| Variable | Description |
|---|---|
| YOUR_CHATWOOT_API_TOKEN | Jeton d'accès API Chatwoot (api_access_token) utilisé pour envoyer les réponses et gérer les assignations |
| VOTRE_SPREADSHEET_ID_EMA_FUITES | ID du Google Sheet où sont enregistrés les leads qualifiés |
| YOUR_TELEGRAM_CHAT_ID | Identifiant du chat/groupe Telegram recevant les notifications |

## Schéma du flux
```
[Webhook - Reçoit messages Chatwoot] --> [Parse message Chatwoot] --> [Si - Message valide ?]
   --> [Charge état conversation] --> [Si - Handover actif ?]
   --> (non) [Prépare prompt DeepSeek] --> [API DeepSeek] --> [Parse réponse DeepSeek] --> [API Chatwoot - Envoie réponse]
   --> [Si - Handover humain ?]
        --> (oui) [API Chatwoot - Marque conversation ouverte] --> [Telegram - Alerte handover]
        --> (non) [Si - Lead qualifié ?] --> [Google Sheets - Sauvegarde lead] --> [Telegram - Notif nouveau lead]
```
