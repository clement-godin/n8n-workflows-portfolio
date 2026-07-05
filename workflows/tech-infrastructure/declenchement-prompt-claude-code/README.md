# DECLENCHEMENT PROMPT CLAUDE CODE

## Description
Ce workflow orchestre l'exécution automatisée de prompts Claude Code sur un serveur distant, à partir d'une file d'attente stockée dans Google Sheets. Un premier déclencheur (4h45) lit la feuille `claude_code_prompts`, filtre les lignes au statut "à faire" prévues pour le jour même, les trie par priorité, puis les traite une par une via `SplitInBatches`. Pour chaque prompt, il marque la ligne "en cours", encode le prompt en base64, l'écrit sur le serveur par SSH et exécute Claude Code en CLI (`--dangerously-skip-permissions --output-format json`), parse le rapport JSON renvoyé, met à jour le statut final dans le Sheet, nettoie le fichier temporaire puis notifie le résultat sur Telegram. Un second déclencheur (toutes les 6h) exécute une commande Claude Code "à vide" pour réinitialiser la fenêtre de session CLI et notifie également le résultat sur Telegram.

## Services / APIs utilisés
- Google Sheets (file d'attente de prompts, lecture/écriture de statut)
- SSH (exécution distante de la CLI Claude Code)
- Telegram (notifications de fin d'exécution / erreurs)
- Claude Code CLI (Anthropic) — exécuté en ligne de commande sur le serveur cible

## Prérequis
- Un Google Sheet avec un onglet `claude_code_prompts` (colonnes : id, titre, repertoire_projet, statut, priorite, date_creation, date_lancement, date_fin, resume, erreur, prompt, modele)
- Une clé SSH privée configurée en credential n8n avec accès au serveur exécutant Claude Code CLI
- Claude Code CLI installé sur le serveur cible (chemin Node.js/nvm spécifique)
- Un bot Telegram pour les notifications
- Accès en écriture au dossier `/home/debian/prompts` sur le serveur cible

## Variables d'environnement
| Variable | Description |
|---|---|
| (aucun secret en dur) | Ce workflow n'utilise que des credentials n8n (Google Sheets OAuth2, SSH Private Key, Telegram API) — rien à substituer |

## Schéma du flux
```
[Schedule Trigger 4h45]
    --> [Lire lignes] (Google Sheets)
    --> [Filtrer et trier] (statut = "à faire", tri par priorité)
    --> [Des lignes ?]
          --> [SplitInBatches]
                --> [Marque en cours] --> [Prépare prompt] --> [Build SSH command]
                --> [SSH - Écrit et exécute] --> [Parse résultat]
                --> [Mise à jour finale] --> [Supprime fichier] --> [Telegram]
                --> (boucle SplitInBatches)

[Schedule Trigger toutes les 6h]
    --> [SSH - Écrit et exécute1] (reset session CLI)
    --> [Code in JavaScript] (parse résultat)
    --> [Telegram1] (notification)
```
