---
name: odoo-market-map
description: Scanner toutes les verticales Odoo et produire un ranking par potentiel.
trigger: /scan
phase: 01-explore
inputs:
  - context/company.md
  - context/odoo-verticals-map.md
  - context/scorecard-criteria.md
  - context/insights.md
outputs:
  - Ranking des verticales candidates
  - Recommandation des 5-10 à explorer en priorité
---

# Odoo Market Map Scanner

## Objectif
Analyser l'ensemble des ~91 verticales Odoo et identifier les candidates prioritaires
pour Enobase. Produire un ranking actionnable avec justification.

## Process
1. Lire context/company.md → différenciateurs Enobase, contraintes, objectifs
2. Lire context/odoo-verticals-map.md → liste complète, statuts actuels
3. Lire context/scorecard-criteria.md → critères de décision
4. Lire context/insights.md → intuitions fondateurs à intégrer
5. Pour chaque verticale non explorée, quick scoring (1-3) sur 4 critères :
   - Complexité opérationnelle (valeur potentielle IA)
   - Taille marché (France/Europe)
   - Insatisfaction outils estimée
   - Fit produit Enobase estimé
6. Classer par score total pondéré
7. Identifier "quick wins" et "long bets"
8. Recommander 5-10 prioritaires avec justification

## Règles
- Utiliser le bon sens et la connaissance générale des industries
- Signaler les verticales où Odoo est déjà très fort (compétition directe)
- Favoriser les verticales où la complexité est élevée (valeur IA maximale)
- Tenir compte des insights fondateurs qui peuvent sur/sous-pondérer des verticales

## Output format

### Ranking Top 10 — Priorités immédiates
| Rang | Verticale | Slug | Score rapide | Catégorie | Justification 1 ligne |
|------|-----------|------|-------------|-----------|----------------------|

### Quick Wins (testables <2 semaines)
Liste avec raison

### Long Bets (grand marché, exploration plus longue)
Liste avec raison

### À éviter (pourquoi)
Liste avec raison

### Recommandation de la semaine
"Explorer en priorité [X] parce que [Y]. Commencer par /explore [slug]."
