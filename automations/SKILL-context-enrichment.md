---
name: context-enrichment
description: Propager un nouveau data point à travers tous les fichiers contexte impactés.
trigger: /insight [texte] ou automatique après /ingest
phase: automation
inputs:
  - Nouveau data point (insight fondateur, résultat call, feedback terrain)
  - context/insights.md
  - context/verticals/{slug}.md (verticales concernées)
  - blueprints/{slug}/ (si blueprints existants)
---

# Context Enrichment

## Règle d'or
Chaque nouveau data point → se demander systématiquement :
1. Quels fichiers de contexte cela impacte-t-il ?
2. Quels scores changent ? Dans quelle direction ?
3. Quels blueprints doivent être régénérés ou mis à jour ?
4. Est-ce un pattern cross-vertical à capitaliser dans learnings.md ?

## Trigger : Insight fondateur (/insight "texte")

### Process
1. Parser l'insight : date = aujourd'hui, catégorie = à inférer ou demander
2. Identifier les verticales concernées (all / liste)
3. Vérifier si ça confirme, challenge ou invalide des insights existants
4. Ajouter à context/insights.md (format standard)
5. Dans chaque verticale concernée :
   - Référencer l'insight dans la section pertinente
   - Identifier si des hypothèses sont impactées
   - Signaler si un score doit être revu
6. Si l'insight est fort et concerne une verticale GO/PROMISING : suggérer /score

### Format dans insights.md
`[YYYY-MM-DD] [CATÉGORIE] [VERTICALES: all | slug1, slug2] Texte de l'insight`
`  → Statut: active`
`  → Challengé par: —`

## Trigger : Nouveau call (/ingest)
Voir SKILL-call-ingestion.md pour le process détaillé.
Après l'ingestion, context-enrichment vérifie :
- Y a-t-il des blueprints à mettre à jour ?
- Y a-t-il des hypothèses à changer de statut ?
- Y a-t-il un pattern cross-vertical à capturer ?

## Trigger : Feedback test (/feedback)
1. Mettre à jour les métriques dans verticals/{slug}.md
2. Mettre à jour les hypothèses testées (validée / invalidée)
3. Proposer re-scoring si résultats significatifs (>2 critères impactés)
4. Mettre à jour les blueprints si le feedback change le messaging ou le ciblage
5. Si pattern cross-vertical : ajouter dans learnings.md

## Matrice d'impact

| Type de data | Fichiers toujours impactés | Fichiers potentiellement impactés |
|-------------|--------------------------|----------------------------------|
| Nouveau call | verticals/{slug}.md, calls/YYYY.md | icp.md, objection-handling.md, scorecard |
| Insight fondateur | insights.md | verticals concernées, scoring |
| Feedback outbound | verticals/{slug}.md | outbound-campaign.md, icp.md |
| Feedback démo | verticals/{slug}.md | demo-script.md, objection-handling.md |
| Deal gagné/perdu | verticals/{slug}.md | tous les blueprints de la verticale |
| Learning cross-vertical | learnings.md | skills concernées (via /learn) |
