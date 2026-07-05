# Workflow 1 Scraping Plombiers

## Description
Ce workflow scrape automatiquement les plombiers du Nord et du Pas-de-Calais (départements 59/62) pour alimenter une liste de prospection B2B partenaires. Il récupère la liste des communes via l'API officielle geo.api.gouv.fr, interroge l'API Google Places (Text Search) pour chaque ville, dédoublonne les résultats par numéro de téléphone, puis enrichit chaque fiche avec le SIREN/SIRET et le dirigeant (API Recherche d'Entreprises), une URL LinkedIn (API Tavily) et un email extrait par scraping du site web. Un système de suivi de quota mensuel (budget Google Maps) envoie une alerte Telegram à 80% et bloque le workflow à 100% du budget. Les résultats sont enregistrés dans Google Sheets puis poussés comme company/deal dans HubSpot.

## Services / APIs utilisés
- Google Places API (Text Search)
- API geo.api.gouv.fr (liste des communes)
- API recherche-entreprises.api.gouv.fr (SIRET/dirigeants)
- Tavily API (recherche LinkedIn)
- Google Sheets
- HubSpot (Company + Deal)
- Telegram Bot API (alerte quota)

## Prérequis
- Une clé API Google Places (credential httpHeaderAuth)
- Un compte Tavily avec clé API
- Un compte HubSpot avec un App Token
- Un Google Sheet de destination avec un onglet "Plombiers"
- Un bot Telegram pour les alertes de quota

## Variables d'environnement
| Variable | Description |
|---|---|
| YOUR_TAVILY_API_KEY | Clé API Tavily utilisée pour retrouver l'URL LinkedIn de l'entreprise |
| YOUR_TELEGRAM_CHAT_ID | Identifiant du chat Telegram destinataire des alertes de dépassement de budget |
| YOUR_GOOGLE_SHEET_ID | ID du Google Sheet où sont enregistrés les plombiers scrapés/enrichis |

## Schéma du flux
```
[Trigger planifié] --> [Vérifie quota Maps] --> [Budget OK ?]
  --> [Communes 59+62 (geo.api.gouv.fr)] --> [Formate requêtes villes]
  --> [Google Places Text Search] --> [Dédoublonne résultats] --> [Limite 50]
  --> [Filtre nouvelles entrées] --> [Enrichit (SIRET, dirigeant, LinkedIn, email)]
  --> [Google Sheets - Enregistre] --> [HubSpot - Crée company] --> [HubSpot - Crée deal]

(branche budget dépassé) --> [Telegram - Alerte quota Maps]
```
