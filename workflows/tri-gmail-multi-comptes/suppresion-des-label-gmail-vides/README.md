# Suppression des labels Gmail vides

## Description
Ce workflow de maintenance parcourt tous les labels (dossiers) d'une boîte Gmail et supprime automatiquement ceux qui sont vides et ne sont pas des labels système. Il récupère la liste complète des labels, va chercher le détail de chacun (nombre de messages), filtre ceux ayant `messagesTotal = 0` et un type différent de `system`, puis les supprime un par un. Utile pour garder une arborescence de tri Gmail propre après des campagnes de classification automatique (ex: tri IA) qui créent parfois des labels inutilisés.

## Services / APIs utilisés
- Gmail (API native via n8n, OAuth2)

## Prérequis
- Un compte Google avec accès Gmail et l'API Gmail activée
- Des scopes OAuth2 incluant la gestion des labels (lecture + suppression)

## Variables d'environnement
| Variable | Description |
|---|---|
| (aucune) | Ce workflow ne contient aucun secret en dur, uniquement une credential Gmail OAuth2 à configurer dans n8n |

## Schéma du flux
```
[Manual Trigger] --> [Get many labels] --> [Get a label info] --> [Filter: label vide & non système] --> [Delete a label]
```
