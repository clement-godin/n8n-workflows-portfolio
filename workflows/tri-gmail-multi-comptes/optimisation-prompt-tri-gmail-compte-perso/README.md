# OPTIMISATION PROMPT TRI GMAIL COMPTE PERSO

## Description
Ce workflow est un système de "meta-learning" hebdomadaire qui audite et améliore automatiquement le prompt utilisé par un autre workflow de tri Gmail. Chaque lundi, il récupère les statistiques de classification de la semaine (emails classés correctement, en erreur, incertains) depuis un Google Sheet, envoie ce rapport à DeepSeek pour analyse, et si des erreurs récurrentes sont détectées, propose une nouvelle version du prompt de classification via Telegram. L'utilisateur valide ou refuse la modification par un bouton inline, ce qui déclenche (si accepté) la désactivation de l'ancien prompt et l'insertion de la nouvelle version en base.

## Services / APIs utilisés
- Google Sheets (stockage des prompts versionnés et des classifications à analyser)
- DeepSeek API (analyse des erreurs de classification et génération d'un prompt amélioré)
- Telegram (notification et validation humaine par bouton inline callback)

## Prérequis
- Un Google Sheet avec un onglet "prompts" (versions successives du prompt) et un onglet "classifications" (historique des classifications avec un statut de vérification manuelle)
- Un compte DeepSeek API
- Un bot Telegram avec webhook/polling configuré pour recevoir les callbacks de boutons

## Variables d'environnement
| Variable | Description |
|---|---|
| YOUR_TELEGRAM_CHAT_ID | Identifiant du chat Telegram recevant les propositions d'optimisation de prompt |

## Schéma du flux
```
[Trigger hebdo lundi 9h] --> [Google Sheets - Prompt actif] --> [Google Sheets - Classifications à analyser]
  --> [Code - Calcule statistiques] --> [Code - Prépare body DeepSeek] --> [API DeepSeek - Analyse prompt]
  --> [Code - Parse analyse] --> [Si modification nécessaire ?]
        --> (oui) [Telegram - Propose nouveau prompt] --> [Boucle marquage classifications]
        --> (non) [Telegram - Prompt OK] --> [Boucle marquage classifications]

[Trigger Telegram callback] --> [Répond callback] --> [Parse callback] --> [Si validé par Clément ?]
  --> [Google Sheets - Récupère prompt actif] --> [Désactive ancien prompt] --> [Insère nouveau prompt]
```
