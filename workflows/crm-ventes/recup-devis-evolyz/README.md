# Récup devis Evolyz

## Description
Ce workflow synchronise en continu les devis du logiciel de facturation Evoliz vers une base de données PostgreSQL interne. Toutes les minutes, il s'authentifie auprès de l'API Evoliz, récupère la liste des devis, puis compare le statut de chacun avec celui déjà enregistré en base. Si le statut a changé (accepté, refusé, envoyé, facturé...), la ligne correspondante est mise à jour (upsert) avec toutes les dates de cycle de vie du devis (création, envoi, acceptation, refus, facturation, clôture). Les devis dont le statut est inchangé sont simplement ignorés pour éviter les écritures inutiles.

## Services / APIs utilisés
- API Evoliz (authentification + récupération des devis)
- PostgreSQL (vérification de statut + upsert)

## Prérequis
- Un compte Evoliz avec clé publique et clé secrète API
- Une base PostgreSQL avec une table `devis` (colonnes : id, numero_devis, client_id, montant_ht, montant_ttc, statut, evoliz_id, et les différentes dates de statut)
- Un credential PostgreSQL configuré dans n8n

## Variables d'environnement
| Variable | Description |
|---|---|
| YOUR_EVOLIZ_PUBLIC_KEY | Clé publique API Evoliz |
| YOUR_EVOLIZ_SECRET_KEY | Clé secrète API Evoliz |

## Schéma du flux
```
[Trigger - Récup devis Evolyz] --> [API Evolyz - Auth] --> [API Evolyz - Récupère devis]
   --> [Prépare - Éclate devis] --> [Loop Over Items] --> [SQL - Vérifie statut devis]
   --> [Switch - Devis existant ?] --> [SQL - Upsert devis] ou [Prépare - Aucun changement]
   --> (retour à la boucle) --> [Loop Over Items] (fin)
```
