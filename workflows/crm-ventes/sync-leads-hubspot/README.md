# SYNC LEADS → HUBSPOT

## Description
Ce workflow centralise dans HubSpot les leads entrants provenant de deux sources : un formulaire du site web (via webhook) et une feuille Google Sheets alimentée par un bot WhatsApp (lue toutes les 5 minutes). Les données brutes sont normalisées (téléphone au format international, email en minuscule, nom complet reconstruit), puis un contact HubSpot est recherché par téléphone/email. Si le contact n'existe pas, il est créé ; sinon il est mis à jour. Une opportunité (deal) est ensuite créée et associée au contact, et une notification Telegram récapitule le nouveau lead à l'équipe.

## Services / APIs utilisés
- HubSpot CRM API (recherche/création/mise à jour de contacts, création de deals)
- Google Sheets (lecture des leads WhatsApp)
- Telegram Bot API (notification d'équipe)
- Webhook n8n (réception des soumissions du formulaire site)

## Prérequis
- Un compte HubSpot avec un token d'application privée (scopes contacts + deals)
- Une feuille Google Sheets contenant les leads WhatsApp avec les colonnes Date/Prénom/Nom/Téléphone/Email/Ville/Type de fuite
- Un bot Telegram créé via @BotFather et son chat ID (groupe ou utilisateur destinataire)
- Credentials n8n configurés : Google Sheets OAuth2, HubSpot App Token, Telegram API

## Variables d'environnement
| Variable | Description |
|---|---|
| SHEETS_LEADS_ID_A_CONFIGURER | ID du Google Sheet contenant l'onglet `leads_whatsapp` |
| YOUR_TELEGRAM_CHAT_ID | Identifiant du chat/groupe Telegram recevant les notifications de nouveaux leads |

## Schéma du flux
```
[Schedule Trigger 5min] --> [Google Sheets - Lire leads] --> [Code JS - Filtre nouvelles lignes] --\
                                                                                                     --> [Code JS - Normalise lead] --> [HubSpot - Rechercher contact] --> [Code JS - Contact existe ?] --> [Si - Nouveau ou existant ?]
[Webhook - Formulaire site] --------------------------------------------------------------------/                                                                              |                    |
                                                                                                                                          [HubSpot - Créer contact]   [HubSpot - Màj contact]
                                                                                                                                                    \                    /
                                                                                                                                                     --> [Code JS - Prépare deal] --> [HubSpot - Créer deal] --> [Telegram - Notif lead]
```
