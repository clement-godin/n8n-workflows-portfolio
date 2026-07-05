# Résumé hebdo des vidéos YouTube

## Description
Ce workflow s'exécute chaque lundi matin et compile automatiquement une veille vidéo personnalisée. Il récupère le contenu de deux playlists YouTube ("IA/SEO" et "veille générale"), les fusionne avec l'historique des vidéos déjà analysées dans un Google Sheets pour éviter les doublons, puis déduplique et pondère les vidéos restantes selon leur fraîcheur et leur sujet détecté (IA ou SEO). Les vidéos retenues (jusqu'à 70) sont ensuite traitées une par une : récupération de la transcription via Supadata, puis analyse par l'API DeepSeek selon un prompt de curation personnalisé (contexte : entrepreneur basé à Lille, no-code/n8n et SEO/EMA Fuites) qui attribue un score, une décision (VOIR / PLUS_TARD / CULTURE_G / IGNORER) et un résumé. Chaque analyse est enregistrée dans Google Sheets, puis un rapport agrégé et formaté est envoyé sur Telegram, découpé en plusieurs messages si nécessaire.

## Services / APIs utilisés
- YouTube Data API (lecture de playlists)
- Google Sheets (historique des vidéos analysées)
- Supadata (récupération de transcriptions vidéo)
- API DeepSeek (scoring et résumé du contenu, `deepseek-v4-flash`)
- Telegram Bot API (envoi du rapport hebdomadaire)

## Prérequis
- Deux playlists YouTube existantes (veille IA/SEO et veille générale)
- Une feuille Google Sheets de suivi avec les colonnes videoId, titre, date_analyse, score, decision, valeur, raison, sujet, url
- Une clé API Supadata valide
- Une clé API DeepSeek valide
- Un bot Telegram configuré pour recevoir le rapport

## Variables d'environnement
| Variable | Description |
|---|---|
| YOUR_YOUTUBE_PLAYLIST_ID_IA_SEO | ID de la playlist YouTube dédiée à la veille IA/SEO |
| YOUR_YOUTUBE_PLAYLIST_ID_VEILLE | ID de la playlist YouTube de veille générale |
| YOUR_GOOGLE_SHEET_ID_VIDEOS_YOUTUBE | ID de la feuille Google Sheets de suivi des vidéos analysées |
| YOUR_SUPADATA_API_KEY | Clé API Supadata utilisée pour récupérer les transcriptions (référencée via credential n8n) |
| YOUR_DEEPSEEK_API_KEY | Clé API DeepSeek utilisée pour le scoring des vidéos (référencée via credential n8n) |
| YOUR_TELEGRAM_CHAT_ID | Identifiant du chat Telegram destinataire du rapport hebdomadaire |

## Schéma du flux
```
[Trigger hebdo] --> [YouTube - Playlist IA/SEO] + [YouTube - Playlist veille] + [Google Sheets - Videos analysées]
  --> [Merge - Playlists YouTube] --> [Merge - Playlists + Analysées] --> [Code JS - Dédoublonne + Pondère]
  --> [Boucle - Vidéos à analyser] --> [Attente anti rate-limit] --> [Supadata - Transcript vidéo]
  --> [Code JS - Prépare prompt DeepSeek] --> [API DeepSeek - Analyse vidéo] --> [Code JS - Parse + Enrichit]
  --> [Sheets - Enregistre analyse] --> [Code JS - Agrège + Trie + Formate] --> [Telegram - Envoie rapport YouTube]
  --> (boucle suivante)
```
