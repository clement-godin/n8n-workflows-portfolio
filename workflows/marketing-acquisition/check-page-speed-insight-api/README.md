# CHECK PAGE SPEED INSIGHT API

## Description
Ce workflow audite automatiquement les performances web (Core Web Vitals) du site emafuites.fr. Il récupère la liste des URLs depuis le sitemap XML du site, y ajoute des URLs de landing pages publicitaires spécifiques, puis boucle sur chaque URL pour lancer un test PageSpeed Insights (desktop et mobile) via l'API Google. Les métriques (FCP, LCP, TBT, CLS, Speed Index, score global) sont extraites, comparées à des seuils, historisées dans Google Sheets, et une alerte Telegram peut être envoyée en cas d'anomalie de performance détectée.

## Services / APIs utilisés
- Google PageSpeed Insights API (Lighthouse)
- Google Sheets
- Telegram Bot API (notification, désactivée par défaut)

## Prérequis
- Une clé API Google PageSpeed Insights (Google Cloud Console)
- Un compte Google Sheets avec une feuille de suivi des scores
- Un sitemap XML accessible publiquement sur le site à auditer
- Optionnel : un bot Telegram pour les alertes de performance

## Variables d'environnement
| Variable | Description |
|---|---|
| YOUR_TELEGRAM_CHAT_ID | ID du chat Telegram destinataire des alertes performance |
| YOUR_GOOGLE_SHEET_ID | ID de la feuille Google Sheets d'historisation des scores |

## Schéma du flux
```
[Schedule Trigger] --> [HTTP Sitemap] --> [Parse XML] --> [Extrait URLs]
  --> [Loop Over Items] --> [Test Desktop] + [Test mobile] (via Wait 1min)
  --> [Métriques desktop] + [Métriques mobile] --> [Merge]
  --> [Filtre timeout] --> [Si erreur PSI] --> [Append row in sheet]
  --> [Calcule anomalies] --> [Si alerte perf] --> [Telegram alerte] (optionnel)
```
