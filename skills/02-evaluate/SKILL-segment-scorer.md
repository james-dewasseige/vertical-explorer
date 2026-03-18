---
name: segment-scorer
description: Identifier et prioriser les sous-segments au sein d'une verticale.
trigger: /segment [vertical]
phase: 02-evaluate
inputs:
  - context/verticals/{slug}.md
  - context/calls/ liés
outputs:
  - Section "Segments" dans verticals/{slug}.md
---

# Segment Scorer

## Objectif
Au sein d'une verticale, identifier les sous-segments distincts et prioriser
le "beachhead" — le segment à attaquer en premier.

## Process
1. Lire context/verticals/{slug}.md + calls liés
2. Identifier les dimensions de segmentation pertinentes pour cette verticale
3. Définir les segments candidats (3-7 segments)
4. Scorer chaque segment sur 5 critères (1-5)
5. Calculer le score total par segment
6. Identifier le beachhead et les segments de follow-up
7. Mettre à jour verticals/{slug}.md avec section Segments

## Dimensions de segmentation
- Taille : solo, TPE (2-10), PME (10-50), ETI (50-250)
- Géographie : local, régional, national, international
- Maturité digitale : paper-based, Excel, multi-outils, ERP
- Sous-spécialité du vertical (ex: event catering → corporate vs weddings vs festivals)
- Douleur principale
- Modèle de revenus (prestation, abonnement, mixte)

## Évaluation par segment (1-5)
| Critère | Description |
|---------|-------------|
| Taille segment | Combien d'entreprises en France/Europe |
| Intensité douleur | À quel point la douleur est-elle aiguë ? |
| Capacité à payer | Budget et vélocité de décision |
| Facilité d'accès | Peut-on les atteindre facilement ? |
| Fit produit | Notre produit répond-il bien à leurs besoins ? |

## Output format

### Tableau des segments
| Segment | Taille | Douleur | WTP | Accès | Fit | Total | Priorité |
|---------|--------|---------|-----|-------|-----|-------|----------|

### Beachhead segment recommandé
**Segment** : [nom]
**Taille estimée** : X entreprises (France)
**Profil type** : description
**Douleur principale** : ...
**Pourquoi ce segment en premier** : justification 3-5 lignes

### Segments de follow-up (après beachhead validé)
Liste avec timing recommandé

### Segments à éviter (pourquoi)
