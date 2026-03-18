---
name: landing-page
description: Landing page copy (md) + version HTML déployable pour une verticale.
trigger: /blueprint landing-page [vertical]
phase: 03-build
depends-on: positioning, icp-definition
inputs:
  - context/verticals/{slug}.md
  - blueprints/{slug}/positioning.md
  - blueprints/{slug}/icp.md
  - blueprints/{slug}/pricing-model.md (si disponible)
outputs:
  - blueprints/{slug}/landing-page.md (copy)
  - blueprints/{slug}/landing-page.html (déployable)
---

# Landing Page

## Process
1. Lire positioning.md → headline, UVP, USPs, messaging
2. Lire icp.md → verbatims douleurs, personas
3. Lire pricing-model.md si disponible
4. Écrire la copy .md structurée section par section
5. Générer le HTML responsive single-file

## Structure copy (.md)

### 1. Hero Section
- **Headline** : UVP en 10 mots (reprend positioning.md)
- **Sub-headline** : développe en 25-30 mots
- **CTA principal** : bouton + texte (ex: "Demander une démo gratuite")
- **Social proof** : X clients / Y économisé / Z temps gagné

### 2. Problem Section — "Vous vous reconnaissez ?"
3 douleurs principales, dans les mots du prospect (verbatims calls) :
- Douleur 1 : titre + description courte
- Douleur 2 : titre + description courte
- Douleur 3 : titre + description courte

### 3. Solution Section — "Enobase change la donne"
Pour chaque douleur : comment Enobase la résout
- Feature/bénéfice 1 : titre + description + proof point
- Feature/bénéfice 2 : titre + description + proof point
- Feature/bénéfice 3 : titre + description + proof point

### 4. Social Proof
- Testimonials (placeholder si pas encore disponibles)
- Logos clients (placeholder)
- Métriques (ex: "Nos clients gagnent en moyenne X heures/semaine")

### 5. Pricing Preview
- Reprendre les tiers de pricing-model.md
- Ou CTA "Tarifs adaptés à votre activité — en discuter"

### 6. CTA Final
- Headline de clôture
- Formulaire simple (nom, email, téléphone, entreprise)
- Réduction de friction (ex: "Démo 30 min, sans engagement")
- Garanties (RGPD, données sécurisées)

## Version HTML (.html)
- Single-file responsive (mobile-first)
- CSS inline ou style tag
- No external dependencies
- Formulaire avec action vers Calendly ou formulaire simple
- Meta tags SEO (title, description, OG)
- Color scheme : [À adapter à la charte Enobase]
- Police : system fonts pour rapidité

## Règles
- Chaque claim doit être vrai et supporté
- Éviter le jargon tech
- CTAs clairs et uniques par section
- Mobile-first obligatoire
