# Sauvegarde Workflow n8n

## Description
Ce workflow assure la sauvegarde automatique et organisée de tous les workflows d'une instance n8n vers Google Drive, en respectant l'arborescence de tags de chaque workflow (1 ou 2 tags = 1 ou 2 niveaux de dossiers). Il récupère la liste des tags via l'API n8n et l'arborescence Drive existante via une commande `rclone` exécutée en SSH, afin de faire correspondre les tags aux dossiers déjà présents. Pour chaque workflow actif, il crée (si besoin) le dossier du workflow puis le sous-dossier du jour, supprime un éventuel doublon du jour, puis exporte le workflow en JSON et l'upload. Il supprime aussi les workflows archivés et envoie un rapport Telegram signalant les workflows actifs sans workflow d'erreur configuré, ainsi qu'un résumé de fin de sauvegarde.

## Services / APIs utilisés
- API n8n (liste des workflows, tags, suppression des workflows archivés)
- Google Drive (création de dossiers, upload/suppression de fichiers JSON)
- SSH (exécution d'une commande `rclone lsf` pour lister l'arborescence Drive)
- Telegram (notifications et rapports d'erreur)

## Prérequis
- Un compte API n8n (clé API de l'instance à sauvegarder)
- Un compte Google Drive OAuth2 avec accès en écriture au dossier de sauvegarde
- Un accès SSH avec `rclone` configuré vers le remote Google Drive ("gdrive:")
- Un bot Telegram et son chat ID
- Une organisation des workflows par tags n8n reflétant les dossiers Drive existants

## Variables d'environnement
| Variable | Description |
|---|---|
| YOUR_GOOGLE_DRIVE_BACKUP_FOLDER_ID | ID du dossier Google Drive racine "BACK UP N8N" |
| YOUR_TELEGRAM_CHAT_ID | Chat ID Telegram destinataire des rapports |

## Schéma du flux
```
[Cron - Toutes les heures] --> [n8n API - Récupère tags] --> [Execute Command - Liste dossiers Drive (SSH/rclone)]
   --> [n8n API - Récupère tous les workflows] --> [Si - Workflow archivé ?]
       --> (oui) [n8n API - Supprime workflow archivé]
       --> (non) [Code JS - Vérifie config notif erreurs] --> [Telegram - Rapport notif erreurs]
                 [Convertit workflow en fichier JSON] --> [SplitInBatches - Boucle principale workflows]
   --> [Code JS - intersection niveau1 & niveau2] --> [Code JS - Normalise tags]
   --> [Google Drive - Cherche dossier tag1] --> (1 ou 2 tags) --> [Cherche/Crée dossier workflow]
   --> [Cherche/Crée dossier date] --> [Vérifie doublon] --> [Supprime doublon si besoin]
   --> [Google Drive - Upload JSON] --> [Telegram - Backup terminé]
```
