# VERIF RE CAPTCHA GOOGLE LANDING RENO MSHDF

## Description
Ce workflow réceptionne les leads soumis depuis le formulaire de la landing page de rénovation MSHDF via un webhook. Il vérifie côté serveur le score reCAPTCHA Enterprise du visiteur avant tout traitement, afin de filtrer les soumissions automatisées (bots/spam). Si le score est valide, le lead est formaté en email HTML et envoyé par Gmail, un contact et un deal sont créés/mis à jour dans HubSpot, puis la ligne est archivée dans Google Sheets. Si le score est trop bas ou le token invalide, le webhook répond avec une erreur 400 sans déclencher les actions commerciales.

## Services / APIs utilisés
- Google reCAPTCHA Enterprise (API assessments)
- Gmail (notification du lead par email)
- HubSpot (création de contact et de deal CRM)
- Google Sheets (archivage des leads)

## Prérequis
- Un compte Google Cloud avec reCAPTCHA Enterprise activé et une clé API (query auth) configurée en credential n8n
- Une clé de site reCAPTCHA (site key) associée au domaine de la landing page
- Un compte HubSpot avec un App Token (scopes contacts + deals)
- Un compte Gmail OAuth2 connecté à n8n
- Un Google Sheet cible pour l'archivage des leads

## Variables d'environnement
| Variable | Description |
|---|---|
| YOUR_RECAPTCHA_SITE_KEY | Clé de site reCAPTCHA Enterprise associée au domaine de la landing page |
| your-email@domain.com | Adresse email de notification des leads (destinataire Gmail) |

## Schéma du flux
```
[Webhook - Réception Lead]
        --> [Code JS - Vérifie reCAPTCHA] (construit la requête d'assessment)
        --> [API Google - Vérifie reCAPTCHA]
        --> [Code JS - Évalue score] (valide/score/raison)
        --> [Si - Score valide ?]
              ├── OUI --> [Webhook - Réponse succès]
              │           --> [Code JS - Formate email]
              │           --> [Gmail - Envoie notif lead]
              │           --> [HubSpot - Upsert contact]
              │           --> [HubSpot - Crée deal]
              │           --> [Sheets - Enregistre lead]
              └── NON --> [Webhook - Réponse erreur]
```
