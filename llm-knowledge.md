# LLM Learner Knowledge Boundary

**Personal file. Do not commit or publish.** It describes one learner's machines, goals, and habits. The GitHub repository this folder belongs to is public, so keep this file out of commits (see "Open questions and corrections").

Last updated: 2026-11-18 (created from the Step 0 diagnostic and two follow-ups; profile split into `goals.md` and `hardware.md`).

## How to use

Handout authors and tutors read this file before assuming, explaining, or assessing a local-LLM idea. It records what the learner knows, uses, and has not met, so handouts neither re-teach nor assume wrongly. The learner's current explicit instruction overrides it.

Every item carries a depth marker:

| Marker | Meaning |
|---|---|
| `use` | Expected. May be read, applied, and reasoned about without re-teaching. |
| `read-only` | Expected when given. The learner interprets it but is not expected to produce it from memory. |
| `not expected` | Outside the boundary. Needs a plain-language explanation before first use and is never itself assessed until taught. |
| `avoid` | Outside the boundary, and an in-scope equivalent exists. Use the named equivalent. |

**Anything not listed is `not expected`.** An item marked `use` on the strength of one diagnostic answer is provisional until a handout's final question confirms it (note "provisional").

Tutoring may always answer a direct question about any item; the markers constrain what learner-facing material may assume, not what help a tutor may give.

## Learner profile

Moved to standalone files so each fact has one home:

- Goals, workload, budget, comfort, constraints: [goals.md](goals.md)
- Desktop, laptop, phone, network: [hardware.md](hardware.md)

Still unconfirmed and recorded under "Open questions and corrections" below. Does not use local models yet.

## Knowledge by theme

Diagnostic Part B (2026-11-18): 8 correct, 4 "Not sure", none wrong. Evidence column cites the question.

### Tokens and generation

| Item | Marker | Evidence |
|---|---|---|
| A model's size label ("7B") counts learned numbers (parameters) | `use` (provisional) | B1 correct, answered in own words with hedging |
| A token is a chunk of text: a word, part of a word, or punctuation | `use` (provisional) | B2 correct |
| One generation step repeats to make text; training versus inference | `not expected` | Not tested; taught in L01 |

### Weights and quantization

| Item | Marker | Evidence |
|---|---|---|
| File size from parameter count and bits per parameter | `not expected` | B3 "Not sure" |
| What quantization does and its trade-off | `not expected` | B4 "Not sure" |
| What GGUF is (a file format for weights) | `not expected` | B10 "Not sure" |
| Format versus quantization level | `not expected` | Not tested; taught in L02 |

### Memory and context

| Item | Marker | Evidence |
|---|---|---|
| A longer context needs more memory because the cache of past tokens grows | `use` (provisional) | B5 correct |
| KV cache formula; fit check (weights plus cache plus overhead, with headroom) | `not expected` | Taught in L03 |
| Usable memory is less than installed memory | `read-only` | Observed live in the desktop readings (41.5 of 64 GB and 4.4 of 8 GB in use); not yet formalized |

### Speed and bandwidth

| Item | Marker | Evidence |
|---|---|---|
| Generation speed is limited mainly by how fast weights can be read from memory | `use` (provisional) | B6 correct, but the first instinct was "RAM"; capacity versus bandwidth needs confirmation by the L04 final question |
| Prefill versus decode; time to first token | `not expected` | Taught in L04 |
| Decode ceiling from bandwidth ÷ file size | `not expected` | Used in `hardware.md` and the market snapshot as read-only illustrations |

### Dense and mixture-of-experts

| Item | Marker | Evidence |
|---|---|---|
| Reading "35B-A3B" as total versus active parameters | `not expected` | B7 "Not sure" |
| Why a mixture-of-experts model suits a machine with large RAM | `not expected` | Taught in L05 (raised priority) |

### Hardware

| Item | Marker | Evidence |
|---|---|---|
| Memory capacity is how much fits; bandwidth is how fast data moves | `use` (provisional) | B8 correct |
| Reading Task Manager for CPU, RAM, GPU, dedicated and shared GPU memory | `use` | Supplied the readings unaided |
| Unified memory, VRAM versus system RAM, and AMD versus NVIDIA support | `not expected` | Taught in L06 |
| Inventory routine for a second machine (modules in use, free disk, update status, battery report) | `not expected` | A task inside L06 |

### Models and cards

| Item | Marker | Evidence |
|---|---|---|
| A model hub hosts downloadable model files and model cards; Hugging Face is one | `read-only` | B11 correct; the learner was unsure whether files can be downloaded there |
| Size tiers, model kinds (base, instruct, reasoning, vision, embedding), reading a card | `not expected` | Taught in L07 |

### Comparing models

| Item | Marker | Evidence |
|---|---|---|
| Hubs and leaderboards exist and can be used to compare | `read-only` | B11 note |
| Benchmarks versus your own task; mini-evaluation; recall as the redaction metric | `not expected` | Taught in L08 |

### Software stack

| Item | Marker | Evidence |
|---|---|---|
| An inference engine runs the model; a chat app is what you type into | `use` (provisional) | B9 correct |
| Server, port, loopback versus network exposure, API compatibility | `not expected` | Taught in L09 |

### Harnesses and retrieval

| Item | Marker | Evidence |
|---|---|---|
| Hosted-model skills and agents (as a user) | `use` | Uses Claude Code skills routinely |
| Harness design, tool calls, structured output, retrieval | `not expected` | Taught in L11 |

### Local versus hosted

| Item | Marker | Evidence |
|---|---|---|
| "Local" does not guarantee data never leaves; it depends on the app and any tools making outside calls | `use` (provisional) | B12 correct |
| Hybrid routing and failure policies; break-even arithmetic | `not expected` | Taught in L12 |

### Private documents and redaction

| Item | Marker | Evidence |
|---|---|---|
| Direct versus quasi-identifiers; placeholders with a local mapping table; extract-then-apply; recall as the metric; leak checks | `not expected` | Taught in L17 |
| The learner's own identifier inventory (types and patterns only) | `not expected` | An exercise in L17; real values stay in an encrypted location outside this folder |

### Operating systems

| Item | Marker | Evidence |
|---|---|---|
| Windows daily use; basic Linux desktop use (Zorin, briefly) | `use` | Self-reported |
| Comparing Windows, Linux, and macOS for a local-LLM machine; Linux inside Windows; boot from a USB stick | `not expected` | Taught in L16 |

### Dedicated-machine operations and remote access

| Item | Marker | Evidence |
|---|---|---|
| Using a mesh VPN to reach machines (Tailscale) | `use` | Installed on most devices and used already |
| Binding a model server to loopback versus a network interface; authentication keys; power-loss recovery | `not expected` | Taught in L13 |

### Travel and phone

| Item | Marker | Evidence |
|---|---|---|
| On-device versus remote versus portable options; phone memory limits | `not expected` | Taught in L18 |

### Buying

| Item | Marker | Evidence |
|---|---|---|
| The memory-price shock and what the four budget tiers buy | `read-only` | `reference_sheets\market-snapshot.md`, 2026-11-18 |
| The buying routine, archetypes, used-market checks | `not expected` | Taught in L14 and L15 |

## Handout progress

Depth matches the curriculum table in `plan.md`. "Skim" means take only the final question as a self-test; a miss means taking the full handout. Handout 001 covers L01 and L02; items it teaches stay `not expected` until the learner reports a self-test result.

| ID | Package | Depth | Status | Date | Self-test result |
|---|---|---|---|---|---|
| L01 | 001 (combined with L02) | Skim candidate | Written; self-test pending | 2026-11-18 | |
| L02 | 001 (combined with L01) | Full (highest priority) | Written; self-test pending | 2026-11-18 | |
| L03 | none | Skim candidate | Not started | | |
| L04 | none | Skim candidate (self-test mandatory) | Not started | | |
| L05 | none | Full (raised priority) | Not started | | |
| L06 | none | Full (recommended override); includes the ThinkPad inventory task | Not started | | |
| L07 | none | Full (recommended override) | Not started | | |
| L08 | none | Full (recommended override) | Not started | | |
| L09 | none | Skim candidate | Not started | | |
| L10 | none | Full | Not started | | |
| L11 | none | Full | Not started | | |
| L12 | none | Full (recommended override; hybrid pattern is core) | Not started | | |
| L13 | none | Full (remote access core) | Not started | | |
| L14 | none | Full | Not started | | |
| L15 | none | Full | Not started | | |
| L16 | none | Full (new) | Not started | | |
| L17 | none | Full (new; may split into L17a and L17b) | Not started | | |
| L18 | none | Full (new) | Not started | | |

## Open questions and corrections

**Privacy of this file and folder**

- The repository `shipunicorn/llm` is **public** on GitHub. `plan.md`, `goals.md`, `hardware.md`, and this file describe one person's machines, budget, goals, and habits. None is committed yet. Decide before any commit whether to keep them out of the repository (an ignore rule), move them to a private location, or make the repository private.
- Real identifiers, document text, and transcriptions never belong in this folder (see `goals.md`, "Standing constraints").

**Facts to collect**

- ThinkPad: memory modules in use ("Slots used"), free disk space, Windows update status and whether Extended Security Updates is enrolled, battery wear (L06 task and the battery investigation).
- Is there a Micro Center within reach? It decides which deals in the snapshot apply to the learner.
- Which documents are redacted most often and which identifiers must always go (the L17 exercise).
- Whether the HP BIOS exposes the memory profile (RAM currently runs at 4000 MT/s against a rated 5600).
- The iPhone 17's actual memory and which local-model apps are acceptable (L18).

**Learner questions to answer in a later handout**

- Can model files be downloaded from a hub, and how do hub pages help compare models? (L07, L08)
- Why is memory bandwidth, not RAM size, the limit on speed, and how do the two relate? (L04, L06)
- Is a local model with a web tool still "local"? (L12, L17)

**Claims to verify at authoring** (from the `plan.md` handout sections and the snapshot)

- What "verifiable privacy" means for the hosted private service named in Tawsif's notes (L12).
- Windows privacy defaults and what Windows 11 features send out (L16).
- iPhone model memory, app behavior, and remote-access security properties (L18).
- The OpenAI Privacy Filter's license and accuracy; benchmark figures for the OCR models (L08, L17).

## Update protocol

Update this file:

1. After the diagnostic (done 2026-11-18).
2. After each handout. Mark newly taught items `use` only after a self-test result; record the handout ID, date, and result in "Handout progress".
3. Whenever the learner demonstrates or asks about something not yet recorded.
4. When the hardware inventory, budget, or travel pattern changes (for example after the ThinkPad inventory or a purchase).

Hardware or goal changes go in `hardware.md` or `goals.md`, not here. A short follow-up edit to the `llm-create-handout` skill should add `llm-knowledge.md`, `goals.md`, and `hardware.md` to its Read First table (not done yet).
