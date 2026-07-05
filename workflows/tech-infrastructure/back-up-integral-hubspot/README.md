# Back up intégral HubSpot

## Description
Ce workflow réalise une sauvegarde complète et quotidienne (3h du matin) de l'ensemble des données HubSpot d'EMA Fuites : deals, contacts, entreprises, engagements (activités) et tickets. Chaque type d'objet est extrait via l'API HubSpot avec ses propriétés détaillées, converti en fichier CSV horodaté, puis uploadé dans un dossier Google Drive dédié. Une routine de nettoyage supprime ensuite automatiquement les sauvegardes de plus d'un mois pour éviter l'accumulation de fichiers sur le long terme.

## Services / APIs utilisés
- HubSpot CRM (deals, contacts, companies, engagements, tickets)
- Google Drive (stockage des CSV de sauvegarde, nettoyage automatique)

## Prérequis
- Un compte HubSpot avec un token d'application privée ayant accès en lecture aux objets deals/contacts/companies/engagements/tickets
- Un compte Google Drive avec des dossiers dédiés pour chaque type de sauvegarde (deals, contacts, entreprises, engagements, tickets)

## Variables d'environnement
| Variable | Description |
|---|---|
| YOUR_HUBSPOT_TOKEN | Token d'application privée HubSpot utilisé pour l'extraction des données |

## Schéma du flux
```
[Trigger quotidien 3h] --> [Search deals] --> [Get deal détaillé] --> [Convertit en CSV] --> [Upload Drive]
   --> [Search contacts] --> [Get contact détaillé] --> [Convertit en CSV] --> [Upload Drive]
   --> [Get many companies] --> [Convertit en CSV] --> [Upload Drive]
   --> [Get many engagements] --> [Convertit en CSV] --> [Upload Drive]
   --> [Get many tickets] --> [Convertit en CSV] --> [Upload Drive]
   --> [Recherche fichiers du dossier BACK UP HUBSPOT] --> [Filtre fichiers > 1 mois] --> [Supprime les anciens fichiers]
```
