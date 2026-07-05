# Relance devis envoyé 24h

## Description
Ce workflow recherche dans HubSpot les deals (devis) créés la veille via l'API de recherche HubSpot, récupère le détail du deal puis le contact associé, et déclenche un appel téléphonique sortant automatisé via Twilio vers le numéro du contact pour relancer le prospect 24h après l'envoi de son devis. C'est une automatisation de relance commerciale visant à augmenter le taux de transformation des devis envoyés.

## Services / APIs utilisés
- HubSpot CRM (recherche de deals, récupération deal/contact)
- Twilio (appel vocal sortant)

## Prérequis
- Un compte HubSpot avec un App Token ayant accès en lecture aux deals et contacts
- Un compte Twilio avec un numéro vocal actif et le service d'appel configuré
- La propriété HubSpot "hs_searchable_calculated_phone_number" renseignée sur les contacts

## Variables d'environnement
| Variable | Description |
|---|---|
| YOUR_TWILIO_PHONE_NUMBER | Numéro Twilio utilisé comme émetteur de l'appel de relance (remplace le "+33600000000" placeholder) |

## Schéma du flux
```
[Trigger manuel] --> [API HubSpot - Recherche devis créés la veille]
  --> [HubSpot - Récupère le deal] --> [HubSpot - Récupère le contact associé]
  --> [API Twilio - Déclenche appel de relance]
```
