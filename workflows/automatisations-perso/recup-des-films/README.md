# Récupération des films (usage personnel)

## Description
Ce workflow personnel centralise dans un seul Google Sheet les films vus au cinéma UGC, à partir de deux sources : les emails de confirmation UGC reçus sur une boîte Gmail, et les événements créés dans deux agendas Google Calendar (un agenda personnel et un agenda partagé "iPhone"). À chaque déclenchement, il récupère en parallèle les emails contenant "ugc" et les événements des deux calendriers dont le lieu contient "UGC", puis ajoute une ligne (date + nom du film) dans une feuille Google Sheets dédiée pour chaque élément détecté.

## Services / APIs utilisés
- Gmail API (recherche d'emails `from:ugc`)
- Google Calendar API (lecture de deux agendas : personnel et "iPhone")
- Google Sheets API (ajout de lignes dans un tableau de suivi des films vus)

## Prérequis
- Un compte Gmail recevant les confirmations de réservation UGC
- Deux agendas Google Calendar accessibles (personnel + partagé), avec les événements cinéma renseignant le champ "lieu" contenant "UGC"
- Un Google Sheet de suivi avec les colonnes "DATE" et "NOM DU FILM"

## Variables d'environnement
| Variable | Description |
|---|---|
| YOUR_GMAIL_OAUTH_CREDENTIAL | Identifiants OAuth2 Gmail |
| YOUR_GOOGLE_CALENDAR_OAUTH_CREDENTIAL | Identifiants OAuth2 Google Calendar |
| YOUR_GOOGLE_SHEETS_OAUTH_CREDENTIAL | Identifiants OAuth2 Google Sheets |
| YOUR_PERSONAL_CALENDAR_ID | ID de l'agenda Google Calendar personnel |
| YOUR_SHARED_CALENDAR_ID | ID de l'agenda Google Calendar partagé (ex. "iPhone") |
| YOUR_FILMS_SPREADSHEET_ID | ID du Google Sheet de suivi des films vus |

## Schéma du flux
```
[Trigger planifié] --+--> [Gmail: emails UGC] --> [Tri par date] --> [Si email UGC ?]
                      |         --> [Sheets: enregistre film (email)]
                      |
                      +--> [Calendar: événements perso] --> [Si événement UGC perso ?]
                      |         --> [Sheets: enregistre film (perso)]
                      |
                      +--> [Calendar: événements iPhone] --> [Si événement UGC iPhone ?]
                                --> [Sheets: enregistre film (iPhone)]
```
