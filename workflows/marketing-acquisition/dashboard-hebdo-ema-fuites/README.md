# Dashboard Hebdo EMA Fuites

## Description
Ce workflow génère chaque lundi à 8h00 un tableau de bord hebdomadaire combinant les performances publicitaires et commerciales de l'entreprise. Il interroge en parallèle l'API Google Ads (dépense, clics, conversions et CPA par campagne sur les 7 derniers jours) et l'API HubSpot (deals créés dans la période). Un nœud de code agrège ces données pour calculer les indicateurs clés (CPA moyen, meilleure/pire campagne, nombre de leads, devis envoyés, deals closés/perdus), les enregistre dans une feuille Google Sheets "DASHBOARD" pour historisation, puis envoie une synthèse formatée sur Telegram.

## Services / APIs utilisés
- Google Ads API (statistiques de campagnes)
- HubSpot API (recherche de deals)
- Google Sheets API (historisation du dashboard)
- Telegram Bot API (notification du rapport)

## Prérequis
- Un compte Google Ads avec accès API et un jeton développeur (developer token)
- Un compte HubSpot avec token d'application
- Une feuille Google Sheets avec un onglet "DASHBOARD" (colonnes : Semaine, Date, Dépense Ads, Clics, Conversions, CPA Moyen, Leads Total, Deals Closés, Deals Perdus, Devis Envoyés, Best/Worst Campagne, Best/Worst CPA)
- Un bot Telegram pour la diffusion du rapport

## Variables d'environnement
| Variable | Description |
|---|---|
| YOUR_GOOGLE_ADS_CUSTOMER_ID | ID client Google Ads (customer ID et login-customer-id) |
| YOUR_GOOGLE_ADS_DEVELOPER_TOKEN | Jeton développeur Google Ads |
| SHEETS_LEADS_ID_A_CONFIGURER | ID de la feuille Google Sheets du dashboard (placeholder déjà présent dans le workflow, à configurer) |
| YOUR_TELEGRAM_CHAT_ID | Identifiant du chat Telegram destinataire du rapport |

## Schéma du flux
```
[Trigger lundi 8h] --> [Google Ads: stats 7 jours] --+
                    --> [HubSpot: deals semaine] -----+--> [Agrège et formate]
   --> [Sheets: enregistre dashboard] --> [Telegram: rapport hebdo]
```
