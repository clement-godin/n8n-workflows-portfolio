# EMA Fuites - Chatbot Messenger IA

## Description
Ce workflow implémente un chatbot conversationnel sur Facebook Messenger pour qualifier automatiquement les leads entrants d'EMA Fuites. Il reçoit les messages via un webhook Meta, filtre les événements non pertinents (échos, accusés de lecture, messages non-texte), puis maintient un état de conversation persistant par utilisateur (mémoire de workflow n8n). Un prompt système envoyé à DeepSeek collecte progressivement le type de fuite, la ville et l'urgence, tout en détectant les demandes explicites de contact humain ou les villes hors zone de couverture. Une fois le lead qualifié, il est enregistré dans Google Sheets et une alerte Telegram est envoyée à l'équipe ; en cas de handover humain, une alerte Telegram dédiée coupe le bot pour cette conversation.

## Services / APIs utilisés
- Meta Graph API (webhook + envoi de messages Messenger)
- DeepSeek API (analyse conversationnelle et extraction d'informations)
- Google Sheets (enregistrement des leads qualifiés)
- Telegram Bot API (notifications d'équipe et alertes de handover)

## Prérequis
- Une Page Facebook connectée à une app Meta avec permission `pages_messaging` et webhook configuré sur le endpoint du workflow
- Un token d'accès Page Facebook valide (à renouveler selon expiration)
- Un compte DeepSeek avec clé API
- Une feuille Google Sheets avec un onglet `leads_messenger`
- Un bot Telegram créé via @BotFather et son chat ID
- Credentials n8n configurés : DeepSeek API, Google Sheets OAuth2, Telegram API

## Variables d'environnement
| Variable | Description |
|---|---|
| YOUR_META_PAGE_ACCESS_TOKEN | Token d'accès de la Page Facebook utilisé pour envoyer les réponses via l'API Graph |
| YOUR_TELEGRAM_CHAT_ID | Identifiant du chat/groupe Telegram recevant les notifications de leads et les alertes de handover |
| VOTRE_SPREADSHEET_ID_EMA_FUITES | ID du Google Sheet où sont enregistrés les leads qualifiés |

## Schéma du flux
```
[Webhook Messenger] --> [Parse message] --> [Si message valide ?] --> [Charge état conversation] --> [Si skip IA ?]
                                                                                                              |
                                                        --> [Si handover/non-texte ?] --> [API Meta - réponse non-texte]
                                                                                                              |
                                                              --> [Prépare prompt DeepSeek] --> [API DeepSeek] --> [Parse réponse]
                                                                                                              |
                                                                                          --> [API Meta - Envoie réponse Messenger] --> [Si handover humain ?]
                                                                                                                                              |
                                                                            --> [Telegram - Alerte handover]      --> [Si lead qualifié ?] --> [Google Sheets - Sauvegarde lead] --> [Telegram - Notif nouveau lead]
```
