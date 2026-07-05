# Requête AntsRoute — Search availabilities Planning

## Description
Ce workflow interroge l'API AntsRoute (outil de planification de tournées d'intervention) pour rechercher les créneaux disponibles sur une période donnée, en fonction d'un type de prestation, d'une durée et de plages horaires souhaitées, ainsi que de la localisation du client (adresse + coordonnées GPS). Il s'agit d'un workflow de test/exploration de l'API de recherche de disponibilités, utilisée pour synchroniser le planning d'intervention avec la disponibilité réelle des techniciens.

## Services / APIs utilisés
- AntsRoute API (planification/tournées)

## Prérequis
- Un compte AntsRoute avec accès à l'API "search-availabilities"
- Une clé API AntsRoute (header "cakey")

## Variables d'environnement
| Variable | Description |
|---|---|
| YOUR_ANTSROUTE_API_KEY | Clé API AntsRoute transmise dans le header "cakey" pour authentifier la requête |

## Schéma du flux
```
[Trigger manuel] --> [API ANTS - Recherche des créneaux disponibles (POST search-availabilities)]
```
