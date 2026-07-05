# Notification de changement de statut

## Description
Ce workflow écoute les événements de changement de statut de tâches sur ClickUp (déclencheur natif ClickUp Trigger). Dès qu'une tâche change de statut, il récupère le détail complet de la tâche via l'API ClickUp, puis envoie une notification formatée sur Telegram indiquant le nom de la tâche, l'ancien statut et le nouveau statut. C'est un workflow simple de notification interne pour rester informé de l'avancement d'un board ClickUp sans avoir à ouvrir l'application.

## Services / APIs utilisés
- ClickUp (trigger + API)
- Telegram Bot API

## Prérequis
- Un compte ClickUp avec une intégration API/Webhook configurée sur l'espace de travail concerné
- Un bot Telegram (via BotFather) et l'identifiant de chat du destinataire des notifications
- Credentials ClickUp et Telegram configurées dans n8n

## Variables d'environnement
| Variable | Description |
|---|---|
| YOUR_TELEGRAM_CHAT_ID | Identifiant du chat Telegram destinataire des notifications de statut |

## Schéma du flux
```
[Trigger ClickUp - changement de statut] --> [ClickUp - Récupère la tâche complète] --> [Telegram - Envoie notification du changement de statut]
```
