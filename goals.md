# Learner Goals and Constraints

**Personal file. Do not commit or publish.** The GitHub repository for this folder is public.

Used by every handout author and by `decision-memo.md`. Hardware is in [hardware.md](hardware.md). What the learner knows is in [llm-knowledge.md](llm-knowledge.md). Intake answers date from 2026-11-18.

## Questions the folder must answer

| # | Question | Handouts |
|---|---|---|
| Q1 | Where do I compare the models available for local use? Is Hugging Face the place? | L07, L08 |
| Q2a | Which software runs easily on my existing machine, given its specs? | L06, L09, L10 |
| Q2b | How do I pick a new machine under $1200 for specific models? | L03–L06, L14, L15 |
| Q3 | What are the pros, cons, and things to know about running a dedicated LLM machine? | L12, L13 |
| Q4 | Explain, step by step, everything in `guides\tawsif_notes.txt` and `guides\tawsif How the local LLM stack works.md` | [sources.md](sources.md) |
| Q5 | Cover only the minimally relevant parts of the four web guides | [sources.md](sources.md) |
| Q6 | Should the machine I buy run Windows, Linux, or macOS? ("ios" was read as macOS; the iPhone is covered in L18) | L16 |
| Q7 | How do I use a local model to transcribe and redact personal documents (taxes, health) so I can share them safely with hosted models, and check the result? | L11, L12, L17 |
| Q8 | What can I run on my iPhone, and how do I use local models while traveling? | L18 |

## Goals

- **Why local:** first, privacy of the data sent out; second, learning how it works and tinkering.
- **Main job:** handling personal data (taxes, health, personal knowledge). A small local model transcribes scanned images and text PDFs, keeps the original or its transcription, and produces a PII-redacted copy to share with hosted models. A small local model also answers questions about material that never leaves. Today scanned pages are transcribed with skills or hosted models; a local replacement must fit in 8 GB of VRAM or run slowly from RAM.
- **Quality bar:** somewhat worse than the best hosted models is fine if the data stays private. A missed identifier in a redaction is a leak, and wrong numbers are not acceptable.
- **Hosted models:** comfortable using them, even for tax advice, once identifiers are removed. Nothing is off-limits after redaction. Never send an unredacted personal document is the proposed default; L12 decides whether unredacted sharing is ever acceptable.
- **Use pattern:** personal use, when turned on; serving other people is out of scope. Travel is a big use case, and the iPhone should be involved. Remote access from the road is core.
- **Comfort:** copy-paste commands from a guide; small scripts and config-file edits. Service and network administration is not assumed, so runbooks stay step by step. Already uses Tailscale, Proton VPN, Cryptomator, Signal, 1Password, Firefox, Chrome, VS Code, Claude Code with custom skills, and hosted chat models. Windows daily, Linux (Zorin) briefly, macOS experience not stated. Does not use local models yet.

## Budget

$1200 all-in (machine, parts, tax, shipping) is the decision ceiling. L14 and L15 show a four-tier ladder: $600, $900, $1200, $1500. The $1500 tier shows what a stretch would buy and is not a recommendation to exceed $1200. Tier contents come from the dated `reference_sheets\market-snapshot.md`, never from handouts or this file.

## Standing constraints and risks

- **Public repository.** `shipunicorn/llm` is public. This file, `hardware.md`, `llm-knowledge.md`, and `plan.md` describe one person's machines, budget, and habits and are not committed. Decide before any commit whether to add ignore rules, move them to a private location, or make the repository private. Real identifiers, document text, and transcriptions never go in this folder; keep them in an encrypted location elsewhere.
- **The desktop may already be enough.** No handout may presuppose a purchase. The memo may say "do not buy" or "upgrade the GPU".
- **Memory prices are in a shock** (a 64 GB DDR5 kit about $870 to $1,360 on 2026-11-18). Do not plan to buy RAM.
- **Redaction cannot be proven complete.** A missed identifier, or several quasi-identifiers that identify together, can still leak. Handouts teach checks and review and state what they do not cover; never promise "safe".
- **Travel with personal documents** adds theft, loss, public-network, and device-inspection risks. Some depend on the country; any legal claim needs an official source.
- **Phone-side claims** (which models and apps run which sizes, battery and heat cost) change quickly; L18 verifies at authoring.
- **AMD GPU on Windows** has narrower, faster-changing software support than NVIDIA. Verify at authoring (L06, L10, L16).
- **What each operating system sends out by default** is a privacy claim that changes by version; L16 and L17 check primary sources.
- **Tawsif's setup is a Mac; the learner uses Windows.** Unified-memory machines are one archetype among several, not the answer.
- **Unverified source claims** (the "verifiable privacy" service, specific model strengths, tool feature gaps) are marked in the handout sections and checked against primary sources at authoring.
- **Volatile prices.** Black Friday is 2026-11-27 and Cyber Monday 2026-11-30. Refresh the market snapshot after 2026-11-30 before any purchase.

## Decisions already made

1. Handouts name products by role; the dated snapshot holds names and prices.
2. Hands-on steps live in runbooks, not graded handouts.
3. Docker deployment is excluded. Retrieval is light in L11 and applied to private documents in L17.
4. L16 (operating system), L17 (private documents), and L18 (travel) are separate handouts, read before L14. The operating system constrains hardware (macOS means Apple hardware), privacy is the first goal, and travel may change what to buy.
5. The four-tier budget ladder replaces a single stretch row.
6. The laptop battery is a side investigation, not a handout.
