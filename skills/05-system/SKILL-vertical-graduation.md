---
name: vertical-graduation
description: Évaluer si une verticale remplit les critères pour passer au stade suivant.
trigger: /graduate [vertical]
phase: 05-system
inputs:
  - context/verticals/{slug}.md
  - blueprints/{slug}/ (tous blueprints)
  - context/odoo-verticals-map.md
outputs:
  - MAJ statut dans verticals/{slug}.md
  - MAJ odoo-verticals-map.md
---

# Vertical Graduation

## Objectif
Valider de façon systématique qu'une verticale remplit tous les critères pour passer
au stade suivant, et identifier précisément ce qui manque.

## Critères par stade

### Not Explored → Discovery
- [ ] Fichier verticals/{slug}.md créé depuis le template
- [ ] Statut mis à jour dans odoo-verticals-map.md

### Discovery → Scoring
- [ ] Fichier de verticale pré-rempli (au moins 50% des sections)
- [ ] 2+ calls discovery réalisés et ingérés
- [ ] Concurrence mappée (au moins 3-4 concurrents identifiés)
- [ ] Premier scoring réalisé (même si tout Low confidence)
- [ ] Hypothèses initiales documentées

### Scoring → Validation
- [ ] Score ≥ 3.5
- [ ] Confidence globale ≥ 5/9 critères à Med+
- [ ] 5+ calls réalisés et ingérés
- [ ] ICP défini (blueprint icp.md créé)
- [ ] Hypothèses critiques (criticité Haute) au moins en cours de test
- [ ] Positioning défini (blueprint positioning.md créé)
- [ ] Concurrent principal analysé (battlecards)

### Validation → Active
- [ ] Score ≥ 4.0
- [ ] Confidence globale ≥ 7/9 critères à Med+
- [ ] Blueprints clés produits : ICP, positioning, outbound, landing-page
- [ ] 1+ test outbound avec résultats positifs (réponses >5%, bookings obtenus)
- [ ] Pricing validé avec 3+ prospects (WTP confirmé)
- [ ] 1+ deal en pipeline (même si early stage)
- [ ] Launch checklist validée

### Active → Scaled
- [ ] 3+ deals closés (revenus réels)
- [ ] Process de vente reproductible (script, objections, pricing)
- [ ] Marketing engine en place (contenu, SEO ou ads)
- [ ] Onboarding standardisé (ne nécessite plus d'intervention fondateur)
- [ ] Métriques de rétention positives (churn < X%)

## Output format

### Évaluation actuelle
**Stade actuel** : [stade]
**Stade cible** : [stade suivant]

**Critères remplis** : ✅ [liste]
**Critères manquants** : ❌ [liste]

### Décision
**[GRADUATION ACCORDÉE | GRADUATION REFUSÉE]**

### Si refusée — plan pour compléter les critères manquants
| Critère manquant | Action | Délai estimé | Responsable |
|-----------------|--------|-------------|-------------|

### Nouvelle entrée journal de décision
[YYYY-MM-DD] Graduation [stade → stade] [accordée/refusée]. Raison : ...
