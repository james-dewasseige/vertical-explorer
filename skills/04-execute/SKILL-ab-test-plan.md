---
name: ab-test-plan
description: Plan de test A/B pour valider message, canal, segment et CTA.
trigger: /test [vertical]
phase: 04-execute
depends-on: outbound-campaign
inputs:
  - blueprints/{slug}/outbound-campaign.md
  - blueprints/{slug}/icp.md
  - context/verticals/{slug}.md
outputs:
  - blueprints/{slug}/ab-test-plan.md
---

# A/B Test Plan

## Ordre de priorité des tests
1. **Message** : quel pain point résonne le mieux ?
2. **Canal** : email vs LinkedIn vs cold call ?
3. **Segment** : quel sous-segment répond le mieux ?
4. **CTA** : démo vs trial vs contenu vs question ?

## Principe
- Tester une seule variable à la fois
- Minimum 30 contacts par variante pour être significatif
- Durée minimale : 2 semaines par test
- Critère de décision défini AVANT le lancement

## Template par test

### Test [n]
| Champ | Détail |
|-------|--------|
| Hypothèse | "Nous pensons que [A] performera mieux que [B] parce que [raison]" |
| Variable testée | Message / Canal / Segment / CTA |
| Variante A (contrôle) | Description |
| Variante B (challenger) | Description |
| Métrique principale | Ouverture / Réponse / Booking |
| Métrique secondaire | [autre] |
| Taille échantillon | X contacts par variante (min 30) |
| Durée | X semaines |
| Critère de victoire | "Variante B gagne si [métrique] ≥ [seuil]" |
| Date de début | YYYY-MM-DD |
| Date de décision | YYYY-MM-DD |
| Résultat | [À remplir] |
| Action | [À remplir : adopter / rejeter / affiner] |

## Plan séquencé 6 semaines
- **S1-2** : Test message (pain point A vs B vs C)
- **S3-4** : Test canal (email vs LinkedIn)
- **S5-6** : Test segment (TPE vs PME) ou test CTA

## Règles
- Ne jamais changer 2 variables en même temps
- Documenter les résultats même si non concluants
- Un test non concluant est une information (= les deux approches sont équivalentes)
- Mettre à jour outbound-campaign.md avec les variantes gagnantes
