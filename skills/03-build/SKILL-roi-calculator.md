---
name: roi-calculator
description: Calculateur ROI par verticale — version .md et HTML interactif.
trigger: /blueprint roi-calculator [vertical]
phase: 03-build
depends-on: icp-definition, pricing-model
inputs:
  - context/verticals/{slug}.md
  - blueprints/{slug}/icp.md
  - blueprints/{slug}/pricing-model.md
  - context/calls/ liés (données temps perdu, coûts actuels)
outputs:
  - blueprints/{slug}/roi-calculator.md
  - blueprints/{slug}/roi-calculator.html (interactif)
---

# ROI Calculator

## Objectif
Permettre à un prospect de calculer lui-même son ROI avec Enobase.
Un bon calculateur ROI réduit l'objection prix et accélère la décision.

## Process
1. Lire calls → extraire données réelles (heures/semaine perdues, coûts actuels, erreurs)
2. Lire pricing-model.md → prix Enobase
3. Lire icp.md → profil type, taille entreprise
4. Définir les variables inputs et les formules de calcul
5. Produire la version .md (documentation)
6. Produire la version HTML interactive (sliders + calcul temps réel)

## Modèle de calcul

### Inputs prospect
- Nombre d'employés impliqués dans les opérations
- CA annuel (pour estimer l'impact d'une erreur)
- Heures/semaine passées sur tâches manuelles évitables
- Budget actuel outils (logiciels, Excel, papier)
- Nombre d'erreurs/mois liées aux process manuels (commandes, factures, stock...)
- Coût estimé d'une erreur (temps de correction, impact client)

### Formules de calcul
- **Temps gagné** : heures/semaine × 52 × coût horaire moyen
- **Erreurs évitées** : nb erreurs/mois × 12 × coût moyen par erreur
- **Delta coût outils** : budget actuel − prix Enobase
- **Gain total annuel** : Temps gagné + Erreurs évitées + Delta coût outils
- **Coût Enobase** : prix annuel (tier recommandé)
- **ROI %** : (Gain total − Coût Enobase) / Coût Enobase × 100
- **Payback period** : Coût Enobase / (Gain total / 12) mois
- **Économies nettes 12 mois** : Gain total − Coût Enobase
- **Temps récupéré/semaine** : heures/semaine × taux d'automatisation estimé

### Valeurs par défaut (basées sur calls)
[À remplir avec les données extraites des calls de la verticale]

## Version HTML (.html)
- Single-file HTML/CSS/JS
- Sliders pour tous les inputs
- Calcul en temps réel (JavaScript)
- Affichage des 4 métriques clés : ROI%, Économies 12 mois, Payback, Temps récupéré
- Bouton "Envoyez-moi mes résultats" (mailto ou formulaire)
- Design sobre, couleurs Enobase
- Mobile responsive
