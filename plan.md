# Local LLM Self-Study Plan

A first-principles path for learning how local LLMs work, choosing software for the desktop already owned, and deciding what (if anything) to buy under $1200, including which operating system it should run. One handout at a time, each at most 16 printed pages.

Status: Step 0 (diagnostic, 2026-11-18) is done. Next: L01 (a skim candidate), alongside the ThinkPad inventory and battery investigation you run yourself. Refresh the market snapshot after Cyber Monday (2026-11-30).

## Shared documents

Read these before authoring any handout.

| File | Holds |
|---|---|
| [goals.md](goals.md) | Questions to answer, goals, workload, budget tiers, standing constraints and risks, decisions made |
| [hardware.md](hardware.md) | Desktop, laptop, phone, network; planning ceilings |
| [llm-knowledge.md](llm-knowledge.md) | What the learner knows, per-handout depth, progress |
| [sources.md](sources.md) | Source coverage maps for Tawsif's notes and the four web guides |
| `reference_sheets\market-snapshot.md` | Dated products, prices, leaderboards, budget-tier contents |
| `guides\` | Source material (read-only) |

## Folder layout

```text
C:\dev\llm\
  plan.md, goals.md, hardware.md, sources.md, llm-knowledge.md
  guides\                  source material (read-only)
  handouts\NNN-slug\       handout.md, answers.md, figures\, two PDFs (made by /llm-create-handout)
  reference_sheets\        formulas, glossary, runbooks, dated market snapshot
  exercise-bank\           optional standalone drills (memory-fit and speed arithmetic)
  textbooks\               optional longer sources
```

## Rules for every handout

1. **Skill and format.** Produce with `llm-create-handout`: concise teaching, interleaved worked examples, mixed tradeoff exercises (true/false, ALWAYS/SOMETIMES/NEVER, matching, ordering, short computations, at most five multiple choice), a very difficult final question, a separate answer key, verified PDFs, at most 16 pages (over the limit means split into parts). Package numbers (`001`, ...) are reserved by the skill's script; record them in the tracker.
2. **First principles; dependencies point backward only.** Use only ideas from earlier handouts or `llm-knowledge.md`. Define new terms in plain language before use.
3. **Mechanisms in handouts, products in the snapshot.** Name tools by role ("inference engine", "model hub", "chat front end"). State any figure a question needs as an `Assume that` sentence inside that question.
4. **Claims are hypotheses until checked.** Re-verify against primary sources (project docs, model cards, vendor spec sheets) at authoring, and recompute all arithmetic with `scripts\llm_check.py`. Nothing is run against a model in the exercises.
5. **Decisions, not trivia.** Exercises mostly ask "given this machine, this model, this constraint, what changes and what does it cost?" The recurring capstone is a change scenario: mark each cost statement.
6. **Hands-on steps are runbooks** in `reference_sheets\`, not handouts. Handouts teach why each step matters and how to judge the result.
7. **Depth.** A skim candidate means the learner takes only the final question as a self-test; a miss means taking the full handout. Update `llm-knowledge.md` after each handout (mark items `use` only after a self-test result).

## Reading paths

IDs are stable names, not positions: L16, L17, and L18 were added after Step 0 and are read before L14.

| Path | Choose when | Sequence |
|---|---|---|
| **Yours** (default) | Privacy-first, personal documents, deciding whether to buy and which OS | L01\*, L02, L03\*, L04\*, L05, L06, L09\*, L10, L07, L08, L11, L12, L17, L13, L16, L18, L14, L15, then `decision-memo.md`. \* = skim candidate |
| Full | Understand the field | L01 → L12, then L17, L13, L16, L18, L14, L15 |
| Run something this week | Software working on the current desktop | L01, L02, L06, L09, L10, then L03, L04. Read L17 before any real personal document goes in |
| Buy soon | A purchase decision is near | L01–L06, L12, L13, L16, L17, L18, L14, L15 |
| Pick models | Comparing models is the need | L01, L02, L05, L07, L08 |

Your path puts the first run (L10) early, with non-personal prompts only, so later handouts can use speeds measured on your own desktop.

## Curriculum at a glance

| ID | Slug | Title | Depends on | Depth |
|---|---|---|---|---|
| L01 | `what-is-a-local-llm` | What a local LLM is and what it is not | none | Skim candidate |
| L02 | `weights-and-quantization` | Weights, precision, and quantization | L01 | Full, highest priority |
| L03 | `context-and-kv-cache` | Context window and the KV cache | L02 | Skim candidate |
| L04 | `speed-and-bandwidth` | Prefill, decode, and memory bandwidth | L02 | Skim candidate; self-test mandatory |
| L05 | `dense-vs-mixture-of-experts` | Dense versus mixture-of-experts | L02, L03, L04 | Full, raised priority |
| L06 | `hardware-map` | Reading a spec sheet; inventorying your machines | L02, L03, L04 | Full |
| L07 | `model-families-and-model-cards` | Model families, sizes, kinds, model cards | L02, L05 | Full |
| L08 | `comparing-models` | Hubs, leaderboards, and your own mini-evaluation | L07 | Full |
| L09 | `the-software-stack` | Weights, engine, server, front end, harness | L01 | Skim candidate |
| L10 | `choosing-software-for-your-machine` | Software and a first model for the machine you have | L06, L09 | Full |
| L11 | `harnesses-tools-and-retrieval` | Harnesses, tool calls, structured output, retrieval (light) | L03, L09 | Full |
| L12 | `local-vs-hosted-vs-private` | Privacy, cost break-even, hybrid routing | L04, L09 | Full; break-even shortened |
| L13 | `running-a-dedicated-machine` | Operations, access, security, failure modes | L06, L09, L12 | Full; remote access core |
| L14 | `buying-under-1200` | Workload → memory → bandwidth → archetype | L03–L06, L12, L13, L16, L17, L18 | Full |
| L15 | `case-shopping-for-a-1200-box` | Case study (fact bank) | L14 | Full |
| L16 | `choosing-an-operating-system` | Windows, Linux, or macOS | L06, L09, L12, L13 | Full |
| L17 | `private-documents-workflow` | Transcribe, redact, check; leak points | L09, L11, L12 | Full; may split into L17a/L17b |
| L18 | `local-models-on-the-go` | Phone, remote access, portable hardware | L04, L06, L13, L16, L17 | Full |

L06, L07, L08, and L12 are skim candidates by the diagnostic rule but are recommended in full: each had one mapped answer testing a single definition, while the handouts carry decisions you need.

Closing deliverable (not a handout): `decision-memo.md`, a one-page recommendation for your desktop, budget, and use cases, written together after L15.

## Handout sections

Each section lists: what to teach (the chain of first-principles ideas), distinctions learners tend to collapse, worked examples, the final question, and sources. Numbers are for planning; `llm_check.py` fixes the real ones.

### L01: What a local LLM is and what it is not

- **Teach:** a language model maps text so far to a probability for each possible next piece; generation repeats that step. Text is split into **tokens**, not words. The model is a large set of learned numbers (**parameters**, or **weights**). Running it (**inference**) needs a program plus the weights plus memory. "Local" means those run on hardware you control.
- **Distinctions:** model (the numbers) versus program that runs it versus app you type into; training versus inference; local versus hosted; "open weights" versus "open source".
- **Worked examples:** tokenizing a short sentence by a toy rule and counting tokens versus words; one generation loop traced for three steps.
- **Final question:** a scenario mixing which part of the system lives where (weights, engine, interface) when a chat app talks to a remote service versus a local file.
- **Sources:** guide 02 (sections 1–3), guide 01 (what is a local LLM), guide 04 (glossary, realistic expectations).

### L02: Weights, precision, and quantization

- **Teach:** parameters × bytes per parameter = file size. Precision (16-bit, 8-bit, 4-bit) sets bytes per parameter. **Quantization** stores weights with fewer bits and some scale information, trading size and speed for a small accuracy loss that grows as bits fall. File **formats** are containers, not different models. Reading a model name: size, kind (instruct), quantization label.
- **Distinctions:** parameter count versus file size versus memory needed; format versus quantization level; "fewer bits" versus "smaller model".
- **Worked examples:** size of one model at three precisions; what a name such as `8B-instruct-Q4` tells you; why overhead makes it "about 4.5 GB", not exactly 4.
- **Final question:** a machine, a model, and two quantization choices where "pick the lowest bits" fails once context memory (previewed, stated as an `Assume that`) is counted.
- **Sources:** guide 04 (memory requirements, quantization), guide 03 (formats, quantization trade-offs), guide 02 (formats, quantization), tawsif stack sections 2 and 6 (bit widths).

### L03: Memory at runtime: context window and the KV cache

- **Teach:** while generating, the model keeps a per-token record of what it has read (the **KV cache**). Cache size grows linearly with context length, layers, and key/value width: `2 × layers × kv_heads × head_dim × tokens × bytes`. Total memory ≈ weights + KV cache + overhead. The **fit routine**: add the three, compare with usable memory, leave headroom.
- **Distinctions:** context window (a limit) versus context used; weights (fixed) versus cache (grows); advertised versus usable memory (system and display take some; some platforms cap what the GPU may use).
- **Worked examples:** cache size at two context lengths; a fit check that passes at a short context and fails at a long one; why architectures differ in bytes per token (Tawsif reports about 24 KB versus about 245 KB per token for two designs).
- **Your inputs:** usable versus installed memory is visible in your desktop readings ([hardware.md](hardware.md)).
- **Final question:** a change scenario on a fixed-memory machine (raise context, switch precision, add a second model); mark cost statements.
- **Sources:** tawsif stack sections 2 and 7 (context budget), guides 04 and 03 (VRAM needs).

### L04: Why generation is slow or fast

- **Teach:** reading a prompt (**prefill**) is compute-heavy and parallel; producing each new token (**decode**) must read essentially all active weights from memory once. So decode speed is bounded by `memory bandwidth ÷ bytes read per token`; faster arithmetic does not fix a bandwidth bound. **Time to first token** versus **tokens per second**. Partial offload (some layers on a slow memory pool) drags the whole token toward the slowest pool. Briefly: **speculative decoding** (a small draft model proposes, the large one checks) as one way around the bound, and why a feature may exist in one tool and not another.
- **Distinctions:** prefill versus decode; speed versus quality; bandwidth versus capacity; peak versus measured speed.
- **Worked examples:** upper-bound tokens per second from a bandwidth and a model size; why the same model is faster on a higher-bandwidth machine; the cliff when part of the model spills to system memory; measuring speed honestly (median of several runs, fixed reply length), echoing the sampling caveats in Tawsif's eval table.
- **Final question:** two candidate machines and one model; rank by decode speed, then time to first token for a long prompt, then say what changes when the model is swapped.
- **Sources:** guides 04 and 02 (GPU versus CPU), guide 03 (inference speed tips), tawsif stack section 7 (tokens per second, draft model, caching).

### L05: Dense versus mixture-of-experts

- **Teach:** a **dense** model uses all its parameters for every token. A **mixture-of-experts** (MoE) model holds many parameter groups but activates a few per token. Memory must hold the **total**; speed follows the **active** share. So MoE trades capacity for speed: a large total can still be fast on modest bandwidth. Name decoding: "35B-A3B" means roughly 35B total and 3B active.
- **Distinctions:** total versus active parameters; fast versus small; "fits in memory" versus "fast enough"; dense models cost more memory per token of context in some designs, per Tawsif.
- **Worked examples:** two models with the same total and different active counts, with memory and speed estimates for each; when a MoE model on mostly system memory is still usable; the same estimates for 64 GB of system RAM and an 8 GB GPU (stated as `Assume that` figures from [hardware.md](hardware.md)).
- **Priority:** high. Large system RAM suits this model class, and the diagnostic answer was "Not sure".
- **Final question:** a budget machine with plenty of fast system memory but a small GPU: choose between a dense and a MoE model for a stated job and justify each cost statement.
- **Sources:** tawsif stack sections 2 and 6, tawsif notes line 3 (a 26B model "may be a stretch"), guide 02 (model sizes).

### L06: Hardware map

- **Teach:** the four memory arrangements (system RAM with CPU; discrete GPU with its own VRAM; unified memory shared by CPU and GPU; small integrated or mobile chips) and what each means for capacity and bandwidth. Which spec-sheet numbers matter (memory capacity, memory bandwidth, GPU vendor support for the chosen software) and which are mostly marketing (clock speeds for this job, headline "AI TOPS", core counts). Non-NVIDIA GPUs and multi-GPU: possible, with software-support and complexity costs. Small devices (phone, laptop) as the low end of one scale.
- **Distinctions:** capacity versus bandwidth; VRAM versus system RAM; unified versus discrete; supported by the software versus merely installed.
- **Worked examples:** three fictional spec sheets reduced to the two numbers that matter; an inventory routine for a Windows desktop (CPU, RAM size and speed, GPU, dedicated GPU memory) feeding `llm-knowledge.md`; the OMEN order listing as a real spec sheet (about 25 lines, about 5 that matter; 8 GB AMD GPU and 64 GB RAM as the running example).
- **Your task:** run the inventory routine on the ThinkPad (memory modules in use, free disk, Windows version and update status, battery report) and record the results in `hardware.md`.
- **Final question:** a short inventory plus a model list; sort models into runs well, runs slowly, does not run, and name the limiting number for each.
- **Sources:** guide 04 (GPU requirements, non-NVIDIA, multi-GPU, no GPU), guide 03 (GPU memory, CPU and hybrid), guide 01 (VRAM), tawsif notes line 4, tawsif stack section 2.

### L07: Model families, sizes, kinds, and model cards

- **Teach:** what to expect by size tier (very small, small, medium, large) for chat, coding, extraction, and web-search-style tasks, as ranges to test, not promises. Kinds: base versus instruction-tuned, reasoning ("thinking"), vision, embedding, code. Why a reasoning model can return an empty reply when thinking uses the whole output budget (tawsif stack section 7). Reading a **model card**: size, license, training claims, recommended settings, context length, and what a card does not prove. Tiny specialized models (one that writes web pages) as a category. Confirm against the hub's own pages that a hub is where models are found, downloaded, and read about (the learner was unsure about downloading).
- **Distinctions:** size versus capability; base versus instruct; thinking tokens versus answer tokens; license versus availability.
- **Worked examples:** a model card read line by line for the facts that decide fit and fit-for-task; a thinking-budget calculation.
- **Final question:** three tasks and three candidate models of different kinds and sizes; assign each task and mark cost statements.
- **Sources:** guide 04 (small models, multimodal), guide 02 (model families), guide 01 (picking a model, model names), tawsif notes lines 2, 3, 6.

### L08: Comparing models honestly

- **Teach:** where comparisons live: model hubs (download plus card plus community pages), public leaderboards, human-preference arenas, vendor blog claims, community threads, and your own test. What each measures, who can game it, and what it ignores. Benchmark versus your task; contamination; sample size and noise; a rate without its denominator; **flip rate** (answers that change between identical runs). A **mini-evaluation**: 20–50 of your own prompts, fixed settings, repeated runs, a pass/fail rule written before looking. Q1 hypothesis to verify: a hub is the best place to find, download, and read about models, but its rankings need critical reading, so teach how to read comparisons rather than naming one winner.
- **Your inputs:** private-document tests use synthetic or redacted copies (or never leave the machine). The pass/fail rule counts wrong numbers and invented facts separately from style. For redaction the headline metric is recall (the share of real identifiers caught), precision second. Test transcription accuracy on your kind of document, including a local OCR or vision model that fits 8 GB of VRAM.
- **Distinctions:** popularity versus quality; benchmark score versus task fit; one run versus a stable result; an instrument bug versus a model difference (Tawsif's broken-instrument finding).
- **Worked examples:** two leaderboard rows read critically; why 3 of 3 correct on a tiny sample proves little; designing a five-prompt pilot.
- **Final question:** three conflicting sources about one model; decide what to trust and what to test, with cost statements.
- **Sources:** tawsif stack section 7, tawsif notes lines 2 and 13, guides 01 and 04 (model picks).

### L09: The software stack

- **Teach:** the five layers. **Weights file** → **inference engine** (loads weights, runs the math, may use GPU) → **server** (exposes a standard network interface) → **front end** (chat window, TUI, or web page) → **harness or agent** (adds tools and loops). One product can fill several layers (an all-in-one desktop app is engine plus server plus front end). Server basics: a **port**, the **loopback** address (this machine only) versus a LAN address, **API compatibility**, why runtimes keep **separate model catalogs**. A **wrapper** sits on a layer below it; its availability (waitlists, shutdowns) is a risk the engine layer does not have.
- **Distinctions:** engine versus app; app versus server; local binding versus LAN exposure; catalog versus loaded model.
- **Worked examples:** classify five products (named by role) by the layers they fill; trace one chat message down and back up the stack; what "connection refused" tells you at each layer.
- **Final question:** a client works from the host but not from a second computer; mark which layer and which setting.
- **Sources:** guide 02 (software stack, ecosystem), guide 03 (tools compared, REST API), guide 01 (UI options), tawsif notes lines 1 and 10, tawsif stack section 3.

### L10: Choosing software and a first model for the machine you have

- **Teach:** a decision routine. Inventory from `hardware.md` → largest model that fits with headroom (L03) → engine supported on your GPU or CPU → front end by comfort (GUI versus command line) → model download source → first-run checks (does it use the GPU, speed in range from L04, quality on one of your own prompts). Compare all-in-one desktop app, command-line runner, and low-level engine by effort, control, and update risk. Stop conditions: when to stay on the desktop, and when the numbers say a purchase is justified.
- **Your inputs:** the desktop and the ThinkPad are the two inventories, a natural contrast. The desktop's AMD GPU is a decision point (which engine backends can use it, which driver prerequisites apply, system RAM as fallback); verify at authoring. First runs use non-personal prompts only; real personal documents wait for L17.
- **Worked examples:** two inventories → two different first choices; diagnose "model runs but is slow" from a symptom table (spilled to system memory, wrong backend, context too large).
- **Runbook:** `reference_sheets\first-run-windows.md`, including optional steps matching your comfort: a short script that calls the local server, and one config-file edit.
- **Final question:** a stated desktop, a stated goal, and one wrong turn; mark each statement about what the better path costs.
- **Sources:** guides 01, 03, 04 (software sections), tawsif notes lines 2 and 10.

### L11: Harnesses, tool calls, structured output, and retrieval (light)

- **Teach:** a **harness** assembles the prompt, calls the model, runs tools the model requests, and manages context. A minimal harness leaves more of a small model's limited context and attention for the task. **Tool calls** and **structured output** (forcing a format), and why a reasoning model may fail at a schema yet succeed with a required tool call (tawsif stack section 7). **Retrieval** in one page: split documents into chunks, find the closest by similarity, add them to the prompt; the context budget limits how many fit. **Web access** is a tool the harness provides, not a model ability. Applying retrieval to private documents is L17.
- **Distinctions:** model versus harness; tool call versus text; retrieved context versus training knowledge; local model with web search versus "offline".
- **Worked examples:** a prompt-budget calculation for retrieval (chunk size × count + instructions + reply); a harness trace with one tool call.
- **Final question:** a small model fails a task in one harness and passes in another; identify the changed factor among context use, tool format, and effort setting.
- **Sources:** tawsif notes lines 2, 8, 10; tawsif stack sections 7–8; guide 04 (RAG, internet access); guide 03 (RAG architecture, concept only).

### L12: Local, hosted, and private-hosted

- **Teach:** compare four options on privacy, cost, capability, reliability, latency, and effort: all-local; hosted frontier service; private-hosted service that claims verifiable privacy; rented GPU. What "local" does and does not guarantee (an app or tool may still call out). **Break-even arithmetic** (hardware cost amortized over use versus per-token fees, with electricity), kept short. The **hybrid pattern**: local for private or cheap tasks, hosted for hard ones, with a stated failure policy (fall back to hosted, show an error, or skip) per task. Tawsif's finding that most failures are missing guards or capability gaps, not speed.
- **Your inputs:** privacy is the first goal, so the privacy comparison gets the space. The hybrid pattern is central: local transcription and redaction in front of hosted models, with "never send an unredacted personal document" as the proposed failure policy. The routing table's strictest rule concerns unredacted documents only; you confirm or change it in the handout. Raise whether private documents need a separate machine or a carefully set-up desktop; L17 works it through.
- **Distinctions:** private versus offline; cheaper per call versus cheaper overall; capability gap versus configuration fault; fail open versus fail closed.
- **Worked examples:** a break-even calculation with `Assume that` prices; a routing table for five tasks with failure policies.
- **Verify at authoring:** what "verifiable privacy" means for the service named in Tawsif's notes, against its own documentation.
- **Final question:** a usage pattern that flips the break-even; mark each statement.
- **Sources:** guides 03 and 04 (why local), guide 02 (cloud versus local), tawsif stack sections 1 and 4, tawsif notes lines 1 and 12.

### L13: Running a dedicated machine

- **Teach:** what changes when a machine serves other devices: headless operation, remote access, who can reach the server (loopback versus LAN versus VPN), authentication keys, and what happens after a power loss (a disk-encryption unlock screen can block a headless restart). Memory discipline: one large model at a time, a watchdog or limit to stop a runaway load, checking what is actually loaded rather than what is in the catalog. Heat, noise, idle power, updates. A symptom-to-first-check troubleshooting table. A pros-and-cons list tied to concrete requirements.
- **Your inputs:** personal use, so serving other people is out of scope. Because you travel, reaching the machine from outside your home is core: remote access, who can reach the server, power-loss recovery, whether the machine can stay on or be woken while away. The running example is a small always-on home machine reached from your phone and laptop over the mesh connection you already use. Service administration is not assumed, so stay step by step.
- **Distinctions:** catalog versus loaded; steady-state versus load-moment memory; client problem versus server problem; convenience versus exposure.
- **Worked examples:** a co-load freeze reconstructed from memory arithmetic; a symptom-table drill.
- **Final question:** a household-server scenario with three requirements; choose bind address, access method, and loaded-model policy, and mark each consequence.
- **Sources:** tawsif stack sections 2, 3, 5, 8, 9; guide 03 (monitoring, Docker and reverse proxy: concept level only); guide 04 (multi-GPU, no GPU).

### L14: Buying under $1200

- **Teach:** a buying routine. **Workload** ([goals.md](goals.md)) → **target model tier** (L07) → **memory needed** (L02, L03, with headroom) → **bandwidth needed** for acceptable speed (L04) → **archetypes** that meet both under budget → **total cost** (power, case, PSU, cooling, storage, shipping, tax, returns) → **risk** (used-market condition, driver and software support, warranty, resale) → **decision or deferral**.
- **Archetypes** (categories; dated prices stay in the snapshot; re-check the list against the market at authoring): upgrade the desktop you own; a used-GPU tower; a small unified-memory desktop; a mini PC with a large shared-memory pool; a many-channel system-memory build aimed at MoE models; a portable machine; and "do not buy: test on the desktop, or use a hosted option".
- **Hypothesis to test, not assume** (from [hardware.md](hardware.md)): the desktop already has capacity for small dense models on the GPU and large MoE models in system RAM; its gaps are 8 GB of VRAM and AMD software support. So "upgrade the desktop" (a GPU swap within the 850 W supply and the case) and "do not buy: test first" are evaluated before any new machine, and a new machine must beat both. A separate machine can also be justified by isolating private data (L17). Check whether replacing parts affects the HP warranty.
- **Configurations to compare:** (A) keep what you have: desktop at home, ThinkPad fixed with a new battery and a supported operating system, iPhone for on-device use; (B) add an always-on home machine (a Mac mini class, for example) reached from laptop and phone over the mesh connection you already use; (C) replace the laptop with a new portable machine that is both travel laptop and local machine. For C, weigh one device always with you that works offline against heat, fan noise, battery life under sustained load, and no later upgrades. For B, weigh low idle power and a machine you can leave on against needing a connection on the road. The workload needs only a small model tier, so capability alone is unlikely to justify a purchase; the reasons would be travel, a laptop at the end of its battery life, or isolation of private data.
- **Budget:** $1200 all-in is the ceiling; show the four tiers ($600, $900, $1200, $1500). For each tier state what the money buys across the archetypes, what it cannot do (largest model that fits, expected speed range, suitability for travel), and what the next tier adds. Tier contents come from the dated snapshot. Do not plan to buy RAM (price shock, [goals.md](goals.md)).
- **Inputs from other handouts:** the operating system (L16) restricts archetypes (macOS exists only on Apple hardware); the workload (L17) adds long documents and scanned-page reading; travel (L18) adds portable hardware, remote access, and on-phone models as alternatives to one box; the battery investigation result decides "fix the old laptop or buy new".
- **Distinctions:** capacity versus bandwidth purchase; new versus used risk; upgradeability versus size; buying for today's model versus a class of models.
- **Worked examples:** two candidate builds scored against the same workload; the same workload walked up the four tiers; a used-market checklist applied to a listing.
- **Final question:** a change scenario (price rises, a model grows, a requirement is added) over a chosen build; mark each statement.
- **Sources:** guide 04 (hardware, multi-GPU, no GPU), guide 03 (recommended configurations), tawsif notes line 4, tawsif stack section 2.

### L15: Case study: shopping for a dedicated machine

- **Format (case profile):** a brief case; one **fact bank** with ID-labeled rows (specs, prices, model sizes, speeds, all dated and stated as assumptions); worked exercises citing fact IDs; closing exercises citing IDs with deliberate distractors.
- **Case:** uses your real intake where possible: the desktop ([hardware.md](hardware.md)), a privacy-first document workload with travel, and a $1200 all-in ceiling shown within the four tiers. Ends with the inputs for `decision-memo.md`.
- **Final question:** choose among three candidate builds under conflicting facts, citing fact IDs and naming what the facts do not say.

### L16: Choosing an operating system: Windows, Linux, or macOS

- **Teach:** the operating system sits between hardware and engine and changes six things:
  1. **Hardware and driver support:** which GPUs the engine can use on each system (AMD and NVIDIA differ by system); unified memory exists only on Apple hardware, so choosing macOS also chooses the hardware archetype.
  2. **Software availability:** apps and engines mostly exist on all three, but some tools arrive first or only on one; checked per tool in the snapshot, not argued here.
  3. **Speed:** decode is bandwidth-bound (L04), so the system usually changes speed little; backend support and overhead (the display using GPU memory) do.
  4. **Privacy surface:** what the system sends out (telemetry, account sign-in, cloud sync, built-in assistants and activity features), who controls those settings, how you check. Privacy of the system and of the model are separate questions.
  5. **Operation and effort:** install and update routines, headless and remote access (SSH, remote desktop), disk-encryption unlock at boot (L13), troubleshooting skill needed.
  6. **Cost and reversibility:** license cost, and whether you can test before committing (live USB, dual-boot, Linux inside Windows).
- **Your inputs:** Windows daily; brief Zorin use (an Ubuntu-family system, to verify, so some knowledge transfers); copy-paste commands and small scripts. The iPhone cannot be the machine's system; it is a client of a home machine and, separately, runs small models (both in L18). You are considering an always-on Mac mini reached over the mesh connection, so weigh macOS for an always-on remote machine and for a portable machine as separate cases.
- **Distinctions:** operating system versus engine; native Linux versus Linux inside Windows; iOS (phone) versus macOS (desktop); best system versus best fit for your skills; privacy of system versus model; a measured difference versus a forum claim.
- **Worked examples:** a weighted decision matrix (privacy surface, setup effort, GPU support for the candidate hardware, remote use, cost) scored for three fictional buyers with different priorities; one model on one machine under two systems, showing which install steps differ; what you can measure on your own desktop before buying (does a Linux live session see the GPU, which backends work, tokens per second versus Windows).
- **Runbook (optional):** `reference_sheets\try-linux-on-your-desktop.md`: boot a live USB session without installing. Starts with a safety step: check whether disk encryption is on and save the recovery key before any boot-related change.
- **Verify at authoring** (against primary sources, for current system versions): engine and GPU-backend support per system for AMD cards; what each system sends out by default and how to change it; what Windows features mean for privacy-sensitive use.
- **Final question:** a buyer scenario (AMD-GPU tower, privacy-first, on-demand use) with three change events (the GPU vendor changes, a remote-access requirement appears, the system vendor adds a cloud-connected feature); mark each cost statement for all three systems.
- **Sources:** guide 01 (Linux and AMD driver steps, comparison level only), guide 04 (non-NVIDIA GPUs), guides 02 and 03 (tools compared, platform support), tawsif stack sections 2 and 3, tawsif notes lines 2 and 4.

### L17: Private documents: transcribe, redact, and share safely

Privacy is the first goal and personal documents the main task; hosted models are acceptable once identifiers are removed, so the local model's main job is transcription and redaction ([goals.md](goals.md)).

- **Teach (eight links):**
  1. **Threat model first:** from what are you protecting the data? A hosted service's logs and training; other people using the machine; malware; a stolen drive; backups that sync to the cloud; the AI app itself.
  2. **Leak points on an otherwise local setup:** app or tool network calls (update checks, telemetry, web-search tools), chat history and search indexes saved in plain text, cloud-synced folders, backups, clipboard and logs, browser front ends and extensions, plugins. The encrypted cloud vault is a special case: uploaded files are ciphertext, but while the vault is unlocked any program can read them, and an index or extracted-text file stored outside the vault is a plaintext copy. Travel adds public Wi-Fi, theft or loss, and device inspection (L18).
  3. **Verification routines:** what you can check (an offline test with the network disabled, a firewall rule, watching connections) and what you cannot.
  4. **Isolation options:** a separate user account; the same desktop kept offline; a separate machine. What disk encryption protects (a stolen drive) and what it does not (a running, compromised system). Per-task failure policy from L12: unredacted personal documents are never routed to a hosted service (fail closed).
  5. **Transcribe:** text PDFs and scanned pages; scans need OCR, either a classic engine or a vision model that fits your hardware. Keep the original and the transcription locally, inside the encrypted vault where appropriate.
  6. **Redact:** direct identifiers (names, tax and account numbers, addresses, dates of birth, contact details) versus quasi-identifiers that re-identify in combination (employer, rare dates, unusual amounts); what the task needs kept (tax advice needs amounts, not names). Methods: rules for fixed patterns, a small model for names and context, and both combined. **Placeholders with a local mapping table:** replace a name with a tag, keep the table locally, and restore real values into the hosted reply on your machine. **Extract-then-apply:** the model lists what to redact and a script applies the list, so the model never rewrites numbers and generation stays short.
  7. **Check before sending:** judged by recall (a missed identifier is a leak), precision second. A deterministic leak check searches the redacted text for every identifier you know from your own records; you review the redacted version; a failed check stops the send (fail closed); keep a log of what was shared. Redaction cannot be proven complete, so state what the check does not cover.
  8. **Local-only questions and checking answers:** retrieval-based question answering with quoted source passages, within the context budget (L03). A model reading a tax form can misread a number or invent a field, so require a quote and page reference, recompute any figure that matters, and treat the model as a second reader, never the authority. For health records the model is a reading aid, not medical advice.
- **Distinctions:** private versus offline; encrypted at rest versus in use; local model versus local app plus tools; answer from the document versus from training; convenience (cloud sync) versus exposure.
- **Worked examples:** a leak-point audit of a described setup; a retrieval prompt budget for a 10-page document; "the model says $X": verifying it against the source passage; building a redacted test set for L08; redacting a fictional tax form with a placeholder table and a round trip through a hosted reply; measuring recall on a test set; your current workflow (scanned pages transcribed by a hosted model) as the "before" case and what a local replacement must match.
- **Your exercise:** build an identifier inventory. For each document type you handle (tax forms, bank statements, health records, identity documents), list the direct identifiers to always remove, the quasi-identifiers to review, and what to keep, as types and patterns only, never real values. The real values the leak check needs live in an encrypted location outside this folder.
- **Runbooks:** `reference_sheets\privacy-checklist.md` (checks on a real setup, including the offline test); `reference_sheets\redaction-pipeline.md` (build the transcribe, redact, check, share pipeline with a short script; test on fictional documents first).
- **Final question:** a described pipeline (scan, local transcription, redaction, hosted model) with a cloud-synced working folder and a web-search tool enabled; mark each leak and cost statement and choose a fix for each leak.
- **Length:** if the handout would exceed 16 pages, split into L17a (threat model, leak points, isolation) and L17b (transcribe, redact, check, local-only questions).
- **Sources:** tawsif notes lines 8, 10, 12; tawsif stack sections 1 and 4; guide 03 (RAG concept); guide 04 (RAG, internet access, multimodal); guide 02 (cloud versus local).

### L18: Using local models while traveling

Travel is a big use case and the iPhone should be involved.

- **Teach:** three ways to have a local model on the road, compared on privacy, capability, speed, battery and power, connectivity, and effort:
  1. **On-device on the phone** (iPhone 17; memory to verify): runs with no network, so it works in flight, and data stays on the device if the app makes no outside calls (L12). Phone memory capacity and bandwidth (L02–L04) set a small model size. Battery, heat, and app availability limit it further.
  2. **Remote access to a home machine:** the phone or laptop sends requests over a private connection (L13). You already use a mesh VPN, so likely the lowest effort, but it needs an always-on machine (the current desktop is not one). Full home-machine capability, but it must be on or wakeable, home internet must stay up, access must be secured (never an open public port), and you need a connection on the road. Hotel and airport networks are untrusted.
  3. **A portable machine:** cost, weight, battery, heat, loss or theft; the data travels with you. The ThinkPad is the baseline ([hardware.md](hardware.md)): its limits are battery, operating system support, and disk, not capability.
  - Plus a **hosted fallback** for non-private tasks, using L12's failure policy.
- **Chain:** travel profile first (trip length, connectivity, power, tasks on the road, which data you must carry, what can stay home) → choose a combination → size a model for each device (L02–L04) → privacy rules while away (L17) → failure and fallback for each case. Text requests need little bandwidth, so remote access works on most trips; on-device matters mainly for flights and outages; untrusted networks make the private connection and the redaction gate matter more.
- **Distinctions:** on-device versus remote; offline versus private; a phone app that runs the model locally versus one that is only a client of a remote service (the data leaves the phone); phone memory capacity versus bandwidth; "one box" versus "home machine plus phone".
- **Worked examples:** a fit and speed estimate for a small model on a phone using `Assume that` memory and bandwidth figures; a table of situations (flight, hotel Wi-Fi, mobile data abroad, home internet outage) against the three options, marking what works; a checklist for leaving home with personal documents (what to carry, what stays encrypted at home).
- **Runbooks:** `reference_sheets\phone-first-run.md` (install a local-model app, load a small model, airplane-mode test, measure speed and battery); `reference_sheets\remote-access-home-machine.md` (reach the home machine over a private connection, test from outside the home network, confirm nothing is exposed publicly).
- **Verify at authoring:** iPhone 17 memory and the model sizes it allows; which local-model phone apps exist and whether they make network calls; remote-access options and their security properties; rules on device inspection at borders (vary by country; use official sources and state cautiously).
- **Final question:** a two-week trip with three events (home internet outage, phone lost, a new need to question a 100-page document); mark each cost statement for all three options.
- **Sources:** tawsif notes lines 2, 4, 10; tawsif stack sections 2 and 3; guides 04 and 02 (small models, no-GPU operation).

## Reference sheets (not handouts)

| Sheet | Purpose | Created after |
|---|---|---|
| `formulas.md` | One page: file size, KV cache, fit check, decode-speed bound, break-even | L04 |
| `glossary.md` | Single glossary; handouts define terms inline and the sheet lists them | grows from L01 |
| `first-run-windows.md` | Runbook: install a desktop app, download a first model, chat, start the local server, check GPU use | L09 |
| `market-snapshot.md` | Dated: model families, hubs and leaderboards, software, hardware archetypes with prices, used-market checks, AMD engine support per OS, Apple hardware within budget, portable machines, what each budget tier buys, iPhone local-model apps | v1 done; refresh before L08, L10, L14, L15, L16, L18 |
| `privacy-checklist.md` | Runbook: leak-point checks on a real setup, including the offline test | L17 |
| `redaction-pipeline.md` | Runbook: transcribe, redact, check, share pipeline with a short script | L17 |
| `try-linux-on-your-desktop.md` | Optional runbook: Linux live USB, GPU recognition, one model; starts with the disk-encryption recovery-key check | L16 |
| `phone-first-run.md` | Runbook: local-model app on the iPhone | L18 |
| `remote-access-home-machine.md` | Runbook: private remote access to the home machine | L18 |
| `laptop-battery-investigation.md` | Side investigation, yours to run now; feeds L14. Steps: (1) measure wear with the Windows battery report (design versus full-charge capacity); (2) rule out software drain (background apps, the cellular modem, power settings, unsupported Windows 10); (3) find Lenovo's hardware maintenance manual for the L15 Gen 2 (is the battery user-replaceable, part number, the safety step to disable the built-in battery before opening); (4) compare genuine battery plus your time against a replacement machine | now |
| `decision-memo.md` | Recommendation for your machine and budget | after L15 |

Market-snapshot rule: every row carries an "as of" date and a source URL read that day; rows older than three months are re-checked before use. The snapshot is researched with web search at authoring time, not recalled.

## Status tracker

Rows follow the full-path order. Record package numbers here once reserved.

| Step | Status |
|---|---|
| Step 0 diagnostic; `llm-knowledge.md`; `market-snapshot.md` v1 | [x] 2026-11-18 |
| `laptop-battery-investigation.md` (yours) | [ ] |
| L01 + L02 (combined as package 001, 11 pages) | [x] 2026-11-18; learner self-test pending |
| L03 | [ ] |
| L04 | [ ] |
| `formulas.md` | [ ] |
| L05 | [ ] |
| L06 | [ ] |
| L07 | [ ] |
| L08 | [ ] |
| L09 | [ ] |
| `first-run-windows.md` | [ ] |
| L10 | [ ] |
| L11 | [ ] |
| L12 | [ ] |
| L17 | [ ] |
| `privacy-checklist.md`, `redaction-pipeline.md` | [ ] |
| L13 | [ ] |
| L16 | [ ] |
| `try-linux-on-your-desktop.md` (optional) | [ ] |
| L18 | [ ] |
| `phone-first-run.md`, `remote-access-home-machine.md` | [ ] |
| L14 | [ ] |
| L15 | [ ] |
| `decision-memo.md` | [ ] |

Follow-up: add `C:\dev\llm\llm-knowledge.md`, `goals.md`, and `hardware.md` to the `llm-create-handout` skill's Read First table (not done yet).
