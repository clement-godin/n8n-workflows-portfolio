# Séquence email B2B

## Description
Ce workflow gère une séquence d'emails de prospection B2B en 5 étapes pour EMA Fuites, avec des délais de 0, 3, 7, 14 et 30 jours entre chaque envoi. Un webhook permet de démarrer la séquence sur un contact HubSpot donné (init de l'étape 0). Un trigger horaire recherche ensuite les contacts HubSpot dont la prochaine étape est due, vérifie via l'API deals qu'ils ne sont pas déjà passés au statut "client" (auquel cas la séquence s'arrête), puis route l'envoi vers le bon email Gmail selon l'étape en cours (prise de contact, cas pratique, argument assurance, témoignage partenaire, offre de partenariat prescripteur). Après chaque envoi, la prochaine étape et sa date sont recalculées et mises à jour dans HubSpot.

## Services / APIs utilisés
- Webhook n8n (authentification par en-tête, démarrage manuel de la séquence)
- HubSpot API (recherche de contacts, recherche de deals, mise à jour des propriétés de contact)
- Gmail (envoi des 5 emails de la séquence)

## Prérequis
- Un compte HubSpot avec les propriétés personnalisées `sequence_b2b_etape` et `sequence_b2b_date_debut` sur l'objet contact
- Un ID de stage de deal HubSpot correspondant au statut "client" pour interrompre la séquence
- Un compte Gmail habilité à l'envoi automatisé
- Un webhook protégé par credential Header Auth pour déclencher la séquence sur un nouveau contact

## Variables d'environnement
| Variable | Description |
|---|---|
| YOUR_HUBSPOT_TOKEN | Jeton d'application HubSpot utilisé pour les appels CRM (référencé via credential n8n) |
| YOUR_WEBHOOK_HEADER_SECRET | Secret d'en-tête protégeant le webhook de démarrage de séquence (référencé via credential n8n) |

## Schéma du flux
```
[Webhook - Démarrer séquence B2B] --> [HubSpot - Init contact B2B]

[Trigger horaire] --> [HubSpot - Contacts B2B à traiter] --> [Code JS - Filtrer contacts B2B]
  --> [HubSpot - Vérifier statut deal] --> [Si - Déjà client]
      --> (non) [Switch - Étape séquence] --> [Gmail - Email B2B étape 0..4]
          --> [Code JS - Calculer prochaine étape] --> [HubSpot - Update étape B2B]
```
