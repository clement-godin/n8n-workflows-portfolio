# Tri Gmail compte perso (version DeepSeek)

## Description
Ce workflow trie automatiquement les emails d'une boîte Gmail personnelle en les classant dans les bons labels grâce à un LLM (DeepSeek). Toutes les heures, il récupère les labels existants, liste les emails anciens (plus de 4 semaines) présents dans la boîte de réception, vérifie s'ils sont déjà classés dans un dossier personnalisé, et sinon les fait classifier par l'IA sur la base d'un prompt dynamique stocké dans Google Sheets. Un mécanisme de "chauffe de cache" optimise le coût des appels API DeepSeek en réutilisant le cache de contexte lorsque le volume d'emails à traiter est important. Chaque classification est ensuite journalisée dans une feuille Google Sheets avec le détail des tokens consommés et le coût estimé.

## Services / APIs utilisés
- Gmail (lecture des threads/emails, gestion des labels)
- Google Sheets (lecture du prompt actif, journalisation des classifications)
- DeepSeek API (classification des emails par LLM)

## Prérequis
- Un compte Gmail connecté en OAuth2
- Un compte Google Sheets connecté en OAuth2, avec une feuille "prompts" (colonnes Prompt/Version/Actif) et une feuille "classifications"
- Une clé API DeepSeek

## Variables d'environnement
| Variable | Description |
|---|---|
| YOUR_DEEPSEEK_API_KEY | Clé API DeepSeek utilisée pour la classification des emails |

## Schéma du flux
```
[Cron horaire] --> [Lire prompt actif (Sheets)] --> [Récupère labels Gmail] --> [Récupère emails INBOX] --> [Tri par date]
   --> [Récupère thread] --> [Détecte dossier existant]
        |--> [Déjà classé] --> [Ajoute label existant] --> [Retire INBOX]
        |--> [Non classé] --> [Prépare chauffe cache] --> [Chauffe DeepSeek (si volume élevé)]
             --> [Nettoie email] --> [Prépare classification] --> [DeepSeek - Classification]
             --> [Parse résultat] --> [Ajoute label classif] --> [Retire INBOX] --> [Log Google Sheets]
```
