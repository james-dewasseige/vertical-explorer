---
name: sales-deck
description: Pitch deck structuré 10-12 slides pour une verticale.
trigger: /blueprint sales-deck [vertical]
phase: 03-build
depends-on: positioning, icp-definition
inputs:
  - context/verticals/{slug}.md
  - blueprints/{slug}/positioning.md
  - blueprints/{slug}/icp.md
  - blueprints/{slug}/pricing-model.md
outputs:
  - blueprints/{slug}/sales-deck.md
---

# Sales Deck

## Process
1. Lire positioning.md + icp.md + pricing-model.md
2. Extraire : douleurs top 3, USPs, différenciateurs, pricing
3. Structurer le deck en 10-12 slides
4. Pour chaque slide : titre, contenu, notes orateur, visuels suggérés

## Structure (10-12 slides)

### Slide 1 — Titre
- Titre : "Enobase pour [Vertical] — [UVP headline]"
- Sous-titre : [Tagline verticale]
- Logo Enobase

### Slide 2 — Le problème
- Titre : "Les [NOM VERTICAL] perdent [X] heures/semaine sur [problème]"
- 3 douleurs avec chiffres (sources calls)
- Visual : illustration du chaos opérationnel

### Slide 3 — L'impact du statu quo
- "Continuer comme ça coûte [X€/an]"
- Calcul du coût de l'inaction
- Opportunité manquée

### Slide 4 — La solution
- "Enobase : [1 phrase positioning]"
- 3 piliers de la solution
- Screenshot ou mockup

### Slide 5 — Feature clé 1
- Douleur → Feature → Bénéfice → Résultat (avant/après)
- Quote call si disponible

### Slide 6 — Feature clé 2
- Même structure

### Slide 7 — Feature clé 3
- Même structure

### Slide 8 — Différenciation
- Tableau comparatif vs alternatives principales
- Nos avantages clés

### Slide 9 — Social Proof
- Testimonials, logos clients, métriques
- (Utiliser placeholders si pas encore de clients dans cette verticale)

### Slide 10 — Pricing
- Tiers et prix
- "Commencez à [X€/mois]"

### Slide 11 — Prochaines étapes
- CTA clair : "Démo 30 min — réservez votre créneau"
- Timeline d'onboarding (combien de temps pour démarrer ?)

### Slide 12 — Contact
- Nom, email, Calendly

## Notes orateur
Pour chaque slide : 3-5 bullets de tips pour l'oral

## Règles
- Max 30 mots par slide (hors notes)
- Un message par slide
- Chaque slide doit faire avancer la conviction, pas juste informer
