# Webhook POST /mshdf-ai

## Description
Ce workflow expose un webhook public qui sert de proxy sécurisé vers l'API Anthropic (Claude) pour un site vitrine de rénovation (marque MSHDF). Il reçoit un prompt utilisateur et un prompt système depuis le frontend, applique un rate-limiting par IP (une requête max toutes les 4 secondes), nettoie et envoie le prompt à Claude Haiku, puis nettoie le markdown de la réponse avant de la retourner en JSON brut au site web appelant.

## Services / APIs utilisés
- Anthropic API (Claude Haiku) pour la génération de texte
- Webhook n8n avec authentification par header

## Prérequis
- Un compte Anthropic avec une clé API valide
- Une authentification par header configurée sur le webhook pour limiter les appels non autorisés
- Un site web frontend capable d'appeler ce webhook en POST avec `{ userPrompt, systemPrompt }`

## Variables d'environnement
| Variable | Description |
|---|---|
| YOUR_ANTHROPIC_API_KEY | Clé API Anthropic (Claude) utilisée pour générer les réponses |
| YOUR_WEBHOOK_SECRET | Secret d'authentification par header protégeant le webhook public |

## Schéma du flux
```
[Webhook POST /mshdf-ai] --> [Rate limit IP] --> [Si non bloqué ?]
                                                        |--> [Prépare prompt Claude] --> [API Anthropic] --> [Nettoie markdown] --> [Répond résultat IA]
                                                        |--> [Répond bloqué]
```
