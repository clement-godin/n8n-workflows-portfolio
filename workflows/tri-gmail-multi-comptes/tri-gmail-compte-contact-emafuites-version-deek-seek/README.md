# Tri Gmail automatique avec DeepSeek (compte contact EMAFUITES)

## Description
Ce workflow classe automatiquement les emails d'une boîte Gmail professionnelle dans les bons dossiers (labels), sans intervention manuelle. Il récupère les labels existants et les nouveaux messages de la boîte de réception, vérifie si un email a déjà été classé via l'historique du thread, puis pour les emails non classés interroge un LLM (DeepSeek) qui choisit le label le plus pertinent parmi une liste stricte fournie en prompt. Une étape de "chauffe de cache" envoie un appel factice au modèle avant la vraie classification afin de maximiser le cache de prompt côté DeepSeek et réduire les coûts/latence. Chaque classification est journalisée dans un Google Sheet (sujet, expéditeur, dossier choisi, raison, tokens consommés). Le mail est ensuite étiqueté avec le bon label et retiré de la boîte de réception.

## Services / APIs utilisés
- Gmail API (lecture des labels, des messages, des threads, ajout/suppression de labels)
- DeepSeek API (chat completions, modèle `deepseek-chat`) pour la classification
- Google Sheets API (journalisation des classifications et du coût en tokens)

## Prérequis
- Un compte Gmail avec des labels personnalisés déjà créés (arborescence de classement)
- Une clé API DeepSeek
- Un compte de service/OAuth2 Google Sheets avec accès à la feuille de log
- Adapter la liste des labels et le prompt système à votre propre arborescence de dossiers avant réutilisation (les IDs de labels et noms de dossiers d'origine ont été anonymisés)

## Variables d'environnement
| Variable | Description |
|---|---|
| YOUR_GMAIL_OAUTH_CREDENTIAL | Identifiants OAuth2 Gmail (lecture/écriture labels et messages) |
| YOUR_DEEPSEEK_API_KEY | Clé API DeepSeek pour les appels de classification |
| YOUR_GOOGLE_SHEETS_OAUTH_CREDENTIAL | Identifiants OAuth2 Google Sheets pour la journalisation |
| YOUR_LOG_SPREADSHEET_ID | ID du Google Sheet de suivi des classifications |

## Schéma du flux
```
[Trigger planifié] --> [Gmail: liste des labels] --> [Filtre labels utilisateur]
   --> [Gmail: liste des messages INBOX] --> [Tri par date] --> [Limite]
   --> [Gmail: récupère le thread] --> [Détecte dossiers déjà appliqués]
   --> [Si déjà classé ?]
         --oui--> [Ajoute label] --> [Retire INBOX]
         --non--> [Évalue nb emails] --> [Si chauffe cache requise]
                     --> [Chauffe cache DeepSeek] --> [Vérifie ratio cache]
                     --> [Si cache chaud ?]
                           --oui--> [Nettoie le texte email] --> [Prépare requête classif]
                              --> [DeepSeek: classifie email] --> [Parse réponse JSON]
                              --> [Ajoute label choisi] --> [Retire INBOX]
                              --> [Google Sheets: journalise ligne]
                           --non--> [Attente 2s] --> (retour chauffe cache)
```
