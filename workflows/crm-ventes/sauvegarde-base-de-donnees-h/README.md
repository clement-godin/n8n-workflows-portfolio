# Sauvegarde base de données /H

## Description
Ce workflow effectue une sauvegarde automatique de la base de données PostgreSQL (Metabase) utilisée par EMA Fuites. Toutes les heures, il se connecte en SSH à un conteneur Docker hébergeant PostgreSQL, exécute un `pg_dump` de la base, convertit le résultat en fichier `.sql` et l'envoie sur Google Drive dans un dossier de sauvegarde dédié. Le nom du fichier inclut un horodatage précis pour faciliter le suivi des versions.

## Services / APIs utilisés
- SSH (connexion à un serveur distant hébergeant Docker/PostgreSQL)
- Docker exec + pg_dump (dump de base de données)
- Google Drive (stockage des sauvegardes)

## Prérequis
- Un serveur accessible en SSH avec une clé privée configurée dans n8n
- Un conteneur Docker exécutant PostgreSQL (ici via Metabase) accessible localement sur ce serveur
- Un compte Google Drive connecté avec les droits d'écriture sur le dossier de sauvegarde cible
- Le mot de passe PostgreSQL configuré comme variable d'environnement lors de l'exécution de la commande

## Variables d'environnement
| Variable | Description |
|---|---|
| YOUR_DB_PASSWORD | Mot de passe de l'utilisateur PostgreSQL utilisé par `pg_dump` (transmis via `PGPASSWORD`) |

## Schéma du flux
```
[Trigger - Toutes les heures] --> [SSH - Dump base de données (pg_dump)] --> [Convertit stdout en fichier .sql] --> [Upload sur Google Drive]
```
