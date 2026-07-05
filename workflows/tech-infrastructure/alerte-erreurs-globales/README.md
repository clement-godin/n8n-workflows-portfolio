# Alerte Erreurs Globales

## Description
Ce workflow est le gestionnaire d'erreurs centralisé de l'instance n8n. Configuré comme "Error Workflow" par défaut sur la plupart des autres workflows de l'instance, il se déclenche automatiquement dès qu'une exécution échoue ailleurs et envoie une alerte Telegram formatée indiquant le nom du workflow en erreur, le nœud fautif, le message d'erreur et un lien direct vers l'exécution dans l'interface n8n. C'est la brique d'observabilité qui permet de réagir rapidement à toute panne d'automatisation.

## Services / APIs utilisés
- Telegram Bot API

## Prérequis
- Un bot Telegram dédié aux alertes techniques
- Configurer ce workflow comme "Error Workflow" dans les settings des autres workflows de l'instance (déjà fait sur la plupart des workflows EMA Fuites, référencé via son ID)

## Variables d'environnement
| Variable | Description |
|---|---|
| YOUR_TELEGRAM_CHAT_ID | Identifiant du chat Telegram destinataire des alertes d'erreur critiques |

## Schéma du flux
```
[Error Trigger - déclenché par l'échec d'un autre workflow] --> [Telegram - Alerte erreur critique avec lien vers l'exécution]
```
