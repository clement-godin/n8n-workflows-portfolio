# Backup DNS OVH

## Description
Ce workflow sauvegarde quotidiennement (1h du matin) la configuration des zones DNS hébergées chez OVH. Il authentifie chaque appel à l'API OVH en générant lui-même la signature attendue (algorithme SHA-1 implémenté en JavaScript pur, combinant clé secrète, timestamp serveur OVH et paramètres de requête), récupère la liste des zones DNS, puis exporte le fichier de zone de chaque domaine et l'upload dans un dossier Google Drive daté du jour. Une seconde partie du workflow applique une politique de rétention : elle recherche les dossiers de sauvegarde plus vieux que 30 jours et les supprime pour éviter l'accumulation.

## Services / APIs utilisés
- API OVH (`eu.api.ovh.com`) — authentification par signature applicative (Application Key / Application Secret / Consumer Key)
- Google Drive API (création de dossiers, upload de fichiers, suppression des anciens backups)

## Prérequis
- Un compte OVH avec une application API créée (Application Key/Secret) et un Consumer Key autorisé sur les droits DNS (lecture des zones + export)
- Un compte Google Drive avec un dossier racine dédié aux backups DNS
- Les identifiants OAuth2 Google Drive configurés dans n8n

## Variables d'environnement
| Variable | Description |
|---|---|
| YOUR_OVH_APPLICATION_KEY | Application Key de l'API OVH |
| YOUR_OVH_APPLICATION_SECRET | Application Secret de l'API OVH |
| YOUR_OVH_CONSUMER_KEY | Consumer Key OVH autorisé sur les droits DNS |
| YOUR_GOOGLE_DRIVE_OAUTH_CREDENTIAL | Identifiants OAuth2 Google Drive |
| YOUR_BACKUP_ROOT_FOLDER_ID | ID du dossier Drive racine des backups DNS |

## Schéma du flux
```
[Trigger planifié 1h] --> [Drive: crée dossier daté] --> [Config API: clés OVH]
   --> [OVH: récupère le timestamp serveur] --> [Code: signe la requête zones]
   --> [OVH: liste les zones DNS] --> [Code: signe la requête export par domaine]
   --> [OVH: exporte la zone] --> [Drive: upload du fichier]
   --> [Drive: liste les dossiers de backup] --> [Code: filtre dossiers > 30j]
   --> [Drive: supprime le dossier ancien]
```
