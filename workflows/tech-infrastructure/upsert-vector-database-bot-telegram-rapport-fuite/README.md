# UPSERT VECTOR DATABASE BOT TELEGRAM RAPPORT FUITE

## Description
Ce workflow alimente automatiquement une base vectorielle Pinecone à partir des rapports d'expertise PDF (recherche de fuites) déposés dans un dossier Google Drive. Toutes les 6 heures, il récupère jusqu'à 10 PDF en attente, en extrait le texte, le nettoie (suppression des en-têtes, pieds de page, coordonnées de l'entreprise) puis le découpe intelligemment par section (conclusion, désordres constatés, investigations) pour créer des "chunks" contextualisés. Chaque chunk est ensuite vectorisé via OpenAI Embeddings et upserté dans l'index Pinecone. Les PDF traités sont enfin copiés dans un dossier d'archive puis supprimés du dossier source pour éviter un retraitement.

## Services / APIs utilisés
- Google Drive (lecture, copie, suppression de fichiers)
- OpenAI (embeddings)
- Pinecone (base de données vectorielle)

## Prérequis
- Un compte Google Drive avec un dossier source ("RAPPORT A ARCHIVER") et un dossier d'archive
- Un index Pinecone existant avec un namespace dédié
- Une clé API OpenAI pour la génération des embeddings

## Variables d'environnement
| Variable | Description |
|---|---|
| YOUR_OPENAI_API_KEY | Clé API OpenAI utilisée par le node Embeddings |
| YOUR_PINECONE_API_KEY | Clé API Pinecone pour l'upsert des vecteurs |
| YOUR_GOOGLE_DRIVE_OAUTH | Credential OAuth2 Google Drive |

## Schéma du flux
```
[Trigger toutes les 6h] --> [Cherche PDFs à traiter] --> [Limite 10 fichiers]
   --> [Télécharge PDF] --> [Extrait texte PDF] --> [Découpe rapport (Code JS)]
   --> [Pinecone - Upsert rapports] (avec Embeddings OpenAI + Text Splitter)
   --> [Copie vers archivés] --> [Supprime original]
```
