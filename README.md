# VEE — Vertical Exploration Engine

A systematic GTM framework for exploring, scoring, and penetrating industry verticals. Built to work with AI assistants (Claude/Cursor) using slash commands and structured context.

## What is VEE?

VEE is a knowledge system that helps founders and GTM teams:

- **Explore** new industry verticals systematically
- **Score** opportunities with traceable confidence levels
- **Build** go-to-market assets (ICP, positioning, outbound campaigns)
- **Execute** tests and capture feedback
- **Learn** across verticals to improve the playbook

Every interaction enriches the context. Nothing gets lost.

## Quick Start

```bash
# 1. Complete your company context
# Edit context/company.md with your specifics

# 2. Scan for vertical candidates
/scan

# 3. Explore your first vertical
/explore event-catering

# 4. Prepare for discovery calls
/interview event-catering
```

See [docs/getting-started.md](docs/getting-started.md) for the full walkthrough.

## Structure

```
vertical-explorer/
├── context/                 # Source of truth
│   ├── company.md           # Your company context
│   ├── verticals/           # One file per vertical
│   ├── calls/               # Structured call transcripts
│   ├── insights.md          # Founder intuitions
│   └── learnings.md         # Cross-vertical learnings
├── skills/                  # How the system works
│   ├── 01-explore/          # Discovery skills
│   ├── 02-evaluate/         # Scoring skills
│   ├── 03-build/            # Blueprint generation
│   ├── 04-execute/          # Launch & test skills
│   └── 05-system/           # Meta skills
├── blueprints/              # Generated deliverables
│   └── _templates/          # HTML templates
├── automations/             # Context enrichment
└── docs/                    # Documentation
```

## Slash Commands

### Exploration
| Command | Description |
|---------|-------------|
| `/scan` | Scan Odoo verticals and rank candidates |
| `/explore [vertical]` | Start exploring a new vertical |
| `/interview [vertical]` | Generate interview guide |
| `/intel [vertical]` | Competitive intelligence |

### Evaluation
| Command | Description |
|---------|-------------|
| `/score [vertical]` | Score or re-score a vertical |
| `/segment [vertical]` | Score segments within a vertical |
| `/compare [v1] [v2]` | Compare verticals side by side |

### Build
| Command | Description |
|---------|-------------|
| `/blueprint [type] [vertical]` | Generate a specific blueprint |
| `/blueprint all [vertical]` | Generate all blueprints |

Blueprint types: `icp`, `positioning`, `pricing`, `landing-page`, `sales-deck`, `outbound`, `objections`, `demo-script`, `roi-calculator`, `hypotheses`

### Execute
| Command | Description |
|---------|-------------|
| `/targets [vertical]` | Generate outbound target list |
| `/test [vertical]` | A/B test plan |
| `/launch [vertical]` | Launch checklist |
| `/feedback [vertical]` | Capture test results |

### System
| Command | Description |
|---------|-------------|
| `/ingest [call-id]` | Ingest Fireflies transcript |
| `/sprint` | Weekly action plan |
| `/status` | Overview of all verticals |
| `/retro [vertical]` | Retrospective |
| `/learn` | Extract cross-vertical learnings |

## Core Principles

1. **Capitalize** — Every market interaction enriches context
2. **Trace** — Hypotheses are explicit, data points have sources
3. **Chain** — Blueprints feed from context, outputs inform scoring
4. **Accelerate** — Speed of validation > perfection of outputs
5. **Enrich** — The system improves continuously

## Vertical Lifecycle

```
Not Explored → Discovery → Scoring → Validation → Active → Parked/Rejected
```

## MCP Connectors

- ✅ Fireflies — Call transcript ingestion
- ✅ Gmail — Outbound sequences
- ✅ Google Calendar — Call scheduling
- 🔜 Attio — CRM pipeline
- 🔜 GitHub — Repo versioning

## License

Private — Built for Enobase GTM operations.
