# Workflow 4 — Déduplication Plombiers pour suppressions

## Description
Ce workflow détecte et supprime les fiches "company" (entreprises de plomberie prospectées) en doublon dans HubSpot. Il récupère l'ensemble des companies et des deals associés via l'API HubSpot, puis un nœud Code compare les numéros de téléphone normalisés de chaque company pour repérer les doublons. Pour chaque doublon détecté, le workflow supprime d'abord le deal associé (s'il existe) puis la fiche company en double, afin de garder un CRM propre pour la prospection.

## Services / APIs utilisés
- HubSpot CRM (companies, deals)

## Prérequis
- Un compte HubSpot avec un App Token (Private App) ayant les droits de lecture/suppression sur les objets companies et deals
- La propriété "phone" renseignée sur les fiches companies pour permettre la détection de doublons

## Variables d'environnement
Aucune valeur sensible codée en dur — l'authentification HubSpot passe entièrement par le système de credentials n8n (App Token référencé par credential, pas de clé en clair dans le JSON).

## Schéma du flux
```
[Trigger manuel] --> [HubSpot - Toutes companies] --> [HubSpot - Tous deals]
                  --> [Code JS - Détecte doublons (comparaison téléphone)]
                  --> [Si - Est doublon ?] --> [Supprime deal] --> [Supprime company]
```
