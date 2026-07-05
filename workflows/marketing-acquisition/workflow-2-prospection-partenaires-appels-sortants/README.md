# Workflow 2 — Prospection Partenaires — Appels Sortants

## Description
Ce workflow automatise les appels sortants de prospection auprès de partenaires (plombiers) enregistrés dans HubSpot. Chaque jour ouvré à 9h, il récupère dans HubSpot les deals du pipeline "Prospection Partenaires" au stade "À appeler", boucle sur chacun d'eux, récupère l'entreprise associée puis déclenche un appel automatisé via l'API Twilio vers un serveur TwiML qui fait parler une voix IA ("Léa"). Après chaque appel, le deal HubSpot est mis à jour et une pause est respectée avant l'appel suivant, pour lisser la cadence d'appels.

## Services / APIs utilisés
- HubSpot CRM API (recherche de deals, lecture de company, mise à jour de deal)
- API Twilio (déclenchement d'appel sortant / Voice API)
- Serveur TwiML propriétaire (script de conversation vocale IA "Léa")

## Prérequis
- Un compte HubSpot avec pipeline "Prospection Partenaires" et un stade "À appeler"
- Un compte Twilio avec un numéro vocal configuré et les credentials (Account SID / Auth Token)
- Un serveur TwiML exposé publiquement qui répond aux appels avec le script vocal IA
- Credentials n8n configurés : HubSpot App Token, Twilio (Basic Auth générique)

## Variables d'environnement
| Variable | Description |
|---|---|
| +33600000000 (champ "From") | Numéro Twilio utilisé comme caller ID pour l'appel sortant, à remplacer par votre numéro Twilio réel |
| https://your-domain.com/twiml-partenaire-plombier | URL du serveur TwiML propriétaire qui gère la conversation vocale IA |

## Schéma du flux
```
[Trigger cron 9h lun-ven] --> [HubSpot - Deals à appeler] --> [Boucle - Deals partenaires]
                                                                       |
                                                     --> [HubSpot - Récupère company] --> [API Twilio - Déclenche appel Léa]
                                                                                                    |
                                                                       --> [HubSpot - Màj deal après appel] --> [Attente - Entre appels] --> (boucle suivante)
```
