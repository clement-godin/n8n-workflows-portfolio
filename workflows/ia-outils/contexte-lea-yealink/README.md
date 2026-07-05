# Contexte Léa → Yealink

## Description
Ce workflow expose un webhook interne qui reçoit du contexte d'appel depuis "Léa" (un agent vocal IA de type Nova Sonic) et transfère cet appel vers un poste téléphonique Yealink via le protocole AMI (Asterisk Manager Interface) d'un serveur FreePBX. Il construit un nom d'appelant personnalisé (nom du contact + motif de l'appel) affiché sur le combiné, transmet des variables de contexte (motif, urgence, adresse) au canal SIP, puis origine l'appel via une connexion socket TCP brute vers l'API AMI avant de répondre au webhook appelant.

## Services / APIs utilisés
- Webhook n8n (authentification par header interne)
- Asterisk Manager Interface (AMI) sur serveur FreePBX (connexion TCP directe en Node.js)
- Téléphonie SIP (poste Yealink)

## Prérequis
- Un serveur FreePBX/Asterisk accessible en réseau interne avec l'interface AMI activée (port 5038)
- Un utilisateur AMI dédié avec les droits d'origination d'appel (Originate)
- Une credential n8n de type Header Auth pour sécuriser le webhook entrant

## Variables d'environnement
| Variable | Description |
|---|---|
| YOUR_AMI_PASSWORD | Mot de passe du compte AMI utilisé pour authentifier la connexion Asterisk Manager Interface |

## Schéma du flux
```
[Webhook - Requête Léa contexte] --> [Code JS - Prépare contexte affiché]
  --> [Code JS - Connexion AMI + Originate vers poste SIP]
  --> [Webhook - Répond à Léa]
```
