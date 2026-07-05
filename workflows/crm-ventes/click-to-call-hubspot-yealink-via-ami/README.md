# Click-to-call HubSpot → Yealink via AMI

## Description
Ce workflow expose un webhook déclenchable depuis une fiche contact HubSpot (via une action click-to-call) qui initie un appel sortant directement depuis le téléphone SIP Yealink de l'assistante. Le numéro reçu est normalisé au format E.164 français, puis un nœud Code JS ouvre une connexion socket brute vers l'interface AMI (Asterisk Manager Interface) du serveur FreePBX, s'authentifie, et envoie une commande `Originate` qui fait sonner le poste SIP de l'assistante puis compose automatiquement le numéro du contact. Le webhook répond ensuite à HubSpot pour confirmer le déclenchement de l'appel.

## Services / APIs utilisés
- Webhook n8n (déclenchement depuis HubSpot)
- Asterisk Manager Interface (AMI) sur serveur FreePBX (connexion TCP brute via le module Node.js `net`)

## Prérequis
- Un serveur FreePBX/Asterisk avec l'interface AMI activée et un compte utilisateur AMI dédié
- Un poste SIP Yealink enregistré sur le FreePBX (ex: SIP/100)
- Une action HubSpot personnalisée (ou extension navigateur) qui appelle ce webhook avec le numéro et l'ID du contact
- Le nœud n8n doit avoir accès réseau au serveur FreePBX (même réseau Docker ou VPN)

## Variables d'environnement
| Variable | Description |
|---|---|
| YOUR_FREEPBX_HOST | Adresse/hostname du serveur FreePBX exposant l'interface AMI |
| YOUR_AMI_USERNAME | Nom d'utilisateur du compte AMI configuré dans `manager.conf` |
| YOUR_AMI_PASSWORD | Mot de passe (secret) du compte AMI, à définir dans `manager.conf` sur le FreePBX |

## Schéma du flux
```
[Webhook - Click-to-call HubSpot] --> [Code JS - Valide et formate numéro] --> [Code JS - AMI Originate] --> [Webhook - Répond à HubSpot]
```
