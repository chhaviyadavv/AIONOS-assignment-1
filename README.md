# TaskAlign

<p align="center">
  <img src="https://img.shields.io/badge/license-MIT-15803D?style=flat-square" alt="License" />
  <img src="https://img.shields.io/badge/TypeScript-5.4-38BDF8?style=flat-square&logo=typescript&logoColor=white" alt="TypeScript" />
  <img src="https://img.shields.io/badge/Python-3.11-3776AB?style=flat-square&logo=python&logoColor=white" alt="Python" />
  <img src="https://img.shields.io/badge/Tailwind_CSS-3.4-38BDF8?style=flat-square&logo=tailwindcss&logoColor=white" alt="Tailwind CSS" />
  <img src="https://img.shields.io/badge/build-passing-15803D?style=flat-square" alt="Build Status" />
</p>

<h3 align="center">The autonomous Chief of Staff for executive leadership.</h3>

<p align="center">
TaskAlign is an enterprise-grade, autonomous executive productivity and scheduling agent built for C-level and senior leadership. It fuses unstructured workplace communication — meeting transcripts, email threads, and chat streams — with real-time calendar realities, programmatically eliminating meeting collisions, tracking two-way accountability, and enabling zero-friction, one-click execution on every commitment made in a room, a thread, or a call.
</p>

---

## Table of Contents

1. [The Executive Problem & Solution](#the-executive-problem--solution)
2. [Core Features & Computational Engines](#core-features--computational-engines)
3. [Interactive Dashboard Navigation](#interactive-dashboard-navigation)
4. [System Architecture](#system-architecture)
5. [Data Schemas & DTO Contracts](#data-schemas--dto-contracts)
6. [Design Tokens & Visual Standards](#design-tokens--visual-standards)
7. [Repository Directory Layout](#repository-directory-layout)
8. [Quick Start Guide](#quick-start-guide)
9. [License & Contributing](#license--contributing)

---

## The Executive Problem & Solution

Conventional calendar tools were built to *store* events, not to *manage executive load*. For a VP of Sales, a CFO, or any leader whose day is a lattice of back-to-back external and internal meetings, a calendar grid is a passive container: it shows what's booked, but it has no opinion about what's broken.

Three failure modes recur at the leadership level, and none of them are solved by a bigger calendar app:

- **Calendar debt** — hard overlaps and sub-30-minute transition windows accumulate silently across a week of delegated scheduling, and by the time they surface, the executive is already double-booked or walking into a client call from a parking lot.
- **Commitment drift** — verbal commitments made in a meeting ("I'll send you the vendor list by Thursday") are rarely captured anywhere durable, so accountability lives in someone's memory instead of in a system of record.
- **Dispatch friction** — even when a follow-up is identified, drafting and sending it competes with the next meeting on the calendar, so the loop stays open far longer than it should.

TaskAlign's synthesis engine addresses all three by treating the executive's calendar, inbox, and transcripts as **one continuously reconciled dataset**. It doesn't wait to be told about a conflict or a promise — it extracts, computes, and resolves them automatically, then gets out of the way with one-click execution paths that respect how executives actually want to work: silently in the background, or reviewed by hand when the stakes call for it.

---

## Core Features & Computational Engines

### 1. Automated Conflict & Buffer Engine (Zero Manual Tagging)

No event needs to be manually flagged as a conflict or a tight turnaround — the engine computes both continuously.

**Hard Clash Detection.** Every new or modified event is evaluated against all existing events on the same calendar surface using standard interval-overlap arithmetic:

$$\text{Clash} = (S_{\text{new}} < E_i) \land (E_{\text{new}} > S_i)$$

Where $S_{\text{new}}$ / $E_{\text{new}}$ are the start and end of the incoming event, and $S_i$ / $E_i$ are the start and end of an existing event $i$. Any pair satisfying this predicate is a true overlap, not a heuristic guess — the check is symmetric and holds across arbitrary numbers of concurrent events. Detected clashes are surfaced instantly across both single-agenda views and multi-calendar team matrices, rendered in the signature **Vampire Hunter** (`#5F0309`) token so they are unmistakable against the rest of the interface.

**Proactive Buffer Calculation.** The engine walks each attendee's contiguous event blocks and flags any transition window of **30 minutes or less** between consecutive meetings — travel time, context-switching time, or simply breathing room. These windows are automatically tagged with a `⚡ [X]m Buffer` warning chip rendered in **Toasty Orange** (`#D96B00`), computed from the actual gap duration (`X`) with no manual pre-tagging required from the executive or their EA.

**Intelligent Team Matrix Resolver.** When a clash or a tight buffer is detected on a meeting with multiple stakeholders, the resolver cross-references the availability matrix of every attendee (e.g., Neha in Marketing, Raghav in Operations, Divya in Finance) and proposes the nearest zero-friction alternative — for example, recognizing that clearing a 9:00 AM focus block is costlier than moving the meeting, and instead recommending an open 2:00 PM slot that every required attendee already has free.

### 2. Two-Way Commitment & Blocker Ledger

TaskAlign maintains a living ledger of every commitment the executive has made or is owed, sourced directly from the same transcripts and threads the calendar engine already ingests.

- **Waiting on Me (Internal Debt).** The NLP pipeline extracts spoken and written commitments — *"Send Vendor List to Raghav"* — and binds each to an inferred or stated deadline. If `now() > deadline` and no outbound communication has been registered against that commitment, it is automatically flagged `OVERDUE` in **Vampire Hunter** (`#5F0309`), and TaskAlign pre-populates a context-aware apology-and-handoff draft so the executive can close the loop in seconds rather than composing one from scratch.
- **Waiting on Others (External Dependencies).** Deliverables promised *to* the executive by collaborators — such as Q3 campaign slide updates from Neha — are tracked with the same rigor, and TaskAlign surfaces one-click nudge dispatches when they run past their expected delivery window.
- **Target Deadlines.** Organization-level milestones with no individual owner yet assigned — for example, a Mumbai office lease renewal due Friday end-of-day — are elevated as they approach their due date, with inline delegation dropdowns so the executive can assign an owner without leaving the dashboard.

### 3. Dual-Path Dispatch Engine

Every actionable item in TaskAlign — a nudge, an apology draft, a delegation — resolves through one of two dispatch paths, chosen by the executive per action:

- **Silent Dispatch (Background Execution).** A single click sends the drafted communication via REST/SMTP under domain-wide delegated authentication, with no further interaction required. On success, TaskAlign automatically transitions the task state to `RESOLVED`, resets the item's overdue metrics, and raises a lightweight success toast.
- **Human-in-the-Loop Web Tab.** For messages the executive wants to review or personalize before sending, TaskAlign instead generates a parameterized `mail.google.com/mail/?view=cm` compose URI, pre-populated with the target contact (e.g. `chhavi.yadav91410@gmail.com`), subject line, and body — opening directly into a native Gmail compose tab for final review and manual send.

---

## Interactive Dashboard Navigation

The dashboard home view surfaces four primary stat cards, each acting as both a live metric and a navigation control:

| Stat Card | Metric Source | Target Tab | Pulse Keyframe |
|---|---|---|---|
| **Hard Overlaps** | Clash Detection Engine | Calendar | `blink-clash` |
| **Tight Gaps** | Proactive Buffer Calculation | Schedule | `blink-buffer` |
| **Waiting on Me** | Internal Commitment Ledger | Commitments & Blockers | `blink-waiting-me` |
| **Waiting on Others** | External Dependency Ledger | Commitments & Blockers | `blink-waiting-others` |

Clicking any stat card performs two actions in sequence: it switches the active view to the card's target tab, and it triggers a temporary, **3-cycle CSS keyframe pulse animation** on the corresponding element within that view — drawing the eye directly to the item(s) responsible for the metric, without requiring a manual search or filter.

```css
@keyframes blink-clash {
  0%, 100% { background-color: transparent; }
  50% { background-color: rgba(95, 3, 9, 0.18); } /* Vampire Hunter */
}

@keyframes blink-buffer {
  0%, 100% { background-color: transparent; }
  50% { background-color: rgba(217, 107, 0, 0.18); } /* Toasty Orange */
}

@keyframes blink-waiting-me {
  0%, 100% { background-color: transparent; }
  50% { background-color: rgba(95, 3, 9, 0.12); }
}

@keyframes blink-waiting-others {
  0%, 100% { background-color: transparent; }
  50% { background-color: rgba(56, 189, 248, 0.15); } /* Macaw Blue */
}
```

Each animation runs for exactly three cycles (`animation-iteration-count: 3`) before settling back to the surface's resting state, so the highlight reads as a deliberate cue rather than a persistent distraction.

---

## System Architecture

TaskAlign is composed of four decoupled tiers, connected by strongly typed contracts so that any tier can be scaled, replaced, or redeployed independently.

```
┌──────────────────────────────────────────────────────────────────────────┐
│  TIER 1 — CLIENT PRESENTATION (SPA Dashboard)                            │
│  HTML5 · Tailwind CSS · Syne (500/700) · Schedule · Calendar · Commit-    │
│  ments & Blockers · Transcripts                                          │
└───────────────────────────────┬────────────────────────────────────────┘
                                 │ REST + WebSocket
                                 ▼
┌──────────────────────────────────────────────────────────────────────────┐
│  TIER 3 — CORE BACKEND ENGINES & ORCHESTRATION                           │
│  ┌────────────────────┐ ┌───────────────────────┐ ┌────────────────────┐ │
│  │ API Gateway         │ │ Interval Arithmetic    │ │ Commitment FSM     │ │
│  │ /api/v1/schedule     │ │ Conflict Evaluator     │ │ Service            │ │
│  │ /api/v1/tasks        │ │                        │ │                    │ │
│  │ /ws/matrix           │ │                        │ │                    │ │
│  └────────────────────┘ └───────────────────────┘ └────────────────────┘ │
│                          ┌───────────────────────┐                       │
│                          │ Templated Dispatch     │                       │
│                          │ Orchestrator           │                       │
│                          └───────────────────────┘                       │
└───────────────────────────────┬───────────────────┬─────────────────────┘
                                 ▲                   │
                     EventDTO /  │                   │ Persistence
              ObligationDTO /    │                   ▼
              DependencyDTO      │   ┌──────────────────────────────────────┐
                                 │   │  TIER 4 — ENTERPRISE PERSISTENCE      │
┌────────────────────────────────┴──┐│  & INTEGRATIONS                       │
│  TIER 2 — INGESTION & MULTIMODAL   ││  PostgreSQL   → events, attendees,    │
│  NLP EXTRACTION PIPELINE           ││                 matrix slots          │
│                                     ││  MongoDB      → task docs, blocker    │
│  Sources: .vtt transcripts ·       ││                 chains, audit logs    │
│  RFC 822 MIME email · .ics /       ││  Pinecone /   → 1536-dim embeddings   │
│  CalDAV feeds · chat streams       ││  pgvector       for semantic context  │
│                                     ││                                        │
│  Pipeline: Tokenization → NER →    ││  Adapters: Google Workspace/Gmail API │
│  HeidelTime Temporal Normalization ││  (users.messages.send) · Microsoft    │
│  (relative → ISO-8601 UTC) →       ││  Graph API · Slack / Teams webhooks   │
│  Speaker Diarization → DTO Emit    ││                                        │
└─────────────────────────────────┘└──────────────────────────────────────┘
```

**Data flow summary:** Tier 2 ingests raw, unstructured sources and emits strongly typed DTOs (`EventDTO`, `ObligationDTO`, `DependencyDTO`) into Tier 3. Tier 3's engines evaluate, reconcile, and orchestrate against those DTOs, exposing results to Tier 1 over REST and WebSocket, while persisting relational, document, and vector state to Tier 4. Tier 4's external adapters close the loop by carrying dispatch actions back out to Gmail, Microsoft Graph, and Slack/Teams.

---

## Data Schemas & DTO Contracts

All cross-tier communication is defined through strongly typed Data Transfer Objects. The two foundational contracts are below.

```typescript
/**
 * EventDTO
 * Emitted by Tier 2 (Ingestion Pipeline) and consumed by the Tier 3
 * Interval Arithmetic Conflict Evaluator.
 */
interface EventDTO {
  id: string;                     // UUID v4
  title: string;
  startTime: string;              // ISO-8601 UTC, normalized via HeidelTime
  endTime: string;                // ISO-8601 UTC, normalized via HeidelTime
  attendees: AttendeeRef[];
  source: 'ics' | 'caldav' | 'manual' | 'inferred';
  calendarId: string;
  isClash: boolean;                // computed: Clash = (S_new < E_i) ∧ (E_new > S_i)
  bufferBeforeMinutes: number | null;  // null if ≥ 30 min, else transition window
  bufferAfterMinutes: number | null;
  embeddingId: string | null;      // pgvector / Pinecone reference for semantic context
}

interface AttendeeRef {
  userId: string;
  name: string;
  department: string;              // e.g. "Marketing", "Operations", "Finance"
  availabilityStatus: 'free' | 'busy' | 'tentative';
}

/**
 * ObligationDTO
 * Emitted by Tier 2 (NLP Extraction Pipeline) and consumed by the Tier 3
 * Commitment Finite State Machine Service.
 */
interface ObligationDTO {
  id: string;                      // UUID v4
  type: 'waiting_on_me' | 'waiting_on_others' | 'target_deadline';
  description: string;             // e.g. "Send Vendor List to Raghav"
  owner: AttendeeRef;               // who owes the deliverable
  beneficiary: AttendeeRef | null;  // who is owed it, if applicable
  sourceType: 'vtt_transcript' | 'email_thread' | 'chat_stream';
  sourceReference: string;          // pointer to originating document/segment
  deadline: string | null;          // ISO-8601 UTC, normalized via HeidelTime
  state: 'PENDING' | 'OVERDUE' | 'DISPATCHED' | 'RESOLVED';
  dependencyChainId: string | null; // links related ObligationDTOs (Tier 4 Mongo)
  dispatchDraft: DispatchDraft | null;
}

interface DispatchDraft {
  mode: 'silent' | 'human_in_the_loop';
  recipientEmail: string;
  subject: string;
  body: string;
  gmailComposeUri: string | null;   // populated only for human_in_the_loop mode
}
```

---

## Design Tokens & Visual Standards

### Typography

TaskAlign uses **Syne** (Google Fonts) as its sole display and UI typeface, restricted to two weights to keep letterform proportions consistent across the interface:

| Weight | Token | Usage |
|---|---|---|
| 500 | `font-syne-medium` | Body copy, labels, table cells, navigation |
| 700 | `font-syne-bold` | Headings, stat card figures, section titles |

### Elevation & Surfaces

Cards use high-contrast, cleanly elevated surfaces rather than hard borders — separation is communicated through shadow and background delta, not stroke:

```css
.surface-card {
  background-color: var(--surface-elevated);
  box-shadow: 0 1px 2px rgba(15, 15, 15, 0.04), 0 4px 12px rgba(15, 15, 15, 0.06);
  border-radius: 0.75rem;
  border: none;
}
```

### Color Tokens

| Token Name | Hex | Usage |
|---|---|---|
| **Vampire Hunter** | `#5F0309` | Hard clashes, `OVERDUE` commitment states |
| **Toasty Orange** | `#D96B00` | Buffer warning chips (`⚡ [X]m Buffer`) |
| **Macaw Blue** | `#38BDF8` | Informational states, "Waiting on Others" accents |
| **Emerald Green** | `#15803D` | Resolved states, success toasts, healthy metrics |

### Theme Toggle

The dark/light switch is a custom Bluetooth/iOS-style sliding toggle: a pill-shaped track containing a circular thumb that carries an inline SVG indicator — a sun glyph in light mode, a moon glyph in dark mode — animating smoothly between the two positions on state change.

---

## Repository Directory Layout

```
taskalign/
├── apps/
│   ├── dashboard/                  # Tier 1 — SPA Dashboard
│   │   ├── src/
│   │   │   ├── components/
│   │   │   │   ├── StatCards/
│   │   │   │   ├── CalendarMatrix/
│   │   │   │   ├── CommitmentLedger/
│   │   │   │   ├── TranscriptViewer/
│   │   │   │   └── ThemeToggle/
│   │   │   ├── hooks/
│   │   │   ├── styles/
│   │   │   │   └── tokens.css
│   │   │   └── App.tsx
│   │   └── tailwind.config.ts
│   └── api-server/                 # Tier 3 — Backend Engines
│       ├── src/
│       │   ├── routes/
│       │   │   ├── schedule.ts
│       │   │   ├── tasks.ts
│       │   │   └── matrix.ws.ts
│       │   ├── engines/
│       │   │   ├── conflictEvaluator.ts
│       │   │   ├── commitmentFSM.ts
│       │   │   └── dispatchOrchestrator.ts
│       │   └── server.ts
│       └── package.json
├── services/
│   └── nlp-pipeline/                # Tier 2 — Ingestion & Extraction
│       ├── ingestion/
│       │   ├── vttParser.py
│       │   ├── mimeParser.py
│       │   └── icsCalDavAdapter.py
│       ├── extraction/
│       │   ├── ner.py
│       │   ├── temporalNormalizer.py   # HeidelTime integration
│       │   └── speakerDiarization.py
│       ├── dto/
│       │   ├── event_dto.py
│       │   └── obligation_dto.py
│       └── requirements.txt
├── infra/
│   ├── postgres/
│   │   └── schema.sql
│   ├── mongo/
│   │   └── collections.json
│   ├── vector/
│   │   └── pgvector_init.sql
│   └── docker-compose.yml
├── integrations/
│   ├── gmail/
│   ├── microsoft-graph/
│   └── slack-teams/
├── docs/
│   └── architecture.md
├── .env.example
├── LICENSE
└── README.md
```

---

## Quick Start Guide

### Option A — Single-File UI Prototype

For evaluating the dashboard experience without standing up the backend:

```bash
git clone https://github.com/your-org/taskalign.git
cd taskalign/apps/dashboard

# Install dependencies
npm install

# Run in prototype mode (mocked DTO fixtures, no live backend required)
npm run dev:prototype
```

The prototype boots with fixture data covering all four dashboard views — Schedule, Calendar, Commitments & Blockers, and Transcripts — so every interaction, including stat card pulse navigation, can be exercised end-to-end.

### Option B — Full API & Database Backend

For a production-representative environment with live ingestion, persistence, and dispatch:

```bash
git clone https://github.com/your-org/taskalign.git
cd taskalign

# 1. Configure environment
cp .env.example .env
# Populate: DATABASE_URL, MONGO_URI, PINECONE_API_KEY (or PGVECTOR settings),
#           GOOGLE_WORKSPACE_CREDENTIALS, MS_GRAPH_CLIENT_ID/SECRET,
#           SLACK_BOT_TOKEN, TEAMS_WEBHOOK_URL

# 2. Start persistence tier
docker compose -f infra/docker-compose.yml up -d postgres mongo pgvector

# 3. Apply schema
psql "$DATABASE_URL" -f infra/postgres/schema.sql

# 4. Install and start the NLP extraction pipeline (Tier 2)
cd services/nlp-pipeline
pip install -r requirements.txt
python -m nlp_pipeline.run

# 5. Install and start the backend engines (Tier 3)
cd ../../apps/api-server
npm install
npm run start

# 6. Install and start the dashboard (Tier 1)
cd ../dashboard
npm install
npm run build
npm run start
```

Once all tiers are running, the dashboard will be available locally, with live WebSocket updates flowing from `/ws/matrix` as new transcripts, emails, and calendar events are ingested.

---

## License & Contributing

TaskAlign is released under the **MIT License** — see [`LICENSE`](./LICENSE) for the full text.

Contributions are welcome. Before opening a pull request:

1. Fork the repository and create a feature branch (`feature/your-feature-name`).
2. Ensure new backend logic (Tier 2/3) includes corresponding unit tests.
3. Run `npm run lint` and `npm run typecheck` in `apps/dashboard` and `apps/api-server` before submitting.
4. Follow the existing design token system when contributing UI — no ad-hoc colors or off-scale typography weights.
5. Open a pull request against `main` with a clear description of the change and its motivation.

For significant architectural changes (new tiers, new persistence stores, new external adapters), please open an issue first to discuss the proposal with maintainers.
