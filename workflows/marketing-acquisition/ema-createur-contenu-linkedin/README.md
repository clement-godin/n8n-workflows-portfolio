# EMA - Créateur de contenu LinkedIn

## Description
Ce workflow génère des brouillons de posts LinkedIn pour l'entreprise EMA Fuites à partir d'un sujet, d'un angle éditorial et de notes brutes envoyés via webhook. Un prompt structuré (ton expert local, ancrage régional Hauts-de-France, structure accroche/développement/CTA, interdits stylistiques) est construit puis envoyé à l'API Claude (Anthropic) pour générer le texte du post ainsi que des accroches alternatives. La réponse est parsée puis envoyée sur Telegram pour validation humaine avant publication. Le webhook d'entrée est protégé par une authentification par en-tête.

## Services / APIs utilisés
- Webhook n8n sécurisé par authentification par en-tête (Header Auth)
- API Anthropic Claude (`api.anthropic.com/v1/messages`, modèle Claude Sonnet)
- Telegram Bot API (envoi du brouillon pour validation)

## Prérequis
- Un secret d'en-tête à configurer côté webhook (et côté appelant) pour sécuriser l'entrée
- Une clé API Anthropic
- Un bot Telegram et l'ID du chat destinataire pour la validation des posts

## Variables d'environnement
| Variable | Description |
|---|---|
| YOUR_WEBHOOK_HEADER_SECRET | Secret utilisé pour authentifier les appels entrants au webhook |
| YOUR_ANTHROPIC_API_KEY | Clé API Anthropic (Claude) pour la génération du post |
| YOUR_TELEGRAM_BOT_TOKEN | Token du bot Telegram utilisé pour la validation |
| YOUR_TELEGRAM_CHAT_ID | ID du chat/utilisateur Telegram recevant le brouillon à valider |

## Schéma du flux
```
[Webhook LinkedIn] --> [Code: prépare le prompt du post]
   --> [Claude: génère le post] --> [Code: parse le JSON de réponse]
   --> [Telegram: envoie le post à valider]
```
