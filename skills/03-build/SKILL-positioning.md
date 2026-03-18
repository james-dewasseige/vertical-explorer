---
name: positioning
description: Positioning statement, UVP/USP et messaging par persona pour une verticale.
trigger: /blueprint positioning [vertical]
phase: 03-build
depends-on: icp-definition
inputs:
  - context/verticals/{slug}.md
  - context/company.md
  - blueprints/{slug}/icp.md
outputs:
  - blueprints/{slug}/positioning.md
---

# Positioning

## Règle d'or
Le positioning part des douleurs réelles (entendues en calls), pas des features produit.
Lire les verbatims dans icp.md avant d'écrire une ligne.

## Process
1. Lire context/verticals/{slug}.md + blueprints/{slug}/icp.md
2. Lire context/company.md → différenciateurs
3. Identifier les 3 douleurs principales (classées par fréquence + intensité)
4. Identifier la principale alternative que les prospects utilisent actuellement
5. Construire le positioning statement
6. Développer UVP → messaging → battlecards
7. Écrire dans blueprints/{slug}/positioning.md

## Structure du blueprint

### 1. Positioning Statement
Template : "Pour [ICP], qui [problème principal], Enobase est [catégorie] qui [bénéfice clé].
Contrairement à [alternative principale], Enobase [différenciateur décisif]."

### 2. UVP (Unique Value Proposition)
- **Headline** : 10 mots max, formule du bénéfice
- **Sub-headline** : 30 mots max, développe le "comment"
- **Preuve** : 1 stat ou résultat concret

### 3. USPs (3-5 Unique Selling Points)
Pour chaque USP :
- Titre court
- Description (2-3 lignes)
- Proof point (call quote, stat, comparaison)
- En quoi c'est différenciant vs la concurrence

### 4. Messaging par persona

#### Décideur
- Pain point (dans SES mots, verbatim si possible)
- Message clé à retenir
- Ton recommandé
- CTA adapté

#### Utilisateur principal
- Pain point
- Message clé
- Ton
- CTA

### 5. Battlecards par concurrent (top 2-3)
| Concurrent | Notre avantage | Leur avantage | Trap question | Réponse à "mais X fait Y" | Never say |
|-----------|---------------|---------------|--------------|--------------------------|-----------|

### 6. Elevator pitches
- **10 secondes** : [texte]
- **30 secondes** : [texte]
- **2 minutes** : [texte structuré]

## Règles
- Pas de jargon tech dans les messages aux décideurs
- Chaque claim doit être supporté par au moins une source
- Éviter les superlatifs non prouvés ("le meilleur", "révolutionnaire")
