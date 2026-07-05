# EMA - Scoring Maturité

## Description
Ce workflow reçoit un identifiant de prospect via webhook, va chercher la ligne correspondante dans une feuille Google Sheets "Prospects", puis demande à Claude (Anthropic) de scorer ce prospect B2B sur 100 selon des critères métier (poste du contact, source, disponibilité d'un email professionnel, présence d'un site web, etc.). Le score et sa justification sont ensuite réécrits dans la feuille Google Sheets. Si le score est supérieur ou égal à 60, un second workflow d'audit digital est déclenché automatiquement ; sinon le prospect est placé en statut "NURTURING" pour un traitement différé.

## Services / APIs utilisés
- n8n Webhook (déclenchement, protégé par header auth)
- Google Sheets API (lecture et mise à jour des prospects)
- Anthropic API (Claude) pour le scoring IA

## Prérequis
- Une feuille Google Sheets "Prospects" avec les colonnes ID, Poste, Entreprise, Persona, Site_web, Email, Source, Ville, Score, Priorité, Statut, Notes
- Un compte Anthropic avec accès à l'API Claude
- Un secret d'authentification pour protéger le webhook entrant
- Un second workflow n8n exposant un webhook `/ema-audit` pour l'audit digital des prospects qualifiés

## Variables d'environnement
| Variable | Description |
|---|---|
| REMPLACER_PAR_ID_GOOGLE_SHEET_PROSPECTS_EMAFUITES | ID de la feuille Google Sheets contenant les prospects |
| YOUR_ANTHROPIC_API_KEY | Clé API Anthropic (gérée via le système de credentials n8n) |

## Schéma du flux
```
[Webhook Scoring] --> [Prépare lecture] --> [GSheets: lire prospect] --> [Extraire par ID]
   --> [Prépare prompt scoring] --> [Claude: scorer] --> [Parser score] --> [GSheets: mettre à jour score]
   --> [IF score >= 60] --oui--> [HTTP: déclencher audit W3]
                        --non--> [GSheets: nurturing]
```
