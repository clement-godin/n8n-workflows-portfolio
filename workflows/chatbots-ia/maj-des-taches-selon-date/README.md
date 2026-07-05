# MAJ des tâches selon date

## Description
Ce workflow synchronise automatiquement le statut des tâches ClickUp en fonction de leur date d'échéance. Toutes les heures (à la 3e minute), il interroge une liste ClickUp spécifique pour récupérer les tâches dont l'échéance tombe dans la journée en cours et dont le statut est "à planifier", "en cours" ou "prévue demain". Chaque tâche trouvée est alors automatiquement basculée au statut "prévue aujourd'hui", ce qui permet de garder une vue à jour des priorités du jour sans intervention manuelle.

## Services / APIs utilisés
- ClickUp API (lecture de tâches filtrées par échéance/statut, mise à jour de statut)

## Prérequis
- Un espace de travail ClickUp avec une liste dédiée à la gestion des tâches quotidiennes
- Des statuts personnalisés ClickUp nommés "planifier", "in progress", "prevue demain" et "prevue auj"
- Credentials n8n configurés : ClickUp API

## Variables d'environnement
| Variable | Description |
|---|---|
| (aucune) | Les identifiants ClickUp (team/space/folder/list) sont à adapter à votre propre espace de travail dans le nœud "ClickUp - Récupère tâches du jour" |

## Schéma du flux
```
[Trigger horaire] --> [ClickUp - Récupère tâches du jour] --> [ClickUp - Met à jour statut tâche]
```
