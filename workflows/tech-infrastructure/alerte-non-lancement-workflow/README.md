# ALERTE NON LANCEMENT WORKFLOW

## Description
Workflow de surveillance qui vérifie chaque jour à 1h30 que le workflow critique "SAUVEGARDE WORKFLOW N8N" s'est bien exécuté dans la journée. Il interroge l'API n8n pour récupérer les 5 dernières exécutions de ce workflow, filtre celles du jour même (fuseau Europe/Paris), et si aucune exécution n'est trouvée pour aujourd'hui, envoie une alerte Telegram signalant que le workflow de sauvegarde ne s'est pas lancé.

## Services / APIs utilisés
- API n8n (self, via credential n8nApi) — récupération de l'historique d'exécutions d'un autre workflow
- Telegram Bot API — notification d'alerte

## Prérequis
- Une clé API n8n (Settings > API) associée au compte propriétaire de l'instance
- L'ID du workflow à surveiller ("SAUVEGARDE WORKFLOW N8N") configuré dans le nœud de récupération d'exécutions
- Un bot Telegram et son chat ID de destination

## Variables d'environnement
| Variable | Description |
|---|---|
| YOUR_TELEGRAM_CHAT_ID | Identifiant du chat/groupe Telegram recevant l'alerte de non-lancement |

## Schéma du flux
```
[Schedule Trigger 01:30] --> [n8n - Récupère exécutions] --> [Code JS - Filtre exécutions du jour]
   --> [Si - Aucune exécution ?] --> (vrai) [Telegram - Alerte non-lancement]
```
