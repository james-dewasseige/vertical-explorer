---
name: interview-guide
description: Guide d'entretien adaptatif basé sur le stade d'exploration et le contexte existant.
trigger: /interview [vertical]
phase: 01-explore
inputs:
  - context/verticals/{slug}.md
  - context/calls/ (liés à cette verticale)
  - context/scorecard-criteria.md
  - context/learnings.md (questions qui marchent)
outputs:
  - Guide d'entretien adapté au stade
---

# Interview Guide Generator

## IMPORTANT : le guide est ADAPTATIF
- 1er call → discovery large (situation, outils, douleurs)
- 3ème call → questions ciblées sur les gaps de scoring
- 5ème call → validation/invalidation d'hypothèses spécifiques

## Process
1. Lire context/verticals/{slug}.md → stade actuel, hypothèses, scoring
2. Compter le nombre de calls déjà réalisés pour cette verticale
3. Identifier les critères en Low confidence → questions prioritaires
4. Lire context/learnings.md → questions qui marchent le mieux
5. Adapter le guide au stade (1er call / call de suivi / validation)
6. Inclure les hypothèses non testées → 1-2 questions par hypothèse
7. Produire un guide structuré et priorisé

## Blocs de questions

### Bloc 1 : Situation actuelle (5 min)
Questions sur l'entreprise, taille, opérations. SKIP si déjà connu.
- "Décrivez-moi votre activité en quelques mots — taille, équipe, clients typiques ?"
- "À quoi ressemble votre journée type opérationnellement ?"
- "Quelle est votre saisonnalité ?"

### Bloc 2 : Outils & process (10 min)
Outils utilisés, frustrations, temps passé sur tâches manuelles, alternatives cherchées.
ADAPTER si on sait déjà quels outils → aller direct aux frustrations.
- "Quels outils utilisez-vous pour gérer [X, Y, Z] ?"
- "Qu'est-ce qui vous prend le plus de temps administrativement ?"
- "Avez-vous déjà cherché une meilleure solution ? Qu'avez-vous trouvé ?"

### Bloc 3 : Douleurs & impact (10 min)
Plus gros problème opérationnel, coût (temps, argent, erreurs), baguette magique.
ADAPTER en creusant les douleurs déjà identifiées dans les calls précédents.
- "Quel est votre plus gros problème opérationnel en ce moment ?"
- "Si vous aviez une baguette magique, qu'est-ce que vous changeriez demain ?"
- "Combien d'heures par semaine votre équipe passe sur des tâches manuelles évitables ?"
- "Quelle est la conséquence de [douleur X] sur votre business ?"

### Bloc 4 : Budget & décision (5 min)
Budget actuel, WTP, décideur, timeline. RÉSERVER pour le 2ème call+.
- "Combien payez-vous actuellement pour vos outils ?"
- "Qui prend les décisions d'achat logiciel dans votre entreprise ?"
- "Si vous trouviez la solution idéale, quel budget seriez-vous prêt à allouer ?"
- "Quel serait votre timeline pour changer d'outil ?"

### Bloc 5 : Validation produit (5 min)
Intérêt pour [X,Y,Z], must-have vs nice-to-have. RÉSERVER pour le 3ème call+.
- "Si un outil faisait [X], est-ce que ça réglerait votre problème ?"
- "Parmi [feature A, B, C], laquelle est absolument indispensable ?"

### Questions hypothèses
Pour chaque hypothèse non testée dans verticals/{slug}.md, 1-2 questions concrètes.

## Notes caller
- Écouter 70%, parler 30%
- Ne pas vendre, explorer
- Noter les verbatims (guillemets exacts)
- Identifier maturité d'achat (1-5) à la fin
- Ne jamais poser une question de budget avant le bloc 4

## Output format
Guide structuré avec :
- Durée totale estimée
- Blocs à inclure (selon stade)
- Questions prioritaires (max 3 par bloc)
- Questions hypothèses spécifiques à cette verticale
- Tips de facilitation adaptés
