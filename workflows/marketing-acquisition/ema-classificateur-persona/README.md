# EMA - Classificateur Persona

## Description
Ce workflow tourne chaque heure et lit une feuille Google Sheets de prospects B2B pour EMA Fuites. Il filtre les prospects au statut "NOUVEAU", puis les traite un par un dans une boucle. Pour chaque prospect, un prompt est construit à partir de son nom, son entreprise, son poste et sa ville, puis envoyé à l'API Anthropic (Claude) qui doit renvoyer un persona (A = décideur direct, B = prescripteur, C = opérationnel, D = hors cible) avec une justification. Selon le persona retourné, le prospect est soit marqué "ÉCARTÉ" (persona D), soit marqué "CLASSIFIÉ" et déclenche alors un appel HTTP vers un second workflow de scoring.

## Services / APIs utilisés
- Google Sheets (lecture et mise à jour de la feuille "Prospects")
- Anthropic Claude API (classification du persona, modèle claude-sonnet-4-6)
- Webhook HTTP interne (déclenchement du workflow de scoring aval)

## Prérequis
- Une feuille Google Sheets "Prospects" avec au minimum les colonnes ID, Nom, Entreprise, Poste, Ville, Statut, Persona, Notes
- Une clé API Anthropic valide
- Le workflow de scoring aval doit exposer un webhook accessible

## Variables d'environnement
| Variable | Description |
|---|---|
| YOUR_ANTHROPIC_API_KEY | Clé API Anthropic utilisée pour la classification (référencée via credential n8n) |
| REMPLACER_PAR_ID_GOOGLE_SHEET_PROSPECTS_EMAFUITES | ID de la feuille Google Sheets contenant les prospects |
| https://your-domain.com/webhook/ema-scoring | URL du webhook interne déclenchant le workflow de scoring |

## Schéma du flux
```
[Schedule - Toutes les heures] --> [GSheets - Lire Prospects] --> [Code - Filtrer NOUVEAU]
  --> [IF - Prospects NOUVEAU] --> [Loop - Un par un] --> [Code - Préparer Classify]
  --> [Claude - Classifier Persona] --> [Code - Parser Persona] --> [IF - Persona D]
      --> (D) [GSheets - Écarter] --> [Loop - Un par un]
      --> (autre) [GSheets - Classifier] --> [HTTP - Déclencher W2 Scoring] --> [Loop - Un par un]
```
