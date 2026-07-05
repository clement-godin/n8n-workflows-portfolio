# Récap journalier manager via Telegram

## Description
Ce workflow transforme des notes vocales transcrites, saisies tout au long de la journée dans un Google Sheet, en un récapitulatif de fin de journée synthétique destiné à un associé. Deux fois par jour (12h et 20h), il récupère les notes marquées "À ENVOYER", les compile, puis les soumet à un modèle LLM (DeepSeek) avec un prompt très strict imposant un style télégraphique, sans blabla, structuré autour de 3 informations (quoi/combien/quand) et se terminant par des questions fermées nécessitant une validation binaire. Le récapitulatif généré est envoyé sur Telegram, puis les notes correspondantes sont marquées comme envoyées dans le Sheet pour éviter les doublons.

## Services / APIs utilisés
- Google Sheets (lecture et mise à jour des notes)
- DeepSeek (LLM via LangChain, génération du récapitulatif)
- Telegram (envoi du message)

## Prérequis
- Un Google Sheet listant les notes vocales transcrites avec colonnes date/heure/note transcript/statut
- Un compte API DeepSeek
- Un bot Telegram et son chat ID

## Variables d'environnement
| Variable | Description |
|---|---|
| YOUR_GOOGLE_SHEET_ID | ID du Google Sheet contenant les notes transcrites |
| YOUR_TELEGRAM_CHAT_ID | Chat ID Telegram destinataire du récapitulatif |

## Schéma du flux
```
[Trigger - Récap journalier (12h/20h)] --> [Sheets - Récupère notes à envoyer]
   --> [Si - Notes à envoyer ?] --> [Code JS - Compile notes]
   --> [DeepSeek - Génère récap journalier] --> [Telegram - Envoie récap]
   --> [Code JS - Récupère lignes] --> [Sheets - Marque notes envoyées]
```
