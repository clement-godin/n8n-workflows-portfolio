# RAPPORT JOURNALIER CAMPAGNE GOOGLE ADS

## Description
Ce workflow envoie chaque matin à 8h un rapport de performance des campagnes Google Ads actives de la veille. Il interroge l'API Google Ads (Google Ads Query Language) pour récupérer impressions, clics, coût, conversions, CTR et CPC par campagne. Un node de code agrège ces métriques, calcule le coût par acquisition (CPA) global et déclenche une alerte si le budget journalier dépasse un seuil défini. Le rapport final est formaté en HTML et envoyé sur Telegram.

## Services / APIs utilisés
- Google Ads API (googleAds:searchStream)
- Telegram Bot API

## Prérequis
- Un compte Google Ads avec accès API et un developer token validé
- Des credentials OAuth2 Google Ads configurés dans n8n
- Un bot Telegram créé via BotFather et son chat ID de destination
- Adapter le customer ID Google Ads (`customers/{id}`) au compte cible

## Variables d'environnement
| Variable | Description |
|---|---|
| YOUR_GOOGLE_ADS_DEVELOPER_TOKEN | Developer token Google Ads (header `developer-token`) |
| YOUR_TELEGRAM_CHAT_ID | ID du chat Telegram destinataire du rapport |

## Schéma du flux
```
[Schedule Trigger 8h] --> [API Google Ads - Métriques campagnes]
  --> [Code JS - Agrège métriques] --> [Code JS - Formate rapport]
  --> [Telegram - Envoie rapport]
```
