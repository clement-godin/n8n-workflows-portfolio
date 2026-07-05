# Séquence omnicanale particuliers

## Description
Ce workflow orchestre une séquence de relance commerciale automatisée en 5 étapes pour des prospects particuliers (Email, SMS, WhatsApp) via HubSpot. Un webhook permet d'initialiser un contact dans la séquence (étape 0), puis un déclencheur horaire vérifie régulièrement quels contacts doivent recevoir la prochaine communication, en fonction de délais cumulés (J0, J+2, J+5, J+10, J+20). Avant chaque envoi, le workflow vérifie que le contact n'est pas déjà devenu client (deal à un stade avancé) pour stopper la séquence automatiquement. Chaque étape combine un canal différent (email, SMS, WhatsApp) avec un message adapté, puis met à jour la progression du contact dans HubSpot.

## Services / APIs utilisés
- HubSpot CRM (recherche et mise à jour de contacts/deals)
- Gmail (envoi d'emails)
- Twilio (envoi de SMS)
- Meta WhatsApp Business API (envoi de messages WhatsApp)

## Prérequis
- Un compte HubSpot avec les propriétés personnalisées `sequence_particulier_etape` et `sequence_particulier_date_debut` sur l'objet contact
- Un compte Gmail connecté en OAuth2
- Un compte Twilio avec un numéro d'expédition SMS
- Un compte Meta Business avec un numéro WhatsApp Business API et un token d'accès
- Un webhook protégé par authentification pour déclencher l'entrée d'un contact dans la séquence

## Variables d'environnement
| Variable | Description |
|---|---|
| YOUR_TWILIO_PHONE_NUMBER | Numéro Twilio utilisé comme expéditeur des SMS (`+33XXXXXXXXX`) |
| YOUR_WHATSAPP_PHONE_NUMBER_ID | ID du numéro WhatsApp Business dans Meta (`VOTRE_PHONE_NUMBER_ID`) |
| YOUR_WHATSAPP_TOKEN | Token d'accès Meta WhatsApp Business API |
| YOUR_WEBHOOK_SECRET | Secret d'authentification protégeant le webhook de démarrage de séquence |

## Schéma du flux
```
[Webhook démarrage séquence] --> [HubSpot - Init contact étape 0]

[Trigger horaire] --> [HubSpot - Contacts à traiter] --> [Filtrer contacts dus]
   --> [HubSpot - Vérifier statut deal] --> [Si déjà client ? (stop si oui)]
   --> [Switch étape 0-4] --> [Email / SMS / WhatsApp selon étape]
   --> [Calculer prochaine étape] --> [HubSpot - Update étape contact]
```
