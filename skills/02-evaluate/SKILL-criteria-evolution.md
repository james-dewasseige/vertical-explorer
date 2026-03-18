---
name: criteria-evolution
description: Analyser les scorecards et proposer des évolutions des critères de scoring.
trigger: /criteria
phase: 02-evaluate
inputs:
  - context/scorecard-criteria.md
  - context/verticals/*.md (toutes scorecards)
  - context/learnings.md
outputs:
  - Proposition de modifications (validation requise avant application)
  - MAJ scorecard-criteria.md si validé
---

# Criteria Evolution

## Trigger
Lancer après 5+ verticales scorées. Ne pas lancer trop tôt — pas assez de data.

## Process
1. Lire scorecard-criteria.md + toutes les scorecards dans verticals/
2. Lire learnings.md pour patterns identifiés
3. Analyser les corrélations :
   - Quels critères prédisent le mieux GO vs REJECT ?
   - Quels critères ne discriminent pas (scores toujours similaires) ?
   - Quels critères ont le plus de Low confidence (hard à mesurer) ?
4. Identifier les critères manquants (situations non couvertes)
5. Analyser si les pondérations sont cohérentes avec les résultats observés
6. Proposer des ajustements avec justification
7. DEMANDER VALIDATION avant toute modification
8. Si validé : mettre à jour scorecard-criteria.md avec versionning

## Règles
- Ne JAMAIS modifier scorecard-criteria.md sans validation explicite
- Tout changement doit être versionné dans le tableau historique
- Expliquer l'impact du changement sur les verticales déjà scorées
- Proposer de re-scorer les verticales impactées si changement majeur

## Output format

### Analyse de l'existant
**Critères discriminants** (varient beaucoup entre verticales) :
**Critères non discriminants** (quasi-identiques partout) :
**Critères difficiles à mesurer** (souvent Low confidence) :

### Propositions de modifications
Pour chaque proposition :
| Modification | Type | Justification | Impact sur scorecards existantes |
|-------------|------|---------------|----------------------------------|

### Critères manquants potentiels
- Critère X : justification, comment le mesurer

### Proposition d'ajustement des pondérations
| Critère | Poids actuel | Poids proposé | Justification |
|---------|-------------|---------------|---------------|

### Question de validation
"Voulez-vous appliquer ces modifications ? Elles impacteront [X verticales].
Je recommanderai de re-scorer [liste] après application."
