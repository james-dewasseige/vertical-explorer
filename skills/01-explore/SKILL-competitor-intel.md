---
name: competitor-intel
description: Intelligence concurrentielle approfondie sur une verticale.
trigger: /intel [vertical]
phase: 01-explore
inputs:
  - context/verticals/{slug}.md
  - context/company.md (différenciateurs)
outputs:
  - MAJ section concurrence dans verticals/{slug}.md
  - Optionnel : blueprints/{slug}/battlecards.md
---

# Competitor Intelligence

## Objectif
Cartographier l'écosystème concurrentiel d'une verticale pour identifier les angles
d'attaque Enobase et comprendre le spectre d'équipement des prospects.

## Process
1. Lire context/verticals/{slug}.md → concurrents déjà identifiés
2. Lire context/company.md → différenciateurs Enobase
3. Web search :
   - "[vertical] management software"
   - "logiciel [vertical] France"
   - "[vertical] ERP comparison"
   - G2 / Capterra catégorie [vertical]
   - Forums spécialisés [vertical]
4. Pour chaque concurrent identifié :
   - Pricing (plans, tarifs, modèle)
   - Forces (ce qu'ils font vraiment bien)
   - Faiblesses (limites, plaintes utilisateurs)
   - Positionnement (à qui ils s'adressent)
   - Taille / notoriété estimée
5. Mapper le spectre d'équipement (% no-tool / inadapté / vertical / ERP)
6. Identifier les angles d'attaque Enobase par concurrent
7. Mettre à jour verticals/{slug}.md section concurrence

## Règles
- Prioriser les reviews G2/Capterra pour les vraies douleurs utilisateurs
- Chercher des quotes de frustration → potentiel switching intent
- Identifier qui a Odoo comme concurrent direct (signal maturité marché)

## Output format

### Tableau concurrents
| Outil | Type | Notoriété | Prix | Forces | Faiblesses | Angle attaque Enobase |
|-------|------|-----------|------|--------|------------|----------------------|

### Spectre d'équipement
- % no tool (Excel/papier) : X%
- % outils inadaptés (bricolage multi-outils) : X%
- % solution verticale dédiée : X%
- % ERP généraliste : X%

### Opportunité Enobase
Analyse 3-5 lignes : où se situe l'opportunité dans ce spectre ?

### Battlecards (si requis)
Pour les 2-3 concurrents principaux :
- Notre avantage vs eux
- Leur avantage vs nous
- Trap question à poser en call
- Réponse à "mais [concurrent] fait déjà ça"
- Never say
