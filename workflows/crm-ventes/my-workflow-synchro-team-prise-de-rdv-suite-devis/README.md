# SYNCHRO TEAM - Prise de RDV suite Devis

## Description
Workflow (en cours de construction, inactif) destiné à automatiser la prise de rendez-vous d'intervention suite à un devis signé. Il expose un webhook sécurisé par en-tête d'authentification qui reçoit la demande de création d'intervention, répond immédiatement au client, puis interroge l'API AntsRoute (logiciel de planification de tournées) pour rechercher les créneaux de disponibilité correspondant à l'adresse et à la durée de l'intervention.

## Services / APIs utilisés
- Webhook n8n sécurisé par authentification header
- API AntsRoute (recherche de disponibilités de tournées / planification d'interventions)

## Prérequis
- Une clé API AntsRoute (cakey)
- Un secret partagé pour sécuriser le webhook entrant (header auth)
- Compte AntsRoute configuré avec les types d'ordres de service utilisés

## Variables d'environnement
| Variable | Description |
|---|---|
| YOUR_ANTSROUTE_API_KEY | Clé API AntsRoute (header `cakey`) utilisée pour rechercher les disponibilités |

## Schéma du flux
```
[Webhook - Prise de RDV Devis] --> [Webhook - Répond création intervention] --> [API ANTS - Recherche disponibilités]
```
