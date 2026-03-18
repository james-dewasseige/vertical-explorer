---
name: outbound-targets
description: Critères de ciblage et spécification de liste outbound pour une verticale.
trigger: /targets [vertical]
phase: 04-execute
depends-on: icp-definition, outbound-campaign
inputs:
  - blueprints/{slug}/icp.md
  - context/verticals/{slug}.md
outputs:
  - blueprints/{slug}/outbound-targets.md
---

# Outbound Targets

## Process
1. Lire blueprints/{slug}/icp.md → firmographics, persona décideur
2. Extraire les critères de ciblage exacts (taille, géo, titre, signaux)
3. Identifier les meilleures sources de données pour cette verticale
4. Spécifier la liste : taille cible, enrichissement, outils recommandés
5. Produire le plan d'exécution semaine par semaine

## Critères de ciblage

### Firmographics
- Taille entreprise (employés) :
- CA estimé :
- Géographie :
- Sous-segment :
- Structure (indépendant / groupe / franchise) :

### Technographics (signaux tools)
- Signaux "Excel/papier" → prospect chaud
- Signaux outil concurrent → évaluer switching intent
- Signaux ERP legacy → opportunité modernisation

### Intent signals
- Levée de fonds récente → budget disponible
- Nouvelle embauche direction → potentiel de changement
- Ouverture nouveau site → besoin d'outils
- Post LinkedIn sur problème opérationnel → douleur consciente
- Offre d'emploi pour rôle opérationnel → croissance

### Persona cible
- Titre exact à chercher sur LinkedIn :
- Titres alternatifs :
- À éviter :

## Sources de données
| Source | Utilisation | Accès | Coût |
|--------|------------|-------|------|
| LinkedIn Sales Navigator | Recherche B2B, titres, entreprises | Abonnement | ~€€ |
| Google Maps | Entreprises locales (HoReCa, retail...) | Gratuit | 0 |
| Pages Jaunes / Kompass | Annuaires professionnels France | Gratuit | 0 |
| Associations professionnelles | Annuaires membres | Variable | 0-€ |
| Societe.com / Pappers | Data légale entreprises France | Gratuit | 0 |
| Salons spécialisés | Exposants = prospects actifs | Variable | € |

## Spécification de liste
- **Taille cible** : 50-100 contacts pour un premier test
- **Enrichissement requis** : email pro, téléphone, LinkedIn URL
- **Outils recommandés** : Apollo.io, Dropcontact, Hunter.io
- **Taux d'enrichissement attendu** : 60-80% des contacts

## Plan d'exécution
- **Semaine 1** : Constituer la liste (50+ contacts qualifiés ICP)
- **Semaine 2** : Enrichir (email, LinkedIn, vérification)
- **Semaine 3** : Lancer séquence A (emails J0)
- **Semaine 4** : Analyser les résultats, itérer sur J3 et J7

## Métriques à tracker
- Liste constituée : X contacts
- Enrichis : X% (cible >70%)
- Envois J0 : X
- Ouvertures J0 : X% (cible >40%)
- Réponses total séquence : X% (cible >5%)
- Meetings bookés : X (cible >2%)
