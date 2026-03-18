---
name: pricing-model
description: Modèle de pricing adapté à une verticale, basé sur la concurrence et le WTP.
trigger: /blueprint pricing [vertical]
phase: 03-build
depends-on: icp-definition, competitor-intel
inputs:
  - context/verticals/{slug}.md
  - context/calls/ liés (données WTP)
  - blueprints/{slug}/icp.md
  - context/company.md
outputs:
  - blueprints/{slug}/pricing-model.md
---

# Pricing Model

## Process
1. Lire calls → extraire toutes les données WTP et budget actuel
2. Analyser les prix concurrents (section concurrence dans verticals/{slug}.md)
3. Estimer la valeur créée (temps gagné, erreurs évitées, coût des alternatives)
4. Définir le modèle de pricing
5. Construire les tiers
6. Calculer les unit economics estimés

## Structure du blueprint

### 1. Analyse concurrentielle des prix
| Concurrent | Modèle | Prix | Features incluses | Limites |
|-----------|--------|------|-------------------|---------|

### 2. WTP (Willingness to Pay)
- **Déclaré** (calls) : "X€/mois" [source: call-XX]
- **Implicite** : budget actuel outil × acceptable multiplier
- **Basé valeur** : coût de la douleur × % de valeur capturée
- **Estimation finale** : [fourchette réaliste]

### 3. Modèle de pricing recommandé
- **Structure** : [per-user | per-company | usage-based | tiered | freemium]
- **Justification** : pourquoi ce modèle pour cette verticale

### 4. Tiers de pricing
| Tier | Nom | Prix/mois | Pour qui | Features incluses | Features exclues |
|------|-----|-----------|---------|-------------------|-----------------|

### 5. Add-ons et options
| Add-on | Prix | Cible |
|--------|------|-------|

### 6. Unit Economics estimés
- ARPA cible : X€/mois
- CAC estimé : X€ (basé sur canaux prévus)
- LTV estimé : ARPA × durée de vie estimée
- LTV/CAC ratio : X
- Payback period : X mois

### 7. Notes de pricing
- Seuils psychologiques à éviter
- Stratégie de discounting (si applicable)
- Pricing onboarding / setup fee (si applicable)

## Règles
- Ne jamais fixer des prix sans data WTP calls
- Marquer [HYPOTHÈSE — à tester] si WTP basé sur estimation seule
- Proposer un test A/B si incertitude sur le bon tier
