# Market Snapshot v1

As of **2026-11-18**. This sheet holds the things that go stale: prices, product names, model names, and software support. Handouts teach the method and name products by role; they take figures from here, stated as `Assume that` sentences.

## How to read this sheet

- **Evidence** column: **Opened** means the page was fetched and read on the as-of date. **Snippet** means only a search-result summary was seen and the page itself was not opened, so treat the row as a lead to confirm, not a fact.
- Prices are in US dollars and **before sales tax** unless stated. Tax depends on your state and store, and is unknown here. The tier tables add a placeholder 8% to test the all-in ceiling.
- Rows older than three months are re-checked before a handout or the decision memo uses them. Prices in this market moved by tens of percent within weeks, so re-check any price the day you buy.
- **Timing:** Black Friday is 2026-11-27 and Cyber Monday 2026-11-30. Several deal threads were posted in the last two weeks and some had already expired or reverted by the time they were read (see section 4). Refresh this sheet after Cyber Monday and before any purchase.

## 1. What changed compared with the plan's assumptions

1. **Memory is in a price shock.** Retail 64 GB DDR5 kits were seen between about $870 and $1,360 (Aug to Nov 2026), several times the 2025 price. The 64 GB inside your desktop is worth roughly that much today, and any new machine with a lot of memory is expensive. Do not plan to buy RAM.
2. **The "used GPU tower" archetype is out of budget.** A used RTX 3090 (24 GB) was asking about $1,600 to $2,000 in November 2026, not the $650 to $900 older guides quote.
3. **Apple small desktops are the cheapest way to get fast memory per dollar right now**, mainly through Micro Center in-store pickup deals (section 4). They have soldered memory and storage, so you pick the size at purchase.
4. **A 16 GB graphics card for your desktop is the cheapest way to lift the 8 GB VRAM limit.** Sub-$900 cards exist, but prices drifted up by about $170 in two months for one model (section 4), so the numbers below may already be old.
5. **Windows 10:** support ended on 2025-10-14, but Microsoft's page says the consumer Extended Security Updates program now protects a device until **2027-10-12**. The plan's earlier statement that updates ended in October 2026 was wrong and is corrected.

## 2. Budget tiers: what each buys

Method: every price is before tax and checked against the tier with 8% tax added (placeholder). "All-in" below means price × 1.08. Prices are the last seen, not guaranteed. Memory bandwidth for Apple chips is from Apple's spec page; a decode-speed ceiling is `bandwidth ÷ file size` (L04) and real speeds are lower.

Your workload (transcribe, redact, small local model) needs only a small model tier, so **every tier, including $0, can do the job**. The tiers differ in speed, convenience, and whether the machine is always on or travels with you.

### $0 baseline (spend nothing)

| Option | What it does | What it cannot do |
|---|---|---|
| Desktop as it is (8 GB GPU, 64 GB RAM at 4000 MT/s) | 8B-class models on the GPU, large mixture-of-experts models in RAM | Always-on use, travel, fast 14B+ dense models |
| ThinkPad with a new battery and a supported Windows | Small models from RAM at modest speed, on the road, with its own 4G modem | Fast generation; needs the battery investigation first |

### $600 all-in

| Candidate | Last-seen price | All-in at 8% | What it adds | What it cannot do |
|---|---|---|---|---|
| 16 GB graphics card in the desktop (Radeon RX 9060 XT 16 GB) | $459 on 2026-08-31 (older; current price not found) | about $496 | Doubles VRAM, so a 14B-class dense model at 4-bit (about 8 GB) fits entirely on the GPU | Larger dense models (a 27B file is about 18 GB); travel; always-on use. Check power connectors and case clearance first |
| Mac mini M4, 16 GB / 256 GB | $599.99 at select Micro Center stores (pickup only), 2026-11-14 | about $648 | A quiet always-on box you can reach through Tailscale. About 11 to 12 GB usable for models, so 8B class | Slightly over $600 all-in; 256 GB disk is small; models above about 8B |

### $900 all-in

| Candidate | Last-seen price | All-in at 8% | What it adds | What it cannot do |
|---|---|---|---|---|
| Mac mini M4, 16 GB / 256 GB | $599.99 | about $648 | As above | As above |
| Mac mini M6, 16 GB / 256 GB | $699.99 at select Micro Center stores, 2026-11-13 | about $756 | Same memory, newer chip (a commenter put multi-core speed about 50% above the M4); 153 GB/s bandwidth, ceiling about 34 tokens/s for an 8B 4-bit file | Same 16 GB limit; storage cannot be upgraded later |
| Mac mini M6, 16 GB / 512 GB | $929.99 at Micro Center, 2026-10-29 | about $1,004 | Room for model files without an external drive | Slightly over $900 all-in |
| Radeon RX 9070 16 GB graphics card (bundle with a 360 mm cooler) | $700, 2026-10-08 (bundle) | about $756 | 16 GB VRAM at higher speed than the 9060 XT class | Needs the cooler to be useful; card alone price not found |

### $1200 all-in (the decision ceiling)

| Candidate | Last-seen price | All-in at 8% | What it adds | What it cannot do |
|---|---|---|---|---|
| Mac mini M6, 24 GB / 512 GB | $1,099.99 at Micro Center (listed 2026-10-29 to 10-31) | about $1,188 | About 16 to 18 GB usable, so up to about 14B dense or about 20B mixture-of-experts models; 170 GB/s, ceiling about 21 tokens/s for a 14B 4-bit file; always on | Tight against $1,200 if your tax is above about 9%; models of 27B and up |
| MacBook Air 13" M5, 16 GB / 512 GB | $1,089 at P.C. Richard & Son, 2026-11-11 | about $1,176 | A new travel laptop that also runs 8B-class models; replaces the ThinkPad | 16 GB limit; not always on |
| Radeon RX 9070 XT 16 GB (desktop upgrade) | $869.99 at Micro Center, 2026-11-13 (expired; was $699 in January and $700 a month earlier) | about $940 | Fast 16 GB VRAM | Needs two 8-pin power connectors, about 340 mm of case length, and an 800 W supply is recommended (yours is 850 W); price may have moved |

### $1500 all-in (shows what a stretch would buy; the decision ceiling stays $1200)

| Candidate | Last-seen price | All-in at 8% | What it adds | What it cannot do |
|---|---|---|---|---|
| MacBook Air 15" M5, 16 GB / 512 GB | $1,299, 2026-11-06 | about $1,403 | Larger screen, same memory | No more model capacity than the 13" |
| Mac mini M6, 32 GB / 1 TB | Not found | n/a | Would hold models up to about 14B comfortably with long context | Price unknown |
| MacBook Air 13" M5, 32 GB / 512 GB | $1,549 at B&H on 2026-10-26, back to $1,699 afterwards | about $1,673 to $1,835 | The cheapest 32 GB portable seen | **Does not fit $1,500 all-in**; a commenter said 32 GB models are hard to find in stock |
| Mac mini M5 Pro, 24 GB | about $1,600 at Amazon, Oct 2026 | about $1,728 | 307 GB/s bandwidth, but 24 GB of memory limits it to the same model sizes as the M6 24 GB; more memory costs $600 to $1,000 extra per the comparison article | Does not fit $1,500 all-in |

What the ladder says:

- **Capability barely changes up the ladder for your job; convenience does.** The jumps are: a 16 GB GPU in the desktop ($600 tier), an always-on box ($600 to $1,200 tiers), and a new portable machine ($1,200 and up).
- **A 32 GB portable does not fit under $1,500 all-in** at the prices seen. A 32 GB model needs roughly $1,650 to $1,850 all-in.
- The $1200 candidates for the three configurations in L14 are: (A) keep what you have, no purchase beyond a battery; (B) the Mac mini M6 24 GB, or a cheaper Mac mini; (C) a 16 GB MacBook Air.

## 3. Hardware rows

| Item | Price or figure | As of | Source | Evidence |
|---|---|---|---|---|
| Mac mini lineup: M6 (12-core CPU, 12-core GPU) with 16 GB or 24 GB base, configurable to 24 or 32 GB; M5 Pro (24 GB base, configurable to 48 or 64 GB). Bandwidth 153 GB/s (16 GB models) and 170 GB/s (24 GB models; the mapping is inferred from the page layout), 307 GB/s for M5 Pro. Maximum continuous power 155 W; idle power not stated | n/a (no prices on Apple's pages) | 2026-11-18 | https://www.apple.com/mac-mini/specs/ | Opened |
| Apple store configurations shown: M6 24 GB / 512 GB, 16 GB / 512 GB, 16 GB / 256 GB, 32 GB / 1 TB; M5 Pro 64 GB / 1 TB, 24 GB / 512 GB | No prices shown | 2026-11-18 | https://www.apple.com/shop/buy-mac/mac-mini | Opened |
| Local-LLM hardware comparison: Mac mini M6 16 GB about $780 at Amazon (Apple lists $899); M5 Pro 24 GB about $1,600 (Amazon); M5 Pro 64 GB $2,699 (Apple); Strix Halo 128 GB mini PCs $3,600 or more; used RTX 3090 renewed $1,830 to $1,880; usable model memory about 11 to 12 GB on a 16 GB Mac and 16 to 18 GB on a 24 GB Mac. The Apple speed figures are estimates, not measurements | see text | 2026-10-21 (article date), read 2026-11-18 | https://computingforgeeks.com/mac-mini-vs-mini-pc-vs-gpu-local-llm/ | Opened |
| 64 GB DDR5 kit | about $869 (lowest listing, Aug 2026) to about $1,360 (median from Nov 2026 Amazon listings); category average $1,272 in Aug 2026, up from about $222 a year earlier | 2026-08 to 2026-11 | https://www.buysellram.com/blog/ram-market-update/ ; https://capitalandcompute.net/memory-prices/ddr5/ | Snippet |
| Used RTX 3090 24 GB | asks about $1,600 average to about $2,000 median; listings fell about a third in four weeks | 2026-11-11 and 2026-11-14 | https://gpudojo.com/market-notes/2026-11-11-rtx-3090-asks-rise ; https://resaleprices.com/gpu/nvidia-rtx-3090 | Snippet (asking prices, not sold prices) |
| Radeon RX 9060 XT 16 GB | $459 (price history), launch price $349 | 2026-08-31 | https://www.techspot.com/products/gpu/amd-radeon-rx-9060-xt-16gb.308866/ | Snippet |
| RTX 5060 Ti 16 GB | Slickdeals price history showed $570 in Sept 2026; trackers disagree ($420 to $780 depending on date) | 2026-09 | https://slickdeals.net/f/18855802-msi-geforce-rtx-5060-ti-16g-shadow-2x-oc-plus-16gb-gddr7-pcie-5-0-graphics-card-369-free-shipping | Snippet; current price not found |
| Strix Halo 128 GB mini PCs (Ryzen AI Max+ 395) | $1,795 to $1,900 in late 2025; $3,099 (Acemagic, Aug 2026) and about $3,600 (Amazon, Oct 2026) | 2026-08 to 2026-10 | https://slickdeals.net/f/19851882-acemagic-m1a-pro-amd-ryzen-ai-max-395-128gb-ddr5-ram-2tb-ssd-3099 | Snippet; out of every tier because memory prices rose |
| Prebuilt gaming desktops with 12 to 16 GB GPUs | $1,499.99 (Acer Predator Orion, RTX 5060 Ti 16 GB, 16 GB RAM); $1,574.70 (refurbished Acer Nitro 60, RTX 5070 12 GB, i7-14700F, 32 GB RAM) | 2026-10-20 and 2026-11 | https://slickdeals.net/computer-deals/ | Opened (front page) and Snippet |

## 4. Slickdeals threads and what the comments said

The comment sections are where the price history and caveats live. Read each thread's comments again before buying.

| Thread | Deal | Posted | Comment takeaways | Evidence |
|---|---|---|---|---|
| https://slickdeals.net/f/20307996-apple-mac-mini-desktop-m4-chip-16gb-ram-256gb-ssd-599-99 | Mac mini M4 16 GB / 256 GB, $599.99, select Micro Center stores, free pickup | 2026-11-14 | Best Buy price-matches if a Micro Center in that state has it in stock and the model numbers match; the M6 for about $100 more is called "the smart play"; a commenter says the M6 has about 50% more multi-core speed; the SSD is not upgradable (use an external drive); minimum spec "not for me" to one commenter; new M4 stock had largely disappeared elsewhere | Opened |
| https://slickdeals.net/f/20304474-apple-mac-mini-desktop-2026-m6-12-core-16gb-ram-256gb-ssd-700 | Mac mini M6 16 GB / 256 GB, $699.99 (listed $900), select Micro Center stores, pickup | 2026-11-13 | 16 GB fine for a light personal assistant but "not ideal" for large models or coding agents; memory and storage are soldered; Best Buy price matches worked for some and not in other locations; the 512 GB model is about $200 more; trade-in of an older mini was mentioned | Opened |
| https://slickdeals.net/f/20236851-apple-mac-mini-m6-12-core-cpu-24gb-unified-memory-512gb-solid-state-drive-1100 | Mac mini M6 16 GB / 512 GB, $929.99 (listed $1,100); thread also lists 24 GB / 512 GB at $1,099.99 and 24 GB / 1 TB at $1,399.99 | 2026-10-29 | Commenters tie prices to memory and flash shortages; no local-LLM discussion; pickup availability varies | Opened |
| https://slickdeals.net/f/20304153-micro-center-powercolor-amd-radeon-rx-9070-xt-hellhound-overclocked-triple-fan-16gb-gddr6-pcie-5-0-gpu-870-free-store-pickup | Radeon RX 9070 XT 16 GB, $869.99 (listed $1,180), Micro Center pickup | 2026-11-13 (marked expired) | Price history from comments: $699 on 2026-01-05, $700 a month before, $749 at one store; "considering the current market, this is a good deal"; card is 340 mm long, 2.5 slots, two 8-pin connectors, 800 W supply recommended; one commenter's case fits only 300 mm; 25+ units in stock at one store | Opened |
| https://slickdeals.net/f/20211873-13-6-apple-macbook-air-laptop-2026-m5-10-core-chip-32gb-ram-512gb-ssd-1549-free-s-h-b-h-photo | MacBook Air 13.6" M5, 32 GB / 512 GB, $1,549 (list $1,699), B&H | 2026-10-26 (expired) | Back to $1,699 after expiry; one commenter expects an M6 Air; no performance reports; battery listed as 53.8 Wh with no runtimes | Opened |
| https://slickdeals.net/f/20291625-13-6-apple-macbook-air-laptop-2026-m5-chip-16gb-ram-512gb-ssd-1089-more-free-s-h | MacBook Air 13.6" M5, 16 GB / 512 GB, $1,089 | 2026-11-11 | Not opened | Snippet |
| https://slickdeals.net/f/20265798-apple-macbook-air-15-m5-16gb-ram-512gb-ssd-more-1299 | MacBook Air 15.3" M5, 16 GB / 512 GB, $1,299 | 2026-11-06 | Not opened | Snippet |

Patterns across the threads:

- **Many offers are local pickup at Micro Center.** If you do not live near one, these prices are unavailable to you (open question in `llm-knowledge.md`).
- **Deals expire fast and prices revert.** Two of the five opened threads were already expired.
- **Stock is thin for the 32 GB configurations** and for the older M4 mini.

## 5. Software by role

| Role | Product | Fact | As of | Source | Evidence |
|---|---|---|---|---|---|
| All-in-one desktop app and server | Ollama | Windows AMD GPU list includes the RX 7600 (and 7600 XT, 7700 XT, 7800 XT, 7900 series, several Radeon PRO cards). Vulkan support is available for other GPUs and is enabled by default once its backend is installed. The Windows list does not include the RX 9070 or 9060 series; the Linux list does | 2026-11-18 | https://docs.ollama.com/gpu | Opened |
| Low-level engine | llama.cpp | Prebuilt Windows builds exist for both Vulkan and ROCm (HIP). For an RX 7600 on Windows, guides call the Vulkan build the safest path; they disagree on ROCm for this card | 2026-11-18 | https://glukhov.org/llm-hosting/comparisons/amd-rocm-vs-vulkan-llm-hosting/ ; https://github.com/ggml-org/llama.cpp/discussions/10879 | Snippet |
| Desktop app | LM Studio | One 2026 guide recommends trying the Vulkan runtime first on desktop AMD GPUs because switching runtimes is easy | 2026-11-18 | https://glukhov.org/llm-hosting/comparisons/amd-rocm-vs-vulkan-llm-hosting/ | Snippet |
| Private connection | Tailscale | Personal plan reported free for up to six users with unlimited personal devices; non-commercial use. The pages are third-party and disagree on some limits | 2026-11-18 | https://www.costbench.com/software/business-vpn/tailscale/free-plan/ | Snippet; check tailscale.com/pricing |
| Phone apps | PocketPal AI, Private LLM, MLC Chat, LLM Farm | Reported to run real models on the iPhone without a connection once the model is downloaded; tokens-per-second figures in the guide are unverified; network behavior of each app not checked | 2026-11-18 | https://www.promptquorum.com/power-local-llm/best-local-llm-apps-iphone-2026 | Snippet |
| Redaction toolkit | Presidio with GLiNER | Presidio has a built-in GLiNER recognizer; GLiNER PII model `urchade/gliner_multi_pii-v1` (Apache 2.0) covers 50+ personal-data types including SSNs. A 2026 comparison reports low absolute scores for open detectors under adversarial tests, so test on your own documents | 2026-11-18 | https://microsoft.github.io/presidio/samples/python/gliner ; https://particula.tech/blog/presidio-vs-gliner-pii-redaction-llm | Snippet |
| CPU-only OCR | Tesseract with OCRmyPDF | Recommended fallback for scanned PDFs without a GPU | 2026-11-18 | https://pasqualepillitteri.it/en/news/25568/best-local-ocr-pc-server-2026 | Snippet |

## 6. Models to try, by job

All entries are leads from secondary sources; read each model card (size, license, recommended settings) before downloading, and test on your own documents (L08). None of them needs more than your desktop already has, except where noted.

| Job | Candidates | Notes | As of | Source | Evidence |
|---|---|---|---|---|---|
| OCR of scanned pages | PaddleOCR-VL-1.6 (Apache 2.0), MinerU2.5-Pro (Apache 2.0), GLM-OCR (MIT); each about 0.9 to 1.2B parameters | Top three on OmniDocBench v1.6 in Roboflow's ranking (96.34%, 95.75%, 95.22%); small enough for 8 GB of VRAM. Benchmarks are reported by vendors and bloggers | 2026-11-18 | https://roboflow.com/blog/best-open-source-ocr-models | Snippet |
| Vision question answering over documents | Qwen3-VL 8B, Gemma 4 (E2B and E4B for phones, 12B for workstations) | General vision models are weaker than the dedicated OCR models for pure transcription | 2026-11-18 | https://www.promptquorum.com/power-local-llm/local-vision-models-llava-ollama-2026 | Snippet |
| Finding personal identifiers | GLiNER PII models; OpenAI Privacy Filter (open-weight, 1.5B-parameter mixture-of-experts, released 2026-04-22); GLiNER2-PII (May 2026) | The Privacy Filter claim comes from one blog; verify its license and accuracy on a model card | 2026-11-18 | https://grepture.com/blog/openai-privacy-filter-pii-redaction | Snippet |
| General assistant, small | Gemma 4 12B, gpt-oss-20b (mixture-of-experts, described as a 16 GB-class model), MiMo-V2.6 (6.6 GB) | gpt-oss-20b is a candidate for system RAM on your desktop | 2026-11-18 | https://www.morphllm.com/best-open-source-llm | Snippet |
| General assistant, larger | Qwen3.8-27B (dense, Apache 2.0, about 18 GB file) | Sources disagree on the Qwen version (3.6 versus 3.8). In your desktop's system RAM it would run near the 4 tokens/s ceiling for a 32B dense model in `hardware.md`, so it is a test rather than a daily model | 2026-11-18 | https://benchlm.ai/best/local-llm | Snippet |

## 7. Hubs and leaderboards

| Place | What it is | Read it critically because | As of | Source | Evidence |
|---|---|---|---|---|---|
| Hugging Face (huggingface.co) | The model hub: downloadable model files with model cards (answers your B11 question: yes, files can be downloaded there) | Popularity and download counts are not quality | 2026-11-18 | https://huggingface.co | Not re-opened (long-standing site) |
| Arena (lmarena.ai, also arena.ai) | Blind head-to-head human votes turned into a ranking, with an open-model filter and license column | A newly added model has few votes and a wide confidence interval; votes reflect chat preference, not your task | 2026-11-18 | https://lmarena.ai/leaderboard/text | Snippet |
| Open LLM Leaderboard (Hugging Face) | Archived in June 2024 | Old bookmarks show stale data | 2026-11-18 | https://tech-insider.org/llm-leaderboard-lmarena-vs-artificial-analysis-2026/ | Snippet |
| benchlm.ai, llmrun.dev, similar aggregator sites | Third-party rankings and "best model for device X" pages | Often SEO-style and vendor-linked; they disagreed with each other in this research | 2026-11-18 | https://benchlm.ai/best/local-llm | Snippet |

## 8. Operating system and phone rows

| Item | Fact | As of | Source | Evidence |
|---|---|---|---|---|
| Windows 10 | End of support was 2025-10-14. The consumer Extended Security Updates program protects a device until **2027-10-12**. Recommended for devices that cannot move to Windows 11; enrollment steps are on a linked page | 2026-11-18 | https://support.microsoft.com/en-us/windows/windows-10-support-has-ended-on-october-14-2025-2ca8b313-1946-43d3-b55c-2b95b107f281 | Opened |
| iPhone 17 | Base model has 8 GB of RAM (Pro models 12 GB). An unofficial estimate puts usable model space on an 8 GB phone at about 2 to 3 GB, so a 3B model at 4-bit may fit and a 7B model will not. Apple's most advanced on-device model needs 12 GB and is not available on the base iPhone 17 | 2026-11-18 | https://www.eweek.com/news/iphone-17-apple-intelligence-siri-ai-12gb/ ; https://llmrun.dev/device/iphone-17 | Snippet |

## 9. Gaps in this version

- **Windows and Linux laptops** (for configuration C) were not researched. Only Apple laptops were priced.
- **Battery prices for the ThinkPad** are not here; that belongs to the battery investigation.
- **Mac mini M6 32 GB** and **M4 bandwidth** were not found.
- **Current prices for RTX 5060 Ti 16 GB and RX 9060 XT 16 GB** were not found; the figures above are older.
- **Idle power** of the Mac mini is not stated on Apple's page.
- **Whether a Micro Center is near you** decides which deals apply.
- Several rows are **Snippet** evidence and need opening before a handout relies on them.

## 10. Refresh log

| Date | What was done |
|---|---|
| 2026-11-18 | v1 created from web research, Slickdeals threads and comments. Next refresh: after Cyber Monday (2026-11-30) and before L08, L10, L14, L15, L16, L18 |
