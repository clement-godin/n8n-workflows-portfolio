# Workflow 2 : Sauvegarder le document signé (asynchrone)

## Description
Ce workflow complète un processus de signature électronique de devis via DocuSeal. Il expose un webhook qui reçoit de façon asynchrone la notification de callback de DocuSeal une fois le document signé par le client. À la réception, il met à jour la page Notion correspondante (identifiée via l'`external_id` du signataire, qui référence l'ID de la page Notion) en y attachant le fichier PDF du devis signé récupéré depuis l'URL fournie par DocuSeal.

## Services / APIs utilisés
- Webhook n8n (réception du callback DocuSeal)
- DocuSeal (signature électronique de documents, source du callback)
- Notion API (mise à jour de page, ajout de fichier à une propriété)

## Prérequis
- Un compte DocuSeal configuré pour envoyer un callback vers l'URL de ce webhook à la signature d'un document
- Que l'`external_id` du soumissionnaire DocuSeal corresponde bien à l'ID de la page Notion à mettre à jour
- Une intégration Notion avec accès en écriture à la base de données concernée, et une propriété fichier nommée de façon cohérente (ex. "DEVIS SIGNE")

## Variables d'environnement
| Variable | Description |
|---|---|
| YOUR_NOTION_API_KEY | Clé d'intégration Notion (accès lecture/écriture à la base de données) |
| YOUR_DOCUSEAL_CALLBACK_SECRET | Secret/validation éventuelle du callback DocuSeal (si authentification ajoutée côté webhook) |

## Schéma du flux
```
[Webhook: callback DocuSeal signature] --> [Notion: attache le devis signé à la page]
```
