# VEE — Vertical Exploration Engine
## Spec complète pour implémentation Claude Code

**Version** : 1.0  
**Date** : 2026-03-18  
**Auteur** : James (GTM Advisor & FDE, Enobase)  
**Destinataire** : Claude Code — ce document est la spec de référence pour construire le système complet.

---

# TABLE DES MATIÈRES

1. [Vue d'ensemble](#1-vue-densemble)
2. [Architecture du repo](#2-architecture-du-repo)
3. [CLAUDE.md — Instructions racine](#3-claudemd--instructions-racine)
4. [Contexte — Tous les fichiers](#4-contexte--tous-les-fichiers)
5. [Skills Phase 1 : EXPLORE (Understand)](#5-skills-phase-1--explore-understand)
6. [Skills Phase 2 : EVALUATE (Score & Decide)](#6-skills-phase-2--evaluate-score--decide)
7. [Skills Phase 3 : BUILD (Blueprints)](#7-skills-phase-3--build-blueprints)
8. [Skills Phase 4 : EXECUTE (Launch & Test)](#8-skills-phase-4--execute-launch--test)
9. [Skills Phase 5 : SYSTEM (Scale & Learn)](#9-skills-phase-5--system-scale--learn)
10. [Automations](#10-automations)
11. [Blueprints — Templates et outputs](#11-blueprints--templates-et-outputs)
12. [Critères de scoring — Détail complet](#12-critères-de-scoring--détail-complet)
13. [Connecteurs MCP](#13-connecteurs-mcp)
14. [Workflows opérationnels](#14-workflows-opérationnels)
15. [Carte des verticales Odoo](#15-carte-des-verticales-odoo)
16. [Plan de build](#16-plan-de-build)

---

# 1. VUE D'ENSEMBLE

## Problème

Enobase est un ERP AI-native modulaire en early-stage. L'équipe doit identifier quelles verticales d'industrie cibler parmi ~90+ verticales identifiées (basées sur le catalogue Odoo). Chaque exploration de verticale nécessite des calls discovery, de la recherche marché, des tests de positionnement — et aujourd'hui, ce contexte se perd entre les conversations. Les calls sont enregistrés (Fireflies) mais pas exploités systématiquement. Il n'y a pas de framework pour comparer les verticales entre elles, ni pour capitaliser sur les apprentissages d'une verticale à l'autre.

## Solution

Un système Claude Code (repo structuré + skills + automations) qui :

1. **Pilote l'exploration** : fournit les tasks à faire, les questions à poser en call, les prochaines étapes
2. **Capitalise sur chaque touchpoint** : chaque call enregistré (Fireflies) enrichit automatiquement le contexte de la verticale
3. **Intègre la vision fondateur** : les intuitions de l'équipe fondatrice sont capturées, catégorisées et propagées aux verticales concernées
4. **Score les verticales** : scorecard pondérée à 9 critères avec niveau de confidence par critère
5. **Score les segments** : au sein d'une verticale, identifie et score les sous-segments les plus prometteurs
6. **Produit des blueprints** : livrables actionnables par verticale (landing page, ICP, pricing, outbound, sales deck, etc.)
7. **Double format** : chaque blueprint existe en .md (itération rapide) et peut être généré en version déployable (HTML, PPTX, etc.)
8. **S'améliore** : les critères de scoring évoluent, les skills se mettent à jour en fonction des learnings, le système apprend ce qui marche

## Principes de design

- **Skills chaînées** : l'output d'une skill est l'input de la suivante. L'ICP informe le positioning. Le positioning informe l'outbound. Le feedback d'outbound met à jour le scoring.
- **Contexte persistant** : tout est dans des fichiers markdown dans le repo, c'est la source de vérité unique
- **Hypothèses explicites** : chaque affirmation est traçable (source: call-XX, web-research, intuition-fondateur) avec un statut (non-testée / validée / invalidée)
- **Dual format** : chaque blueprint a une version .md pour itérer et valider, puis peut être généré en version déployable
- **Vitesse > perfection** : le système privilégie la validation rapide des verticales. On veut scorer le maximum de verticales le plus vite possible
- **Vision forte** : les insights fondateurs ont un poids élevé mais sont challengeables par les données terrain
- **Cercle vertueux** : explorer → scorer → builder → tester → apprendre → explorer mieux

## Inspirations intégrées

### phuryn/pm-skills
100+ skills Claude Code organisées en plugins, avec commandes chaînées. Pattern retenu : skills = domain knowledge (fichiers SKILL.md), commands = workflows qui enchaînent les skills. Chaque skill a un format standardisé. Les skills sont regroupées en plugins thématiques.

### Othmane-Khadri/gtm-engineer-playbook
10 skills GTM pour Claude Code : ICP Architect → Signal Scanner → Account Research → Qualification Scorer → Outreach Sequence → Meeting Prep → Battlecards → CRM Hygiene → Weekly Report → Agent Architecture. Pattern retenu : tout écrit dans un dossier docs/, chaque skill lit les outputs des précédentes. Les skills chaînent automatiquement.

### Maja Voje (12 GTM Skills for Claude Code)
Framework en 4 phases avec 100 tasks across 12 skills :
- Phase 1-3 UNDERSTAND : GTM Foundations, Collecting Intelligence, Validating Customers
- Phase 4-6 BUILD : Building Product, Setting Pricing, Crafting Positioning
- Phase 7-9 LAUNCH : Preparing Launch Assets, Building Communication Engine, Executing Launch
- Phase 10-12 SCALE : Building GTM System, Running Marketing, Executing Sales

Pattern retenu : chaque phase build sur la précédente. Rien n'existe en isolation. Les outputs sont sauvegardés pour que les skills ultérieures les référencent automatiquement.

---

# 2. ARCHITECTURE DU REPO

```
vee/
├── CLAUDE.md                              # Instructions globales Claude Code (lu automatiquement)
│
├── context/                               # TOUT CE QUE LE SYSTÈME SAIT
│   ├── company.md                         # Identité Enobase (vision, produit, équipe, contraintes)
│   ├── scorecard-criteria.md              # Critères de scoring (évolutifs, pondérés, versionnés)
│   ├── odoo-verticals-map.md              # Liste maîtresse ~90 verticales Odoo + statut exploration
│   ├── insights.md                        # Intuitions fondateurs (datées, catégorisées, liées aux verticales)
│   ├── learnings.md                       # Apprentissages cross-verticales (patterns, anti-patterns)
│   │
│   ├── verticals/                         # UN FICHIER PAR VERTICALE EXPLORÉE
│   │   ├── _template.md                   # Template vierge
│   │   ├── event-catering.md              # Verticale en cours (exemple)
│   │   └── ...
│   │
│   └── calls/                             # TRANSCRIPTS INGÉRÉS ET STRUCTURÉS
│       ├── _template.md                   # Template de call
│       └── ...                            # YYYY-MM-DD_nom-prospect.md
│
├── skills/                                # COMMENT LE SYSTÈME TRAVAILLE
│   ├── 01-explore/                        # Phase UNDERSTAND
│   │   ├── SKILL-vertical-scan.md         # Recherche initiale sur une verticale
│   │   ├── SKILL-odoo-market-map.md       # Scanner les verticales Odoo comme source
│   │   ├── SKILL-interview-guide.md       # Générer les questions de call (adaptatif)
│   │   └── SKILL-competitor-intel.md      # Intelligence concurrentielle sur un vertical
│   │
│   ├── 02-evaluate/                       # Phase SCORE & DECIDE
│   │   ├── SKILL-vertical-scorecard.md    # Scorer une verticale complète
│   │   ├── SKILL-segment-scorer.md        # Scorer les segments au sein d'une verticale
│   │   └── SKILL-criteria-evolution.md    # Faire évoluer les critères de scoring
│   │
│   ├── 03-build/                          # Phase BUILD BLUEPRINTS
│   │   ├── SKILL-icp-definition.md        # Définition ICP détaillée
│   │   ├── SKILL-positioning.md           # Positioning statement, UVP/USP, messaging
│   │   ├── SKILL-pricing-model.md         # Modèle de pricing par verticale
│   │   ├── SKILL-landing-page.md          # Landing page copy + HTML déployable
│   │   ├── SKILL-sales-deck.md            # Pitch deck structuré
│   │   ├── SKILL-outbound-campaign.md     # Séquences outbound multi-canal
│   │   ├── SKILL-objection-handling.md    # Guide d'objections basé sur les calls
│   │   ├── SKILL-demo-script.md           # Script de démo adapté au vertical
│   │   ├── SKILL-roi-calculator.md        # Calculateur ROI par vertical
│   │   └── SKILL-hypotheses.md            # Carte d'hypothèses traçables
│   │
│   ├── 04-execute/                        # Phase LAUNCH & TEST
│   │   ├── SKILL-outbound-targets.md      # Critères de ciblage + listes
│   │   ├── SKILL-ab-test-plan.md          # Plan de test (message, canal, segment)
│   │   ├── SKILL-launch-checklist.md      # Checklist opérationnelle de lancement
│   │   └── SKILL-feedback-loop.md         # Capturer et intégrer les résultats
│   │
│   └── 05-system/                         # Phase SCALE & LEARN
│       ├── SKILL-retrospective.md         # Analyse d'une verticale (what worked / didn't)
│       ├── SKILL-playbook-update.md       # Mettre à jour les skills avec les learnings
│       └── SKILL-vertical-graduation.md   # Critères pour graduer un vertical
│
├── automations/                           # COMMENT LE CONTEXTE S'ENRICHIT
│   ├── SKILL-call-ingestion.md            # Ingérer un transcript Fireflies
│   ├── SKILL-context-enrichment.md        # Propager un nouveau data point
│   └── SKILL-weekly-sprint.md             # Sprint planning hebdomadaire
│
├── blueprints/                            # CE QUE LE SYSTÈME PRODUIT
│   ├── _templates/                        # Templates réutilisables
│   │   ├── landing-page-template.html
│   │   └── roi-calculator-template.html
│   │
│   └── {vertical-slug}/                   # Un dossier par verticale active
│       ├── icp.md
│       ├── positioning.md
│       ├── pricing-model.md
│       ├── landing-page.md
│       ├── landing-page.html              # Version déployable
│       ├── sales-deck.md
│       ├── outbound-campaign.md
│       ├── objection-handling.md
│       ├── demo-script.md
│       ├── roi-calculator.md
│       ├── roi-calculator.html            # Version déployable
│       ├── hypotheses.md
│       ├── outbound-targets.md
│       └── ab-test-plan.md
│
└── docs/
    ├── getting-started.md                 # Comment démarrer avec le système
    └── workflow-reference.md              # Référence rapide des commandes
```

---

# 3. CLAUDE.md — INSTRUCTIONS RACINE

Ce fichier est lu automatiquement par Claude Code à chaque session. C'est le cerveau opérationnel du système.

```markdown
# VEE — Vertical Exploration Engine

## Identité
Tu es un GTM engineer spécialisé dans l'exploration systématique de verticales
d'industrie pour Enobase, un ERP AI-native modulaire. Ton objectif : identifier,
scorer et pénétrer les verticales les plus prometteuses, le plus vite possible,
de façon rentable.

## Principes fondamentaux
1. CAPITALISER : Chaque interaction avec le marché (call, email, feedback) enrichit
   le contexte. Rien ne se perd.
2. TRACER : Les hypothèses sont explicites. Chaque data point a une source
   (call-XX, web, intuition-fondateur). Chaque score a un niveau de confidence.
3. CHAÎNER : Les blueprints se nourrissent du contexte. L'ICP informe le positioning.
   Le positioning informe l'outbound. Le feedback outbound met à jour le scoring.
4. ACCÉLÉRER : La vitesse de validation prime sur la perfection des outputs.
5. VISION : Les insights fondateurs (dans context/insights.md) ont un poids fort
   mais sont challengeables par les données terrain.
6. ENRICHIR : Le système s'améliore continuellement. Les critères de scoring évoluent.
   Les skills intègrent les learnings.

## Structure du système
- context/         → Tout ce que tu sais (entreprise, verticales, calls, insights)
- skills/          → Comment tu travailles (processus, frameworks, par phase)
- blueprints/      → Ce que tu produis (livrables actionnables par verticale)
- automations/     → Comment le contexte s'enrichit automatiquement

## Commandes slash

### Exploration
/explore [vertical]          → Lance l'exploration d'une nouvelle verticale (Phase 1)
/scan                        → Scanner les verticales Odoo et identifier les candidates
/interview [vertical]        → Générer un guide d'entretien adapté au stade d'exploration
/intel [vertical]            → Intelligence concurrentielle sur une verticale

### Évaluation
/score [vertical]            → Scorer ou re-scorer une verticale (Phase 2)
/segment [vertical]          → Scorer les segments au sein d'une verticale
/criteria                    → Revue et évolution des critères de scoring
/compare [v1] [v2] ...      → Comparer plusieurs verticales côte à côte

### Build
/blueprint [type] [vertical] → Générer un blueprint spécifique (Phase 3)
  Types : icp, positioning, pricing, landing-page, sales-deck, outbound,
          objections, demo-script, roi-calculator, hypotheses
/blueprint all [vertical]    → Générer tous les blueprints pour une verticale

### Exécution
/targets [vertical]          → Générer une liste de cibles outbound (Phase 4)
/test [vertical]             → Plan de test A/B pour une verticale
/launch [vertical]           → Checklist de lancement
/feedback [vertical]         → Capturer les résultats d'un test

### Système
/ingest [call-id]            → Ingérer un transcript de call Fireflies (Automation)
/insight [texte]             → Ajouter un insight fondateur
/sprint                      → Générer le plan d'action hebdomadaire
/status                      → Vue d'ensemble de toutes les verticales + métriques
/retro [vertical]            → Retrospective sur une verticale (Phase 5)
/graduate [vertical]         → Évaluer si un vertical est prêt à graduer
/learn                       → Extraire les learnings cross-verticales

## Workflow de chaînage
Les skills lisent TOUJOURS le contexte avant de produire. Ordre de lecture :
1. context/company.md (identité, constraints)
2. context/verticals/{slug}.md (tout ce qu'on sait sur cette verticale)
3. context/calls/ (calls liés à cette verticale)
4. context/insights.md (intuitions fondateurs pertinentes)
5. blueprints/{slug}/ (blueprints déjà produits pour cette verticale)

## Conventions
- Langue par défaut : français (sauf blueprints outbound qui peuvent être en anglais)
- Scores : 1-5 avec confidence (Low/Med/High) et source
- Statuts verticale : Not Explored → Discovery → Scoring → Validation → Active → Parked → Rejected
- Chaque blueprint : version .md (itération) + optionnellement version déployable
- Les fichiers de contexte sont la source de vérité unique
- Les dates au format YYYY-MM-DD partout
- Les slugs de verticales en kebab-case (ex: event-catering)

## Connecteurs MCP
- Fireflies → transcripts de calls (connecté)
- Gmail → outbound, suivi réponses (connecté)
- Google Calendar → planification calls (connecté)
- Attio → CRM, pipeline par verticale (à connecter)
- GitHub → versionner le repo VEE (à connecter)

## Règle d'or
Avant de produire un output, TOUJOURS lire le contexte pertinent.
Avant de poser des questions en call, TOUJOURS lire les calls précédents.
Ne jamais recommencer à zéro — capitaliser sur ce qui existe.
Quand un nouveau data point arrive, se demander : "Quels fichiers de contexte
et quels scores cela impacte-t-il ?"
```

---

# 4. CONTEXTE — TOUS LES FICHIERS

## 4.1 context/company.md

```markdown
# Enobase — Contexte Entreprise

## Vision
ERP AI-native modulaire. L'IA n'est pas une feature ajoutée — elle est au cœur
de l'architecture. Objectif : remplacer les workflows manuels et les spreadsheets
par un système intelligent qui apprend des opérations de l'entreprise.

## Produit
- URL : enobase.com
- Stack : Next.js, Supabase (+ détails à compléter)
- Architecture : modular table definitions (tableDef.basic(), tableDef.standard())
- Modules existants : [À COMPLÉTER — lister les modules disponibles]
- Modules en dev : [À COMPLÉTER]
- Stade : Early-stage
- Différenciateurs vs concurrence (Odoo, etc.) :
  - AI-native (pas un bolt-on IA sur un ERP legacy)
  - UX moderne (pas d'interface années 2000)
  - Modularité réelle (on active ce qu'on utilise, pas de bloat)
  - Vitesse de déploiement (jours, pas mois)
  - [À COMPLÉTER — autres différenciateurs]

## Équipe fondatrice
- Taille : [À COMPLÉTER]
- Profils : [À COMPLÉTER — technique, business, domaine]
- Capacité d'exécution GTM disponible : [À COMPLÉTER — heures/semaine dédiées GTM]

## Contraintes
- Budget marketing mensuel : [À COMPLÉTER]
- Runway : [À COMPLÉTER]
- Objectifs à 90 jours : [À COMPLÉTER]
- Ressources techniques pour customisation par vertical : [À COMPLÉTER]
- Marchés géographiques prioritaires : [France ? Europe ? Francophone ?]

## Verticale actuelle
- Event catering (en cours d'exploration)

## Positionnement actuel
[À COMPLÉTER — comment Enobase se positionne aujourd'hui de façon générale]

## Métriques système à tracker
- Nombre de verticales explorées vs. total Odoo
- Nombre de verticales scorées avec confidence Med+
- Nombre de calls par verticale
- Time-to-score : jours entre 1er contact et scorecard fiable (confidence Med+)
- Conversion exploration → verticale active
- Nombre de blueprints produits
- Résultats des tests outbound (taux réponse, meetings bookés)
```

## 4.2 context/scorecard-criteria.md

```markdown
# Critères de Scoring des Verticales

## Version : 1.0
## Dernière mise à jour : 2026-03-18
## Prochaine revue prévue : après 5 verticales scorées

---

## Critères actifs

### 1. Complexité opérationnelle (poids: 1.2x)
Plus les opérations sont complexes, plus un ERP AI a de la valeur.
- 1 = Opérations simples, peu de flux, une seule activité linéaire
- 2 = Quelques flux parallèles, gestion basique
- 3 = Multi-flux, quelques intégrations nécessaires, saisonnalité
- 4 = Complexe, multi-parties prenantes, workflows imbriqués
- 5 = Très complexe, chaîne logistique, multi-sites, réglementations

### 2. Taille de marché adressable (poids: 1.0x)
Nombre d'entreprises cibles × revenu potentiel par client (ARPA).
- 1 = <1 000 entreprises cibles, ARPA <100€/mois
- 2 = 1 000-5 000 entreprises, ARPA 100-300€/mois
- 3 = 5 000-20 000 entreprises, ARPA 300-500€/mois
- 4 = 20 000-50 000 entreprises, ARPA 500-1000€/mois
- 5 = >50 000 entreprises, ARPA >1000€/mois

### 3. Insatisfaction outils existants (poids: 1.3x)
Le critère le plus pondéré. Spectre de satisfaction :
- 1 = Bien équipés ET satisfaits (marché saturé, loyal aux solutions en place)
- 2 = Bien équipés mais quelques lacunes mineures (pas de switching intent)
- 3 = Équipés mais outils inadaptés (Excel, bricolage multi-outils, workarounds)
- 4 = Sous-équipés, douleur consciente, recherchent activement une solution
- 5 = Aucun outil adapté, douleur forte, prêts à changer immédiatement

### 4. Capacité à payer (poids: 1.1x)
Montant ET rapidité de décision d'achat.
- 1 = Budget très limité (<100€/mois possible), cycles longs (>6 mois)
- 2 = Budget limité, cycle 3-6 mois
- 3 = Budget moyen (200-500€/mois), cycle 2-3 mois
- 4 = Budget correct (500-1000€/mois), cycle 1-2 mois
- 5 = Budget significatif (>1000€/mois), décision <1 mois

### 5. Facilité de distribution / accès (poids: 1.0x)
Comment atteindre les décideurs de cette verticale.
- 1 = Marché très fragmenté, pas de canaux identifiés, décideurs inaccessibles
- 2 = Quelques canaux existent mais accès difficile
- 3 = Canaux identifiés (salons, associations, médias spécialisés), accès moyen
- 4 = Communautés actives, quelques influenceurs, LinkedIn actif
- 5 = Communautés très actives, influenceurs identifiés, accès direct aux décideurs

### 6. Fit produit Enobase (poids: 1.2x)
Combien de modules existants sont directement applicables sans dev majeur.
- 1 = Nécessite 80%+ de développement custom
- 2 = 60-80% de dev custom nécessaire
- 3 = 50% réutilisable, 50% à adapter
- 4 = 70-80% des modules existants applicables
- 5 = 80%+ directement utilisable, adaptations mineures seulement

### 7. Densité de données / potentiel IA (poids: 0.8x)
Volume et richesse des données opérationnelles exploitables par l'IA.
- 1 = Peu de données structurées, opérations principalement manuelles
- 2 = Quelques données mais peu exploitables
- 3 = Données transactionnelles classiques (commandes, factures, stock)
- 4 = Données riches et variées, potentiel de prédiction
- 5 = Données très riches, forte automatisation possible, insights IA à forte valeur

### 8. Viralité / effet réseau (poids: 0.7x)
Le produit crée-t-il une adoption organique via l'écosystème ?
- 1 = Usage totalement isolé, pas d'effet réseau
- 2 = Interactions limitées avec l'extérieur
- 3 = Quelques interactions (fournisseurs, partenaires) pourraient tirer l'adoption
- 4 = Effet réseau modéré (clients/fournisseurs invités dans le système)
- 5 = Fort effet réseau (clients, fournisseurs, partenaires utilisent le même outil)

### 9. Vitesse de validation (poids: 1.1x)
Peut-on tester cette verticale en <4 semaines avec les moyens actuels ?
- 1 = Impossible sans développement produit significatif (>2 mois de dev)
- 2 = Nécessite du dev notable (1-2 mois)
- 3 = Faisable avec adaptations mineures (2-4 semaines de dev)
- 4 = Testable rapidement avec configuration seulement
- 5 = Testable immédiatement avec le produit actuel tel quel

---

## Formule de scoring
Score pondéré = Σ (score_i × poids_i) / Σ poids_i
Somme des poids = 1.2 + 1.0 + 1.3 + 1.1 + 1.0 + 1.2 + 0.8 + 0.7 + 1.1 = 9.4
Score max théorique = 5.0

## Seuils de décision
- ≥ 4.0 avec confidence Med+ sur 7+ critères → GO : lancer les blueprints immédiatement
- 3.5 - 3.9 → PROMISING : accélérer la collecte de data, planifier 2-3 calls ciblés
- 3.0 - 3.4 → INVESTIGATE : besoin de plus de data sur critères Low confidence
- 2.5 - 2.9 → PARK : mettre de côté, revisiter dans 3 mois
- < 2.5 → REJECT : ne pas poursuivre (documenter pourquoi)

## Indicateur de fiabilité du score
Confidence globale = nombre de critères à Med ou High / 9
- ≥ 7/9 → Score fiable, décision possible
- 4-6/9 → Score indicatif, besoin de data additionnelle
- ≤ 3/9 → Score non fiable, exploration insuffisante

## Historique des évolutions
| Date | Version | Changement | Raison |
|------|---------|-----------|--------|
| 2026-03-18 | 1.0 | Création initiale | 9 critères + pondérations hypothétiques |

## Notes
- Les poids sont des hypothèses initiales. Après 5+ verticales scorées,
  lancer /criteria pour recalibrer en fonction des résultats réels.
- Le critère "Insatisfaction outils" est le plus pondéré (1.3x) car c'est
  historiquement le meilleur prédicteur de conversion rapide.
- La confidence par critère est aussi importante que le score. Un score de
  4.2 avec 6 critères en "Low" = on n'a pas assez de data pour décider.
```

## 4.3 context/odoo-verticals-map.md

```markdown
# Carte des Verticales Odoo

Source : odoo.com/all-industries (scraped 2026-03-18)
Statuts : Not Explored | Discovery | Scoring | Validation | Active | Parked | Rejected

## Business Services (11)
| Verticale | Slug | Statut | Score | Notes |
|-----------|------|--------|-------|-------|
| Accounting Firm | accounting-firm | Not Explored | — | |
| Billboard Rental | billboard-rental | Not Explored | — | |
| Audit & Certification | audit-certification | Not Explored | — | |
| Fossil Fuel Trading | fossil-fuel-trading | Not Explored | — | |
| Environmental Agency | environmental-agency | Not Explored | — | |
| Talent Acquisition | talent-acquisition | Not Explored | — | |
| Law Firm | law-firm | Not Explored | — | |
| IT Hardware & Support | it-hardware-support | Not Explored | — | |
| Marketing Agency | marketing-agency | Not Explored | — | |
| Odoo Partner | odoo-partner | Not Explored | — | |
| Software Reseller | software-reseller | Not Explored | — | |

## Culture & Arts (8)
| Verticale | Slug | Statut | Score | Notes |
|-----------|------|--------|-------|-------|
| Arts & Crafts Store | arts-and-crafts | Not Explored | — | |
| Concert Halls | concert-halls | Not Explored | — | |
| Gallery | gallery | Not Explored | — | |
| Library | library | Not Explored | — | |
| Museum | museum | Not Explored | — | |
| Photography | photography | Not Explored | — | |
| Tattoo Shop | tattoo-shop | Not Explored | — | |
| Theater | theater | Not Explored | — | |

## Education & Training (4)
| Verticale | Slug | Statut | Score | Notes |
|-----------|------|--------|-------|-------|
| DIY Workshops | diy-workshops | Not Explored | — | |
| Driving School | driving-school | Not Explored | — | |
| eLearning Platform | elearning-platform | Not Explored | — | |
| Student Organization | student-organization | Not Explored | — | |

## Events, Community & Nonprofits (9)
| Verticale | Slug | Statut | Score | Notes |
|-----------|------|--------|-------|-------|
| Coworking | coworking | Not Explored | — | |
| Event Management | event-management | Not Explored | — | |
| Members Club | members-club | Not Explored | — | |
| Nonprofit Organisation | nonprofit-organisation | Not Explored | — | |
| Sport Events | sport-events | Not Explored | — | |
| Sports Club | sports-club | Not Explored | — | |
| Summer Camps | summer-camps | Not Explored | — | |
| Team Sports Club | team-sports-club | Not Explored | — | |
| Wedding Planner | wedding-planner | Not Explored | — | |

## Food & Beverage (10)
| Verticale | Slug | Statut | Score | Notes |
|-----------|------|--------|-------|-------|
| Bakery | bakery | Not Explored | — | |
| Bar & Pub | bar-pub | Not Explored | — | |
| Beverages Distributor | beverages-distributor | Not Explored | — | |
| Cake Store | cake-store | Not Explored | — | |
| Candy Shop | candy-shop | Not Explored | — | |
| Fast Food | fast-food | Not Explored | — | |
| Food Trucks | food-trucks | Not Explored | — | |
| Fine Dining Restaurant | fine-dining-restaurant | Not Explored | — | |
| Takeaway Restaurant | takeaway-restaurant | Not Explored | — | |
| Catering | catering | Not Explored | — | Adjacent event-catering (verticale en cours) |

## Health, Wellness & Personal Care (8)
| Verticale | Slug | Statut | Score | Notes |
|-----------|------|--------|-------|-------|
| Climbing Gym | climbing-gym | Not Explored | — | |
| Eyewear Store | eyewear-store | Not Explored | — | |
| Fitness Center | fitness-center | Not Explored | — | |
| Hair Salon | hair-salon | Not Explored | — | |
| Personal Trainer | personal-trainer | Not Explored | — | |
| Pharmacy | pharmacy | Not Explored | — | |
| Veterinary Clinic | veterinary-clinic | Not Explored | — | |
| Wellness Practitioner | wellness-practitioner | Not Explored | — | |

## Hospitality, Tourism & Leisure (11)
| Verticale | Slug | Statut | Score | Notes |
|-----------|------|--------|-------|-------|
| Bowling Alleys | bowling-alleys | Not Explored | — | |
| Campsite | campsite | Not Explored | — | |
| Escape Rooms | escape-rooms | Not Explored | — | |
| Guest House | guest-house | Not Explored | — | |
| Guided Tours | guided-tours | Not Explored | — | |
| Holiday House | holiday-house | Not Explored | — | |
| Hotel | hotel | Not Explored | — | |
| Night Clubs | night-clubs | Not Explored | — | |
| Outdoor Activities | outdoor-activities | Not Explored | — | |
| Spa Resort | spa-resort | Not Explored | — | |
| Yoga & Pilates Studio | yoga-pilates | Not Explored | — | |

## Manufacturing & Supply Chain (8)
| Verticale | Slug | Statut | Score | Notes |
|-----------|------|--------|-------|-------|
| Third-party Logistics | third-party-logistics | Not Explored | — | |
| Carpenter | carpenter | Not Explored | — | |
| Corporate Gifts | corporate-gifts | Not Explored | — | |
| Custom Furniture Production | custom-furniture-production | Not Explored | — | |
| Food Distribution | food-distribution | Not Explored | — | |
| Metal Fabricator | metal-fabricator | Not Explored | — | |
| Microbrewery | microbrewery | Not Explored | — | |
| Textile Manufacturing | textile-manufacturing | Not Explored | — | |

## Real Estate, Construction & Maintenance (7)
| Verticale | Slug | Statut | Score | Notes |
|-----------|------|--------|-------|-------|
| Architecture Firm | architecture-firm | Not Explored | — | |
| Property Owner Association | property-owner-association | Not Explored | — | |
| Construction | construction | Not Explored | — | |
| HVAC Services | hvac-services | Not Explored | — | |
| Real Estate (Management) | estate-management | Not Explored | — | |
| Real Estate Agency | real-estate-agency | Not Explored | — | |
| Solar Energy Systems | solar-energy | Not Explored | — | |

## Retail & eCommerce (13)
| Verticale | Slug | Statut | Score | Notes |
|-----------|------|--------|-------|-------|
| Agricultural Store | agriculture-store | Not Explored | — | |
| Automobile Spare Parts | automobile-spare-parts | Not Explored | — | |
| Bookstore | bookstore | Not Explored | — | |
| Clothing Store | clothing-store | Not Explored | — | |
| Cosmetics Store | cosmetics-store | Not Explored | — | |
| Dropshipping | dropshipping | Not Explored | — | |
| Electronic Store | electronic-store | Not Explored | — | |
| Florist | florist | Not Explored | — | |
| Grocery Store | grocery-store | Not Explored | — | |
| Furniture Store | furniture-store | Not Explored | — | |
| Hardware Store | hardware-store | Not Explored | — | |
| Toy Store | toy-store | Not Explored | — | |
| Wine Shop | wine-shop | Not Explored | — | |

## Trades & Home Services (8)
| Verticale | Slug | Statut | Score | Notes |
|-----------|------|--------|-------|-------|
| Bike Leasing | bike-leasing | Not Explored | — | |
| Bike Shop | bike-shop | Not Explored | — | |
| Cleaning Services | cleaning-services | Not Explored | — | |
| Electrician | electrician | Not Explored | — | |
| Gardening | gardening | Not Explored | — | |
| Handyman Services | handyman | Not Explored | — | |
| Shoemaker Services | shoemaker | Not Explored | — | |
| Surveying & Mapping | surveying-mapping | Not Explored | — | |

## On the way — Odoo en dev (3)
| Verticale | Slug | Statut | Score | Notes |
|-----------|------|--------|-------|-------|
| Vineyard | vineyard | Not Explored | — | Odoo developing |
| Catering (Odoo) | catering-odoo | Not Explored | — | Odoo developing — overlap verticale en cours |
| Pet Groomer | pet-groomer | Not Explored | — | Odoo developing |

**Total : ~91 verticales**
```

## 4.4 context/insights.md

```markdown
# Insights Fondateurs

## Comment utiliser ce fichier
- Ajouter via : /insight [texte]
- Chaque insight est daté, catégorisé et rattaché aux verticales concernées
- Les insights sont propagés vers les fichiers de verticales pertinents
- Les insights peuvent être challengés quand des données terrain les contredisent

## Catégories
- VISION : direction stratégique, convictions sur le marché
- MARCHÉ : observations sur les clients, tendances, opportunités
- PRODUIT : idées features, limitations, roadmap
- CONCURRENCE : observations sur les concurrents
- DISTRIBUTION : canaux, partenariats, accès aux clients
- PRICING : modèles, willingness-to-pay, packaging

## Format
[DATE] [CATÉGORIE] [VERTICALES: all | slug1, slug2] Texte de l'insight
  → Statut: [active | challenged | superseded]
  → Challengé par: [source si applicable]

## Insights
<!-- Rempli via /insight -->
```

## 4.5 context/learnings.md

```markdown
# Learnings Cross-Verticales

Mis à jour par /retro et /learn.
Contient les patterns et anti-patterns découverts en explorant plusieurs verticales.

## Patterns qui marchent
<!-- Découvert via : [verticales] -->

## Anti-patterns (ce qui ne marche pas)
<!-- Découvert via : [verticales] -->

## Améliorations du processus

## Questions de call qui marchent le mieux

## Signaux d'achat les plus fiables
```

## 4.6 context/verticals/_template.md

```markdown
# Verticale : {{NOM}}

## Metadata
- Slug : {{slug}}
- Catégorie Odoo : {{catégorie}}
- Statut : Discovery
- Date début exploration : {{YYYY-MM-DD}}
- Responsable : {{nom}}
- Dernière mise à jour : {{YYYY-MM-DD}}

---

## Scorecard
| # | Critère | Score (1-5) | Poids | Confidence | Source | Notes |
|---|---------|-------------|-------|------------|--------|-------|
| 1 | Complexité opérationnelle | — | 1.2x | — | — | — |
| 2 | Taille de marché adressable | — | 1.0x | — | — | — |
| 3 | Insatisfaction outils existants | — | 1.3x | — | — | — |
| 4 | Capacité à payer | — | 1.1x | — | — | — |
| 5 | Facilité de distribution | — | 1.0x | — | — | — |
| 6 | Fit produit Enobase | — | 1.2x | — | — | — |
| 7 | Densité données / potentiel IA | — | 0.8x | — | — | — |
| 8 | Viralité / effet réseau | — | 0.7x | — | — | — |
| 9 | Vitesse de validation | — | 1.1x | — | — | — |

**Score pondéré** : —/5.0
**Confidence globale** : —/9 critères à Med+
**Décision** : — (GO / PROMISING / INVESTIGATE / PARK / REJECT)

---

## Marché

### Taille & structure
- TAM (nombre d'entreprises cibles) :
- TAM France :
- TAM Europe :
- Segment géographique prioritaire :
- Taille typique des entreprises cibles (employés) :
- CA typique :
- Saisonnalité :

### Acteurs & écosystème
- Associations/fédérations professionnelles :
- Salons/événements clés :
- Influenceurs/leaders d'opinion :
- Communautés en ligne :
- Médias spécialisés :
- Partenaires potentiels :

### Position d'Odoo
- Module vertical dédié ? [oui/non, lien]
- Maturité du module : [inexistant | basique | avancé]
- Partenaires Odoo spécialisés :
- Limites connues d'Odoo :
- Retours utilisateurs (forum, reviews) :

---

## Concurrence & outils en place
| Outil/Solution | Type | Part de marché estimée | Forces | Faiblesses | Prix | Source |
|---------------|------|----------------------|--------|------------|------|--------|

### Spectre d'équipement
- % no tool (Excel/papier) :
- % outils inadaptés (bricolage) :
- % solution verticale :
- % ERP généraliste :

---

## Douleurs identifiées
<!-- [source: call-XX | web | intuition] [récurrence: 1x | 2-3x | 4+x] Description -->

---

## Personas / ICP préliminaire

### Décideur
- Titre typique :
- Préoccupations :
- Triggers d'achat :
- Objections prévisibles :
- Canaux :

### Utilisateur principal
- Titre :
- Frustrations quotidiennes :
- Outils actuels :
- Tech-savviness :

---

## Hypothèses actives
| ID | Hypothèse | Statut | Méthode de test | Critère(s) | Résultat | Date |
|----|-----------|--------|-----------------|------------|----------|------|
| H1 | | Non testée | | | | |

---

## Conversations & calls
<!-- [YYYY-MM-DD] [Nom, Titre, Entreprise] [Stade]
  → Key takeaways :
  → Impact scoring :
  → Lien : calls/YYYY-MM-DD_nom.md -->

---

## Blueprints générés
<!-- Liens + date de dernière génération -->

---

## Journal de décision
<!-- [YYYY-MM-DD] Décision : ... Raison : ... Basée sur : ... -->

---

## Next actions
- [ ] (priorité: haute/moyenne/basse) (deadline: YYYY-MM-DD)
```

## 4.7 context/calls/_template.md

```markdown
# Call : {{NOM_PROSPECT}} — {{DATE}}

## Metadata
- Date : {{YYYY-MM-DD}}
- Durée : {{minutes}} min
- Verticale : {{slug}}
- Prospect : {{nom}}, {{titre}}, {{entreprise}}
- Taille entreprise : {{employés}} employés
- Fireflies ID : {{id}}
- Stade : [cold | warm | qualified | customer]
- Call # dans cette verticale : {{n}}

## Résumé (3 lignes max)

## Données extraites

### Douleurs mentionnées
- [force: forte | moyenne | faible] Description → Quote: "..."

### Outils utilisés actuellement
| Outil | Usage | Satisfaction (1-5) | Ce qui manque |
|-------|-------|--------------------|---------------|

### Budget / Willingness to pay
- Budget actuel outils :
- WTP pour Enobase :
- Signaux : [explicite | implicite | aucun]
- Vélocité décision : [<1m | 1-3m | 3-6m | >6m]

### Objections
- "..." → Réponse : ... → Efficacité : [convaincante | partielle | inefficace]

### Besoins fonctionnels
- Besoin : [must-have | nice-to-have] → Couvert Enobase : [oui | partiellement | non]

### Quotes clés
> "..."

### Maturité d'achat : [1-5]
### Signaux d'achat : [liste]

## Impact sur le scoring
| Critère | Ancien | Nouveau | Confidence avant → après |
|---------|--------|---------|--------------------------|

## Actions suivantes
- [ ]

## Insights à propager
```

---

# 5. SKILLS PHASE 1 : EXPLORE (Understand)

## 5.1 SKILL-vertical-scan.md

```markdown
---
name: vertical-scan
description: Recherche initiale approfondie sur une verticale. Crée et pré-remplit le fichier de contexte.
trigger: /explore [vertical]
phase: 01-explore
inputs:
  - context/company.md
  - context/odoo-verticals-map.md
  - context/insights.md (filtré sur la verticale)
outputs:
  - context/verticals/{slug}.md (créé depuis _template.md, pré-rempli)
  - Mise à jour context/odoo-verticals-map.md (statut → Discovery)
---

# Vertical Scan

## Objectif
Quand l'utilisateur lance /explore [vertical] :
1. Créer le fichier de verticale depuis le template
2. Web search : taille de marché, acteurs, outils existants, douleurs connues
3. Rechercher la position d'Odoo (module dédié ? forums ? retours ?)
4. Pré-remplir un maximum de sections
5. Transformer les lacunes d'info en hypothèses à tester
6. Produire un premier scoring desk research (confidence Low)
7. Générer les 3-5 premières actions
8. Mettre à jour odoo-verticals-map.md

## Recherches web à effectuer
- "[vertical] market size France" / "[vertical] marché France taille"
- "[vertical] software tools" / "logiciel [vertical]"
- "[vertical] ERP" / "best [vertical] software 2025 2026"
- "odoo [vertical]" / forum Odoo
- "[vertical] associations professionnelles France"
- "[vertical] salons événements France"
- G2/Capterra pour la catégorie

## Règles
- Ne pas inventer de données. Si introuvable, écrire "[À DÉCOUVRIR — via calls]"
- Croiser au moins 2 sources pour les chiffres de marché
- Checker odoo.com/industries pour le module vertical
- Consulter context/insights.md pour les intuitions fondateurs pertinentes
- Toujours proposer 3-5 prospects à contacter et comment les trouver
```

## 5.2 SKILL-odoo-market-map.md

```markdown
---
name: odoo-market-map
description: Scanner toutes les verticales Odoo et produire un ranking par potentiel.
trigger: /scan
phase: 01-explore
inputs:
  - context/company.md
  - context/odoo-verticals-map.md
  - context/scorecard-criteria.md
  - context/insights.md
outputs:
  - Ranking des verticales candidates
  - Recommandation des 5-10 à explorer en priorité
---

# Odoo Market Map Scanner

## Process
1. Analyser chaque catégorie Odoo vs. fit Enobase (AI-native ERP)
2. Quick scoring (1-3) par verticale : complexité, taille marché, insatisfaction, fit produit
3. Classer par score total, recommander 5-10 prioritaires
4. Identifier "quick wins" (testables immédiatement) et "long bets" (gros marché, exploration plus longue)
5. Intégrer les insights fondateurs dans le ranking
```

## 5.3 SKILL-interview-guide.md

```markdown
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

## Blocs de questions

### Bloc 1 : Situation actuelle (5 min)
Questions sur l'entreprise, taille, opérations. SKIP si déjà connu.

### Bloc 2 : Outils & process (10 min)
Outils utilisés, frustrations, temps passé sur tâches manuelles, alternatives cherchées.
ADAPTER si on sait déjà quels outils → aller direct aux frustrations.

### Bloc 3 : Douleurs & impact (10 min)
Plus gros problème opérationnel, coût (temps, argent, erreurs), baguette magique.
ADAPTER en creusant les douleurs déjà identifiées dans les calls précédents.

### Bloc 4 : Budget & décision (5 min)
Budget actuel, WTP, décideur, timeline. RÉSERVER pour le 2ème call+.

### Bloc 5 : Validation produit (5 min)
Intérêt pour [X,Y,Z], must-have vs nice-to-have. RÉSERVER pour le 3ème call+.

### Questions hypothèses
Pour chaque hypothèse non testée dans verticals/{slug}.md, 1-2 questions concrètes.

## Notes caller
- Écouter 70%, parler 30%
- Ne pas vendre, explorer
- Noter les verbatims
- Identifier maturité d'achat (1-5)
```

## 5.4 SKILL-competitor-intel.md

```markdown
---
name: competitor-intel
description: Intelligence concurrentielle sur une verticale.
trigger: /intel [vertical]
phase: 01-explore
inputs:
  - context/verticals/{slug}.md
  - context/company.md (différenciateurs)
outputs:
  - MAJ section concurrence dans verticals/{slug}.md
  - Optionnel : blueprints/{slug}/battlecards.md
---

# Competitor Intelligence

## Process
1. Web search : "[vertical] management software", reviews G2/Capterra, forums
2. Pour chaque concurrent : pricing, forces, faiblesses, positionnement, taille
3. Mapper le spectre d'équipement (% no-tool / inadapté / vertical / ERP)
4. Identifier les angles d'attaque Enobase par concurrent
5. Mettre à jour verticals/{slug}.md
```

---

# 6. SKILLS PHASE 2 : EVALUATE (Score & Decide)

## 6.1 SKILL-vertical-scorecard.md

```markdown
---
name: vertical-scorecard
description: Scorer ou re-scorer une verticale sur les 9 critères pondérés.
trigger: /score [vertical]
phase: 02-evaluate
inputs:
  - context/verticals/{slug}.md (toutes sections)
  - context/calls/ (liés)
  - context/scorecard-criteria.md
  - context/insights.md
  - context/learnings.md
outputs:
  - MAJ scorecard dans verticals/{slug}.md
  - Recommandation GO / PROMISING / INVESTIGATE / PARK / REJECT
  - MAJ odoo-verticals-map.md
---

# Vertical Scorecard

## Process
1. Lire tout le contexte disponible
2. Pour chaque critère (1-9) :
   - Lister data points avec source
   - Scorer 1-5 selon échelles dans scorecard-criteria.md
   - Confidence : Low (desk research seul) / Med (2-3 sources + 1 call) / High (3+ sources + 2+ calls)
   - Justifier en 1-2 phrases
3. Calculer score pondéré : Σ(score × poids) / Σ(poids)
4. Confidence globale : critères à Med+ / 9
5. Appliquer seuils de décision
6. Si INVESTIGATE : identifier quels critères Low et quelles actions pour monter en confidence
7. Comparer aux autres verticales scorées si applicable
8. Mettre à jour verticals/{slug}.md et odoo-verticals-map.md
```

## 6.2 SKILL-segment-scorer.md

```markdown
---
name: segment-scorer
description: Identifier et prioriser les sous-segments au sein d'une verticale.
trigger: /segment [vertical]
phase: 02-evaluate
inputs:
  - context/verticals/{slug}.md
  - context/calls/ liés
outputs:
  - Section "Segments" dans verticals/{slug}.md
---

# Segment Scorer

## Dimensions de segmentation
- Taille : solo, TPE (2-10), PME (10-50), ETI (50-250)
- Géographie : local, régional, national, international
- Maturité digitale : paper-based, Excel, multi-outils, ERP
- Sous-spécialité du vertical
- Douleur principale

## Évaluation par segment (1-5)
Taille segment, intensité douleur, capacité à payer, facilité d'accès, fit produit

## Output
Tableau ranking + identification du beachhead segment (1er à attaquer)
```

## 6.3 SKILL-criteria-evolution.md

```markdown
---
name: criteria-evolution
description: Analyser les scorecards et proposer des évolutions de critères.
trigger: /criteria
phase: 02-evaluate
inputs:
  - context/scorecard-criteria.md
  - context/verticals/*.md (toutes scorecards)
  - context/learnings.md
outputs:
  - Proposition de modifications (validation requise avant application)
  - MAJ scorecard-criteria.md si validé
---

# Criteria Evolution

## Trigger : après 5+ verticales scorées
1. Analyser corrélations : quels critères prédisent le mieux GO vs REJECT ?
2. Identifier critères non discriminants (scores toujours similaires)
3. Identifier critères manquants
4. Proposer ajustements : nouveaux critères, pondérations, seuils
5. Demander validation avant modification
6. Versionner dans l'historique
```

---

# 7. SKILLS PHASE 3 : BUILD (Blueprints)

**Règle commune** : Toujours lire context/verticals/{slug}.md + company.md + blueprints existants avant de produire.

## 7.1 SKILL-icp-definition.md

```markdown
---
name: icp-definition
trigger: /blueprint icp [vertical]
phase: 03-build
depends-on: vertical-scan (minimum)
outputs: blueprints/{slug}/icp.md
---

## Structure du blueprint
1. **Firmographics** : taille, CA, géo, sous-vertical, structure
2. **Persona Décideur** : titre, objectifs, frustrations (quotes calls), triggers d'achat, peurs, canaux d'info, process décision
3. **Persona Utilisateur** : titre, frustrations quotidiennes, outils actuels, rêves, tech-savviness
4. **Buying Signals** : prêt à acheter, en exploration, red flags
5. **Anti-personas** : qui NE PAS cibler et pourquoi
6. **Scoring de qualification** : framework HOT / WARM / COLD / PARK
```

## 7.2 SKILL-positioning.md

```markdown
---
name: positioning
trigger: /blueprint positioning [vertical]
phase: 03-build
depends-on: icp-definition
outputs: blueprints/{slug}/positioning.md
---

## Structure du blueprint
1. **Positioning Statement** : Pour [ICP], qui [problème], Enobase est [catégorie] qui [bénéfice]. Contrairement à [alternative], Enobase [différenciateur].
2. **UVP** : headline (10 mots), sub-headline (30 mots), preuve
3. **USPs** (3-5) : titre, description, proof point — spécifiques au vertical
4. **Messaging par persona** : pain point (dans ses mots), message clé, ton, CTA
5. **Battlecards par concurrent** : notre avantage, leur avantage, trap question, réponse à "mais X fait Y", never say
6. **Elevator pitches** : 10 sec, 30 sec, 2 min
```

## 7.3 SKILL-pricing-model.md

```markdown
---
name: pricing-model
trigger: /blueprint pricing [vertical]
phase: 03-build
depends-on: icp-definition, competitor-intel
outputs: blueprints/{slug}/pricing-model.md
---

## Structure du blueprint
1. **Analyse concurrentielle des prix**
2. **WTP** : déclaré (calls), implicite (budget actuel), estimé (valeur douleur)
3. **Modèle proposé** : structure (per-user/company/usage/tiered), tiers, prix, features par tier
4. **Unit economics** : ARPA, CAC estimé, LTV estimé, LTV/CAC
5. **Packaging vertical-specific** : modules par tier, add-ons, pricing onboarding
```

## 7.4 SKILL-landing-page.md

```markdown
---
name: landing-page
trigger: /blueprint landing-page [vertical]
phase: 03-build
depends-on: positioning, icp-definition
outputs:
  - blueprints/{slug}/landing-page.md (copy)
  - blueprints/{slug}/landing-page.html (déployable)
---

## Structure copy (.md)
1. **Hero** : headline (UVP), sub-headline, CTA, social proof
2. **Problem** : 3 douleurs (mots du prospect, quotes calls)
3. **Solution** : comment Enobase résout chaque douleur, features clés (bénéfices pas features)
4. **Social Proof** : testimonials, logos, métriques
5. **Pricing Preview** : reprend pricing-model ou "Demandez une démo"
6. **CTA Final** : formulaire/booking, réduction de friction

## Version HTML
Single-file, responsive, CSS inline, SEO optimisé, formulaire fonctionnel
```

## 7.5 SKILL-sales-deck.md

```markdown
---
name: sales-deck
trigger: /blueprint sales-deck [vertical]
phase: 03-build
depends-on: positioning, icp-definition
outputs: blueprints/{slug}/sales-deck.md
---

## Structure (10-12 slides)
1. Titre : Enobase pour [Vertical] — [UVP]
2. Le problème (3 douleurs, chiffres)
3. L'impact (coût de ne rien faire)
4. La solution (1 phrase + 3 pilliers)
5-7. Features clés (1/slide, bénéfice + avant/après)
8. Différenciation (vs alternatives)
9. Social proof
10. Pricing
11. Prochaines étapes (CTA)
12. Contact
```

## 7.6 SKILL-outbound-campaign.md

```markdown
---
name: outbound-campaign
trigger: /blueprint outbound [vertical]
phase: 03-build
depends-on: icp-definition, positioning
outputs: blueprints/{slug}/outbound-campaign.md
---

## Séquence email (5 touchpoints)
- J0 : "pain-first" — question sur douleur principale, pas de pitch
- J3 : "insight" — partager un insight, montrer expertise
- J7 : "social proof" — case study ou résultat
- J12 : "objection preempt" — adresser objection #1
- J18 : "breakup" — dernier message, ton léger

## Séquence LinkedIn (3 touchpoints)
- Connexion + note personnalisée
- Message insight
- Follow-up engagement

## Variantes par persona et par segment
## Métriques cibles : ouverture >40%, réponse >5%, booking >2%
```

## 7.7 SKILL-objection-handling.md

```markdown
---
name: objection-handling
trigger: /blueprint objections [vertical]
phase: 03-build
depends-on: au moins 2-3 calls réalisés
outputs: blueprints/{slug}/objection-handling.md
---

## Pour chaque objection (top 10)
1. Objection (verbatim)
2. Fréquence (combien de calls)
3. Intent réel derrière
4. Réponse : Acknowledge → Reframe → Respond → CTA
5. Ce qui a marché en call
6. Ce qui n'a PAS marché
7. Preuve à montrer

## Catégories : Prix, Timing, Concurrence, Complexité, Confiance, Fonctionnel, Interne
## Re-générer après 2+ nouveaux calls avec nouvelles objections
```

## 7.8 SKILL-demo-script.md

```markdown
---
name: demo-script
trigger: /blueprint demo-script [vertical]
phase: 03-build
depends-on: icp-definition, positioning
outputs: blueprints/{slug}/demo-script.md
---

## Structure (20-30 min)
1. Ouverture (2 min) : contexte, douleurs, plan
2. Douleur #1 → Solution (5-7 min) : problème → résolution → "aha"
3. Douleur #2 → Solution (5-7 min)
4. Douleur #3 → Solution (5-7 min)
5. Vue d'ensemble (3 min) : dashboard, valeur AI-native
6. Q&A (5 min) : questions anticipées
7. Close (2 min) : résumé, next step, timeline
```

## 7.9 SKILL-roi-calculator.md

```markdown
---
name: roi-calculator
trigger: /blueprint roi-calculator [vertical]
phase: 03-build
depends-on: icp-definition, pricing-model
outputs:
  - blueprints/{slug}/roi-calculator.md
  - blueprints/{slug}/roi-calculator.html (interactif)
---

## Modèle
- Inputs prospect : employés, CA, heures/semaine sur tâches manuelles, coût outils, erreurs/mois
- Calculs : temps gagné, erreurs évitées, delta coût outils, ROI %
- Output : ROI 12 mois, payback period, économies annuelles, temps récupéré/semaine
- HTML : sliders, calcul temps réel, bouton "envoyez-moi les résultats"
```

## 7.10 SKILL-hypotheses.md

```markdown
---
name: hypotheses
trigger: /blueprint hypotheses [vertical]
phase: 03-build
outputs:
  - blueprints/{slug}/hypotheses.md
  - MAJ section hypothèses dans verticals/{slug}.md
---

## Pour chaque hypothèse
- ID, énoncé testable, catégorie, critère(s) impacté(s)
- Statut : non-testée | en test | validée | invalidée
- Méthode de test, critère de validation, critère d'invalidation
- Résultat, impact décisionnel

## Priorisation : hypothèses critiques (changeraient GO/PARK) en premier
## Dashboard : total, validées, invalidées, non-testées, % couverture
```

---

# 8. SKILLS PHASE 4 : EXECUTE (Launch & Test)

## 8.1 SKILL-outbound-targets.md

```markdown
---
name: outbound-targets
trigger: /targets [vertical]
phase: 04-execute
depends-on: icp-definition, outbound-campaign
outputs: blueprints/{slug}/outbound-targets.md
---

## Critères de ciblage (basés ICP)
Firmographics, technographics, signaux, intent signals

## Sources de données
LinkedIn Sales Nav, Google Maps, annuaires pro, associations, salons, Societe.com/Pappers

## Spécification de liste
Taille cible, enrichissement (email, tel, LinkedIn), outils recommandés (Apollo, Dropcontact)

## Plan d'exécution
Semaine 1: constituer (50+) → Semaine 2: enrichir → Semaine 3: séquence A → Semaine 4: analyser, itérer
```

## 8.2 SKILL-ab-test-plan.md

```markdown
---
name: ab-test-plan
trigger: /test [vertical]
phase: 04-execute
depends-on: outbound-campaign
outputs: blueprints/{slug}/ab-test-plan.md
---

## Priorité de test
1. Message (quel pain point résonne ?)
2. Canal (email vs LinkedIn vs cold call ?)
3. Segment (quel sous-segment répond ?)
4. CTA (démo vs trial vs content ?)

## Par test : hypothèse, variante A/B, métrique, taille échantillon, durée, critère de décision
## Plan séquencé sur 6 semaines
```

## 8.3 SKILL-launch-checklist.md

```markdown
---
name: launch-checklist
trigger: /launch [vertical]
phase: 04-execute
outputs: checklist avec statut par item
---

## Pré-requis
- [ ] Scorecard ≥ 3.5, confidence ≥ 5/9 Med+
- [ ] ICP validé (3+ calls)
- [ ] Positioning finalisé
- [ ] 1+ blueprint outbound prêt
- [ ] Pricing model défini

## Assets
- [ ] Landing page, outbound séquence, target list (50+), sales deck, demo script, objection guide, ROI calculator

## Infrastructure
- [ ] Attio pipeline configuré, tracking outbound, calendrier booking, email signature

## Go / No-Go : revue finale
```

## 8.4 SKILL-feedback-loop.md

```markdown
---
name: feedback-loop
trigger: /feedback [vertical]
phase: 04-execute
outputs:
  - MAJ verticals/{slug}.md
  - MAJ blueprints concernés
  - MAJ learnings.md si patterns cross-verticales
---

## Process
1. Collecter résultats : outbound (ouvertures, réponses, bookings), landing page, calls, deals
2. Analyser : what worked / didn't, hypothèses validées/invalidées
3. Mettre à jour : re-scorer si justifié, MAJ hypothèses, MAJ blueprints
4. Décider : itérer / accélérer / pivoter / parker
```

---

# 9. SKILLS PHASE 5 : SYSTEM (Scale & Learn)

## 9.1 SKILL-retrospective.md

```markdown
---
name: retrospective
trigger: /retro [vertical]
phase: 05-system
outputs:
  - Rapport retro dans verticals/{slug}.md (journal de décision)
  - MAJ learnings.md
---

## Structure
- Timeline, métriques (calls, prospects, pipeline, deals, temps investi)
- What worked (messages, canaux, segments, features)
- What didn't work
- Surprises
- Learnings à propager cross-verticales
- Recommandation : intensifier / ajuster / parker / rejeter
```

## 9.2 SKILL-playbook-update.md

```markdown
---
name: playbook-update
trigger: /learn
phase: 05-system
outputs:
  - Propositions de MAJ des skills
  - MAJ learnings.md
---

## Process
1. Analyser tous les learnings accumulés
2. Identifier patterns récurrents (3+ occurrences)
3. Pour chaque pattern : quelle(s) skill(s) impactée(s)
4. Proposer modifications concrètes
5. Validation requise avant application
6. Versionner
```

## 9.3 SKILL-vertical-graduation.md

```markdown
---
name: vertical-graduation
trigger: /graduate [vertical]
phase: 05-system
outputs: MAJ statut + odoo-verticals-map.md
---

## Critères par stade

### Discovery → Scoring
- Fichier créé et pré-rempli
- 2+ calls discovery
- Concurrence mappée
- Premier scoring (même Low confidence)

### Scoring → Validation
- Score ≥ 3.5, confidence ≥ 5/9 Med+
- 5+ calls
- ICP défini
- Hypothèses critiques testées

### Validation → Active
- Score ≥ 4.0, confidence ≥ 7/9 Med+
- Blueprints clés produits (ICP, positioning, outbound)
- 1+ test outbound avec résultats positifs
- Pricing validé (3+ prospects)
- 1+ deal en pipeline

### Active → Scaled
- 3+ deals closés
- Process de vente reproductible
- Marketing engine en place
- Onboarding standardisé
```

---

# 10. AUTOMATIONS

## 10.1 SKILL-call-ingestion.md

```markdown
---
name: call-ingestion
trigger: /ingest [fireflies-meeting-id | "latest"]
phase: automation
outputs:
  - context/calls/YYYY-MM-DD_nom.md
  - MAJ context/verticals/{slug}.md
  - Suggestions de MAJ blueprints
---

## Process
1. Récupérer transcript via MCP Fireflies (fireflies_fetch ou fireflies_get_transcript)
2. Extraire : info prospect, verticale, douleurs+quotes, outils+satisfaction, budget/WTP, objections+efficacité réponse, besoins fonctionnels, maturité achat, signaux achat
3. Créer fichier call depuis template
4. Enrichir verticals/{slug}.md : nouvelles douleurs, outils, conversations, hypothèses, scores
5. Signaler impacts : quels critères changent, quels blueprints à MAJ, quelles hypothèses changent de statut, suggestions next actions
6. Proposer insights cross-verticales si applicable → insights.md / learnings.md
```

## 10.2 SKILL-context-enrichment.md

```markdown
---
name: context-enrichment
trigger: /insight [texte] ou automatique après /ingest
phase: automation
---

## Règles de propagation

### Insight fondateur (/insight)
1. Ajouter à insights.md (date, catégorie, verticales)
2. Référencer dans chaque verticale concernée
3. Signaler si un score doit être revu

### Nouveau call (/ingest)
1. Fichier call (via call-ingestion)
2. MAJ verticals/{slug}.md
3. Vérifier blueprints impactés
4. Vérifier hypothèses

### Feedback test (/feedback)
1. MAJ métriques dans verticals/{slug}.md
2. MAJ hypothèses
3. Proposer re-scoring si résultats significatifs

### Règle d'or
Chaque nouveau data point → quels fichiers impactés ? quels scores changent ? quels blueprints à régénérer ? pattern cross-vertical ?
```

## 10.3 SKILL-weekly-sprint.md

```markdown
---
name: weekly-sprint
trigger: /sprint
phase: automation
---

## Process
1. État des lieux par verticale active (statut, score, confidence, calls, blueprints, retards)
2. Prioriser : verticales proches d'un GO, tests en cours, nouvelles à explorer, blueprints à produire
3. Plan semaine : lundi=revue, mar-mer=calls, jeudi=blueprints, vendredi=ingestion+prépa
4. Métriques hebdo : verticales explorées, calls, blueprints, scores, décisions
5. Recommandation : "Cette semaine, concentrer sur [X] parce que [Y]. Actions : [1,2,3]."
```

---

# 11. BLUEPRINTS — TEMPLATES ET OUTPUTS

## Chaîne de dépendances

```
vertical-scan
  └── icp-definition
         ├── positioning
         │     ├── landing-page → .html
         │     ├── sales-deck
         │     ├── outbound-campaign → outbound-targets → ab-test-plan
         │     ├── demo-script
         │     └── objection-handling
         ├── pricing-model → roi-calculator → .html
         └── hypotheses
```

## Versions déployables

| Blueprint | Format | Quand générer |
|-----------|--------|--------------|
| Landing page | HTML single-file responsive | Positioning validé (Med+) |
| ROI Calculator | HTML interactif JS inline | Pricing model défini |
| Sales deck | PPTX (via skill Claude Code) | Positioning + pricing validés |
| Outbound sequences | Copy prêt à coller | ICP + positioning ready |

---

# 12. CRITÈRES DE SCORING — RÉCAPITULATIF

| # | Critère | Poids | Rationale |
|---|---------|-------|-----------|
| 1 | Complexité opérationnelle | 1.2x | AI shines on complexity |
| 2 | Taille de marché | 1.0x | Important mais pas suffisant seul |
| 3 | Insatisfaction outils | 1.3x | Meilleur prédicteur conversion rapide |
| 4 | Capacité à payer | 1.1x | Marché qui ne paie pas = pas viable |
| 5 | Facilité de distribution | 1.0x | Impacte le CAC |
| 6 | Fit produit Enobase | 1.2x | Pas de fit = pas de deal |
| 7 | Densité données / IA | 0.8x | Différenciateur long-terme |
| 8 | Viralité / réseau | 0.7x | Bonus, pas bloquant |
| 9 | Vitesse de validation | 1.1x | On veut aller vite |

Somme poids = 9.4 | Score max = 5.0

---

# 13. CONNECTEURS MCP

## Connectés
| Connecteur | Usage | Skills |
|------------|-------|--------|
| Fireflies | Transcripts calls | call-ingestion |
| Gmail | Outbound, suivi réponses | outbound execution |
| Google Calendar | Planification calls | weekly-sprint |

## À connecter
| Connecteur | Usage | Priorité |
|------------|-------|----------|
| Attio | CRM pipeline par verticale | HAUTE |
| GitHub | Versionner le repo | MOYENNE |

## Souhaitables
| Connecteur | Usage | Workaround |
|------------|-------|------------|
| LinkedIn | Outbound, scraping profils | Copy dans blueprints, exécution manuelle |
| Apollo/Dropcontact | Enrichissement target lists | Specs dans outbound-targets, enrichissement manuel |

---

# 14. WORKFLOWS OPÉRATIONNELS

## Workflow 1 : Explorer une nouvelle verticale (2-3 semaines)

```
JOUR 1
  /scan (si pas encore fait) → identifier les candidates
  /explore [vertical] → contexte, web research, scoring Low
  /interview [vertical] → questions pour 1er call

JOUR 2-7
  3-5 calls discovery
  /ingest [call-id] × 3-5 → enrichir contexte auto
  /interview [vertical] → adapter questions à chaque call

JOUR 7-8
  /score [vertical] → scoring Med
  /intel [vertical] → compléter la concurrence
  DÉCISION : GO / INVESTIGATE / PARK

SI GO ou PROMISING :
JOUR 8-10
  /blueprint icp [vertical]
  /blueprint positioning [vertical]
  /blueprint pricing [vertical]
  /blueprint hypotheses [vertical]

JOUR 10-12
  /blueprint outbound [vertical]
  /blueprint landing-page [vertical]
  /targets [vertical]
  /launch [vertical] → checklist

JOUR 12-14
  Exécuter outbound (50-100 contacts)
  /test [vertical] si A/B pertinent

JOUR 14+
  /feedback [vertical] → résultats
  /score [vertical] → re-scorer
  DÉCISION : ACCÉLÉRER / ITÉRER / PIVOTER / PARKER
```

## Workflow 2 : Sprint hebdomadaire

```
LUNDI    : /sprint → plan semaine + /status → vue d'ensemble
MAR-JEU  : calls + /ingest + blueprints + outbound
VENDREDI : /feedback + /score si nouvelles données + prépa semaine suivante
```

## Workflow 3 : Ingérer un call (5 min)

```
/ingest [call-id] → extraction → MAJ contexte → suggestions
/interview [vertical] → régénérer guide pour prochain call
```

## Workflow 4 : Ajouter un insight fondateur (1 min)

```
/insight "texte de l'insight"
→ Ajout insights.md + identification verticales impactées + signal re-scoring
```

---

# 15. CARTE DES VERTICALES ODOO — RÉSUMÉ

| Catégorie | Nb | Exemples |
|-----------|-----|----------|
| Business Services | 11 | Accounting, Law, Marketing Agency |
| Culture & Arts | 8 | Gallery, Museum, Photography |
| Education & Training | 4 | Driving School, eLearning |
| Events/Community/Nonprofits | 9 | Event Mgmt, Coworking, Wedding Planner |
| Food & Beverage | 10 | Bakery, Restaurant, Catering, Food Trucks |
| Health/Wellness/Personal Care | 8 | Fitness, Pharmacy, Hair Salon |
| Hospitality/Tourism/Leisure | 11 | Hotel, Campsite, Guided Tours |
| Manufacturing & Supply Chain | 8 | Metal, Textile, Food Distribution |
| Real Estate/Construction | 7 | Construction, Architecture, HVAC, Solar |
| Retail & eCommerce | 13 | Clothing, Electronics, Grocery |
| Trades & Home Services | 8 | Electrician, Cleaning, Handyman |
| On the way (Odoo dev) | 3 | Vineyard, Catering, Pet Groomer |

**Total : ~91 verticales** (liste détaillée dans context/odoo-verticals-map.md)

---

# 16. PLAN DE BUILD

## Phase A : Fondations
1. Créer structure complète du repo (dossiers)
2. Écrire CLAUDE.md
3. Créer context/company.md (compléter avec l'équipe)
4. Créer context/scorecard-criteria.md
5. Créer context/odoo-verticals-map.md (pré-rempli ~91 verticales)
6. Créer context/insights.md, context/learnings.md (structure vide)
7. Créer templates : verticals/_template.md, calls/_template.md

## Phase B : Skills d'exploration (Phase 1)
8. SKILL-vertical-scan.md
9. SKILL-odoo-market-map.md
10. SKILL-interview-guide.md
11. SKILL-competitor-intel.md

## Phase C : Automations (critique — capitaliser dès maintenant)
12. SKILL-call-ingestion.md (+ test avec Fireflies MCP)
13. SKILL-context-enrichment.md
14. SKILL-weekly-sprint.md

## Phase D : Skills d'évaluation (Phase 2)
15. SKILL-vertical-scorecard.md
16. SKILL-segment-scorer.md
17. SKILL-criteria-evolution.md

## Phase E : Skills de build (Phase 3) — ordre de dépendances
18. SKILL-icp-definition.md
19. SKILL-positioning.md
20. SKILL-pricing-model.md
21. SKILL-hypotheses.md
22. SKILL-landing-page.md
23. SKILL-sales-deck.md
24. SKILL-outbound-campaign.md
25. SKILL-objection-handling.md
26. SKILL-demo-script.md
27. SKILL-roi-calculator.md

## Phase F : Skills d'exécution (Phase 4)
28. SKILL-outbound-targets.md
29. SKILL-ab-test-plan.md
30. SKILL-launch-checklist.md
31. SKILL-feedback-loop.md

## Phase G : Skills système (Phase 5)
32. SKILL-retrospective.md
33. SKILL-playbook-update.md
34. SKILL-vertical-graduation.md

## Phase H : Intégrations & templates déployables
35. Connecter Attio MCP
36. Configurer GitHub pour versionner
37. Créer template landing-page.html
38. Créer template roi-calculator.html

---

# ANNEXE : FORMAT STANDARD D'UNE SKILL

```markdown
---
name: [nom-kebab-case]
description: [1 ligne]
trigger: [commande slash]
phase: [01-explore | 02-evaluate | 03-build | 04-execute | 05-system | automation]
inputs:
  - [fichier ou source lu]
outputs:
  - [fichier produit ou mis à jour]
depends-on: [skills pré-requises]
---

# [Nom de la skill]

## Objectif
[Ce que cette skill accomplit en 2-3 phrases]

## Process
[Étapes numérotées et détaillées]

## Rules
[Contraintes, cas limites, garde-fous]

## Output format
[Structure attendue]
```

---

*Fin de la spec VEE v1.0 — 38 items de build, 22 skills, 3 automations, 91 verticales, 9 critères de scoring.*
*Ce document est la référence unique pour construire le système dans Claude Code.*