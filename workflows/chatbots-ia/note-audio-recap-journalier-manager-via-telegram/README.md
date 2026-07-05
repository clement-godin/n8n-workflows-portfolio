# Note audio recap journalier manager via Telegram

## Description
Ce workflow permet à un utilisateur d'envoyer une note vocale ou un message texte via Telegram pour alimenter un récapitulatif journalier. Si l'utilisateur envoie un vocal, celui-ci est transcrit automatiquement via l'API OpenAI (Whisper), puis la transcription est affichée avec des boutons de validation ("Valider" / "Recommencer") avant d'être enregistrée. Si l'utilisateur envoie un message texte, il est directement enregistré sans étape de validation. Chaque note validée est archivée dans une feuille Google Sheets avec l'heure, la date et le statut "A ENVOYER", en vue d'un traitement ultérieur (récapitulatif journalier).

## Services / APIs utilisés
- Telegram Bot API (trigger, envoi de messages, boutons inline, récupération de fichiers vocaux)
- OpenAI API (transcription audio, Whisper)
- Google Sheets API (enregistrement des notes)

## Prérequis
- Un bot Telegram configuré avec webhook actif dans n8n
- Un compte OpenAI avec accès à l'API de transcription audio
- Un compte Google avec accès en écriture à la feuille Google Sheets cible
- Une feuille Google Sheets avec les colonnes : date, heure, note transcript, statut

## Variables d'environnement
| Variable | Description |
|---|---|
| YOUR_GOOGLE_SHEET_ID | ID de la feuille Google Sheets où sont enregistrées les notes |

## Schéma du flux
```
[Trigger Telegram] --> [Switch: texte / audio / validation / annulation]
   texte  --> [Sheets: enregistre note texte] --> [Telegram: confirme note texte]
   audio  --> [Telegram: récupère vocal] --> [OpenAI: transcrit vocal] --> [Telegram: montre transcription + boutons]
   valider --> [Sheets: enregistre note audio] --> [Telegram: confirme note audio]
   annuler --> [Telegram: annule vocal]
```
