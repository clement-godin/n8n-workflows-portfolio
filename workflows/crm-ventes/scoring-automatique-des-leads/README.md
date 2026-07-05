# Scoring automatique des leads

## Description
Ce workflow reçoit un lead entrant via webhook (nom, ville, urgence déclarée, budget, description) et le fait analyser par un modèle Claude (Anthropic) avec un prompt système métier propre à EMA Fuites. Le modèle retourne un score de 1 à 10, un niveau de priorité (URGENT/HAUTE/NORMALE/BASSE), une estimation du délai d'intervention et une estimation de la valeur du dossier. Un nœud Switch route ensuite la notification vers l'un de trois messages Telegram différenciés selon la priorité, pour que l'équipe puisse rappeler les leads les plus chauds en priorité.

## Services / APIs utilisés
- Webhook n8n (authentification header)
- Anthropic Claude (node LangChain, modèle claude-haiku-4-5)
- Telegram Bot API (3 bots différents selon la priorité)

## Prérequis
- Une clé API Anthropic valide
- Un webhook sécurisé par Header Auth pour recevoir les leads entrants (ex: depuis un formulaire de site ou un CRM)
- Un ou plusieurs bots Telegram configurés pour recevoir les notifications de leads

## Variables d'environnement
| Variable | Description |
|---|---|
| YOUR_ANTHROPIC_API_KEY | Clé API Anthropic utilisée pour le scoring des leads (référencée via credential n8n) |
| YOUR_TELEGRAM_CHAT_ID | Identifiant du chat Telegram destinataire des notifications de leads |

## Schéma du flux
```
[Webhook - Scoring lead] --> [Claude - Score lead IA] --> [Code JS - Parse score Claude]
  --> [Switch - Priorité lead] --> [Telegram URGENT | Telegram HAUTE | Telegram NORMALE/BASSE]
```
