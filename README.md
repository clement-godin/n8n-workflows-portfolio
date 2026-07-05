# n8n Workflows Portfolio

Portfolio technique de **83 workflows n8n** issus d'une instance self-hosted réelle, dont une quarantaine actifs en production. Les workflows archivés sont des expérimentations, versions remplacées ou projets abandonnés — publiés ici pour illustrer la variété des cas d'usage explorés et la culture d'itération. Cette instance automatise les opérations d'une agence de services (EMA Fuites — recherche de fuites non destructive, Nord / Hauts-de-France) et de plusieurs entités sœurs (MSHDF, MEA, IEA ADMI).

Ce dépôt documente des automatisations **effectivement déployées en production** — pas des démos de test — couvrant l'acquisition, le CRM, la relation client par IA, la téléphonie, l'infrastructure et les outils internes. Toutes les données sensibles (clés API, tokens, emails, téléphones, IDs de documents, IPs) ont été remplacées par des placeholders avant publication ; la logique et l'architecture de chaque workflow sont conservées à l'identique.

## Contexte métier

EMA Fuites est une entreprise de détection de fuites d'eau non destructive. Le volume de leads, de devis, de rendez-vous terrain et d'échanges clients a rendu nécessaire l'automatisation quasi complète du cycle de vente et du support : qualification de leads par IA, CRM, prise de rendez-vous, signature électronique de devis, chatbots multi-canaux (WhatsApp, Messenger, Chatwoot), optimisation de tournées terrain, et une couche d'infrastructure (sauvegardes, monitoring, alerting) pour fiabiliser le tout.

## Technologies utilisées

| Catégorie | Outils |
|---|---|
| Orchestration | [n8n](https://n8n.io) (self-hosted) |
| IA / LLM | Anthropic (Claude), OpenAI (GPT, Whisper), DeepSeek, LangChain (agents, RAG), Pinecone (vector store) |
| CRM / Facturation | HubSpot, Evoliz, Pennylane |
| Messagerie / Chatbots | WhatsApp Cloud API (Meta), Messenger, Telegram, Chatwoot |
| Téléphonie | Twilio, FreePBX, Asterisk (AMI), Yealink |
| Géolocalisation / Tournées | OSRM, VROOM, Nominatim, ANTs Route |
| Signature électronique | DocuSeal |
| Google Workspace | Sheets, Drive, Calendar, Gmail, Search Console, Ads |
| Infra / Sécurité | OVH API (DNS), PostgreSQL, SSH, reCAPTCHA |
| Autres | ClickUp, Notion, YouTube Data API, Tavily (recherche web) |

## Structure du dépôt

```
n8n-workflows-portfolio/
├── README.md                 (ce fichier)
├── .env.example               (checklist complète des variables/credentials)
└── workflows/
    ├── tri-gmail-multi-comptes/
    ├── marketing-acquisition/
    ├── crm-ventes/
    ├── tech-infrastructure/
    ├── chatbots-ia/
    ├── ia-outils/
    ├── legal-administratif/
    └── automatisations-perso/
        └── <nom-du-workflow>/
            ├── workflow.json   (export n8n nettoyé, ré-importable)
            └── README.md       (description, APIs, prérequis, variables, schéma)
```

## Comment utiliser ce dépôt

1. Parcourir la catégorie qui vous intéresse et ouvrir le `README.md` du workflow pour comprendre son fonctionnement.
2. Importer `workflow.json` dans une instance n8n (`Import from File` ou copier-coller dans l'éditeur).
3. Configurer les credentials n8n nécessaires (HubSpot, Anthropic, Google OAuth, etc.) et remplacer les variables `YOUR_XXX` listées dans le `README.md` du workflow — voir `.env.example` à la racine pour la liste complète.
4. Adapter les IDs de ressources (feuilles Google Sheets, dossiers Drive, pipelines HubSpot...) à votre propre environnement.

## Sommaire des workflows

### Tri Gmail multi-comptes (IA) (9)
| Workflow | Statut | Dossier |
|---|---|---|
| ANALYSE DES NEWSLETTERS GMAIL PERSO | Actif | [`workflows/tri-gmail-multi-comptes/analyse-des-newsletters-gmail-perso/`](workflows/tri-gmail-multi-comptes/analyse-des-newsletters-gmail-perso/) |
| OPTIMISATION PROMPT TRI GMAIL COMPTE PERSO | Actif | [`workflows/tri-gmail-multi-comptes/optimisation-prompt-tri-gmail-compte-perso/`](workflows/tri-gmail-multi-comptes/optimisation-prompt-tri-gmail-compte-perso/) |
| SUPPRESSION DES LABELS GMAIL VIDES | Inactif/archivé | [`workflows/tri-gmail-multi-comptes/suppresion-des-label-gmail-vides/`](workflows/tri-gmail-multi-comptes/suppresion-des-label-gmail-vides/) |
| TRI GMAIL COMPTE EMAFUITES version deek seek | Actif | [`workflows/tri-gmail-multi-comptes/tri-gmail-compte-emafuites-version-deek-seek/`](workflows/tri-gmail-multi-comptes/tri-gmail-compte-emafuites-version-deek-seek/) |
| TRI GMAIL COMPTE MEA version deek seek | Actif | [`workflows/tri-gmail-multi-comptes/tri-gmail-compte-mea-version-deek-seek/`](workflows/tri-gmail-multi-comptes/tri-gmail-compte-mea-version-deek-seek/) |
| TRI GMAIL COMPTE PERSO version deek seek | Actif | [`workflows/tri-gmail-multi-comptes/tri-gmail-compte-perso-version-deek-seek/`](workflows/tri-gmail-multi-comptes/tri-gmail-compte-perso-version-deek-seek/) |
| TRI GMAIL COMPTE CONTACT EMAFUITES version deek seek | Inactif/archivé | [`workflows/tri-gmail-multi-comptes/tri-gmail-compte-contact-emafuites-version-deek-seek/`](workflows/tri-gmail-multi-comptes/tri-gmail-compte-contact-emafuites-version-deek-seek/) |
| TRI GMAIL COMPTE CONTACT MEA version deek seek | Actif | [`workflows/tri-gmail-multi-comptes/tri-gmail-compte-contact-mea-version-deek-seek/`](workflows/tri-gmail-multi-comptes/tri-gmail-compte-contact-mea-version-deek-seek/) |
| TRI GMAIL COMPTE IEA ADMI version deek seek | Inactif/archivé | [`workflows/tri-gmail-multi-comptes/tri-gmail-compte-iea-admi-version-deek-seek/`](workflows/tri-gmail-multi-comptes/tri-gmail-compte-iea-admi-version-deek-seek/) |

### Marketing & Acquisition (19)
| Workflow | Statut | Dossier |
|---|---|---|
| ALERTE CPA GOOGLE ADS | Actif | [`workflows/marketing-acquisition/alerte-cpa-google-ads/`](workflows/marketing-acquisition/alerte-cpa-google-ads/) |
| Brief Stratégique hebdomadaire | Actif | [`workflows/marketing-acquisition/brief-strategique-hebdomadaire/`](workflows/marketing-acquisition/brief-strategique-hebdomadaire/) |
| CHECK PAGE SPEED INSIGHT API | Inactif/archivé | [`workflows/marketing-acquisition/check-page-speed-insight-api/`](workflows/marketing-acquisition/check-page-speed-insight-api/) |
| DASHBOARD HEBDO EMA FUITES | Inactif/archivé | [`workflows/marketing-acquisition/dashboard-hebdo-ema-fuites/`](workflows/marketing-acquisition/dashboard-hebdo-ema-fuites/) |
| EMA - AUDIT DIGITAL + ICEBREAKER | Inactif/archivé | [`workflows/marketing-acquisition/ema-audit-digital-icebreaker/`](workflows/marketing-acquisition/ema-audit-digital-icebreaker/) |
| EMA - CLASSIFICATEUR PERSONA | Inactif/archivé | [`workflows/marketing-acquisition/ema-classificateur-persona/`](workflows/marketing-acquisition/ema-classificateur-persona/) |
| EMA - CRÉATEUR CONTENU LINKEDIN | Inactif/archivé | [`workflows/marketing-acquisition/ema-createur-contenu-linkedin/`](workflows/marketing-acquisition/ema-createur-contenu-linkedin/) |
| EMA - SCORING MATURITÉ | Inactif/archivé | [`workflows/marketing-acquisition/ema-scoring-maturite/`](workflows/marketing-acquisition/ema-scoring-maturite/) |
| EMA - SETTER IA | Inactif/archivé | [`workflows/marketing-acquisition/ema-setter-ia/`](workflows/marketing-acquisition/ema-setter-ia/) |
| Rapport hebdo Google Search Console | Inactif/archivé | [`workflows/marketing-acquisition/rapport-hebdo-google-search-console/`](workflows/marketing-acquisition/rapport-hebdo-google-search-console/) |
| RAPPORT JOURNALIER CAMPAGNE GOOGLE ADS | Inactif/archivé | [`workflows/marketing-acquisition/rapport-journalier-campagne-google-ads/`](workflows/marketing-acquisition/rapport-journalier-campagne-google-ads/) |
| RESUMER HEBDO DES VIDEOS YOUTUBE | Actif | [`workflows/marketing-acquisition/resumer-hebdo-des-videos-youtube/`](workflows/marketing-acquisition/resumer-hebdo-des-videos-youtube/) |
| Surveillance avis Google | Inactif/archivé | [`workflows/marketing-acquisition/surveillance-avis-google/`](workflows/marketing-acquisition/surveillance-avis-google/) |
| SÉQUENCE EMAIL B2B | Actif | [`workflows/marketing-acquisition/sequence-email-b2b/`](workflows/marketing-acquisition/sequence-email-b2b/) |
| SÉQUENCE OMNICANALE PARTICULIERS | Inactif/archivé | [`workflows/marketing-acquisition/sequence-omnicanale-particuliers/`](workflows/marketing-acquisition/sequence-omnicanale-particuliers/) |
| Workflow 1 Scraping Plombiers | Inactif/archivé | [`workflows/marketing-acquisition/workflow-1-scraping-plombiers/`](workflows/marketing-acquisition/workflow-1-scraping-plombiers/) |
| Workflow 2 — Prospection Partenaires — Appels Sortants | Inactif/archivé | [`workflows/marketing-acquisition/workflow-2-prospection-partenaires-appels-sortants/`](workflows/marketing-acquisition/workflow-2-prospection-partenaires-appels-sortants/) |
| Workflow 3 — Prospection Partenaires — Résultats Appels | Inactif/archivé | [`workflows/marketing-acquisition/workflow-3-prospection-partenaires-resultats-appels/`](workflows/marketing-acquisition/workflow-3-prospection-partenaires-resultats-appels/) |
| Workflow 4 — Déduplication Plombiers pour suppressions | Inactif/archivé | [`workflows/marketing-acquisition/workflow-4-deduplication-plombiers-pour-suppresions/`](workflows/marketing-acquisition/workflow-4-deduplication-plombiers-pour-suppresions/) |

### CRM & Ventes (20)
| Workflow | Statut | Dossier |
|---|---|---|
| AJOUT DES APPELS TWILLO SUR HUBSPOT | Inactif/archivé | [`workflows/crm-ventes/ajout-des-appels-twillo-sur-hubspot/`](workflows/crm-ventes/ajout-des-appels-twillo-sur-hubspot/) |
| Automatic Travel Time Updates Between Google Calendar Appointments | Inactif/archivé | [`workflows/crm-ventes/automatic-travel-time-updates-between-google-calendar-appoin/`](workflows/crm-ventes/automatic-travel-time-updates-between-google-calendar-appoin/) |
| Click-to-call HubSpot → Yealink via AMI | Inactif/archivé | [`workflows/crm-ventes/click-to-call-hubspot-yealink-via-ami/`](workflows/crm-ventes/click-to-call-hubspot-yealink-via-ami/) |
| DEMANDE D'AVIS GOOGLE AUTOMATIQUE | Actif | [`workflows/crm-ventes/demande-d-avis-google-automatique/`](workflows/crm-ventes/demande-d-avis-google-automatique/) |
| ENVOIE SIGNATURE DEVIS DOCU SEAL | Actif | [`workflows/crm-ventes/envoie-signature-devis-docu-seal/`](workflows/crm-ventes/envoie-signature-devis-docu-seal/) |
| EXECUTION REQUETE SQL | Inactif/archivé | [`workflows/crm-ventes/execution-requete-sql/`](workflows/crm-ventes/execution-requete-sql/) |
| MAJ DEAL HUBSPOT SUITE SIGNATURE | Inactif/archivé | [`workflows/crm-ventes/maj-deal-hubspot-suite-signature/`](workflows/crm-ventes/maj-deal-hubspot-suite-signature/) |
| My workflow SYNCHRO TEAM Prise de RDV suite Devis | Inactif/archivé | [`workflows/crm-ventes/my-workflow-synchro-team-prise-de-rdv-suite-devis/`](workflows/crm-ventes/my-workflow-synchro-team-prise-de-rdv-suite-devis/) |
| QUESTIONNAIRE SATISFACTION | Inactif/archivé | [`workflows/crm-ventes/questionnaire-satisfaction/`](workflows/crm-ventes/questionnaire-satisfaction/) |
| RELANCE DEVIS AUTOMATIQUE | Actif | [`workflows/crm-ventes/relance-devis-automatique/`](workflows/crm-ventes/relance-devis-automatique/) |
| RELANCE DEVIS ENVOYE 24H | Inactif/archivé | [`workflows/crm-ventes/relance-devis-envoye-24h/`](workflows/crm-ventes/relance-devis-envoye-24h/) |
| REQUETE ANTS ROUTE Search availabilities PLANNING | Inactif/archivé | [`workflows/crm-ventes/requete-ants-route-search-availabilities-planning/`](workflows/crm-ventes/requete-ants-route-search-availabilities-planning/) |
| récup devis evolyz | Inactif/archivé | [`workflows/crm-ventes/recup-devis-evolyz/`](workflows/crm-ventes/recup-devis-evolyz/) |
| sauvegarde base de données /H | Inactif/archivé | [`workflows/crm-ventes/sauvegarde-base-de-donnees-h/`](workflows/crm-ventes/sauvegarde-base-de-donnees-h/) |
| SCORING AUTOMATIQUE DES LEADS | Actif | [`workflows/crm-ventes/scoring-automatique-des-leads/`](workflows/crm-ventes/scoring-automatique-des-leads/) |
| Sync Annuaire HubSpot → FreePBX | Inactif/archivé | [`workflows/crm-ventes/sync-annuaire-hubspot-freepbx/`](workflows/crm-ventes/sync-annuaire-hubspot-freepbx/) |
| SYNC LEADS → HUBSPOT | Inactif/archivé | [`workflows/crm-ventes/sync-leads-hubspot/`](workflows/crm-ventes/sync-leads-hubspot/) |
| SYNCHRO HUBSPOT VERS EVOLYZ | Inactif/archivé | [`workflows/crm-ventes/synchro-hubspot-vers-evolyz/`](workflows/crm-ventes/synchro-hubspot-vers-evolyz/) |
| synchro pennylane | Inactif/archivé | [`workflows/crm-ventes/synchro-pennylane/`](workflows/crm-ventes/synchro-pennylane/) |
| Workflow 2 _ De l_Intervention Finie à la Facturation_Archivage | Inactif/archivé | [`workflows/crm-ventes/workflow-2-de-l-intervention-finie-a-la-facturation-archivag/`](workflows/crm-ventes/workflow-2-de-l-intervention-finie-a-la-facturation-archivag/) |

### Tech & Infrastructure (15)
| Workflow | Statut | Dossier |
|---|---|---|
| AJOUT PROMPT CLAUDE CODE (WEBHOOK) | Actif | [`workflows/tech-infrastructure/ajout-prompt-claude-code-webhook/`](workflows/tech-infrastructure/ajout-prompt-claude-code-webhook/) |
| ALERTE NON LANCEMENT WORKFLOW | Actif | [`workflows/tech-infrastructure/alerte-non-lancement-workflow/`](workflows/tech-infrastructure/alerte-non-lancement-workflow/) |
| BACK UP DNS OVH | Actif | [`workflows/tech-infrastructure/back-up-dns-ovh/`](workflows/tech-infrastructure/back-up-dns-ovh/) |
| BACK UP INTEGRAL HUBSPOT | Actif | [`workflows/tech-infrastructure/back-up-integral-hubspot/`](workflows/tech-infrastructure/back-up-integral-hubspot/) |
| BACK UP NOTION | Inactif/archivé | [`workflows/tech-infrastructure/back-up-notion/`](workflows/tech-infrastructure/back-up-notion/) |
| DECLENCHEMENT PROMPT CLAUDE CODE | Actif | [`workflows/tech-infrastructure/declenchement-prompt-claude-code/`](workflows/tech-infrastructure/declenchement-prompt-claude-code/) |
| GRAPHIFY REBUILD NOCTURNE | Actif | [`workflows/tech-infrastructure/graphify-rebuild-nocturne/`](workflows/tech-infrastructure/graphify-rebuild-nocturne/) |
| NETTOYAGE BACK UP N8N | Actif | [`workflows/tech-infrastructure/nettoyage-back-up-n8n/`](workflows/tech-infrastructure/nettoyage-back-up-n8n/) |
| NETTOYAGE DES DOSSIER BACK UP VPS | Actif | [`workflows/tech-infrastructure/nettoyage-des-dossier-back-up-vps/`](workflows/tech-infrastructure/nettoyage-des-dossier-back-up-vps/) |
| SAUVEGARDE WORKFLOW N8N | Actif | [`workflows/tech-infrastructure/sauvegarde-workflow-n8n/`](workflows/tech-infrastructure/sauvegarde-workflow-n8n/) |
| UPSERT VECTOR DATABASE BOT TELEGRAM RAPPORT FUITE | Actif | [`workflows/tech-infrastructure/upsert-vector-database-bot-telegram-rapport-fuite/`](workflows/tech-infrastructure/upsert-vector-database-bot-telegram-rapport-fuite/) |
| VERIF RE CAPTCHA GOOGLE LANDING EMA FUITES | Actif | [`workflows/tech-infrastructure/verif-re-captcha-google-landing-ema-fuites/`](workflows/tech-infrastructure/verif-re-captcha-google-landing-ema-fuites/) |
| VERIF RE CAPTCHA GOOGLE LANDING RENO MSHDF | Actif | [`workflows/tech-infrastructure/verif-re-captcha-google-landing-reno-mshdf/`](workflows/tech-infrastructure/verif-re-captcha-google-landing-reno-mshdf/) |
| Webhook POST /mshdf-ai | Actif | [`workflows/tech-infrastructure/webhook-post-mshdf-ai/`](workflows/tech-infrastructure/webhook-post-mshdf-ai/) |
| 🚨 Alerte Erreurs Globales | Actif | [`workflows/tech-infrastructure/alerte-erreurs-globales/`](workflows/tech-infrastructure/alerte-erreurs-globales/) |

### Chatbots IA (11)
| Workflow | Statut | Dossier |
|---|---|---|
| BOT TELEGRAM ASSISTANT REDACTION RAPPORT | Actif | [`workflows/chatbots-ia/bot-telegram-assistant-redaction-rapport/`](workflows/chatbots-ia/bot-telegram-assistant-redaction-rapport/) |
| bot transcription audio OPEN AI whisper | Actif | [`workflows/chatbots-ia/bot-transcription-audio-open-ai-whisper/`](workflows/chatbots-ia/bot-transcription-audio-open-ai-whisper/) |
| EMA Fuites - Chatbot - WhatsApp | Actif | [`workflows/chatbots-ia/ema-fuites-chatbot-whatsapp/`](workflows/chatbots-ia/ema-fuites-chatbot-whatsapp/) |
| EMA Fuites - Chatbot Chatwoot IA | Actif | [`workflows/chatbots-ia/ema-fuites-chatbot-chatwoot-ia/`](workflows/chatbots-ia/ema-fuites-chatbot-chatwoot-ia/) |
| EMA Fuites - Chatbot Messenger IA | Actif | [`workflows/chatbots-ia/ema-fuites-chatbot-messenger-ia/`](workflows/chatbots-ia/ema-fuites-chatbot-messenger-ia/) |
| EMA Fuites - Vérification Webhook Messenger | Actif | [`workflows/chatbots-ia/ema-fuites-verification-webhook-messenger/`](workflows/chatbots-ia/ema-fuites-verification-webhook-messenger/) |
| RECAP JOURNALIER MANAGER VIA TELEGRAM | Actif | [`workflows/chatbots-ia/recap-journalier-manager-via-telegram/`](workflows/chatbots-ia/recap-journalier-manager-via-telegram/) |
| NOTE AUDIO RECAP JOURNALIER MANAGER VIA TELEGRAM | Actif | [`workflows/chatbots-ia/note-audio-recap-journalier-manager-via-telegram/`](workflows/chatbots-ia/note-audio-recap-journalier-manager-via-telegram/) |
| MAJ DES TACHES SELON DATE | Actif | [`workflows/chatbots-ia/maj-des-taches-selon-date/`](workflows/chatbots-ia/maj-des-taches-selon-date/) |
| NOTIFICATION DE CHANGEMENT DE STATUT | Inactif/archivé | [`workflows/chatbots-ia/notification-de-changement-de-statut/`](workflows/chatbots-ia/notification-de-changement-de-statut/) |
| PLANIFICATION DE TACHES PERSONNELLES | Actif | [`workflows/chatbots-ia/planification-de-taches-personnelles/`](workflows/chatbots-ia/planification-de-taches-personnelles/) |

### IA & Outils (voix, tournées, transcription) (5)
| Workflow | Statut | Dossier |
|---|---|---|
| Contexte Léa → Yealink | Inactif/archivé | [`workflows/ia-outils/contexte-lea-yealink/`](workflows/ia-outils/contexte-lea-yealink/) |
| EMA — Archive Transcription Appel | Inactif/archivé | [`workflows/ia-outils/ema-archive-transcription-appel/`](workflows/ia-outils/ema-archive-transcription-appel/) |
| EMAFUITES - Moteur créneaux | Inactif/archivé | [`workflows/ia-outils/emafuites-moteur-creneaux/`](workflows/ia-outils/emafuites-moteur-creneaux/) |
| EMAFUITES - Notification tournée J-1 | Inactif/archivé | [`workflows/ia-outils/emafuites-notification-tournee-j-1/`](workflows/ia-outils/emafuites-notification-tournee-j-1/) |
| EMAFUITES - Optimisation tournée | Inactif/archivé | [`workflows/ia-outils/emafuites-optimisation-tournee/`](workflows/ia-outils/emafuites-optimisation-tournee/) |

### Légal & Administratif (2)
| Workflow | Statut | Dossier |
|---|---|---|
| Workflow 1 : Générer le lien Iframe Synchronie | Actif | [`workflows/legal-administratif/workflow-1-generer-le-lien-iframe-synchronie/`](workflows/legal-administratif/workflow-1-generer-le-lien-iframe-synchronie/) |
| Workflow 2 : Sauvegarder le document signé Asynchrone | Actif | [`workflows/legal-administratif/workflow-2-sauvegarder-le-document-signe-asynchrone/`](workflows/legal-administratif/workflow-2-sauvegarder-le-document-signe-asynchrone/) |

### Automatisations personnelles (2)
| Workflow | Statut | Dossier |
|---|---|---|
| RECUP DES FILMS | Actif | [`workflows/automatisations-perso/recup-des-films/`](workflows/automatisations-perso/recup-des-films/) |
| SCAN TICKET ET ETIQUETTE | Actif | [`workflows/automatisations-perso/scan-ticket-et-etiquette/`](workflows/automatisations-perso/scan-ticket-et-etiquette/) |

## Note sur la confidentialité

Ce portfolio est publié à des fins de démonstration technique dans le cadre de candidatures pour des postes Ops & Automatisation. Toutes les données à caractère personnel ou toutes les données clients réelles (emails, téléphones, noms) ont été anonymisées. Les identifiants de ressources internes (feuilles de calcul, dossiers de sauvegarde, webhooks) ont été remplacés par des placeholders génériques. Les noms des entités (EMA Fuites, MSHDF, MEA, IEA ADMI) sont conservés à titre de contexte métier — il s'agit des propres structures de l'auteur de ce portfolio.
