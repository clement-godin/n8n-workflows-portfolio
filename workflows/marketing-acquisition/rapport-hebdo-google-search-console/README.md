# Rapport hebdo Google Search Console

## Description
Ce workflow planifié interroge l'API Google Search Console pour comparer les performances SEO de la semaine écoulée avec celles de la semaine précédente (clics, impressions, CTR, position moyenne) sur les requêtes et pages du site. Il calcule les variations de position pour chaque requête, identifie les meilleures progressions et les principales régressions, puis génère un rapport texte formaté en HTML. Ce rapport est envoyé automatiquement via Telegram chaque semaine, permettant un suivi SEO sans ouvrir la Search Console.

## Services / APIs utilisés
- Google Search Console API (searchAnalytics/query, via OAuth2)
- Telegram Bot API (envoi du rapport formaté)

## Prérequis
- Un compte Google Search Console avec accès vérifié au domaine du site suivi.
- Des identifiants OAuth2 Google configurés dans n8n avec le scope Search Console.
- Un bot Telegram dédié pour la diffusion du rapport hebdomadaire.
- Le workflow est actuellement désactivé (`active: false`) dans l'instance source ; à réactiver et planifier selon le besoin.

## Variables d'environnement
| Variable | Description |
|---|---|
| YOUR_TELEGRAM_CHAT_ID | ID du chat Telegram recevant le rapport SEO hebdomadaire |

## Schéma du flux
```
[Trigger hebdo] --> [HTTP Request - Semaine courante] --> [HTTP Request - Semaine précédente]
                  --> [Code JS - Calcule variations SEO] --> [Code JS - Formate rapport SEO]
                  --> [Telegram - Envoie rapport SEO]
```
