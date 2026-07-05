# Bot transcription audio OpenAI Whisper

## Description
Ce workflow implémente un bot Telegram qui transcrit automatiquement les messages vocaux ou fichiers audio envoyés par l'utilisateur. Un switch distingue les deux types de médias Telegram (message vocal `voice` ou fichier audio `audio`), récupère le fichier correspondant, puis l'envoie à l'API de transcription Whisper d'OpenAI. Le texte transcrit est renvoyé à l'utilisateur directement dans la conversation Telegram.

## Services / APIs utilisés
- Telegram Bot API (trigger, récupération de fichiers, envoi de messages)
- OpenAI Whisper (transcription audio via le nœud LangChain OpenAI)

## Prérequis
- Un bot Telegram dédié à la transcription.
- Une clé API OpenAI avec accès à l'API de transcription audio (Whisper).

## Variables d'environnement
| Variable | Description |
|---|---|
| YOUR_TELEGRAM_BOT_TOKEN | Token du bot Telegram (stocké via le système de credentials n8n) |
| YOUR_OPENAI_API_KEY | Clé API OpenAI (stockée via le système de credentials n8n) |

## Schéma du flux
```
[Trigger Telegram] --> [Switch - Type message audio]
                          ├─ voice --> [Récupère fichier vocal] --> [OpenAI Whisper] --> [Envoie transcription]
                          └─ audio --> [Récupère fichier audio] --> [OpenAI Whisper] --> [Envoie transcription]
```
