---
name: vertical-scorecard
description: Scorer ou re-scorer une verticale sur les 9 critères pondérés.
trigger: /score [vertical]
phase: 02-evaluate
inputs:
  - context/verticals/{slug}.md (toutes sections)
  - context/calls/ (liés)
  - context/scorecard-criteria.md
  - context/insights.md
  - context/learnings.md
outputs:
  - MAJ scorecard dans verticals/{slug}.md
  - Recommandation GO / PROMISING / INVESTIGATE / PARK / REJECT
  - MAJ odoo-verticals-map.md
---

# Vertical Scorecard

## Objectif
Produire un score pondéré fiable sur une verticale, identifier les gaps de data,
et formuler une décision claire (GO / PROMISING / INVESTIGATE / PARK / REJECT).

## Process
1. Lire TOUT le contexte disponible (company.md, verticals/{slug}.md, calls liés,
   insights.md, learnings.md)
2. Pour chaque critère (1-9) :
   a. Lister les data points disponibles avec leur source
   b. Scorer 1-5 selon les échelles définies dans scorecard-criteria.md
   c. Assigner la confidence :
      - Low = desk research seul, aucun call
      - Med = 2-3 sources distinctes OU 1 call avec data directe
      - High = 3+ sources croisées + 2+ calls avec data directe
   d. Justifier en 1-2 phrases avec les sources citées
3. Calculer le score pondéré : Σ(score × poids) / Σ(poids)
4. Calculer la confidence globale : nb critères Med+ / 9
5. Appliquer les seuils de décision (voir scorecard-criteria.md)
6. Si INVESTIGATE : identifier précisément quels critères sont Low et
   quelles actions concrètes permettent de monter en confidence
7. Comparer aux autres verticales scorées si applicable
8. Mettre à jour verticals/{slug}.md (scorecard) et odoo-verticals-map.md (score + statut)

## Règles
- Ne jamais inventer de données pour "améliorer" un score
- La confidence est aussi importante que le score — être honnête
- Un score 3.8 avec 8/9 Med+ > un score 4.2 avec 3/9 Med+
- Si re-scoring : expliquer ce qui a changé depuis le dernier scoring

## Output format

### Scorecard complète
| # | Critère | Score | Poids | Pondéré | Confidence | Sources | Justification |
|---|---------|-------|-------|---------|------------|---------|---------------|
| 1 | Complexité opérationnelle | X | 1.2 | X×1.2 | Low/Med/High | ... | ... |
...

**Score pondéré total** : X.X/5.0
**Confidence globale** : X/9 critères à Med+
**Fiabilité** : [Score fiable | Score indicatif | Score non fiable]

### Décision : [GO | PROMISING | INVESTIGATE | PARK | REJECT]
Justification en 3-5 lignes.

### Si INVESTIGATE — gaps à combler
| Critère Low | Action pour monter en Med | Délai estimé |
|------------|--------------------------|--------------|

### Comparaison avec autres verticales scorées (si applicable)

### Prochaines étapes recommandées
