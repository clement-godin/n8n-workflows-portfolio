# GRAPHIFY REBUILD NOCTURNE

## Description
Workflow d'infrastructure qui déclenche chaque nuit à 3h un script de rebuild ("graphify") sur le serveur applicatif EMA Fuites via SSH. Le résultat (stdout/stderr) de l'exécution est évalué pour déterminer si le rebuild s'est terminé avec succès, puis un résumé est envoyé sur Telegram avec le statut, l'horodatage et un extrait des éventuelles erreurs.

## Services / APIs utilisés
- SSH (exécution de commande à distance sur le serveur applicatif)
- Telegram Bot API — notification du statut du rebuild

## Prérequis
- Un accès SSH par clé privée au serveur hébergeant `ema-fuites/app`
- Le script `graphify-rebuild.sh` déployé sur le serveur dans `/home/debian/scripts/`
- Un bot Telegram et son chat ID de destination

## Variables d'environnement
| Variable | Description |
|---|---|
| YOUR_TELEGRAM_CHAT_ID | Identifiant du chat/groupe Telegram recevant la notification de rebuild |

## Schéma du flux
```
[Schedule Trigger 03:00] --> [Execute a command (SSH)] --> [Evaluer Resultat] --> [Telegram - Notification statut]
```
