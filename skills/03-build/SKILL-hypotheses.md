---
name: hypotheses
description: Carte d'hypothèses traçables par verticale — statuts et résultats.
trigger: /blueprint hypotheses [vertical]
phase: 03-build
inputs:
  - context/verticals/{slug}.md
  - context/calls/ liés
outputs:
  - blueprints/{slug}/hypotheses.md
  - MAJ section hypothèses dans verticals/{slug}.md
---

# Hypotheses Map

## Objectif
Rendre explicites toutes les hypothèses sur lesquelles reposent les décisions.
Chaque hypothèse est testable, tracée et mise à jour au fur et à mesure.

## Process
1. Lire verticals/{slug}.md → hypothèses existantes, calls, scoring
2. Identifier les hypothèses implicites dans le scoring et les blueprints
3. Catégoriser et prioriser (les hypothèses critiques en premier)
4. Définir des méthodes de test concrètes
5. Documenter les résultats au fur et à mesure

## Catégories d'hypothèses
- **MARCHÉ** : taille, structure, comportements d'achat
- **DOULEUR** : existence, intensité, fréquence du problème
- **SOLUTION** : notre approche résout bien le problème
- **PRICING** : willingness to pay, modèle accepté
- **DISTRIBUTION** : canaux fonctionnent, personas accessibles
- **COMPÉTITION** : notre différenciation est perçue et valorisée

## Pour chaque hypothèse

### Template
| Champ | Contenu |
|-------|---------|
| ID | H-[n] |
| Hypothèse | Énoncé testable : "Nous croyons que [X] fera [Y]" |
| Catégorie | MARCHÉ / DOULEUR / SOLUTION / PRICING / DISTRIBUTION / COMPÉTITION |
| Criticité | Haute (changerait GO/PARK) / Moyenne / Basse |
| Critère(s) scorecard | [n°] |
| Statut | Non testée / En test / Validée / Invalidée |
| Méthode de test | [call discovery / outbound / A/B / landing page...] |
| Critère de validation | "Si X alors validée" |
| Critère d'invalidation | "Si Y alors invalidée" |
| Résultat | [vide si non testée] |
| Source | [call-XX, web, outbound...] |
| Date | YYYY-MM-DD |
| Impact décisionnel | Quoi faire si validée / si invalidée |

## Priorisation
Traiter en priorité les hypothèses qui, si invalidées, changeraient la décision GO/PARK.

## Dashboard
- Total hypothèses : X
- Validées : X (X%)
- Invalidées : X
- En test : X
- Non testées : X
- Couverture scoring : X/9 critères avec au moins 1 hypothèse testée
