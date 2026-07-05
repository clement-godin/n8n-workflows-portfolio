# Synchro HubSpot vers Evolyz

## Description
Ce workflow est déclenché par un webhook HubSpot lorsqu'un deal est modifié. Il récupère les détails complets du deal concerné via l'API HubSpot à partir de l'ID d'objet transmis par le webhook. Il s'agit d'un workflow en version initiale : seule l'étape de récupération du deal est implémentée à ce stade, la synchronisation effective vers l'outil de comptabilité/facturation Evolyz reste à construire.

## Services / APIs utilisés
- Webhook HubSpot (déclenchement sur modification de deal)
- HubSpot API (récupération des détails du deal)

## Prérequis
- Un abonnement webhook HubSpot configuré sur l'évènement de modification de deal
- Un compte HubSpot avec un token d'application pour l'accès API

## Variables d'environnement
Aucune variable spécifique à configurer pour la partie actuellement implémentée (les identifiants HubSpot sont gérés via le système de credentials n8n).

## Schéma du flux
```
[Webhook - Deal HubSpot modifié] --> [HubSpot - Récupère deal]  (suite du flux vers Evolyz à implémenter)
```
