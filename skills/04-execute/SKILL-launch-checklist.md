---
name: launch-checklist
description: Checklist opérationnelle complète avant de lancer une verticale.
trigger: /launch [vertical]
phase: 04-execute
inputs:
  - context/verticals/{slug}.md
  - blueprints/{slug}/ (tous les blueprints)
outputs:
  - Checklist avec statut par item (prêt / en cours / manquant)
---

# Launch Checklist

## Process
1. Lire le statut de tous les blueprints dans blueprints/{slug}/
2. Lire la scorecard dans verticals/{slug}.md
3. Évaluer chaque item de la checklist
4. Indiquer le statut : ✅ Prêt | 🔄 En cours | ❌ Manquant
5. Identifier le chemin critique (items bloquants)
6. Formuler la recommandation GO / NO-GO

## Pré-requis stratégiques
- [ ] Scorecard ≥ 3.5 avec confidence ≥ 5/9 Med+
- [ ] ICP validé (3+ calls réalisés, au moins 2 personas confirmés)
- [ ] Positioning finalisé et relu
- [ ] Au moins 1 blueprint outbound prêt (séquence email ou LinkedIn)
- [ ] Pricing model défini et assumé

## Assets de lancement
- [ ] Landing page déployée (même basique) OU Calendly booking page
- [ ] Séquence outbound finalisée (5 emails + 3 messages LinkedIn)
- [ ] Target list : 50+ contacts qualifiés ICP, enrichis (email + LinkedIn)
- [ ] Sales deck prêt pour démos (même en version draft)
- [ ] Demo script préparé
- [ ] Objection handling guide créé
- [ ] ROI calculator prêt (même en version .md)

## Infrastructure
- [ ] Attio : pipeline configuré pour cette verticale (stages : Lead → Contacted → Meeting → Demo → Proposal → Closed)
- [ ] Tracking outbound en place (opens, replies, bookings)
- [ ] Calendrier booking opérationnel (Calendly ou équivalent)
- [ ] Email signature avec CTA pertinent
- [ ] Domaine de tracking email vérifié (pour éviter spam)

## Hypothèses & risques
- [ ] Hypothèses critiques (criticité Haute) identifiées
- [ ] Plan de test pour les 2-3 hypothèses critiques
- [ ] Risques principaux documentés + mitigation

## Alerte si items critiques manquants
**Items bloquants** (sans eux, ne pas lancer) :
- Scorecard ≥ 3.5
- Target list 50+ contacts
- Au moins 1 séquence outbound finalisée

**Items importants** (lancer avec ces gaps est risqué mais possible) :
- Landing page déployée
- ROI calculator

## Décision finale
**GO si** : tous les items bloquants ✅ + au moins 70% des items importants ✅
**NO-GO si** : items bloquants ❌

### Recommandation :
[GO avec date de lancement prévue] OU [NO-GO : manque [X], délai estimé [Y]]
