# Surveillance avis Google

## Description
Ce workflow surveille en continu (toutes les 30 minutes) les nouveaux avis Google déposés sur la fiche Google My Business d'EMA Fuites. Il interroge l'API Google My Business pour récupérer le compte, les établissements puis les avis, compare leur date de mise à jour à un timestamp de dernière vérification stocké en mémoire statique du workflow, et ne garde que les avis réellement nouveaux. Pour chaque nouvel avis, Claude (Anthropic) rédige une proposition de réponse professionnelle adaptée à la note (remerciement personnalisé pour les avis positifs, excuses et proposition de résolution en privé pour les avis négatifs). Le résultat est mis en forme puis envoyé sur Telegram, avec un lien vers Google Business, pour validation et publication manuelle par l'équipe.

## Services / APIs utilisés
- Google My Business API (comptes, établissements, avis) via OAuth2
- API Anthropic Claude (rédaction de la réponse à l'avis, modèle Haiku)
- Telegram Bot API (notification de l'avis + réponse suggérée)

## Prérequis
- Un compte Google My Business avec accès OAuth2 configuré (scopes lecture avis)
- Une clé API Anthropic
- Un bot Telegram et l'ID du chat destinataire des alertes

## Variables d'environnement
| Variable | Description |
|---|---|
| YOUR_GOOGLE_MY_BUSINESS_OAUTH_CREDENTIAL | Identifiants OAuth2 Google My Business |
| YOUR_ANTHROPIC_API_KEY | Clé API Anthropic (Claude) pour la rédaction des réponses |
| YOUR_TELEGRAM_BOT_TOKEN | Token du bot Telegram utilisé pour les alertes |
| YOUR_TELEGRAM_CHAT_ID | ID du chat/utilisateur Telegram recevant les alertes d'avis |

## Schéma du flux
```
[Trigger planifié 30 min] --> [API GMB: liste des comptes] --> [API GMB: liste des établissements]
   --> [API GMB: liste des avis] --> [Code: filtre les nouveaux avis]
   --> [Si nouveaux avis ?] --> [Claude: rédige une réponse]
   --> [Code: formate le message] --> [Telegram: envoie l'alerte]
```
