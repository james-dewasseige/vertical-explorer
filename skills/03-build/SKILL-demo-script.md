---
name: demo-script
description: Script de démo adapté aux douleurs spécifiques d'une verticale.
trigger: /blueprint demo-script [vertical]
phase: 03-build
depends-on: icp-definition, positioning
inputs:
  - context/verticals/{slug}.md
  - blueprints/{slug}/icp.md
  - blueprints/{slug}/positioning.md
  - blueprints/{slug}/objection-handling.md (si disponible)
outputs:
  - blueprints/{slug}/demo-script.md
---

# Demo Script

## Objectif
Un script de démo qui maximise la conviction en 20-30 minutes, en partant des
douleurs spécifiques à la verticale — pas d'une visite générique du produit.

## Process
1. Lire icp.md → top 3 douleurs, persona décideur vs utilisateur
2. Lire positioning.md → messages clés, "aha moments"
3. Identifier les 3 moments de la démo qui créent le plus de valeur
4. Construire le script avec transitions fluides
5. Anticiper les questions et objections pendant la démo

## Structure (20-30 min)

### Ouverture (2 min)
- Contexte de la démo : "Pour que je vous montre ce qui vous sera le plus utile..."
- Confirmer les douleurs : "Vous m'aviez parlé de [X] et [Y] — c'est bien ça ?"
- Annoncer le plan : "Je vais vous montrer comment on règle [X], [Y], [Z]"

### Douleur #1 → Solution (5-7 min)
- Planter le décor : "Aujourd'hui, vous faites comment pour [X] ?"
- Montrer le problème dans le produit (si applicable)
- Montrer la solution Enobase
- "Aha moment" : moment où le prospect réalise la valeur
- Question de validation : "Est-ce que ça règle ce que vous me décriviez ?"

### Douleur #2 → Solution (5-7 min)
- Même structure

### Douleur #3 → Solution (5-7 min)
- Même structure

### Vue d'ensemble (3 min)
- Dashboard global → vision d'ensemble
- Valeur AI-native : "Ce que les autres ne font pas..."
- ROI rapide : "En moyenne, nos clients récupèrent [X] heures/semaine"

### Q&A (5 min)
- Questions anticipées avec réponses
- Utiliser objection-handling.md si disponible

### Close (2 min)
- Résumé : "Ce qu'on a vu aujourd'hui..."
- Demander : "Qu'est-ce qui vous a le plus parlé ?"
- Next step clair : "La prochaine étape, c'est [X]. Est-ce que ça vous convient ?"
- Timeline : "On pourrait démarrer dans [X semaines]"

## Tips démo
- Toujours partir d'un scénario réaliste pour la verticale
- Ne jamais montrer plus de features que nécessaire
- Laisser le prospect interagir si possible
- Préparer les données de démo spécifiques à la verticale
- Enregistrer la démo si accord prospect (Fireflies)

## Questions anticipées pendant la démo
[Extraites des calls précédents pour cette verticale]
