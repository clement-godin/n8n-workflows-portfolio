# Brief Stratégique hebdomadaire

## Description
Ce workflow s'exécute chaque lundi à 6h00 pour produire un brief de veille stratégique hebdomadaire. Il collecte des actualités depuis plusieurs sources en parallèle : recherches Tavily sur l'IA, le business, la cybersécurité et le développement web, flux RSS d'actualités locales (Hauts-de-France) et flux Google Alerts personnalisés (Claude/Anthropic, DeepSeek, n8n, détection de fuite d'eau, automatisation no-code). Toutes ces sources sont fusionnées puis compilées en un corpus texte, qui est envoyé à l'API DeepSeek pour être analysé et structuré en thèmes avec points clés, source et caractère actionnable. Le digest généré est enfin formaté en HTML et envoyé sur Telegram, en le découpant en plusieurs messages si nécessaire pour respecter la limite de caractères de Telegram.

## Services / APIs utilisés
- Tavily API (recherche web par thématique)
- Flux RSS (actualités locales et Google Alerts)
- DeepSeek API (analyse et structuration du corpus en JSON)
- Telegram Bot API (envoi du brief)

## Prérequis
- Un compte Tavily avec clé API
- Un compte DeepSeek avec clé API
- Des flux Google Alerts configurés sur les thématiques souhaitées
- Un bot Telegram pour la diffusion du brief

## Variables d'environnement
| Variable | Description |
|---|---|
| YOUR_TELEGRAM_CHAT_ID | Identifiant du chat Telegram destinataire du brief |
| YOUR_GOOGLE_ALERTS_ID | Identifiant de compte Google Alerts (partie de l'URL des flux RSS) |
| YOUR_ALERT_ID_1..5 | Identifiant de chaque alerte Google Alerts spécifique |

## Schéma du flux
```
[Trigger lundi 6h] --> [Tavily IA/Business/Sécurité/Dev] --> [Merge recherches Tavily] --+
                    --> [RSS Actu Lille/ICI Nord] --> [Merge actu locale] ---------------+--> [Merge toutes sources]
                    --> [RSS Alerts x5] --> [Merge alertes Google] ----------------------+
   --> [Compile corpus] --> [Prépare prompt] --> [DeepSeek: analyse corpus]
   --> [Formate brief] --> [Telegram: envoie brief]
```
