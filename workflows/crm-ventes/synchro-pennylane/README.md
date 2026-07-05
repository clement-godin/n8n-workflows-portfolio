# synchro pennylane

## Description
Ce workflow synchronise la liste des clients enregistrés dans Pennylane (logiciel de comptabilité) vers une base de données Postgres interne. Il est déclenché manuellement, interroge l'API Pennylane pour récupérer les clients triés par ID décroissant, éclate le tableau de résultats en lignes individuelles, puis effectue un upsert (insertion ou mise à jour) dans la table `clients` de la base Postgres en utilisant l'identifiant externe Pennylane comme clé de correspondance.

## Services / APIs utilisés
- Pennylane (API externe v2 — endpoint `/customers`)
- PostgreSQL (base de données interne)

## Prérequis
- Un compte Pennylane avec accès API (clé d'authentification par header)
- Une base de données Postgres accessible avec une table `clients` correspondant au schéma attendu (id, id_externe, nom_client, type_client, email, telephone, adresse, code_postal, ville, date_creation)
- Credential n8n de type "Header Auth" configuré pour l'API Pennylane
- Credential n8n de type Postgres configuré

## Variables d'environnement
| Variable | Description |
|---|---|
| (aucune en dur) | L'authentification Pennylane passe par un credential n8n "Header Auth" (à configurer côté n8n, pas de clé en dur dans le JSON) |

## Schéma du flux
```
[Trigger manuel] --> [API Pennylane - Récupère clients]
                  --> [Éclate le tableau clients]
                  --> [Upsert clients dans Postgres]
```
