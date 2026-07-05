# Demande d'avis Google automatique

## Description
Ce sous-workflow, appelé par un autre workflow (via Execute Workflow Trigger) avec les informations d'un contact, envoie automatiquement une demande d'avis Google après une intervention. Il formate et normalise le numéro de téléphone au format international, puis choisit le canal de contact : un message WhatsApp Business (Meta API) si un téléphone est disponible, sinon un e-mail via Gmail. Dans les deux cas, le message remercie le client et l'invite à laisser un avis Google via un lien direct. Une fois la demande envoyée, le deal HubSpot correspondant est marqué comme "avis demandé".

## Services / APIs utilisés
- WhatsApp Business API (Meta / Graph API) - envoi de message
- Gmail (envoi d'e-mail)
- HubSpot CRM (mise à jour du deal)

## Prérequis
- Un compte WhatsApp Business API avec un numéro de téléphone validé et un token d'accès Meta
- Un compte Gmail OAuth2
- Un compte HubSpot avec un token d'application privée
- Ce workflow doit être appelé par un autre workflow (Execute Workflow Trigger) qui lui transmet prenom, telephone, email, contactId, dealId

## Variables d'environnement
| Variable | Description |
|---|---|
| YOUR_WHATSAPP_PHONE_NUMBER_ID | ID du numéro de téléphone WhatsApp Business (Meta) |

## Schéma du flux
```
[Execute Workflow Trigger] --> [Code JS - Formate contact pour avis] --> [Si - A un téléphone ?]
   --> (oui) [API Meta - Demande avis WhatsApp] --> [HubSpot - Marquer avis demandé]
   --> (non) [Gmail - Demande avis email] --> [HubSpot - Marquer avis demandé]
```
