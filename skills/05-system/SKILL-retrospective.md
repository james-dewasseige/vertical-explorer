---
name: retrospective
description: Analyse complète d'une exploration de verticale — what worked / didn't.
trigger: /retro [vertical]
phase: 05-system
inputs:
  - context/verticals/{slug}.md (journal complet)
  - context/calls/ liés
  - blueprints/{slug}/ (tous blueprints)
outputs:
  - Rapport retro dans verticals/{slug}.md (journal de décision)
  - MAJ learnings.md
---

# Retrospective

## Quand lancer
- Après décision de parker / rejeter une verticale
- Après 4+ semaines d'exécution sur une verticale active
- À mi-parcours d'une exploration longue (>3 semaines)

## Process
1. Lire TOUT le contexte de la verticale (verticals/{slug}.md + tous les calls)
2. Reconstruire la timeline complète
3. Analyser les métriques
4. Identifier patterns et anti-patterns
5. Extraire les learnings cross-verticales
6. Formuler la recommandation
7. Mettre à jour verticals/{slug}.md et learnings.md

## Structure du rapport

### Timeline
| Date | Événement | Impact |
|------|-----------|--------|
| YYYY-MM-DD | Premier call | ... |

### Métriques finales
- Durée de l'exploration : X semaines
- Calls réalisés : X
- Prospects contactés (outbound) : X
- Réponses outbound : X (X%)
- Meetings bookés : X
- Démos réalisées : X
- Deals en pipeline : X (valeur : X€)
- Temps total investi (estimé) : X heures
- Score final : X/5.0 (confidence X/9)

### What worked
- Messages qui ont résonné : [exemples]
- Canaux les plus efficaces : [email / LinkedIn / cold call]
- Segments qui ont répondu : [description]
- Features qui ont créé le plus d'intérêt : [liste]
- Questions de call les plus utiles : [liste]

### What didn't work
- Messages qui n'ont pas fonctionné : [exemples + hypothèse pourquoi]
- Canaux inefficaces : [avec raison]
- Segments non réactifs : [avec raison]

### Surprises (positives et négatives)

### Hypothèses — bilan
| Hypothèse | Statut | Ce qu'on a appris |
|-----------|--------|-------------------|

### Learnings à propager cross-verticales
(Seront ajoutés à learnings.md)
- Learning 1 : [description + verticales concernées]

### Recommandation finale
**[INTENSIFIER | AJUSTER | PARKER | REJETER]**

Justification en 5-10 lignes.

### Si PARKER : conditions de réouverture
"Reprendre si [événement déclencheur] ou dans 3 mois."
