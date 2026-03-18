# VEE — Référence rapide des commandes

## Toutes les commandes slash

### Phase 1 — EXPLORE (Comprendre)

| Commande | Description | Inputs lus | Outputs produits |
|---------|-------------|-----------|-----------------|
| `/scan` | Scanner les ~91 verticales Odoo, produire un ranking | company.md, odoo-verticals-map.md, scorecard-criteria.md | Ranking top 10 + recommandations |
| `/explore [vertical]` | Démarrer l'exploration d'une verticale | company.md, odoo-verticals-map.md, insights.md | verticals/{slug}.md (créé + pré-rempli) |
| `/interview [vertical]` | Guide d'entretien adaptatif | verticals/{slug}.md, calls liés, learnings.md | Guide structuré par blocs |
| `/intel [vertical]` | Intelligence concurrentielle | verticals/{slug}.md, company.md | MAJ section concurrence + battlecards |

### Phase 2 — EVALUATE (Scorer)

| Commande | Description | Inputs lus | Outputs produits |
|---------|-------------|-----------|-----------------|
| `/score [vertical]` | Scorer sur 9 critères pondérés | verticals/{slug}.md, calls, scorecard-criteria.md | MAJ scorecard + décision GO/PARK/REJECT |
| `/segment [vertical]` | Identifier et scorer les sous-segments | verticals/{slug}.md, calls liés | Section Segments dans verticals/{slug}.md |
| `/criteria` | Analyser et faire évoluer les critères | scorecard-criteria.md, toutes scorecards | Propositions de modifications |
| `/compare [v1] [v2]` | Comparer plusieurs verticales | verticals/{v1}.md, verticals/{v2}.md | Tableau comparatif côte à côte |

### Phase 3 — BUILD (Produire)

| Commande | Description | Inputs | Output |
|---------|-------------|--------|--------|
| `/blueprint icp [v]` | ICP détaillé | verticals/{slug}.md, calls | blueprints/{slug}/icp.md |
| `/blueprint positioning [v]` | Positioning + messaging | icp.md, verticals/{slug}.md | blueprints/{slug}/positioning.md |
| `/blueprint pricing [v]` | Modèle de pricing | icp.md, verticals/{slug}.md | blueprints/{slug}/pricing-model.md |
| `/blueprint landing-page [v]` | Landing page copy + HTML | positioning.md, icp.md | landing-page.md + landing-page.html |
| `/blueprint sales-deck [v]` | Pitch deck 10-12 slides | positioning.md, icp.md | blueprints/{slug}/sales-deck.md |
| `/blueprint outbound [v]` | Séquences email + LinkedIn | icp.md, positioning.md | blueprints/{slug}/outbound-campaign.md |
| `/blueprint objections [v]` | Guide d'objections | calls liés, positioning.md | blueprints/{slug}/objection-handling.md |
| `/blueprint demo-script [v]` | Script de démo | icp.md, positioning.md | blueprints/{slug}/demo-script.md |
| `/blueprint roi-calculator [v]` | Calculateur ROI | icp.md, pricing-model.md | roi-calculator.md + roi-calculator.html |
| `/blueprint hypotheses [v]` | Carte d'hypothèses | verticals/{slug}.md, calls | blueprints/{slug}/hypotheses.md |
| `/blueprint all [v]` | Tous les blueprints | Tout le contexte | Tous les fichiers ci-dessus |

### Phase 4 — EXECUTE (Lancer)

| Commande | Description | Inputs | Output |
|---------|-------------|--------|--------|
| `/targets [vertical]` | Critères + spec de liste outbound | icp.md, outbound-campaign.md | blueprints/{slug}/outbound-targets.md |
| `/test [vertical]` | Plan de tests A/B | outbound-campaign.md, icp.md | blueprints/{slug}/ab-test-plan.md |
| `/launch [vertical]` | Checklist opérationnelle de lancement | Tout blueprints/{slug}/ | Checklist GO/NO-GO |
| `/feedback [vertical]` | Intégrer les résultats terrain | verticals/{slug}.md, blueprints | MAJ contexte + blueprints + learnings |

### Phase 5 — SYSTEM (Apprendre)

| Commande | Description | Inputs | Output |
|---------|-------------|--------|--------|
| `/retro [vertical]` | Retrospective complète | Tout le contexte verticale | Rapport retro + MAJ learnings.md |
| `/graduate [vertical]` | Évaluer la graduation de stade | verticals/{slug}.md, blueprints | Décision graduation + plan |
| `/learn` | Extraire learnings cross-verticales | learnings.md, toutes verticals | Propositions MAJ skills |

### Automations

| Commande | Description | Inputs | Output |
|---------|-------------|--------|--------|
| `/ingest [call-id]` | Ingérer transcript Fireflies | Transcript Fireflies + context | calls/YYYY-MM-DD_nom.md + MAJ verticale |
| `/ingest latest` | Ingérer le dernier call | Dernier transcript + context | Idem |
| `/insight "texte"` | Ajouter insight fondateur | insights.md + verticales concernées | MAJ insights.md + propagation |
| `/sprint` | Plan hebdomadaire | Toutes verticales actives | Plan semaine jour par jour |
| `/status` | Vue d'ensemble globale | odoo-verticals-map.md + verticals | Dashboard toutes verticales |

---

## Statuts d'une verticale

```
Not Explored → Discovery → Scoring → Validation → Active → Scaled
                                                    ↓
                                                  Parked
                                                    ↓
                                                 Rejected
```

| Statut | Définition | Critères d'entrée |
|--------|-----------|-------------------|
| Not Explored | Pas encore touchée | — |
| Discovery | Exploration en cours | Fichier créé, 2+ calls |
| Scoring | Score en cours de construction | 2 calls, premier scoring |
| Validation | Score ≥ 3.5, confidence 5+/9 Med | 5+ calls, ICP défini |
| Active | En mode lancement | Score ≥ 4.0, blueprints prêts, test outbound positif |
| Scaled | Process reproductible | 3+ deals closés |
| Parked | Mis de côté temporairement | Score < 3.0 ou pas de signal marché |
| Rejected | Ne pas poursuivre | Score < 2.5 + raison documentée |

---

## Chaîne de dépendances blueprints

```
/explore
  └── /score → GO
        └── /blueprint icp
              ├── /blueprint positioning
              │     ├── /blueprint landing-page → .html
              │     ├── /blueprint sales-deck
              │     ├── /blueprint outbound → /targets → /test
              │     ├── /blueprint demo-script
              │     └── /blueprint objections
              ├── /blueprint pricing → /blueprint roi-calculator → .html
              └── /blueprint hypotheses
```

---

## Scores de confidence

| Niveau | Définition | Critères |
|--------|-----------|---------|
| Low | Desk research seul | Aucun call, sources web uniquement |
| Med | Données croisées | 2-3 sources distinctes OU 1 call avec data directe |
| High | Données solides | 3+ sources + 2+ calls avec data directe |

## Formule de scoring
```
Score pondéré = Σ(score_i × poids_i) / 9.4
Seuils : ≥4.0 GO | 3.5-3.9 PROMISING | 3.0-3.4 INVESTIGATE | 2.5-2.9 PARK | <2.5 REJECT
```
