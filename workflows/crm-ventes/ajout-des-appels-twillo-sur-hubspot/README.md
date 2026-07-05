# Ajout des appels Twilio sur HubSpot

## Description
Ce workflow enregistre manuellement un appel téléphonique (typiquement passé via Twilio) comme un "engagement" de type appel sur la fiche contact HubSpot correspondante. Il part d'un déclenchement manuel, récupère d'abord le contact HubSpot concerné, puis crée un engagement d'appel (durée, statut, numéros appelant/appelé) rattaché à ce contact. Il sert de brique de synchronisation entre la téléphonie (Twilio) et le CRM HubSpot lorsque l'intégration native n'est pas utilisée.

## Services / APIs utilisés
- HubSpot API (récupération de contact, création d'engagement d'appel) via App Token

## Prérequis
- Un compte HubSpot avec un App Token disposant des scopes CRM (contacts + engagements)
- Connaître l'ID du contact HubSpot à mettre à jour (ou adapter le workflow pour le rechercher dynamiquement, par exemple depuis un événement Twilio)

## Variables d'environnement
| Variable | Description |
|---|---|
| YOUR_HUBSPOT_APP_TOKEN | Token d'application HubSpot (accès CRM contacts/engagements) |
| YOUR_HUBSPOT_CONTACT_ID | ID du contact HubSpot concerné par l'appel |
| YOUR_PHONE_NUMBER | Numéro de téléphone (appelant/appelé) à associer à l'engagement d'appel |

## Schéma du flux
```
[Trigger manuel] --> [HubSpot: récupère le contact] --> [HubSpot: crée l'engagement d'appel]
```
