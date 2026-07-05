# Back Up Notion

## Description
Ce workflow réalise une sauvegarde complète de l'espace de travail Notion vers Google Drive. Il crée (ou réutilise) un dossier daté du jour, récupère toutes les bases de données Notion, puis pour chacune, toutes les pages et leurs blocs de contenu. Chaque page est reconstituée en un fichier JSON complet (métadonnées, propriétés, contenu des blocs) puis uploadée dans une arborescence de dossiers Drive miroir de la structure Notion (un dossier par base de données, un sous-dossier par page). Les fichiers et images attachés aux pages (propriétés ou blocs) sont également téléchargés depuis Notion et re-uploadés sur Drive à côté du JSON de la page.

## Services / APIs utilisés
- Notion API (bases de données, pages, blocs)
- Google Drive (création de dossiers, upload de fichiers)
- HTTP Request générique (téléchargement des fichiers Notion)

## Prérequis
- Un compte d'intégration Notion avec accès en lecture à l'espace de travail à sauvegarder
- Un compte Google Drive OAuth2 avec accès en écriture au dossier de backup
- Un dossier racine Google Drive dédié aux sauvegardes Notion

## Variables d'environnement
| Variable | Description |
|---|---|
| YOUR_GOOGLE_DRIVE_BACKUP_FOLDER_ID | ID du dossier Google Drive racine "BACK UP NOTION" |

## Schéma du flux
```
[Trigger - Backup Notion] --> [Search files and folders] --> [Si - Dossier backup existe]
   --> [Create folder / Create folder2] --> [Get many databases1] --> [Create folder1]
   --> [Merge - Notion + Drive] --> [Get many database pages] --> [Code JS - Extrait titres]
   --> [Get many child blocks] --> [Code JS - Construit JSON pages]
   --> [Si - Fichiers à télécharger] --> [Split Out] --> [API HTTP - Télécharge fichier]
   --> [Upload file1] (fichiers) / [Upload file2] (JSON page)
```
