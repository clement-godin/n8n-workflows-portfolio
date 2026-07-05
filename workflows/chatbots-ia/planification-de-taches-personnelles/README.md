# Planification de tâches personnelles

## Description
Ce workflow permet de créer des tâches ClickUp à partir de messages Telegram, vocaux ou texte. Un message vocal est transcrit via l'API OpenAI (Whisper), puis un message texte ou la transcription est analysé par un LLM (Claude Haiku via LangChain) pour en extraire un titre, une date, une heure et une description structurés en JSON. Un message de confirmation est renvoyé sur Telegram avec des boutons "OUI/NON" ; en cas de validation, la tâche est créée dans ClickUp avec la date d'échéance calculée, et le message Telegram est édité pour confirmer la création.

## Services / APIs utilisés
- Telegram (trigger messages/vocaux/callbacks, envoi et édition de messages)
- OpenAI (transcription audio - Whisper)
- Anthropic Claude (LangChain, extraction structurée des informations de tâche)
- ClickUp (création de tâches)

## Prérequis
- Un bot Telegram dédié à la planification de tâches
- Un compte API OpenAI (transcription audio)
- Un compte API Anthropic (modèle Claude Haiku)
- Un compte ClickUp avec un espace de travail, un espace et une liste dédiés aux tâches
- Un identifiant d'assigné ClickUp valide

## Variables d'environnement
| Variable | Description |
|---|---|
| YOUR_CLICKUP_TEAM_ID | ID du workspace (team) ClickUp |
| YOUR_CLICKUP_SPACE_ID | ID de l'espace ClickUp |
| YOUR_CLICKUP_LIST_ID | ID de la liste ClickUp cible |
| YOUR_CLICKUP_ASSIGNEE_ID | ID ClickUp de l'assigné par défaut |

## Schéma du flux
```
[Trigger - Telegram tâches] --> [Switch - Type message]
   --> (vocal) [Get a file] --> [Transcribe a recording] --> [Send a text message1 (confirmation vocale)]
   --> (texte) [Basic LLM Chain] --> [Code JS - Parse réponse LLM] --> [Send a text message (OUI/NON)]
   --> (callback tache_ok) [Answer Query a callback] --> [Code JS - Prépare tâche ClickUp]
       --> [Create a task] --> [Edit a text message1]
   --> (callback annuler) [Send a text message2 / 3]
```
