# VEE — Getting Started

## Bienvenue dans le Vertical Exploration Engine

Ce système t'aide à explorer, scorer et pénétrer les verticales d'industrie les plus
prometteuses pour Enobase — de façon systématique, rapide et avec un contexte qui
s'accumule au lieu de se perdre.

---

## Démarrage en 5 minutes

### Étape 1 — Compléter le contexte entreprise
Ouvrir `context/company.md` et remplir toutes les sections marquées `[À COMPLÉTER]` :
- Modules Enobase disponibles
- Taille de l'équipe et capacité GTM
- Budget marketing
- Objectifs 90 jours
- Marchés géographiques prioritaires

### Étape 2 — Scanner les verticales candidates
```
/scan
```
Obtenir un ranking des 10 verticales à explorer en priorité basé sur le fit Enobase.

### Étape 3 — Explorer la première verticale
```
/explore [slug]
```
Exemple : `/explore event-catering`

Le système va :
1. Créer `context/verticals/event-catering.md`
2. Faire une recherche web (marché, concurrence, douleurs)
3. Produire un premier scoring (Low confidence)
4. Suggérer les 3-5 premières actions

### Étape 4 — Préparer le premier call
```
/interview [slug]
```
Obtenir un guide d'entretien adapté au stade d'exploration.

### Étape 5 — Ingérer le call après
```
/ingest [fireflies-id]
```
ou `/ingest latest` pour le dernier call enregistré.

---

## Workflow type : Explorer une verticale (2-3 semaines)

```
JOUR 1
  /scan → identifier les candidats
  /explore [vertical] → créer contexte + scoring initial
  /interview [vertical] → préparer le 1er call

JOUR 2-7
  3-5 calls discovery
  /ingest [id] après chaque call
  /interview [vertical] → adapter le guide à chaque call

JOUR 7-8
  /score [vertical] → scoring Med confidence
  /intel [vertical] → compléter la concurrence
  → DÉCISION : GO / INVESTIGATE / PARK

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

JOUR 14+
  /feedback [vertical] → résultats
  /score [vertical] → re-scorer
  → DÉCISION : ACCÉLÉRER / ITÉRER / PIVOTER / PARKER
```

---

## Sprint hebdomadaire

Chaque lundi matin :
```
/sprint → plan de la semaine
/status → vue d'ensemble de toutes les verticales
```

---

## Commandes essentielles

| Commande | Usage | Fréquence |
|---------|-------|-----------|
| `/scan` | Scanner les verticales Odoo | 1x au démarrage, puis au besoin |
| `/explore [v]` | Démarrer une nouvelle verticale | À chaque nouvelle verticale |
| `/interview [v]` | Guide d'entretien | Avant chaque call |
| `/ingest [id]` | Ingérer un call Fireflies | Après chaque call |
| `/score [v]` | Scorer/re-scorer | Après 5+ calls ou nouveau batch |
| `/blueprint [type] [v]` | Produire un livrable | Selon la phase |
| `/sprint` | Plan hebdo | Chaque lundi |
| `/status` | Vue d'ensemble | Chaque lundi |

---

## Structure des fichiers

```
vee/
├── CLAUDE.md               ← Lu automatiquement à chaque session
├── context/                ← Source de vérité
│   ├── company.md          ← À compléter en premier
│   ├── scorecard-criteria.md
│   ├── odoo-verticals-map.md
│   ├── insights.md
│   ├── learnings.md
│   ├── verticals/          ← Un fichier par verticale
│   └── calls/              ← Transcripts structurés
├── skills/                 ← Comment le système travaille
│   ├── 01-explore/
│   ├── 02-evaluate/
│   ├── 03-build/
│   ├── 04-execute/
│   └── 05-system/
├── automations/            ← Enrichissement automatique
├── blueprints/             ← Livrables produits
│   └── _templates/         ← Templates HTML
└── docs/                   ← Documentation
```

---

## Connecteurs MCP disponibles

- **Fireflies** ✅ — Ingestion automatique des transcripts de calls
- **Gmail** ✅ — Envoi/suivi des séquences outbound
- **Google Calendar** ✅ — Planification des calls

**À connecter prochainement** :
- Attio (CRM)
- GitHub (versioning du repo)

---

## Premier jour — checklist

- [ ] Compléter context/company.md
- [ ] Lancer `/scan` pour identifier les candidats
- [ ] Choisir la première verticale à explorer
- [ ] Lancer `/explore [vertical]`
- [ ] Prendre 3 RDV discovery dans la semaine
