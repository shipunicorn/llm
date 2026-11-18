## 1. The short version

```callout ok Conclusion
A small part of the work runs on models on the Mac Studio. That part is the web chat lane, the coding sessions, and a few read-only or advisory wiki surfaces. Everything that edits the wiki on its own, all ingestion, and serious coding still go to Claude. Two reasons: private content should stay on my machines, and local models are free per call. Claude stays because most wiki jobs fail on missing guards, a missing transport or a capability gap, not on speed.
```

```callout warn Right now, nightly local surfaces have nothing to call
Three facts stack up as of 2026-10-05. LM Studio now rests empty by owner policy, with no auto-load and no JIT load. It was last observed listening on the Studio's loopback only, so the Air could not connect. The paradox recheck then returns UNCLEAR, not a Claude answer.

It has entered no rechecks since 2026-09-21. The cause is not determined. The open items are N1 and N2 in the local-only roadmap.
```

```kv cols=2
Studio: M3 Ultra, 96 GB unified memory, always on, FileVault on
Air: M4, 16 GB, the client and coding machine
Runtimes: LM Studio, oMLX, Splash, TensorFold, plus on-demand big-model lanes on port 9797
Rest state: LM Studio empty (owner decision 2026-10-05)
Private lane: local first, Claude only as the named trusted fallback
Standing decision: local-LLM-only direction (2026-07-26), but shadow and propose-only until Tawsif says ready
One auto-editing exception: the paradox recheck, since 2026-08-16
Biggest open item: give the nightly surfaces a reachable, loaded model
```

## 2. The hardware

The Studio serves. Everything else is a client over Tailscale. The Air is small and fragile under load.

| Machine | Memory | Job | Hard limit |
|---|---|---|---|
| Mac Studio, M3 Ultra | 96 GB unified | Runs every model server. Bought 2026-07-13. | A boot daemon caps GPU wired memory at 87040 MB, which is 85.0 GiB. A second big load then fails fast. |
| MacBook Air, M4 | 16 GB | Runs `claude-local`, opencode, pi, PI WEB, the wiki gateway and the phone API. | On 2026-08-15 swap hit 95 percent and windows drew blank. Check swap first. |

The usable inference budget on the Studio is about 80 to 88 GB (memory note, 2026-07-13). The same note says to keep weights under about 70 GB, so context memory has room.

| Fits | Does not fit |
|---|---|
| The MoE tier up to about 120B total parameters. Examples: Qwen 3.6 35B-A3B, gpt-oss-120b (about 63 GB), GLM-4.5-Air (about 63 GB at Q4). | GLM-5.2 (744B, about 241 GB even at 2-bit). Kimi K2 (about 1T). Route those to hosted services. |
| Hy3 295B at 1-bit, 66 GB, as an on-demand lane that runs alone. | Any second big model beside Hy3. That combination froze the box on 2026-07-17. |

Memory per token matters as much as weights. The memory note of 2026-07-25 records about 24 KB per token for the 80B hybrid model. A dense 27B model needs about 245 KB per token. So a dense model is a poor orchestrator.

FileVault means no auto-login. After a power loss the Studio waits at the unlock screen until someone types the password once.

## 3. The runtimes and lanes

Several separate programs can hold model memory. They do not share a catalog. `orch status` shows LM Studio only. Check all four status commands before loading anything.

| Lane | Port | What it serves | How it starts and what guards it | Who uses it |
|---|---|---|---|---|
| LM Studio | 1234 | MLX models from its own catalog. It also speaks the Anthropic message format. | Headless service. `orch load` and `orch unload`. `worker-swap.sh` for fixed sets. Rest state empty. | Wiki gateway, `claude-local`, opencode, pi. Last observed on loopback only, so the Air needs an SSH tunnel. |
| oMLX | 8000 | A second runtime with its own catalog. Holds the small Gemma E4B for Open WebUI. Runs Ornith 6-bit for coding. | `orch omlx`. One API key guards both inference and admin. The key lives in four places and one command rotates all four. | Open WebUI, pi, and the household study app (moved here 2026-09-16). |
| Splash | 8000 | An alternative occupant of the same port. Never up with oMLX. | `orch splash`. Fresh key each start. The Air reaches it through an SSH tunnel. | pi and pi-web coding runs. |
| TensorFold | 8091 | The stock 4-bit Qwen3.8-27B, started at medium effort. It was the long-build pick until 2026-10-08, when Dirk on oMLX replaced it. No fast path for 6-bit or Dirk builds. | `orch tf`. It refuses to start if memory arithmetic fails. | pi. |
| Big-model lane | 9797 | Hy3 295B. Laguna S 2.1 shares the port, but Tawsif deleted its weights on 2026-08-15. | `hy3-serve.sh` runs preflight checks. A watchdog kills it on low memory. One lane at a time. | Evals only. Loopback only. |

Two more lanes exist. The `tess-server` engine uses port 8787 and sits behind the same preflight pattern. The `nothink` proxies on ports 1235 and 1236 appear in the docs only. I did not verify them here. Open WebUI itself listens on 8080, covered on the other page.

A rule keeps these lanes apart. Never put one big model beside another. On 2026-10-01 oMLX refused a prompt past its 76.5 GB prefill cap, because a second 27B had loaded beside the first.

## 4. How a request routes

One config key decides where a wiki call runs. The code is `model_router.provider_for`. It returns `lmstudio` only when `local_llm.enabled` is on and the surface name is in `local_llm.surfaces`. Each call site must ask for that answer itself. A name added to the list does nothing if the call site never asks.

```flow TB
(Wiki surface) -> {Surface listed in local_llm.surfaces?}
{Surface listed in local_llm.surfaces?} -> [Claude path]: no
{Surface listed in local_llm.surfaces?} -> [Safety checks]: yes
[Safety checks] -> *[LM Studio on the Studio]: pass
[Safety checks] -> {Surface policy}: refused or error
[LM Studio on the Studio] -> (Local answer)
{Surface policy} -> [Claude path]: fail open
{Surface policy} -> (Error shown, UNCLEAR, or skipped): closed or local only
```

Text version: a surface that is not listed goes straight to Claude. A listed surface passes two safety checks and then reaches LM Studio. If a check refuses or the server errors, the surface's own policy decides between Claude and an error.

The two safety checks are real code. The first is `host_is_local`. It accepts only loopback, private ranges, the Tailscale range, `.local` and `.ts.net`. Anything else raises a vault-safety error.

The LM Studio provider is on the vault-safe list in code. The older gateway design note omits it, so follow the code. The second is `studio_guard`. It refuses a load while the 9797 lane is up. It also refuses to start a load on top of resident big models.

| Surface policy | Surfaces | What happens when local fails |
|---|---|---|
| Fail open to Claude | `/query`, Ask Mirat (opt in per request), self-improve diagnosis, verify-claims (gated per model, documented not routed) | One retry on Claude. |
| Fail closed | `/journal`, today-parse | The user sees an error. Journal text never leaves the machine. |
| Local only, no fallback | dashboard gloss, mirror select, day plan | The run skips that night. |
| Returns UNCLEAR | the paradox recheck | No Claude fallback. UNCLEAR is not a model opinion. |
| Never local | Every `agent_run` job: ingest, auto-idea, consolidate-deep, fission. Shell pipelines through `wiki_cost run`, such as `research.sh`. Session-based `fix_untruths`. The Quartz chat server. | There is no local path. |

I checked the last row at the code. `agent_run` raises `NotImplementedError` for anything except Claude and OpenAI. `research.sh` has zero references to the gateway. `fix_untruths` builds a `ClaudeSession`. So ingestion cannot go local until someone writes an LM Studio transport for `agent_run`. That is item N10.

The live surface list is a gitignored file, so I did not read it. The docs default is four surfaces. A 2026-09-11 note lists seven live. I did not verify that here. By grep, ten modules now ask `provider_for`. The 2026-08-19 claim of "exactly four" is out of date.

## 5. The standing rules and the guards

On 2026-07-17 a second model loaded while the 66 GB Hy3 lane was up. About 92 GB went to wired memory on a 96 GB box. The machine froze and needed a power cycle. Prose rules had failed. So the rules now live in code.

| Layer | What it does | Verified here |
|---|---|---|
| Wired-limit daemon | Sets the 87040 MB ceiling at boot and reads it back. | Script and plist template exist. |
| Memory watchdog | Kills only the 9797 lane or the tess engine when free memory drops under 6 GB. It warns at 10 GB. | Yes, constants in script. |
| Auto-restart on freeze | A macOS setting reboots a hung box. | Documented, not verified. |
| Serve wrappers | `hy3-serve.sh` and siblings check for other loads first and prove readiness with a real completion. | Files exist. |
| Choke point in code | `assert_lmstudio_load_allowed` blocks a load while 9797 answers. `jit_refusal_reason` blocks a JIT load on top of residents. Both fail open if they cannot read state. | Yes. |
| Day-plan residency check | `model_residency` fails toward refusal. It never lets a background poller load a model. | Yes. |
| `worker-swap.sh` | A closed list of modes. It refuses to swap a model that is generating. It never touches the 80B. | Yes, in the working tree. |
| `orch load` and `orch unload` | Open ids with the same safety checks. No LLM takes part. | Documented. |
| Nightly tripwire | Reports drift from the rest state. It warns and never unloads. | Yes, in the working tree. |

The rest-state guards changed on 2026-10-05, in files not yet committed. `worker-swap.sh` now has a `none` mode and calls `mobile` retired. The tripwire now expects LM Studio to be empty. The last commit, `docs.md`, the index page and the `studio-remote` skill still say `mobile`. The tracked opencode config also tells its orchestrator to run `worker-swap.sh mobile`. That mode now exits with a usage error.

The pinned 7B model is gone. Tawsif retired it on 2026-09-16 and deleted its weights. The code still carries a pinned-model exemption. Treat it as leftover.

| Standing rule | Why |
|---|---|
| Never co-load two big models. | The 2026-07-17 freeze. The hazard is the load moment, not steady residency. |
| One Studio-touching agent at a time per session. Announce before acting. Shortest job first. | On 2026-09-05 two subagents of one session collided. An arm of 37 of 51 cases errored. |
| Read residency from `/api/v0/models`, never `/v1/models`. | The `/v1` list is the catalog. It showed 24 ids on a box holding 4. |
| Guard every `lms load` with a residency check. | Loading a resident id spawns a second copy named `:2`. |
| Expect load refusals after big downloads. | The guardrail counts free memory, not reclaimable inactive memory. |
| Park PI WEB sessions during evals. | Its daemon keeps running after a tab closes and can load a model mid-run (2026-10-01). |
| Private lane and integrity checks stay propose-only. | Decision of 2026-07-26. The web lane left this rule on 2026-08-19. |
| Paradox is the one auto-editing local cell. | Gemma only, gated per exact model, since 2026-08-16. |

## 6. Which models run what

Models change often. This table follows the docs, with dates. Only the small Gemma E4B stays loaded by design now, and that sits in oMLX (documented, not verified here).

| Model | Where | Role | Dated note |
|---|---|---|---|
| (rest state) | LM Studio | Empty. Load a model on purpose, unload when done. | Owner policy, 2026-10-05. |
| Gemma 4 E4B, 4-bit, about 6.4 GB | oMLX | Open WebUI chat, default for both users. | Per the other page and the studio-remote skill. |
| Gemma 4 26B-A4B, 16.14 GB | LM Studio | Config default for local wiki surfaces since 2026-09-04. Vision seat. Paradox recheck. | A text eval alone cannot move it, because vision binds its seat. |
| Qwen 3.6 35B-A3B, 20.43 GB | LM Studio, on demand | Former workhorse. Left the rest set 2026-09-04. | `worker-swap.sh 35b`. |
| Qwen 3.8 27B, 16.08 GB | LM Studio and TensorFold | Long coding builds until 2026-10-08, when Dirk on oMLX replaced it (see the routing table). A 27B variant is the journal API override (config note, 2026-09-11). | 37.0 tokens per second on 2026-08-16: median of 3 runs, 299-token replies, 4-bit, no speculative decoding. |
| Tess-4-27B, 16.08 GB | LM Studio, on demand | Implementor in the three-seat topology. Won the form eval on 2026-07-22. | Capability set 36 of 40 on 2026-07-21. |
| Ornith 1.5 35B-A3B | oMLX, 6-bit | Short coding tasks and parallel workers. | Coding table in section 8. |
| Qwen3-Next 80B Thinking, 44.86 GB | LM Studio, session only | Orchestrator candidate. Not a permanent seat. | Owner decision 2026-08-14. |
| Hy3 295B, IQ1_M, 66 GB | Port 9797 | Eval lane, one at a time. | RAG 33 of 35 on 2026-07-18 (before the instrument repair). |
| Qwen 2.5 7B | none | Retired 2026-09-16. | Weights deleted. |

## 7. What the evals found

Two cautions come first. Every `/query` score recorded before 2026-08-14 came from a broken instrument. Five cases passed on their own question text. Do not compare those scores with later ones. Also, a rate means little without its denominator. Each row gives it.

| Date | Question | Verdict | Sample and caveat |
|---|---|---|---|
| 2026-07 (pre-repair) | Does a frontier model beat local on wiki Q and A? | No. Opus 49.0, Sonnet 49.0, local 35B 49.0 of 51. They differed on zero stable cases. | 51 cases, 3 runs each, broken instrument. Trust only the null result. |
| 2026-08-14 | Same, on the repaired instrument. | 35B scored 44.0 of 51. Gemma scored 44.7. A tie. | 51 cases, flip floor 8 of 51. |
| 2026-08-16 | Can gemma judge paradox? | Yes for this one cell. Flip rate 0.000 against Claude's 0.500. | 18 pairs. Accuracy 15 of 18 equals an always-CONTRADICTION guess. Recall of 3 of 3 is what separates it. |
| 2026-08-19 | Do vault answers stay grounded? | No. 31.2 and 45.8 percent carried an unsupported claim. | Guards fix this, not a bigger model. |
| 2026-09-04 | Cut the context budget for speed? | No. 41, 38 and 36 of 51 at full, 36k and 20k characters. | Rule was zero regressions. Nothing shipped. |
| 2026-09-05 | Swap gemma for Qwen 3.6? | No change. 40 of 51 each, with 3 flips each way. | One case is within noise. About 22k-token prompt, cache busted, thinking suppressed. |
| 2026-08-01 | Can reasoning models return JSON schema output? | No. The 35B returned empty text. The 7B worked. | Use tool calls with `tool_choice` set to `required`. |
| 2026-08 | Can LM Studio turn thinking off? | No. It drops `enable_thinking` and `reasoning_effort`. | Only oMLX enforces it. Any "thinking off" label there is false. |
| 2026-09-05 | Speed up decode with a draft model? | Blocked. MLX draft mode exists only in the TypeScript SDK. | LM Studio 0.4.21. |
| 2026-09-05 | Keep a fixed cached core of pages? | Dead end. No faster, and decode slowed. | Context-first prompt order shipped. Same-context follow-ups take about 1 second. |
| 2026-10-05 | Which model for nightly sites? | Keep gemma for paradox. Name no winner elsewhere. | Paradox n=18. Coherence n=36, no arm differs. Propose-only. |

One design finding stands out. The untruths prompt contains no date. Claude gets the date from its CLI. Local models do not. So gemma cannot judge staleness at all. That is a prompt fault, not a model fault.

## 8. The coding lanes

Coding is where local models get the most real use. The wiki cost log does not count it. On 2026-08-19, only 8 percent of logged local calls were production. The rest were evals.

| Lane | What it is | Note |
|---|---|---|
| `claude-local` | Claude Code pointed at LM Studio. | Uses an isolated config folder. Global skills and hooks do not load. Keep it CLI only. |
| opencode | Open-source harness with a three-seat topology: orchestrator, implementor, a11y reviewer. | The 80B orchestrator is no longer a seat. |
| pi, PI WEB, omp | The standard harness (version 0.85.1). PI WEB runs on the Air at port 8504. | Sessions outlive the browser tab. |
| pi-delegate | Delegation helpers, moved 2026-09-25 to a separate local repo. | I did not open that repo. Use its skill pointer. |
| `pipeline.py` | A six-stage local build, review and remediate pipeline. | Ends with a hard final gate. |

Measured on 2026-10-02 and updated 2026-10-05:

| Job | Choice | Result |
|---|---|---|
| Short, constrained tasks | Ornith 6-bit on oMLX | 6 of 6 on the constraint task, in 3 to 10 minutes. |
| Several workers | Ornith, up to 4 on oMLX | 1.59 times throughput. LM Studio holds 2 and crashes at 3 or more. |
| Long multi-stage builds | Dirk-Qwen3.8-27B-MLX-oQ6e on oMLX, with multi-token prediction (MTP) on and thinking left unset | Tied three other 27B setups on quality (8.00 to 8.67 of 10), in a mean 63 minutes against 95 to 112. Superseded 2026-10-08: the stock Qwen 3.8 27B on TensorFold at medium effort. |
| Planner plus builder | 27B plans, Ornith builds | Failed: 9 of 30, every run hit the 3 hour cap. |
| Ornith in phases | Sequential or partly parallel | 26 and 27 of 30, against 21 of 30 solo. |
| Speed and score first | Sonnet 5.5 | 10 of 10 in 3 to 4.5 minutes. |

TensorFold and LM Studio ignore pi's effort setting. The 27B's default then collides with a 32k turn cap, and most long builds score zero. If you use TensorFold, start it with `--reasoning-effort medium`. Since 2026-10-08 the plain Dirk id carries these settings; before that oMLX ran it with thinking off, and it scored 5.67. Open question: perfect h5 scores were 0 of 6 with MTP and 3 of 6 without (p about 0.18). There is no claim yet.

## 9. When it breaks, and where the docs disagree

Single points of failure: one Studio, one untracked routing file, one person's labels, and one Mullvad exit node for outside traffic. Losing the routing file silently reverts the paradox cell to a worse Claude model. No alarm fires, because Claude still answers.

| Symptom | Likely cause | First check |
|---|---|---|
| Local surfaces error or fall to Claude | Studio asleep or off the tailnet. LM Studio down after a reboot. Loopback-only bind. Empty by policy. | `orch status`, then the model list through a tunnel. |
| Nothing answers after a power cut | FileVault is waiting at the unlock screen. | Look at the Studio's screen. |
| Load refused | Co-load guard, or free memory looks low while inactive memory is large. | All four status commands: LM Studio, oMLX, Splash, TensorFold. |
| A model vanished mid-run | Another session swapped, JIT evicted it, or a PI WEB session loaded one. | Read `/api/v0/models`. |
| Empty reply from a reasoning model | Thinking used the whole token budget. | Give at least 2000 output tokens. |
| Box froze | Co-load. The wired limit reads 0 if the boot daemon regressed. | Check the wired-limit value. |
| oMLX lists a stale or missing model | It reads its catalog only at start. | Restart oMLX when idle. |
| Paradox recheck returns UNCLEAR | No model loaded at the 23:45 nightly run. | Roadmap items N1 and N2. |
| Air windows draw blank | Swap exhaustion. | `sysctl vm.swapusage` and `vm_stat`. |
| Search or fetch dies, chat works | The one Mullvad exit node. | See the Open WebUI page. |

```callout warn Where the docs disagree
Docs and code give different answers on six points. This page follows the code. Where only docs exist, it follows the later dated note.
```

- Rest state: docs.md, the index, the using page and the studio-remote skill say `mobile`. The uncommitted code says empty.
- Bind address: the index says tailnet only. The 2026-10-05 observation says loopback only. Follow the later one.
- Pinned 7B: the determination page calls it the one hard SLA. Tawsif retired it on 2026-09-16.
- Call sites: "exactly four" dates from 2026-08-19. Ten modules call `provider_for` today.
- Ask Mirat local share: "6 local ever" (2026-08-19) became 268 local against 53 Claude in the 2026-09-07 correction.
- The index status block says "confirmed 2026-09-11" but still holds older layers.

## 10. Where to read more

- wiki/projects/local-llm/index.md
- wiki/projects/local-llm/using.md
- wiki/projects/local-llm/determination.md
- wiki/projects/local-llm/local-only-roadmap.md
- wiki/projects/local-llm/nightly-local-model-eval.md
- wiki/projects/local-llm/omlx-openwebui.md
- wiki/docs.md (Local LLM server, Local coding sessions)
- wiki/meta/llm-gateway-design.md
- wiki/meta/omlx-security-secret.md (design only)
- wiki/meta/local-vs-frontier-model-discernment.md
- wiki/meta/model-routing.md (superseded by the determination page)
- scripts/studio-guards/README.md
- scripts/studio-guards/worker-swap.sh, mem-watchdog.sh, set-wired-limit.sh
- portability/studio_guard.py and portability/providers/lmstudio_provider.py
- portability/llm_client.py
- wiki_engine/core/model_router.py
- wiki_engine/orchestration/nightly_tripwire.py
- CONFIGURATION.md (the local_llm keys)
- .claude/skills/studio-remote/SKILL.md and .claude/skills/pi-delegate/SKILL.md
- wiki/learning/hub/how-my-openwebui-works.html
