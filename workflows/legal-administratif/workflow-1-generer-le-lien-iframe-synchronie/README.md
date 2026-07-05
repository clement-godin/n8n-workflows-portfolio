# Workflow 1 : Générer le lien Iframe Synchronie

## Description
Ce workflow expose un webhook interne permettant à un autre système (typiquement Notion ou un CRM) de demander la génération d'un lien de signature électronique. Il reçoit le nom, l'email et l'identifiant de la page source du signataire, appelle l'API de la plateforme de signature (self-hosted, compatible DocuSeal) pour créer une "submission" à partir d'un template prédéfini, puis renvoie au demandeur l'URL publique de l'iframe de signature à intégrer. C'est un micro-service interne qui découple la génération de lien de signature du reste de la logique métier.

## Services / APIs utilisés
- API de signature électronique self-hosted (compatible DocuSeal) — `sign.emafuites.fr`
- Webhook n8n interne (authentification par header)

## Prérequis
- Une instance de signature électronique self-hosted exposant une API `/api/submissions`
- Un template de document de signature déjà créé sur la plateforme (`template_id`)
- Un credential HTTP Header Auth pour sécuriser l'appel entrant et un credential pour authentifier l'appel sortant vers l'API de signature

## Variables d'environnement
| Variable | Description |
|---|---|
| YOUR_WEBHOOK_SECRET | Secret du header d'authentification protégeant le webhook entrant |
| YOUR_DOCUSEAL_API_KEY | Clé API de la plateforme de signature électronique (credential HTTP Header Auth) |

## Schéma du flux
```
[Webhook - Demande lien Synchronie] --> [API Synchronie - Génère iframe] --> [Webhook - Répond lien Synchronie]
```
