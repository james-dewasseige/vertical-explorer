---
name: objection-handling
description: Guide complet d'objections basé sur les calls réalisés.
trigger: /blueprint objections [vertical]
phase: 03-build
depends-on: au moins 2-3 calls réalisés
inputs:
  - context/verticals/{slug}.md
  - context/calls/ liés (objections enregistrées)
  - blueprints/{slug}/positioning.md
outputs:
  - blueprints/{slug}/objection-handling.md
---

# Objection Handling

## Prérequis
Minimum 2-3 calls réalisés pour avoir des données réelles.
Si < 2 calls, produire un guide hypothétique clairement marqué [HYPOTHÈSE].

## Process
1. Lire tous les calls liés → extraire toutes les objections notées
2. Regrouper par catégorie et fréquence
3. Pour chaque objection : analyser l'intent réel derrière les mots
4. Construire la réponse en 4 temps : Acknowledge → Reframe → Respond → CTA
5. Documenter ce qui a marché / pas marché en call
6. Produire le guide complet

## Catégories d'objections
- **Prix** : "C'est trop cher", "Mon budget est limité"
- **Timing** : "Pas le bon moment", "On a d'autres priorités"
- **Concurrence** : "On utilise déjà X", "X fait la même chose"
- **Complexité** : "C'est trop compliqué à déployer", "La migration..."
- **Confiance** : "Vous êtes une jeune boîte", "Pas de références"
- **Fonctionnel** : "Il manque la feature X"
- **Interne** : "Je dois en parler à [X]", "Ce n'est pas ma décision"

## Pour chaque objection (top 10)

### Template
**Objection** : [verbatim exact]
**Fréquence** : [nombre de calls, %]
**Intent réel** : [ce qu'ils veulent vraiment dire derrière les mots]

**Réponse framework (A-R-R-CTA)** :
- *Acknowledge* : "Je comprends..."
- *Reframe* : "La vraie question c'est..."
- *Respond* : [réponse concrète + preuve]
- *CTA* : [prochaine étape]

**Ce qui a marché** : [approches testées positives]
**Ce qui n'a PAS marché** : [à éviter]
**Preuve à montrer** : [document, stat, témoignage]

## Règles
- Ne jamais ignorer une objection ou changer de sujet
- Toujours valider la préoccupation avant de répondre
- Les preuves > les arguments
- Re-générer ce guide après 2+ nouveaux calls avec nouvelles objections
