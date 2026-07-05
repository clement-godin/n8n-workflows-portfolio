# Nettoyage Back Up N8N

## Description
Ce workflow s'exécute automatiquement chaque nuit à 1h00 pour nettoyer les doublons présents dans le dossier de sauvegarde des workflows n8n hébergé sur Google Drive. Il se connecte au serveur en SSH pour lancer une commande `rclone dedupe` (mode "newest", conserve la version la plus récente en cas de doublon), puis analyse la sortie du terminal pour détecter les erreurs et compter le nombre de doublons supprimés. Un rapport de synthèse (succès ou erreur) est ensuite envoyé sur Telegram.

## Services / APIs utilisés
- n8n Schedule Trigger (planification quotidienne)
- SSH (exécution de la commande `rclone dedupe` sur le serveur)
- rclone (synchronisation/dédoublonnage vers Google Drive)
- Telegram Bot API (notification du rapport)

## Prérequis
- Un accès SSH avec clé privée configuré vers le serveur hébergeant n8n
- rclone installé et configuré sur le serveur avec un remote Google Drive nommé `gdrive`
- Un bot Telegram pour recevoir les rapports de nettoyage

## Variables d'environnement
| Variable | Description |
|---|---|
| YOUR_TELEGRAM_CHAT_ID | Identifiant du chat Telegram destinataire du rapport |

## Schéma du flux
```
[Trigger planifié 1h00] --> [SSH: rclone dedupe] --> [Code: parse résultat] --> [Telegram: rapport nettoyage]
```
