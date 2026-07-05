# Sync Annuaire HubSpot → FreePBX

## Description
Ce workflow planifié récupère l'ensemble des contacts HubSpot et génère un annuaire téléphonique au format XML compatible avec les téléphones IP Yealink (protocole `YealinkIPPhoneDirectory`), utilisé par le PBX FreePBX de l'entreprise. Pour chaque contact possédant un numéro de téléphone, il compose un nom d'affichage (prénom, nom, société) et normalise le numéro. Il ajoute également une liste de postes internes fixes (assistante, techniciens, direction, conférence), puis journalise le nombre de contacts synchronisés.

## Services / APIs utilisés
- API HubSpot (récupération de tous les contacts)
- Conversion JSON vers XML (nœud natif n8n)
- FreePBX / téléphonie Yealink (consommateur final du fichier XML, hors n8n)

## Prérequis
- Un token d'application HubSpot avec accès en lecture aux contacts.
- Un serveur FreePBX/Yealink configuré pour aller chercher (ou recevoir) le fichier annuaire XML généré.
- Adapter la liste des postes internes (`internes`) aux extensions réelles du PBX.

## Variables d'environnement
| Variable | Description |
|---|---|
| YOUR_HUBSPOT_TOKEN | Token d'application HubSpot |

## Schéma du flux
```
[Trigger horaire] --> [HubSpot - Récupère tous contacts] --> [Génère XML annuaire]
                    --> [Convertit JSON vers XML] --> [Log résultat]
```
