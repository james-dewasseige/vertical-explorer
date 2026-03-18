---
name: icp-definition
description: Définition ICP détaillée basée sur les calls et le contexte de la verticale.
trigger: /blueprint icp [vertical]
phase: 03-build
depends-on: vertical-scan (minimum)
inputs:
  - context/verticals/{slug}.md
  - context/calls/ liés
  - context/company.md
outputs:
  - blueprints/{slug}/icp.md
---

# ICP Definition

## Règle d'or
Lire TOUT le contexte avant de produire. L'ICP doit refléter ce qu'on a entendu
en calls, pas ce qu'on suppose.

## Process
1. Lire context/verticals/{slug}.md (personas, douleurs, calls)
2. Lire tous les calls liés → extraire patterns récurrents
3. Lire context/company.md → contraintes, modules disponibles
4. Identifier les clients idéaux vs. les mauvais fits sur base des data
5. Produire le blueprint ICP complet
6. Écrire dans blueprints/{slug}/icp.md

## Structure du blueprint

### 1. Firmographics
- Taille entreprise (employés, CA)
- Géographie
- Sous-segment vertical (beachhead)
- Structure organisationnelle
- Maturité digitale actuelle

### 2. Persona Décideur
- Titre typique
- Responsabilités principales
- Objectifs professionnels (ce par quoi il est jugé)
- Frustrations quotidiennes (verbatims calls)
- Triggers d'achat (quand décide-t-il de chercher une solution ?)
- Peurs et objections prévisibles
- Canaux d'information (où apprend-il ?)
- Process de décision (solo, comité, budget délégué ?)

### 3. Persona Utilisateur principal
- Titre
- Frustrations quotidiennes (verbatims calls)
- Outils actuels + satisfaction
- Rêves (ce qu'ils voudraient que ça fasse)
- Tech-savviness (1-5)

### 4. Buying Signals
**Prêt à acheter (HOT)** :
- Signal 1
- Signal 2

**En exploration (WARM)** :
- Signal 1

**Red flags (COLD/PARK)** :
- Signal 1

### 5. Anti-personas (qui NE PAS cibler)
| Profil | Pourquoi éviter | Signal d'identification |
|--------|-----------------|------------------------|

### 6. Framework de qualification HOT / WARM / COLD / PARK
Critères spécifiques à cette verticale pour qualifier un prospect rapidement.

## Règles
- Chaque affirmation doit avoir une source (call-XX, web, intuition-fondateur)
- Les verbatims calls sont en italiques avec source
- Marquer [HYPOTHÈSE — non testée] si pas de données directes
