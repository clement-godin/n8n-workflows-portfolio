# Automatic Travel Time Updates Between Google Calendar Appointments

## Description
Ce workflow ajoute et maintient automatiquement des blocs "temps de trajet" dans Google Calendar entre les rendez-vous physiques d'une journée. À chaque création ou modification d'un événement (avec une adresse valide, hors visio/téléphone/domicile), il récupère tous les événements du jour, identifie l'événement précédent et suivant, puis calcule le temps de trajet réel via l'API Google Maps Distance Matrix (origine = domicile si premier rendez-vous, adresse du rendez-vous précédent/suivant sinon). Un événement de trajet est créé, mis à jour ou supprimé selon les cas, avec un tag caché dans la description permettant de retrouver et lier les événements de trajet à l'événement d'origine lors des exécutions suivantes.

## Services / APIs utilisés
- Google Calendar (trigger création/mise à jour, lecture/écriture/suppression d'événements)
- Google Maps Distance Matrix API (calcul du temps de trajet routier)
- n8n Data Table (stockage de l'adresse du domicile par utilisateur)

## Prérequis
- Un compte Google Calendar OAuth2
- Une clé Google Maps API (Distance Matrix)
- Une Data Table n8n "liste domicile" avec une colonne email et une colonne adresse
- Configurer l'adresse du domicile et le nom de l'événement de trajet dans le node "Workflow Configuration"

## Variables d'environnement
| Variable | Description |
|---|---|
| VOTRE_ADRESSE_DOMICILE | Adresse de départ/retour par défaut pour le calcul du premier/dernier trajet |
| your-email@domain.com | Adresse du calendrier Google à surveiller |

## Schéma du flux
```
[Google Calendar Trigger (create/update)] --> [Filter (adresse physique valide)]
   --> [Workflow Configuration] --> [Get All Events of the Day]
   --> [If] --> [Find Old Linked Travel] / [Sort Events and Find Adjacent]
   --> [Has Previous Event? / Has Next Event?]
   --> [Get Travel Time Before/After (CREATE/UPDATE)] (Google Maps API)
   --> [Create/Update/Delete Previous/Next Travel Time Event] (Google Calendar)
```
