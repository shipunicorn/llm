# Learner Hardware

**Personal file. Do not commit or publish.** The GitHub repository for this folder is public (see [goals.md](goals.md), "Standing constraints").

Used by L03, L05, L06, L10, L13, L14, L15, L16, L18 and `decision-memo.md`. Sources: order listings, Task Manager readings (2026-11-18), and the learner's reports. Peak ceilings are hypotheses until L10's first run measures real speeds. Handout questions state any figure they need as an `Assume that` sentence; they do not cite this file.

Ceiling formula: `memory bandwidth ÷ bytes read per token`. Bytes use 4.5 bits per weight as a rough 4-bit allowance. `scripts\llm_check.py` recomputes before a handout uses these.

## Desktop: HP OMEN 35L (Windows 11 Pro)

| Part | Value | Notes |
|---|---|---|
| CPU | Intel Core i7-14700F, 20 cores, 28 threads | Runs models from system RAM. The F suffix means no integrated graphics, so the RX 7600 also drives the display |
| RAM | 64 GB DDR5, rated 5600 MT/s (XMP), **measured 4000 MT/s**, 4 of 4 slots used | Two channels at 4000 MT/s give about 64 GB/s peak (4000 × 8 bytes × 2), not the 90 GB/s of the rated speed. Four-DIMM builds often run slower, or the XMP profile may be off. Whether the HP BIOS exposes the profile, and whether four DIMMs stay stable at 5600, is unknown. Checking is free |
| Memory in use | 41.5 GB used, 22.3 GB available in an ordinary session; the 14 listed apps use about 18 GB (Firefox about 6.8 GB, VS Code about 3.8 GB) | Usable is not installed (L03). A roughly 20 GB model file leaves little room with usual apps open |
| GPU | AMD Radeon RX 7600, 8.0 GB GDDR6 dedicated; 47.8 GB shared (borrowed system RAM, far slower than VRAM) | About 288 GB/s peak (vendor figure, re-check). 4.4 GB of VRAM was in use with ordinary apps open, so about 3.6 GB is free until apps close. Ollama's documentation lists the RX 7600 for Windows. Newer Radeon cards (RX 9070, 9060) are listed on Windows only through Vulkan. Driver dated 2026-06-22 |
| Storage | 2 TB PCIe Gen4 NVMe, 1.4 TB free; a further 1.5 TB of cloud storage opened through an encrypted vault (Cryptomator) | Room for many model files (a few GB to about 20 GB each). The vault is a leak-point topic in L17 |
| Power and cooling | 850 W 80 Plus Gold supply, 240 mm liquid cooler | Headroom for a larger GPU if the case and power connectors allow it (check at L14) |
| Warranty | HP, 1 year | Check whether replacing parts affects it before any upgrade |
| Always on? | No; cannot stay on while away | A second, always-on machine is therefore a real option |
| Noise in the listing | Software trials, Game Pass, keyboard and mouse lines, region and chassis codes, Wi-Fi card | L06 uses it to practice separating signal from spec-sheet noise (about 5 of about 25 lines carry the numbers that matter) |

### Desktop ceilings

| Case | Bytes per token | Memory pool, peak bandwidth | Ceiling |
|---|---|---|---|
| 8B dense, 4-bit, all in GPU memory | about 4.5 GB | GPU, about 288 GB/s | about 64 tokens/s |
| 8B dense, 4-bit, in system RAM | about 4.5 GB | RAM, about 64 GB/s | about 14 tokens/s |
| 35B-A3B mixture-of-experts, 4-bit, in system RAM (file about 20 GB) | about 1.7 GB (3B active) | RAM, about 64 GB/s | about 38 tokens/s |
| 32B dense, 4-bit, in system RAM (file about 18 GB) | about 18 GB | RAM, about 64 GB/s | about 4 tokens/s |

Hypotheses:

- Capacity is large but usable capacity is smaller: about 22 GB free RAM and about 3.6 GB free VRAM with usual apps open. Closing browsers and editors before a run is part of the routine (L03, L10).
- VRAM is the weakness: 8 GB fits about an 8B model at 4-bit with moderate context, not a 14B model.
- At 4000 MT/s the system-RAM ceilings are about 30% below what the rated 5600 would give.
- Large dense models in system RAM are slow (bandwidth); mixture-of-experts models are the exception (L05).
- Long-document questions are prefill-heavy, and prefill is compute-bound (L04). Time to first token depends on the GPU and on whether the engine can use the AMD GPU.
- A second machine needs a reason beyond raw capability: isolating private data (L17), travel (L18), or a stronger GPU. L14 evaluates "upgrade the GPU in the desktop" and "do not buy: test first" before any new machine. The memo may conclude that no purchase is justified.

## Laptop: ThinkPad L15 Gen 2 (bought about February 2022; listing only, not measured)

| Part | Value | Notes |
|---|---|---|
| CPU | Intel Core i7-1165G7 (4 cores, 8 threads per published specs; verify) | Runs small models from system RAM |
| RAM | 32 GB DDR4-3200 SODIMM; modules in use unknown | About 51 GB/s with two modules (dual channel), about 26 GB/s with one. The listing's "Intel UHD Graphics" (rather than the usual Iris Xe) often means a single module; check "Slots used" on Task Manager's Memory tab |
| Graphics | Integrated only | No dedicated GPU memory, so models run from system RAM. Engine support for Intel integrated graphics varies (verify at L10) |
| Storage | 512 GB SSD; free space unknown | Tight for model files plus documents |
| OS | Windows 10 Pro | Standard support ended October 2025. Consumer Extended Security Updates run to 2027-10-12 if enrolled (enrollment unknown). Holding personal documents on an unsupported system is a privacy risk, so moving to a supported system is a prerequisite for using it in the L17 pipeline. The chip and its TPM 2.0 are new enough for Windows 11 (verify with PC Health Check) |
| Battery | 45 Wh, 3-cell, described as integrated; about 4.75 years old; drains fast | Side investigation: `reference_sheets\laptop-battery-investigation.md` (probably past warranty; the learner is willing to open it). A separate file `lenovo_battery_repair.md` already exists in this folder |
| Mobile broadband | Built-in 4G LTE modem (SIM and function unverified) | Own connectivity on the road when hotel Wi-Fi is poor |

Unknowns to collect (a task inside L06): memory modules in use, free disk space, Windows update status and ESU enrollment, battery wear.

### Laptop ceilings (peak, 4-bit)

| Case | Bytes per token | Dual channel (about 51 GB/s) | Single channel (about 26 GB/s) |
|---|---|---|---|
| 4B dense | about 2.3 GB | about 23 tokens/s | about 11 tokens/s |
| 8B dense | about 4.5 GB | about 11 tokens/s | about 6 tokens/s |
| 35B-A3B mixture-of-experts (file about 20 GB) | about 1.7 GB | about 30 tokens/s | about 15 tokens/s |

Hypotheses:

- The laptop can already run the small models the workload needs, at modest speed. A redaction job that outputs a short list of spans (L17's extract-then-apply design) is mostly prompt reading, so slow generation matters little.
- A 20 GB mixture-of-experts file in 32 GB is tight once Windows and a browser run.
- Its real problems are the battery, the unsupported operating system, and the small disk, not capability. A new battery plus a supported operating system may already cover the travel case.
- L14 therefore asks what money adds beyond a fixed ThinkPad: a travel machine that is also a stronger local machine, or an always-on home box reached remotely.

## Phone: iPhone 17

Base model reported as 8 GB of RAM (verify). That limits it to small models, planning range about 1B to 4B at 4-bit (L18).

## Network

Hotel Wi-Fi (untrusted), sometimes mobile data or a local Wi-Fi of about 30 Mbps, little connectivity in flight. Text requests need little bandwidth, so remote access to a home machine works on these links. Tailscale (mesh VPN) is already installed on most of the learner's machines.
