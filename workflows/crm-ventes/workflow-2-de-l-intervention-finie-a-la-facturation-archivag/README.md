# Workflow 2 : De l'Intervention Finie à la Facturation / Archivage

## Description
Ce workflow (inactif, à l'état de brouillon) reçoit via webhook la notification de fin d'intervention envoyée par le workflow de synchronisation d'équipe. Il distingue deux cas selon le statut transmis : intervention terminée (`state: done`) ou intervention en échec (`state: failed`). Dans le cas d'une intervention terminée, le flux est censé déclencher la création d'une facture dans HubSpot. Le nœud HTTP Request vers HubSpot n'est pas encore configuré (paramètres vides), ce qui indique un chantier en cours d'implémentation plutôt qu'un flux de production terminé.

## Services / APIs utilisés
- n8n Webhook (authentification par header secret)
- API HubSpot (création de facture — intégration non finalisée)

## Prérequis
- Un secret partagé (header HTTP) pour authentifier l'appel webhook entrant, envoyé par le workflow amont de fin d'intervention.
- Un token d'application HubSpot avec les scopes nécessaires à la création d'objets facture/deal (à configurer sur le nœud HTTP Request).

## Variables d'environnement
| Variable | Description |
|---|---|
| YOUR_HUBSPOT_TOKEN | Token d'application HubSpot (à configurer via le système de credentials n8n) |

## Schéma du flux
```
[Webhook - Intervention finie] --> [Switch - Type action]
                                      ├─ done   --> [API HubSpot - Crée facture] (non finalisé)
                                      └─ failed --> (aucune branche connectée)
```
