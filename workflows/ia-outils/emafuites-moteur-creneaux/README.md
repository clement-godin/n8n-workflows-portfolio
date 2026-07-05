# EMAFUITES - Moteur de créneaux

## Description
Ce workflow expose un webhook interne qui calcule les 3 meilleurs créneaux de rendez-vous disponibles pour une intervention, en tenant compte de la charge et de la proximité géographique des techniciens. Il géocode l'adresse du client via Nominatim (OpenStreetMap), génère les prochains jours ouvrés, interroge HubSpot pour connaître les interventions déjà planifiées, puis calcule un score par jour/technicien basé sur la distance moyenne aux interventions existantes et la charge de la journée. Les 3 meilleurs créneaux sont renvoyés sous forme de réponse JSON, avec une phrase reformulée destinée à un assistant conversationnel ("Léa").

## Services / APIs utilisés
- Nominatim / OpenStreetMap (géocodage d'adresse)
- HubSpot CRM (recherche des deals planifiés)
- Webhook n8n interne (protégé par header d'authentification)

## Prérequis
- Un compte HubSpot avec les propriétés personnalisées `rdv_lat`, `rdv_lng`, `rdv_date`, `technicien_assigne`, `duree_intervention`
- Un secret d'authentification pour protéger le webhook entrant
- Coordonnées de départ (domicile/dépôt) de chaque technicien à renseigner dans le code

## Variables d'environnement
| Variable | Description |
|---|---|
| YOUR_WEBHOOK_PATH | Chemin du webhook interne à sécuriser (remplacé ici par un placeholder) |
| YOUR_HUBSPOT_TOKEN | Jeton d'application HubSpot (configuré via le système de credentials n8n) |

## Schéma du flux
```
[Webhook - Moteur créneaux] --> [Prépare les jours ouvrés] --> [Géocode adresse (Nominatim)]
   --> [Extrait coordonnées] --> [Cherche deals planifiés (HubSpot)]
   --> [Calcule score par jour/technicien] --> [Formate réponse] --> [Respond to Webhook]
```
