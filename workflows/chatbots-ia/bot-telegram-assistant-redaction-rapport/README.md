# Bot Telegram Assistant Rédaction Rapport

## Description
Ce workflow implémente un assistant Telegram basé sur un agent IA (Claude, via LangChain) spécialisé dans la rédaction de rapports d'expertise de recherche de fuites pour MEA-Tech. Le technicien peut envoyer une note textuelle ou un message vocal décrivant son intervention ; l'agent consulte une base vectorielle Pinecone d'archives de rapports passés (RAG) pour rédiger un paragraphe technique conforme au style et à la terminologie métier, avec des règles anti-hallucination strictes (ne jamais inventer de conclusion non sourcée). Pour les messages vocaux, l'audio est d'abord transcrit via OpenAI Whisper puis proposé en validation (boutons "Valider"/"Recommencer") avant d'être transmis à l'agent de rédaction.

## Services / APIs utilisés
- Telegram Bot API (trigger, récupération de fichiers vocaux, envoi de messages et boutons interactifs)
- Anthropic Claude (modèle de langage de l'agent, en tant que LLM et outil RAG)
- OpenAI Whisper (transcription audio) et OpenAI Embeddings (vectorisation pour la recherche)
- Pinecone (base vectorielle des archives de rapports d'expertise)

## Prérequis
- Un bot Telegram dédié à l'assistant.
- Une clé API Anthropic (Claude Sonnet).
- Une clé API OpenAI (transcription + embeddings).
- Un index Pinecone contenant les archives de rapports MEA découpées en "Super-Chunks" (conclusion, constatations, détail d'investigation).

## Variables d'environnement
| Variable | Description |
|---|---|
| YOUR_TELEGRAM_BOT_TOKEN | Token du bot Telegram (stocké via le système de credentials n8n) |
| YOUR_ANTHROPIC_API_KEY | Clé API Anthropic (stockée via le système de credentials n8n) |
| YOUR_OPENAI_API_KEY | Clé API OpenAI (stockée via le système de credentials n8n) |
| YOUR_PINECONE_API_KEY | Clé API Pinecone (stockée via le système de credentials n8n) |

## Schéma du flux
```
[Trigger Telegram] --> [Switch - Type message]
  ├─ texte           --> [Agent Rédaction MEA (texte)] --RAG--> [Pinecone/Claude] --> [Répond agent texte]
  ├─ vocal           --> [Récupère vocal] --> [OpenAI Whisper] --> [Propose transcription (boutons)]
  ├─ callback "OK"    --> [Agent Rédaction MEA (audio)] --RAG--> [Pinecone/Claude] --> [Répond agent audio]
  └─ callback "annuler" --> [Annule vocal]
```
