---
name: outbound-campaign
description: Séquences outbound multi-canal (email + LinkedIn) pour une verticale.
trigger: /blueprint outbound [vertical]
phase: 03-build
depends-on: icp-definition, positioning
inputs:
  - context/verticals/{slug}.md
  - blueprints/{slug}/icp.md
  - blueprints/{slug}/positioning.md
  - context/calls/ liés (objections entendues)
outputs:
  - blueprints/{slug}/outbound-campaign.md
---

# Outbound Campaign

## Règle d'or
Les emails partent des douleurs, pas du produit. Ne jamais pitcher d'abord.
Langue : anglais si cibles internationales, français sinon. Adapter.

## Process
1. Lire icp.md → douleur principale, persona décideur, triggers
2. Lire positioning.md → messages clés, USPs
3. Lire les calls → objections entendues, verbatims
4. Écrire 3 variantes de séquence (par douleur principale)
5. Écrire les messages LinkedIn
6. Définir les métriques cibles

## Séquence email — 5 touchpoints

### J0 — "Pain-first"
- Objet : question sur leur douleur principale (pas de pitch)
- Corps : 3-4 lignes max. Identifier douleur → question simple
- CTA : "Est-ce que ça vous parle ?"
- Ton : curieux, pas commercial

### J3 — "Insight"
- Objet : partager un insight utile sur leur industrie
- Corps : insight + lien avec leur situation → soft mention Enobase
- CTA : "Vous avez déjà cherché à résoudre ça ?"
- Ton : expert, généreux

### J7 — "Social proof"
- Objet : résultat concret (même si verticale adjacente)
- Corps : mini case study en 3 lignes → "Vous pourriez avoir les mêmes résultats"
- CTA : "15 minutes pour voir si ça peut marcher pour vous ?"
- Ton : factuel

### J12 — "Objection preempt"
- Objet : adresser l'objection #1 de cette verticale
- Corps : "Beaucoup de [NOM VERTICAL] nous disent [objection]. Voilà pourquoi..."
- CTA : lien Calendly direct
- Ton : empathique, direct

### J18 — "Breakup"
- Objet : message léger, dernière tentative
- Corps : "Je ne vais pas continuer à vous écrire — mais si jamais [trigger]..."
- CTA : "Une dernière question rapide"
- Ton : léger, humain

## Séquence LinkedIn — 3 touchpoints

### Connexion + note
- 1 phrase personnalisée basée sur leur profil
- Pas de pitch à cette étape

### Message insight (J+2 après acceptation)
- Insight sectoriel → "Vous avez le même problème chez [leur entreprise] ?"
- Max 5 lignes

### Follow-up engagement (J+7)
- Commenter un de leurs posts → envoyer message
- "Votre post sur [X] m'a fait penser à..."

## Variantes par persona
- Variante Décideur (axée ROI, budget, risque)
- Variante Utilisateur (axée gain de temps, frustrations quotidiennes)

## Métriques cibles
- Ouverture : >40%
- Réponse : >5%
- Booking : >2%
- Review après 50 envois — optimiser les sujets et J0 si sous les targets
