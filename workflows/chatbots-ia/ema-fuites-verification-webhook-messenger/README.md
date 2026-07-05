# EMA Fuites - Vérification Webhook Messenger

## Description
Ce workflow implémente l'étape de vérification (handshake) exigée par Meta lors de la configuration d'un webhook Messenger/Facebook. Il reçoit la requête GET de vérification envoyée par Meta, compare le `hub.verify_token` reçu à un token secret configuré, puis renvoie le `hub.challenge` tel quel si le mode est `subscribe` et que le token correspond. C'est un prérequis technique obligatoire avant que Meta n'active l'envoi des événements Messenger réels vers le webhook applicatif.

## Services / APIs utilisés
- Webhook n8n (réception de la requête de vérification Meta)
- Plateforme Meta / Facebook Messenger (côté appelant, vérification de webhook)

## Prérequis
- Une page Facebook/Messenger configurée avec un webhook pointant vers l'URL de ce workflow
- Un token de vérification (`verify_token`) identique des deux côtés (Meta et n8n)

## Variables d'environnement
| Variable | Description |
|---|---|
| YOUR_MESSENGER_VERIFY_TOKEN | Token de vérification partagé avec la configuration webhook Meta |

## Schéma du flux
```
[Webhook: requête de vérification Meta] --> [Code: vérifie le token]
   --> [Webhook: répond avec le challenge Meta]
```
