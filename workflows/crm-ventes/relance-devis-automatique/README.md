# Relance devis automatique

## Description
Ce workflow automatise la relance des clients ayant reçu un devis (deals HubSpot au statut `qualifiedtobuy`) mais n'ayant pas encore répondu. Toutes les heures, il interroge l'API HubSpot pour trouver les deals à relancer, calcule le délai écoulé depuis la dernière relance et applique une cadence en trois temps : J+2 par WhatsApp, J+5 par email (Gmail), puis J+10 par alerte Telegram au commercial pour un suivi manuel. Après chaque relance automatique, le workflow met à jour les propriétés HubSpot du deal (nombre de relances, date de dernière relance) et journalise l'action sous forme de note HubSpot.

## Services / APIs utilisés
- API HubSpot (recherche de deals, associations de contacts, mise à jour de propriétés, création de note)
- API WhatsApp Cloud (Meta Graph API) pour la relance J+2
- Gmail (OAuth2) pour la relance J+5
- Telegram Bot API pour l'alerte commerciale J+10

## Prérequis
- Un token d'application HubSpot avec accès aux objets deals/contacts/engagements.
- Un compte WhatsApp Business (Meta) avec un numéro de téléphone configuré et un token d'accès.
- Un compte Gmail connecté en OAuth2 pour l'envoi des relances email.
- Un bot Telegram pour les alertes commerciales.
- Des propriétés personnalisées HubSpot sur l'objet deal : `nb_relances`, `derniere_relance`, `a_relancer_manuellement`.

## Variables d'environnement
| Variable | Description |
|---|---|
| YOUR_HUBSPOT_TOKEN | Token d'application HubSpot |
| YOUR_WHATSAPP_PHONE_NUMBER_ID | ID du numéro de téléphone WhatsApp Business (Meta Graph API) |
| YOUR_WHATSAPP_TOKEN | Token d'accès Meta WhatsApp (stocké via le système de credentials n8n) |
| YOUR_TELEGRAM_CHAT_ID | ID du chat Telegram recevant les alertes de relance manuelle |

## Schéma du flux
```
[Trigger horaire] --> [HubSpot - Deals devis envoyé] --> [Filtre deals à relancer]
                    --> [Get associations contact] --> [Extraire contact] --> [A un contact ?]
                    --> [Get contact] --> [Données contact] --> [Switch - Canal de relance]
                         ├─ J+2  --> [WhatsApp]
                         ├─ J+5  --> [Gmail]
                         └─ J+10 --> [Telegram - Alerte commercial]
                    --> [Prépare mise à jour] --> [Est J10 ?] --> [HubSpot - Update/Tag] --> [HubSpot - Create note]
```
