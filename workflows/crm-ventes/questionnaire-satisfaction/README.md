# Questionnaire satisfaction

## Description
Ce workflow automatise l'envoi et le traitement d'une enquête de satisfaction (NPS) après chaque intervention EMA Fuites terminée dans HubSpot. Toutes les 30 minutes, il recherche les deals au statut "intervention terminée" n'ayant pas encore reçu de questionnaire, récupère le profil complet du contact associé, attend 24h, puis envoie la question de satisfaction sur le canal adapté au type de client (WhatsApp pour les particuliers, formulaire par email pour syndics/bailleurs, email court pour experts/plombiers). Les réponses WhatsApp sont routées et interprétées automatiquement (score NPS 0-10) : les promoteurs (9-10) reçoivent une demande d'avis Google, les détracteurs (<7) déclenchent une alerte Telegram immédiate pour rappel commercial. Un second flux traite les réponses arrivant par formulaire Google Sheets de la même façon. Toutes les réponses sont journalisées dans Google Sheets et mettent à jour les propriétés NPS du contact dans HubSpot.

## Services / APIs utilisés
- HubSpot CRM (recherche deals/contacts, mise à jour des scores NPS)
- Meta WhatsApp Business API (envoi/réception des messages NPS)
- Gmail (envoi des questionnaires par email selon le segment client)
- Google Sheets (lecture des réponses formulaire, journalisation)
- Telegram (alertes en cas de score négatif)
- Sous-workflow n8n dédié à la demande d'avis Google (déclenché via Execute Workflow)

## Prérequis
- Un compte HubSpot avec les propriétés `satisfaction_envoyee`, `date_satisfaction`, `canal_satisfaction`, `nps_score`, `nps_date`, `nps_commentaire`, `hs_lead_status`
- Un numéro WhatsApp Business API Meta avec token d'accès
- Un compte Gmail connecté en OAuth2
- Une feuille Google Sheets "satisfaction" pour la journalisation
- Un bot Telegram pour les alertes de scores négatifs
- Un sous-workflow n8n "Demande Avis Google" existant (référencé par ID)

## Variables d'environnement
| Variable | Description |
|---|---|
| YOUR_WHATSAPP_PHONE_NUMBER_ID | ID du numéro WhatsApp Business Meta utilisé pour l'envoi/réponse des messages NPS |
| YOUR_WHATSAPP_TOKEN | Token d'accès Meta WhatsApp Business API |
| YOUR_TELEGRAM_CHAT_ID | ID du chat Telegram recevant les alertes NPS négatif |
| YOUR_TELEGRAM_BOT_TOKEN | Token du bot Telegram utilisé pour les alertes |
| YOUR_HUBSPOT_TOKEN | Token d'application privée HubSpot |
| VOTRE_SPREADSHEET_ID_EMA_FUITES | ID de la feuille Google Sheets de journalisation satisfaction |
| YOUR_WEBHOOK_ID | Identifiant du webhook interne de transfert vers le chatbot WhatsApp |

## Schéma du flux
```
[Trigger 30min] --> [HubSpot - Deals intervention terminée] --> [Filtrer sans satisfaction]
   --> [Marquer satisfaction_envoyee] --> [Get contact associé] --> [Get profil complet]
   --> [Attente 24h] --> [Switch type_client]
        |--> particulier/expert_sinistre --> [Stocke attente NPS] --> [WhatsApp - Question NPS]
        |--> syndic/bailleur --> [Email formulaire complet]
        |--> plombier --> [Email court]
        |--> default --> [Email formulaire simplifié]

[Webhook WA] --> [Router NPS vs Chatbot] --> [Si réponse NPS attendue ?]
   --> [Parser score] --> [Si score valide ?] --> [Switch Promoteur/Passif/Détracteur]
        |--> Promoteur --> [Merci + lien avis Google] --> [Sous-workflow Demande Avis]
        |--> Passif --> [Merci + question amélioration]
        |--> Détracteur --> [Alerte Telegram] --> [MAJ HubSpot INSATISFAIT]

[Trigger Google Sheets (réponses formulaire)] --> [Parser score] --> [Switch Promoteur/Passif/Détracteur] --> [MAJ HubSpot + alertes]
```
