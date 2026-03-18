---
name: call-ingestion
description: Ingérer un transcript Fireflies et enrichir automatiquement le contexte de la verticale.
trigger: /ingest [fireflies-meeting-id | "latest"]
phase: automation
inputs:
  - Transcript Fireflies (via MCP fireflies_get_transcript ou fireflies_fetch)
  - context/verticals/{slug}.md
  - context/calls/_template.md
outputs:
  - context/calls/YYYY-MM-DD_nom.md (nouveau fichier call)
  - MAJ context/verticals/{slug}.md
  - Suggestions de MAJ blueprints
---

# Call Ingestion

## Objectif
Transformer chaque call enregistré en data structurée qui enrichit automatiquement
le contexte de la verticale. Rien ne se perd.

## Process
1. Récupérer le transcript via MCP Fireflies :
   - `fireflies_get_transcript(transcriptId)` si ID fourni
   - `fireflies_get_transcripts()` puis sélectionner si "latest"
   - `fireflies_get_summary(transcriptId)` pour le résumé
2. Identifier la verticale concernée (depuis le contexte du call ou demander)
3. Extraire structurellement :
   - Info prospect : nom, titre, entreprise, taille
   - Douleurs mentionnées + quotes exacts + intensité (forte/moyenne/faible)
   - Outils actuellement utilisés + niveau de satisfaction (1-5)
   - Budget / WTP : chiffres évoqués, signaux implicites
   - Objections entendues + quelle réponse a été donnée + efficacité
   - Besoins fonctionnels : must-have vs nice-to-have + couverture Enobase
   - Maturité d'achat (1-5)
   - Signaux d'achat positifs ou négatifs
4. Créer le fichier call depuis _template.md :
   `context/calls/YYYY-MM-DD_nom-prospect.md`
5. Enrichir context/verticals/{slug}.md :
   - Ajouter à "Conversations & calls"
   - Mettre à jour "Douleurs identifiées" (nouvelles + récurrences)
   - Mettre à jour "Outils en place"
   - Mettre à jour "Personas / ICP"
   - Mettre à jour "Hypothèses actives" (testées / validées / invalidées)
6. Calculer l'impact sur le scoring :
   - Quels critères ont de nouvelles données ?
   - Les scores changent-ils ? La confidence monte-t-elle ?
7. Identifier les impacts sur les blueprints existants
8. Proposer les next actions

## Output du /ingest

### Résumé du call créé
Fichier : `context/calls/YYYY-MM-DD_nom.md` ✅

### Enrichissements du contexte verticale
- **Nouvelles douleurs** : [liste]
- **Douleurs confirmées** : [liste]
- **Outils ajoutés** : [liste]
- **Hypothèses testées** : [avec nouveau statut]

### Impact scoring
| Critère | Ancien score | Nouveau score | Confidence avant | Confidence après |
|---------|-------------|---------------|-----------------|-----------------|

### Blueprints à mettre à jour
- [ ] icp.md — raison
- [ ] outbound-campaign.md — raison
- [ ] objection-handling.md — raison

### Insights cross-verticales potentiels
[Le cas échéant, patterns qui méritent d'aller dans insights.md ou learnings.md]

### Next actions suggérées
- [ ] (priorité haute) ...
- [ ] (priorité moyenne) ...

## Règles
- Utiliser les verbatims exacts (guillemets) — ne jamais paraphraser les quotes
- Si la verticale n'est pas claire, demander avant d'ingérer
- Ne jamais inventer ce qui n'a pas été dit dans le call
- Si transcript peu informatif, le noter et créer le fichier quand même
