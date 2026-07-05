# Alerte CPA Google Ads

## Description
Ce workflow surveille en continu le coût par acquisition (CPA) des campagnes Google Ads actives d'EMA Fuites. Toutes les 6 heures, il interroge l'API Google Ads pour récupérer les statistiques du jour (coût, clics, conversions) de chaque campagne active, calcule le CPA réel, et envoie une alerte Telegram dès qu'une campagne dépasse le seuil de 100€ de CPA. Un mécanisme de déduplication basé sur le stockage interne du workflow (staticData, réinitialisé chaque jour) évite d'envoyer plusieurs fois la même alerte pour une campagne déjà signalée dans la journée.

## Services / APIs utilisés
- Google Ads API (recherche de statistiques de campagnes via `searchStream`)
- Telegram (envoi des alertes)

## Prérequis
- Un compte Google Ads avec accès API (OAuth2 + developer token)
- Un compte Manager Google Ads (MCC) si applicable, avec l'ID client de connexion (`login-customer-id`)
- Un bot Telegram et l'ID du chat destinataire des alertes

## Variables d'environnement
| Variable | Description |
|---|---|
| YOUR_GOOGLE_ADS_DEVELOPER_TOKEN | Developer token de l'API Google Ads |
| YOUR_GOOGLE_ADS_CUSTOMER_ID | ID du compte client Google Ads (et du login-customer-id si MCC) |
| YOUR_TELEGRAM_CHAT_ID | ID du chat Telegram recevant les alertes CPA |
| YOUR_TELEGRAM_BOT_TOKEN | Token du bot Telegram utilisé pour les alertes |

## Schéma du flux
```
[Trigger - Toutes les 6h] --> [Google Ads - Stats du jour] --> [Calcul CPA + dédoublonnage] --> [Telegram - Alerte CPA]
```
