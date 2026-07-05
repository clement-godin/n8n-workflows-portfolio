# EMAFUITES - Optimisation tournée

## Description
Ce workflow optimise automatiquement les tournées d'intervention des techniciens à chaque création/modification d'un rendez-vous dans HubSpot (deal). Il vérifie que le rendez-vous a une date et une adresse, géocode l'adresse via Nominatim (OpenStreetMap) si les coordonnées GPS ne sont pas déjà connues, puis interroge HubSpot pour récupérer tous les autres rendez-vous du même jour. Le technicien le moins chargé est assigné automatiquement, puis le moteur d'optimisation de tournées Vroom calcule l'ordre optimal des interventions à partir du domicile de chaque technicien. Les résultats (ordre de tournée, heure d'arrivée estimée, technicien assigné) sont réécrits dans HubSpot et un évènement est créé dans le Google Calendar du technicien concerné.

## Services / APIs utilisés
- HubSpot API (lecture/mise à jour des deals - rendez-vous d'intervention)
- Nominatim / OpenStreetMap (géocodage d'adresse)
- Vroom (moteur d'optimisation de tournées, instance auto-hébergée)
- Google Calendar API (création d'évènements de planning)

## Prérequis
- Des propriétés personnalisées HubSpot sur l'objet "deal" : rdv_date, rdv_adresse, rdv_lat, rdv_lng, duree_intervention, technicien_assigne, ordre_tournee
- Une instance Vroom accessible sur le réseau (ici via le nom de service Docker `vroom`)
- Les coordonnées GPS du point de départ/retour de chaque technicien
- Un calendrier Google par technicien pour la création des évènements

## Variables d'environnement
| Variable | Description |
|---|---|
| YOUR_TECH1_HOME_COORDS | Coordonnées GPS (latitude/longitude) du point de départ du technicien 1 |
| YOUR_TECH2_HOME_COORDS | Coordonnées GPS (latitude/longitude) du point de départ du technicien 2 |

## Schéma du flux
```
[Webhook / HubSpot Trigger] --> [Si RDV complet] --> [Si coords manquantes]
   --oui--> [Nominatim: géocode adresse] --> [HubSpot: update deal] --+
   --non-------------------------------------------------------------+--> [Merge coords résolues]
   --> [Normaliser deal] --> [HubSpot: cherche deals du jour] --> [Assigne technicien]
   --> [Build payload Vroom] --> [Vroom: optimise route] --> [Traite réponse Vroom]
   --> [HubSpot: update deal (ordre + technicien)] --> [Google Calendar: créer évènement]
```
