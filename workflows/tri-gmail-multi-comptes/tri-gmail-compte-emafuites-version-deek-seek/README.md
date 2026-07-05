# Tri Gmail compte EMAFUITES (version DeepSeek)

## Description
Ce workflow trie automatiquement les emails entrants de la boîte Gmail professionnelle (société MEA) dans une arborescence stricte de dossiers/labels. Il récupère les labels existants pour construire dynamiquement la liste des dossiers autorisés, puis pour chaque email de la boîte de réception, vérifie d'abord si le fil de discussion a déjà été classé manuellement. Si ce n'est pas le cas, un prompt DeepSeek (avec une étape de "chauffe du cache" pour optimiser les coûts sur de gros volumes) classe l'email selon des règles métier précises : correspondances exactes sur des dossiers juridiques/contextes pro, puis déduction sémantique par domaine (RH, prospection, outils, fournisseurs, leads entrants). Le résultat est appliqué comme label Gmail, l'email est retiré de la boîte de réception, et chaque classification est journalisée dans Google Sheets avec le détail de consommation de tokens.

## Services / APIs utilisés
- Gmail API (lecture des labels, des emails et threads, ajout/retrait de labels)
- DeepSeek API (classification des emails par IA, avec optimisation de cache de contexte)
- Google Sheets (journalisation des classifications et de la consommation de tokens)

## Prérequis
- Un compte Gmail professionnel avec une arborescence de labels déjà en place
- Un compte DeepSeek avec clé API
- Une feuille Google Sheets de journalisation avec les colonnes Sujet/Expéditeur/Dossier/Résumé/Tokens
- Credentials n8n configurés : Gmail OAuth2, DeepSeek API, Google Sheets OAuth2

## Variables d'environnement
| Variable | Description |
|---|---|
| GOOGLE_SHEET_ID_A_CONFIGURER | ID du Google Sheet de journalisation des classifications |
| CLIENT_JURIDIQUE_1 à CLIENT_JURIDIQUE_4 | Placeholders remplaçant les noms réels de dossiers juridiques utilisés pour la correspondance exacte dans le prompt de classification (à adapter à votre propre arborescence) |

## Schéma du flux
```
[Trigger] --> [Get many labels] --> [Filtre labels] --> [Get many messages] --> [Trie par date] --> [Limite emails]
                                                                                                              |
                                                                        --> [Get a thread] --> [Détecte dossiers] --> [Si email déjà classé ?]
                                                                                                                              |
                                    --> [Add label to message] (déjà classé)          --> [Évalue nb emails] --> [Si seuil nb emails ?] --> [Si chauffe nécessaire ?] --> [Chauffe cache DeepSeek] --> [Si cache chaud ?]
                                                                                                                                                                                                                  |
                                                                                                        --> [Nettoie email] --> [Build req classif] --> [Prépare classif] --> [API DeepSeek - Classifie] --> [Parse classif]
                                                                                                                                                                                                                  |
                                                                                                                                            --> [Add label to message1] --> [Remove label from message] --> [Append row in sheet1]
```
