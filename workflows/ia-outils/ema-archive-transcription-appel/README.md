# EMA — Archive Transcription Appel

## Description
Ce workflow (encore à l'état de brouillon/inactif) archive automatiquement les appels téléphoniques une fois terminés. Déclenché par un webhook Twilio (événement call.completed), il extrait les métadonnées de l'appel, télécharge l'enregistrement audio et l'héberge sur Google Drive. En parallèle, il récupère la transcription via un service Nova Sonic (Amazon Bedrock) et génère un résumé structuré (sentiment, issue de l'appel) via DeepSeek. Les données fusionnées sont ensuite insérées (upsert idempotent) dans une base PostgreSQL, puis un engagement d'appel est créé dans HubSpot.

## Services / APIs utilisés
- Twilio (webhook de notification de fin d'appel + téléchargement de l'enregistrement)
- Google Drive (stockage des fichiers audio)
- Amazon Bedrock / Nova Sonic (récupération de la transcription vocale)
- DeepSeek API (résumé, sentiment et issue de l'appel)
- PostgreSQL (archivage structuré des appels, upsert par twilio_call_sid)
- HubSpot (création d'un engagement de type Call)

## Prérequis
- Un numéro Twilio configuré pour envoyer un webhook call.completed
- Des identifiants Twilio (Basic Auth) pour télécharger les enregistrements
- Un compte Google Drive OAuth2
- Un endpoint Nova Sonic / Bedrock exposant la transcription par call_sid
- Un compte DeepSeek API
- Une base PostgreSQL avec une table call_transcripts (contrainte unique sur twilio_call_sid)
- Un compte HubSpot API

## Variables d'environnement
| Variable | Description |
|---|---|
| YOUR_DEEPSEEK_API_KEY | Clé API DeepSeek pour la génération du résumé/sentiment de l'appel |
| YOUR_NOVA_SONIC_ENDPOINT | URL du service de transcription Nova Sonic / Bedrock |

## Schéma du flux
```
[Webhook Twilio call.completed] --> [Extraire données appel] --> [Attendre recording disponible]
  --> [Télécharger audio Twilio] --> [Upload Drive] ----------------------\
                                                                            --> [Merge données]
[Récupérer transcript Nova Sonic] --> [Résumé LLM DeepSeek] --> [Parser réponse LLM] --/

[Merge données] --> [INSERT PostgreSQL] --> [HubSpot - Créer engagement Call]
```
