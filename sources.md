# Source Coverage Maps

Used by every handout author. Each author reads the relevant source bodies in full before drafting and cites only what was read. Coverage was scoped from the guides' headings. Dated or vendor-written guides (01 comes from a hardware-matching vendor; 03 targets 2025) are claims to verify against primary sources, not authority.

Citation shorthand in `plan.md`: "guide 01–04" are the four web guides in `guides\`; "tawsif notes" is `tawsif_notes.txt` (by file line); "tawsif stack" is `tawsif How the local LLM stack works.md` (by section).

## tawsif_notes.txt

Transferable parts only. The notes describe one person's Mac Studio setup and personal wiki; extract the principles (unified memory, runtimes, guards, routing, evals) and skip the personal infrastructure.

| Line | Content (paraphrased) | Concept to teach | Handout |
|---|---|---|---|
| 1 | A product is a "wrapper" and is still invite-only | Wrapper versus engine layers; availability risk | L09, L12 |
| 2 | A desktop app works on Windows; a small, smart model for web-search tasks, evaluated many times | Engine plus server; small-model fit; evals as evidence | L09, L07, L08 |
| 3 | More is possible; some small models from one lab; a 26B model "may be a stretch"; ask your own AI | Fit routine; model families; limits of AI-generated advice | L03, L07, L10 |
| 4 | Smallest Mac Studio (96 GB), a Mac Air, even phones; "you don't need expensive hardware" | Unified memory; capacity and bandwidth, not price, set what runs | L06, L14, L16, L18 |
| 6 | A tiny model that writes HTML, on a model hub | Specialized small models; model cards | L07 |
| 8 | Use a minimal harness for small models | Harness design | L11 |
| 10 | Desktop app as engine and server; a 1.7B model; download from hub or app; TUI instead of GUI; a multi-harness GUI | Stack layers; front-end types; model sources; harness versus front end | L09, L10, L11 |
| 12 | Private AI with verifiable privacy and no hardware | Private-hosted option; claim verification | L12 |
| 13 | Run evals to see how much can be switched over | Evaluation before switching | L08, L12 |

## tawsif How the local LLM stack works.md

| Section | Transferable concepts | Handout |
|---|---|---|
| 1 Short version | Hybrid split; reasons; failures from guards, not speed | L12 |
| 2 Hardware | Usable versus advertised memory; wired-memory cap; MoE fit; per-token cache; boot-time unlock and headless restart | L03, L05, L06, L13, L16 |
| 3 Runtimes and lanes | Server basics; separate catalogs; ports; loopback; tunnels; one big model at a time | L09, L13, L18 |
| 4 Request routing | Routing policy per task: config switch, safety checks, fail open, fail closed, local only, never local | L12, L17 |
| 5 Standing rules and guards | Co-load freeze; watchdogs; residency checks; load-moment hazard | L13 |
| 6 Which models run what | Sizing models to roles; reading a speed number with its conditions | L07, L04 |
| 7 What the evals found | Broken instruments; denominators; flip rate; thinking off; JSON failures; draft models; caching | L08, L07, L11, L04 |
| 8 Coding lanes | Harnesses; throughput versus latency | L11, L13 |
| 9 When it breaks; where docs disagree | Symptom-to-check table; trust code and dated notes over stale docs | L13, L08 |
| 10 Where to read more | Personal wiki paths; not accessible | excluded |

Excluded as personal infrastructure: wiki-surface names, vault and journal specifics, lane port numbers, owner policies and dates. They appear only as worked-example material when a principle needs a concrete case, stripped of names.

## The four web guides

| Guide | Include | Light touch | Exclude |
|---|---|---|---|
| 01 LLM Configurator beginner guide | VRAM concept; model names; glossary; AMD driver steps (the learner's GPU is AMD) | First install and first model (runbook); common troubleshooting | Linux steps, except at comparison level in L16 and the optional trial runbook |
| 02 local-llm.net | What an LLM is; inference; formats and quantization; hardware basics; model sizes; misconceptions (as true/false items); ecosystem categories | Getting-started steps | none |
| 03 SitePoint developer guide | Why and when local beats cloud; GPU memory; formats (GGUF, GPTQ, AWQ, EXL2); tools compared; REST API idea | RAG concept (feeds L17); speed and troubleshooting tips; monitoring idea | Docker Compose, GPU container toolkit, reverse proxy, rate limiting, code listings, Modelfiles |
| 04 TechTactician beginner guide | Realistic expectations; GPU requirements; non-NVIDIA and multi-GPU; CPU-only; quantization; small starter models; software choices | Multimodal (scanned documents, L17); RAG; internet access (a leak point in L17) | Role-play front ends and character apps |
