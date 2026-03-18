---
name: feedback-loop
description: Capturer et intégrer les résultats terrain dans le contexte de la verticale.
trigger: /feedback [vertical]
phase: 04-execute
inputs:
  - context/verticals/{slug}.md
  - blueprints/{slug}/ (blueprints actifs)
outputs:
  - MAJ verticals/{slug}.md (métriques, hypothèses, scoring)
  - MAJ blueprints concernés
  - MAJ learnings.md si patterns cross-verticales
---

# Feedback Loop

## Process
1. Collecter les résultats disponibles (demander à l'utilisateur ou lire les notes)
2. Analyser : what worked / didn't
3. Mettre à jour les hypothèses (validées / invalidées)
4. Re-scorer les critères impactés si justifié
5. Mettre à jour les blueprints si les résultats changent le messaging/ciblage
6. Décider : itérer / accélérer / pivoter / parker
7. Propager vers learnings.md si pattern cross-vertical

## Données à collecter

### Outbound
- Emails envoyés, ouvertures (%), réponses (%), bookings
- Messages LinkedIn envoyés, acceptations connexion, réponses
- Variante qui a le mieux performé
- Objections les plus fréquentes dans les réponses

### Landing page (si déployée)
- Visiteurs, temps sur page, formulaires soumis
- Source du trafic
- Section avec le meilleur engagement

### Calls & démos
- Calls réalisés, démos faites
- Taux de conversion call → démo
- Objections nouvelles entendues
- Niveau d'intérêt (1-5 maturité achat)

### Pipeline
- Deals créés dans Attio
- Valeur totale pipeline
- Stage actuel
- Deals perdus + raison

## Analyse framework
**What worked** : [messages, canaux, segments, features qui résonnent]
**What didn't** : [approches qui n'ont pas fonctionné]
**Surprises** : [inattendu, positif ou négatif]
**Hypothèses impactées** : [liste avec nouveau statut]
**Critères à re-scorer** : [liste avec nouveau score/confidence]

## Décision
- **ACCÉLÉRER** : résultats positifs → doubler la mise (plus de contacts, plus de budget)
- **ITÉRER** : résultats mitigés → changer le message ou le segment
- **PIVOTER** : sous-segment ciblé ne réagit pas → essayer un autre
- **PARKER** : résultats trop faibles malgré itérations → mettre de côté 3 mois

## Propagation cross-verticales
Si pattern identifié (ex: "l'objection X revient dans toutes les verticales") →
ajouter dans learnings.md et potentiellement mettre à jour les skills concernées.
