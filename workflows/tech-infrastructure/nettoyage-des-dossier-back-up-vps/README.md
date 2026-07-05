# Nettoyage des dossiers de backup VPS

## Description
Ce workflow assure la rétention automatique des sauvegardes stockées sur Google Drive. Chaque jour à 4h du matin, il liste les sous-dossiers présents dans le dossier de backup du VPS (dossiers nommés par date), filtre ceux dont le nom de dossier correspond à une date antérieure à 30 jours, puis les supprime définitivement. Cela évite l'accumulation indéfinie d'archives de sauvegarde et maîtrise l'espace de stockage utilisé sur Drive.

## Services / APIs utilisés
- Google Drive API (listing de dossiers, suppression de dossiers)

## Prérequis
- Un compte Google Drive avec un dossier racine dédié aux sauvegardes VPS
- Une convention de nommage des sous-dossiers en date ISO (parsable par Luxon `DateTime.fromISO`)
- Les identifiants OAuth2 Google Drive configurés dans n8n

## Variables d'environnement
| Variable | Description |
|---|---|
| YOUR_GOOGLE_DRIVE_OAUTH_CREDENTIAL | Identifiants OAuth2 Google Drive |
| YOUR_BACKUP_ROOT_FOLDER_ID | ID du dossier Drive racine contenant les sauvegardes VPS |

## Schéma du flux
```
[Trigger planifié 4h] --> [Drive: liste les dossiers de backup]
   --> [Code: filtre les dossiers de plus de 30 jours]
   --> [Drive: supprime le dossier ancien]
```
