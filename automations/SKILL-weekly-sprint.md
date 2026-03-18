---
name: weekly-sprint
description: Générer le plan d'action hebdomadaire basé sur l'état de toutes les verticales.
trigger: /sprint
phase: automation
inputs:
  - context/odoo-verticals-map.md (vue d'ensemble)
  - context/verticals/*.md (toutes les verticales actives)
  - context/company.md (contraintes, objectifs)
outputs:
  - Plan de sprint semaine en cours
---

# Weekly Sprint

## Process
1. Lire odoo-verticals-map.md → toutes les verticales avec statut et score
2. Lire chaque verticale en Discovery, Scoring, Validation, Active :
   - Statut actuel, score, confidence
   - Calls en attente d'ingestion
   - Blueprints à produire
   - Actions overdue (deadline dépassée)
   - Prochaine graduation possible ?
3. Lire context/company.md → contraintes et objectifs actuels
4. Prioriser les actions de la semaine
5. Structurer le plan jour par jour

## Analyse par verticale active
Pour chaque verticale (Discovery → Active) :
| Verticale | Statut | Score | Conf | Calls faits | Blueprints | Retards | Priorité semaine |
|-----------|--------|-------|------|------------|------------|---------|-----------------|

## Règles de priorisation
1. **Urgent** : Verticales avec calls non ingérés (contexte se perd)
2. **Haute** : Verticales proches d'un GO (score 3.5+, manque data sur 1-2 critères)
3. **Haute** : Tests outbound en cours avec résultats à analyser
4. **Moyenne** : Nouvelles verticales à explorer (selon backlog /scan)
5. **Moyenne** : Blueprints à produire pour verticales GO
6. **Basse** : Mise à jour système, learnings, rétros

## Plan de sprint

### LUNDI — Revue & Planning
- /status → vue d'ensemble
- Ingérer les calls de la semaine passée
- Prioriser les verticales de la semaine

### MARDI-MERCREDI — Calls & Découverte
- Calls planifiés avec prospects
- Préparation calls avec /interview
- Ingestion immédiate post-call avec /ingest

### JEUDI — Blueprints & Scoring
- Produire les blueprints des verticales GO
- Re-scorer les verticales avec nouvelles données
- Mettre à jour les séquences outbound

### VENDREDI — Feedback & Préparation
- /feedback sur les tests outbound en cours
- Re-scorer si justifié
- Préparer la semaine suivante (prospects, calls, séquences)

## Output

### Vue d'ensemble semaine [n] — [dates]
**Verticales actives** : X
**Calls à faire cette semaine** : X
**Blueprints à produire** : X
**Tests outbound en cours** : X

### Top 3 priorités de la semaine
1. [Action] sur [verticale] — raison — deadline
2. [Action] sur [verticale] — raison — deadline
3. [Action] sur [verticale] — raison — deadline

### Plan détaillé jour par jour
| Jour | Actions | Verticale | Durée estimée |
|------|---------|-----------|--------------|

### Métriques hebdo à tracker
- Verticales explorées cette semaine : X
- Calls réalisés : X
- Blueprints produits : X
- Re-scorings : X
- Décisions GO/PARK/REJECT : X

### Recommandation de la semaine
"Cette semaine, concentrer sur [X] parce que [Y]. Actions prioritaires : [1, 2, 3]."
