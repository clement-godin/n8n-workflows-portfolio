# Workflow 3 — Prospection Partenaires — Résultats Appels

## Description
Ce workflow réceptionne les résultats des appels sortants automatisés de prospection auprès de partenaires potentiels (probablement passés via Twilio/agent vocal IA en amont). Un webhook reçoit le compte-rendu de l'appel (identifiant de l'entreprise, intérêt exprimé, contact, résumé, SID d'appel). Selon le résultat (intéressé, non intéressé, ou contact établi sans réponse claire), le deal HubSpot correspondant est mis à jour en conséquence.

## Services / APIs utilisés
- Webhook n8n (réception des résultats d'appels, typiquement depuis Twilio)
- HubSpot CRM (mise à jour du statut/étape du deal partenaire)

## Prérequis
- Un système d'appels sortants (Twilio ou équivalent) configuré pour envoyer un callback vers ce webhook à la fin de chaque appel
- Un compte HubSpot avec les deals de prospection partenaires déjà créés

## Variables d'environnement
| Variable | Description |
|---|---|
| YOUR_HUBSPOT_TOKEN | Token d'application privée HubSpot utilisé pour la mise à jour des deals |

## Schéma du flux
```
[Webhook - Résultats appels partenaires] --> [Si appel abouti ?] --> [Prépare données résultat]
   --> [Switch résultat appel]
        |--> intéressé --> [HubSpot - Màj deal intéressé]
        |--> non intéressé --> [HubSpot - Màj deal non intéressé]
        |--> contact établi (autre) --> (aucune action)
```
