# EMAFUITES - Notification tournée J-1

## Description
Ce workflow prépare et diffuse la veille au soir la tournée d'interventions du lendemain pour les techniciens EMA Fuites. Chaque jour à 19h (sauf si le lendemain est un week-end), il recherche dans HubSpot les rendez-vous programmés pour le jour suivant, les regroupe par technicien assigné, puis fait appel au moteur d'optimisation de tournées VROOM pour calculer l'ordre optimal des visites et les heures d'arrivée estimées à partir du domicile de chaque technicien. Le résultat est formaté en un message lisible et envoyé par Telegram à chaque technicien, puis le CRM HubSpot et le Google Calendar sont mis à jour avec les informations de tournée.

## Services / APIs utilisés
- HubSpot CRM (recherche des deals/rendez-vous, mise à jour)
- VROOM / OSRM (moteur d'optimisation de tournées, calcul d'itinéraire)
- Telegram (notification de la tournée au technicien)
- Google Calendar (mise à jour de l'événement)

## Prérequis
- Un compte HubSpot avec les propriétés personnalisées `rdv_date`, `rdv_lat`, `rdv_lng`, `rdv_adresse`, `technicien_assigne`, `duree_intervention`, `ordre_tournee`, `eta_arrivee`
- Une instance VROOM accessible en interne (ex: conteneur Docker `vroom`)
- Un bot Telegram par technicien (ou un bot central) et son chat ID
- Un compte Google Calendar OAuth2

## Variables d'environnement
| Variable | Description |
|---|---|
| YOUR_TELEGRAM_CHAT_ID | Chat ID Telegram du technicien destinataire |

## Schéma du flux
```
[Trigger - Notif tournée J-1 (19h)] --> [Code - Date demain] --> [Si - Jour ouvré]
   --> [API HubSpot - Cherche RDV demain] --> [Code - Groupe deals par tech]
   --> [Code - Build Vroom final] --> [API Vroom - Calcule tournée]
   --> [Code - Formate notification] --> [Boucle - Par technicien]
   --> [Telegram - Envoie tournée] + [HubSpot - Maj deal tournée] --> [Calendar - Maj événement]
```
