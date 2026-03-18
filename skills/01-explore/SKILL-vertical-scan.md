---
name: vertical-scan
description: Recherche initiale approfondie sur une verticale. Crée et pré-remplit le fichier de contexte.
trigger: /explore [vertical]
phase: 01-explore
inputs:
  - context/company.md
  - context/odoo-verticals-map.md
  - context/insights.md (filtré sur la verticale)
outputs:
  - context/verticals/{slug}.md (créé depuis _template.md, pré-rempli)
  - Mise à jour context/odoo-verticals-map.md (statut → Discovery)
---

# Vertical Scan

## Objectif
Quand l'utilisateur lance /explore [vertical] :
1. Créer le fichier de verticale depuis le template
2. Web search : taille de marché, acteurs, outils existants, douleurs connues
3. Rechercher la position d'Odoo (module dédié ? forums ? retours ?)
4. Pré-remplir un maximum de sections
5. Transformer les lacunes d'info en hypothèses à tester
6. Produire un premier scoring desk research (confidence Low)
7. Générer les 3-5 premières actions
8. Mettre à jour odoo-verticals-map.md

## Recherches web à effectuer
- "[vertical] market size France" / "[vertical] marché France taille"
- "[vertical] software tools" / "logiciel [vertical]"
- "[vertical] ERP" / "best [vertical] software 2025 2026"
- "odoo [vertical]" / forum Odoo
- "[vertical] associations professionnelles France"
- "[vertical] salons événements France"
- G2/Capterra pour la catégorie

## Règles
- Ne pas inventer de données. Si introuvable, écrire "[À DÉCOUVRIR — via calls]"
- Croiser au moins 2 sources pour les chiffres de marché
- Checker odoo.com/industries pour le module vertical
- Consulter context/insights.md pour les intuitions fondateurs pertinentes
- Toujours proposer 3-5 prospects à contacter et comment les trouver

## Output format
Fichier context/verticals/{slug}.md créé et pré-rempli avec :
- Toutes les sections connues renseignées (sources citées)
- Sections inconnues marquées [À DÉCOUVRIR — via calls]
- Scorecard avec scores Low confidence basés sur desk research
- 3-5 hypothèses initiales à tester en call
- Liste de 3-5 premiers prospects à contacter + canaux
- Next actions priorisées
