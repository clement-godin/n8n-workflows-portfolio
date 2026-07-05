# ENVOIE SIGNATURE DEVIS DOCU SEAL

## Description
Ce workflow automatise l'envoi en signature électronique des devis EMA Fuites validés dans HubSpot. Il recherche les deals au stade "à signer", récupère le devis PDF attaché ainsi que le contact associé, héberge le PDF sur Google Drive en lecture publique, puis crée une enveloppe de signature via l'API SignatureAPI avec les informations du contact. Le déclenchement se fait via un webhook interne sécurisé.

## Services / APIs utilisés
- HubSpot (CRM : recherche de deals, récupération de contacts et d'engagements, fichiers)
- Google Drive (upload et partage public du PDF du devis)
- SignatureAPI (création d'enveloppes de signature électronique)
- Webhook n8n interne (déclenchement sécurisé par header auth)

## Prérequis
- Un compte HubSpot avec un App Token configuré et un pipeline de deals incluant un stage "à signer"
- Un compte Google Drive connecté en OAuth2
- Un compte SignatureAPI avec une clé API
- Un secret de header pour sécuriser le webhook d'entrée

## Variables d'environnement
| Variable | Description |
|---|---|
| YOUR_SIGNATUREAPI_KEY | Clé API SignatureAPI (header X-API-Key) pour la création d'enveloppes de signature |

## Schéma du flux
```
[Webhook - Demande signature devis] --> [HubSpot - Cherche deals à signer] --> [HubSpot - Récupère deal]
  --> [HubSpot - Récupère contact] --> [HubSpot - Récupère engagement] --> [API HubSpot - URL fichier signé]
  --> [HTTP - Télécharge PDF] --> [Drive - Uploade PDF] --> [Drive - Partage PDF public]
  --> [API SignatureAPI - Crée envelope]
```
