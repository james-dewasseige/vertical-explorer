---
name: playbook-update
description: Mettre à jour les skills avec les learnings accumulés sur plusieurs verticales.
trigger: /learn
phase: 05-system
inputs:
  - context/learnings.md
  - context/verticals/*.md (toutes)
  - skills/**/*.md (toutes les skills)
outputs:
  - Propositions de MAJ des skills
  - MAJ learnings.md
---

# Playbook Update

## Quand lancer
- Après 3+ verticales complètement explorées (scorées + au moins 4 semaines d'exécution)
- Quand learnings.md commence à avoir des patterns récurrents
- Après une retrospective qui a identifié des learnings cross-système

## Process
1. Lire context/learnings.md → tous les patterns identifiés
2. Lire tous les verticals/{slug}.md → extraire patterns récurrents
3. Identifier patterns récurrents (3+ occurrences = signal fort)
4. Pour chaque pattern : identifier quelle(s) skill(s) doit évoluer
5. Proposer des modifications concrètes
6. DEMANDER VALIDATION avant application
7. Si validé : mettre à jour les skills concernées et versionner

## Règles
- Ne jamais modifier une skill sans validation explicite
- Distinguer : amélioration du process vs changement de stratégie
- Documenter chaque modification avec la raison et le learning source
- Versionner les skills modifiées (ajouter date de MAJ dans le header)

## Output format

### Patterns identifiés
| Pattern | Nb occurrences | Verticales | Confiance |
|---------|---------------|-----------|-----------|
| Pattern 1 | 3 | event-catering, catering, hotel | Med |

### Propositions de modifications

#### Skill X — modification proposée
**Modification** : [description précise]
**Raison** : [learning source]
**Impact estimé** : [sur quoi ça améliore le process]
**Skills concernées** : [liste]
**Effort de mise à jour** : [faible | moyen | élevé]

### Modifications rejetées (et pourquoi)

### Mise à jour learnings.md
Nouveaux patterns à ajouter :
- Pattern X : [description + verticales + date découverte]
