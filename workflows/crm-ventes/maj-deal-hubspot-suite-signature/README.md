# MAJ DEAL HUBSPOT SUITE SIGNATURE

## Description
Ce workflow réagit à la complétion d'une signature électronique de devis via SignatureAPI. Lorsqu'un webhook `envelope.completed` est reçu, le deal HubSpot correspondant est mis à jour au statut "contrat envoyé". Le workflow récupère ensuite l'enveloppe signée depuis SignatureAPI, télécharge le PDF final signé, l'upload dans la bibliothèque de fichiers HubSpot (dossier privé `/devis_signes`), puis crée une note sur le deal HubSpot avec le PDF en pièce jointe pour tracer la signature.

## Services / APIs utilisés
- SignatureAPI (webhook de complétion + récupération/téléchargement de l'enveloppe signée)
- HubSpot CRM API (mise à jour de deal, upload de fichiers, création de notes)

## Prérequis
- Un compte SignatureAPI configuré pour envoyer un webhook `envelope.completed` avec les métadonnées `hubspot_deal_id` dans l'enveloppe
- Un compte HubSpot avec un token d'application privée (scopes deals, fichiers, notes)
- Le pipeline HubSpot doit contenir un stage `contractsent`
- L'association HubSpot entre notes et deals doit utiliser l'`associationTypeId` 214 (vérifier ce type dans votre portail)

## Variables d'environnement
| Variable | Description |
|---|---|
| VOTRE_UUID_WEBHOOK_SIGNATURE | Path du webhook n8n recevant les callbacks SignatureAPI (à générer aléatoirement) |
| YOUR_SIGNATUREAPI_KEY | Clé API SignatureAPI (X-API-Key), utilisée pour récupérer l'enveloppe et le PDF signé |
| VOTRE_DEAL_ID_HUBSPOT | ID du deal HubSpot par défaut configuré dans le nœud de mise à jour (à adapter selon le déclenchement réel) |

## Schéma du flux
```
[Webhook - Signature complète] --> [Si - Signature complète ?]
   --> [HubSpot - Màj deal signé] --> [API SignatureAPI - Récupère envelope] --> [API SignatureAPI - Télécharge PDF signé]
   --> [API HubSpot - Upload PDF signé] --> [API HubSpot - Crée note deal]
```
