# SCAN TICKET ET ETIQUETTE

## Description
Ce workflow personnel permet, via un bot Telegram, d'envoyer une photo ou un PDF d'un ticket de caisse ou d'une étiquette produit pour en extraire automatiquement les articles et alimenter une feuille Google Sheets de comparaison de prix. L'utilisateur peut d'abord envoyer un message texte pour mémoriser le nom du magasin, puis envoyer le fichier. Claude (API Anthropic, vision) analyse l'image/PDF, extrait chaque article (nom, catégorie, prix unitaire, prix au kg, poids) en s'appuyant sur les catégories et noms de produits déjà existants dans le sheet pour uniformiser la nomenclature. Chaque article extrait est ajouté comme une nouvelle ligne, puis une confirmation récapitulative est renvoyée sur Telegram.

## Services / APIs utilisés
- Telegram Bot API (déclencheur + récupération de fichier + notifications)
- Anthropic Claude API (modèle claude-sonnet-4, vision) — extraction structurée des articles depuis image/PDF
- Google Sheets — lecture des articles existants (pour uniformiser) et écriture des nouveaux articles

## Prérequis
- Un bot Telegram dédié créé via @BotFather
- Une clé API Anthropic (Claude) avec accès vision
- Une feuille Google Sheets "comparatif des liste de courses" avec les colonnes Magasin/Catégorie/Nom exact de l'article/Prix unitaire/Prix au kg/Poids/Date
- Credentials n8n configurés : Telegram API, Anthropic API, Google Sheets OAuth2

## Variables d'environnement
| Variable | Description |
|---|---|
| YOUR_TELEGRAM_CHAT_ID | Identifiant du chat Telegram utilisé pour les confirmations et notifications |

## Schéma du flux
```
[Telegram Trigger] --> [Code JS - Détecte type message] --> [Si - Est texte ou image ?]
   --> (texte) [Code JS - Mémo magasin] --> [Telegram - Confirme mémo magasin]
   --> (fichier) [Telegram - Récupère fichier] --> [Encode base64] --\
                 [Google Sheets - Existants] ------------------------> [Merge] --> [Prépare prompt Claude]
   --> [API Claude - Analyse image/PDF] --> [Parse + Structure lignes] --> [Sheets - Enregistre articles]
   --> [Prépare confirmation] --> [Telegram - Envoie confirmation]
```
