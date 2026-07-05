# VERIF RE CAPTCHA GOOGLE LANDING EMA FUITES

## Description
Ce workflow reçoit les leads envoyés depuis le formulaire de contact du site EMA Fuites via un webhook, et les protège contre le spam/bots grâce à Google reCAPTCHA Enterprise. Il évalue le score de risque retourné par Google (seuil à 0.5) : si le score est valide, il répond au formulaire avec succès, formate un email HTML détaillé (coordonnées, page source, UTM, score reCAPTCHA) et l'envoie par Gmail. Des étapes supplémentaires (création de contact/deal HubSpot, sauvegarde Google Sheets) existent mais sont désactivées dans la version actuelle.

## Services / APIs utilisés
- Google reCAPTCHA Enterprise API
- Gmail
- HubSpot CRM (contacts + deals, désactivé)
- Google Sheets (désactivé)

## Prérequis
- Un projet Google Cloud avec reCAPTCHA Enterprise activé et une clé API
- Une site key reCAPTCHA configurée côté formulaire front-end
- Un compte Gmail connecté en OAuth2 pour l'envoi de notification
- Optionnel : un compte HubSpot et une feuille Google Sheets si les étapes CRM sont réactivées

## Variables d'environnement
| Variable | Description |
|---|---|
| your-email@domain.com | Adresse email de réception des notifications de lead |
| YOUR_GOOGLE_SHEET_ID | ID de la feuille Google Sheets de sauvegarde des leads (étape désactivée) |

## Schéma du flux
```
[Webhook - Réception Lead] --> [Vérifie reCAPTCHA] --> [API Google reCAPTCHA]
  --> [Évalue score] --> [Si score valide ?]
       oui --> [Réponse succès] --> [Formate email] --> [Gmail notif]
              --> [HubSpot upsert contact] --> [HubSpot crée deal] --> [Sheets enregistre] (désactivés)
       non --> [Réponse erreur]
```
