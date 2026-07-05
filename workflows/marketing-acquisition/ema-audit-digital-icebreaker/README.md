# EMA - AUDIT DIGITAL + ICEBREAKER

## Description
Ce workflow réalise un audit digital automatisé d'un prospect B2B puis génère un message d'accroche ("icebreaker") personnalisé via IA. Déclenché par un webhook avec l'ID de la ligne prospect, il vérifie si le site web du prospect répond et récupère sa fiche Google My Business (note, nombre d'avis). Ces données d'audit sont ensuite transmises à Claude (Anthropic) qui génère un icebreaker court et factuel, centré sur la problématique métier (gestion des fuites d'eau). Le résultat est sauvegardé dans Google Sheets et une notification est envoyée sur Telegram pour informer qu'un nouveau prospect est prêt à être contacté.

## Services / APIs utilisés
- Webhook n8n (authentification par header)
- Google Sheets (lecture/écriture prospects)
- HTTP Request générique (vérification disponibilité site web)
- Google Places API (fiche Google My Business)
- Anthropic Claude API (génération de l'icebreaker)
- Telegram (notification)

## Prérequis
- Un compte Google Sheets OAuth2 avec accès à la feuille "Prospects"
- Une clé Google Places API (Query Auth)
- Une clé API Anthropic (Claude)
- Un bot Telegram et son chat ID
- Un webhook sécurisé par un header secret partagé

## Variables d'environnement
| Variable | Description |
|---|---|
| REMPLACER_PAR_ID_GOOGLE_SHEET_PROSPECTS_EMAFUITES | ID du Google Sheet contenant la liste des prospects |
| YOUR_TELEGRAM_CHAT_ID | Chat ID Telegram destinataire de la notification |

## Schéma du flux
```
[Webhook - Audit] --> [Code - Extraire row_id] --> [GSheets - Lire Prospect Audit]
   --> [Code - Extraire Prospect] --> [HTTP - Vérifier Site Web] --> [Code - Résultat Site]
   --> [HTTP - Google Places GMB] --> [Code - Fusionner Audit] --> [Claude - Icebreaker]
   --> [Code - Parser Icebreaker] --> [GSheets - Sauvegarder Icebreaker] --> [Telegram - Prospect Prêt]
```
