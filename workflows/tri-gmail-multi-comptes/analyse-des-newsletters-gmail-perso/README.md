# Analyse des newsletters Gmail perso

## Description
Ce workflow génère chaque lundi matin un digest condensé des newsletters reçues dans la semaine sur une boîte Gmail personnelle. Il récupère tous les emails portant un label "newsletters" reçus dans les 7 derniers jours, nettoie leur contenu HTML, puis les envoie à DeepSeek qui les analyse et regroupe les informations utiles par thématique en ignorant le contenu promotionnel et les redites. Le digest est envoyé sur Telegram (en plusieurs messages si nécessaire pour respecter la limite de caractères), puis les emails traités sont marqués par un label "digest fait" pour ne pas être réanalysés la semaine suivante.

## Services / APIs utilisés
- Gmail API (lecture des emails par label, ajout de label)
- DeepSeek API (synthèse et classification thématique du contenu)
- Telegram Bot API (envoi du digest hebdomadaire)

## Prérequis
- Un compte Gmail avec un label appliqué aux newsletters à analyser (filtre Gmail dédié recommandé)
- Un second label Gmail "digest fait" pour marquer les emails déjà traités
- Un compte DeepSeek avec clé API
- Un bot Telegram créé via @BotFather et son chat ID
- Credentials n8n configurés : Gmail OAuth2, DeepSeek API, Telegram API

## Variables d'environnement
| Variable | Description |
|---|---|
| YOUR_TELEGRAM_CHAT_ID | Identifiant du chat/groupe Telegram recevant le digest hebdomadaire |

## Schéma du flux
```
[Trigger hebdo lundi 6h] --> [Gmail - Récupère newsletters 7j] --> [Code JS - Prépare (nettoie HTML)] --> [Si newsletters trouvées ?]
                                                                                                                    |
                                                                          --> [Code JS - Prépare prompt DeepSeek] --> [API DeepSeek - Analyse]
                                                                                                                    |
                                                                    --> [Code JS - Parse réponse] --> [Telegram - Envoie digest] --> [Gmail - Ajoute label digest]
```
