# Exécution requête SQL

## Description
Ce workflow utilitaire permet d'exécuter manuellement une requête SQL directe sur la base de données PostgreSQL de l'entreprise, déclenché à la demande via un trigger manuel. Il est utilisé ponctuellement pour des opérations de maintenance ou de nettoyage de données (par exemple vider une table). La requête est codée en dur dans le node PostgreSQL et doit être adaptée avant chaque exécution.

## Services / APIs utilisés
- PostgreSQL (exécution de requête directe)

## Prérequis
- Un accès PostgreSQL avec les droits nécessaires sur la base de données ciblée
- Vérifier et adapter la requête SQL avant chaque exécution manuelle (workflow potentiellement destructif : DELETE, TRUNCATE, etc.)

## Variables d'environnement
| Variable | Description |
|---|---|
| YOUR_POSTGRES_HOST | Hôte de la base de données PostgreSQL |
| YOUR_POSTGRES_USER | Utilisateur PostgreSQL |
| YOUR_POSTGRES_PASSWORD | Mot de passe PostgreSQL |
| YOUR_POSTGRES_DATABASE | Nom de la base de données ciblée |

## Schéma du flux
```
[Trigger manuel] --> [PostgreSQL - Exécute requête SQL]
```
