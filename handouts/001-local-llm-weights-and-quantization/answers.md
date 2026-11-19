---
title: "Handout 001: Local LLMs, Weights, and Quantization — Answer Key"
---

# 001: Local LLMs, Weights, and Quantization — Answer Key

## Exercises

### Q1

**Answer: C**

- **A (Incorrect):** A weights file holds weights and the vocabulary and settings that go with them. The interface is a separate part.
- **B (Incorrect):** Quantization is applied to a trained model's stored numbers. It says nothing about how much text the model was trained on.
- **C (Correct):** File size is parameters × bytes per parameter. 8 billion × 2 bytes is 16 GB, and 8 billion × 4.5 bits ÷ 8 is 4.5 GB. Same parameters, fewer bits each.
- **D (Incorrect):** Confuses the parameter count with the file size. Both files are stated to belong to the same 8-billion-parameter model, so the count is the same.


### Q2

**1.** **6 tokens.** `sat` is one piece. `on` has no matching piece, so it becomes `o` and `n`. `the` is one piece. `cats` becomes `cat` and `s`. That is 1 + 2 + 1 + 2 = 6.

**2.** **4 tokens and 2 words.** `breakable` becomes `break`, `able` (2 tokens). `cats` becomes `cat`, `s` (2 tokens).

### Q3

| Statement | Answer |
| --- | --- |
| After two steps the generated text is `my dog barks at`. | **TRUE.** Step 1 picks `barks` (0.45 is the largest). Step 2 picks `at` (0.50 is the largest). |
| The second row is computed using the piece chosen at step 1. | **TRUE.** The row is for the text `my dog barks`, which exists only after step 1 chose `barks`. A different step-1 choice would use a different row. |
| Running these two steps changed the model's weights. | **FALSE.** Inference reads the weights and never rewrites them. Only training changes weights. |
| The model produced the whole reply in a single step. | **FALSE.** Each piece takes one step, so this reply took two. |

### Q4

| Change wanted | Part to replace |
| --- | --- |
| Support for a GPU brand the current program cannot use | **ENGINE.** The engine is the program that does the arithmetic on the hardware, so hardware support lives there. |
| A voice-input button in the window | **INTERFACE.** The interface is where text is collected and shown. |
| A smaller file of the same model, so it fits a smaller machine | **WEIGHTS FILE.** A different quantization level of the same model is a different weights file. The engine and interface stay (assuming the engine reads that format). |

### Q5

| Claim | Holds |
| --- | --- |
| An app whose engine runs on your own machine sends the text you type to another computer. | **SOMETIMES.** The local engine does not send it, but the app or a tool it enables (such as a web search, an update check, or synced history) can. |
| Ordinary inference rewrites the weights file. | **NEVER.** Inference reads the weights. Training is what changes them. |
| A weights file's size on disk changes with the length of the prompt you send. | **NEVER.** The file is fixed. The prompt affects memory used during a run, not the file. |
| A hosted chat service runs its engine on hardware you control. | **NEVER.** If the engine runs on hardware you control, the setup is local by definition. |
| The number of tokens in a text equals its number of words. | **SOMETIMES.** It holds when every word happens to be one vocabulary piece, as with `the cat sat`, and fails for words that split, such as `cats`. |

### Q6

**1.** **24 GB.** 12,000,000,000 × 16 ÷ 8 = 24,000,000,000 bytes.

**2.** **6.75 GB.** 12,000,000,000 × 4.5 ÷ 8 = 6,750,000,000 bytes.

**3.** **4.25 bits per weight and 6.375 GB.** Bits per block = 64 × 4 + 16 = 272. Per weight: 272 ÷ 64 = 4.25. File: 12,000,000,000 × 4.25 ÷ 8 = 6,375,000,000 bytes. A larger block spreads the 16-bit scale over more weights, so the overhead per weight falls from 0.5 bit (blocks of 32) to 0.25 bit.

### Q7

**1.** **F2, F1, F3.** F2 (8B at 4.5 bits) is 4.5 GB. F1 (3B at 16 bits) is 6.0 GB. F3 (8B at 8.5 bits) is 8.5 GB.

**2.**

| Statement | Answer |
| --- | --- |
| Files F2 and F3 hold the same number of parameters. | **TRUE.** Both are 8B. They differ only in the quantization level. |
| The `.gguf` ending tells you each file's quantization level. | **FALSE.** The ending names the format, the container. The level is the `Q4`, `Q8`, or `F16` part. |

### Q8

Needed memory is file size + 1.5 GB, compared with 8 GB.

| File | Fits? |
| --- | --- |
| 8B parameters, Q4 | **FITS.** 8 × 4.5 ÷ 8 = 4.5 GB, plus 1.5 = 6.0 GB. |
| 8B parameters, 8.5 bits per weight | **DOES NOT FIT.** 8.5 GB, plus 1.5 = 10.0 GB. An 8-bit file is 8.5 GB, not 8 GB. |
| 12B parameters, Q4 | **DOES NOT FIT.** 12 × 4.5 ÷ 8 = 6.75 GB, plus 1.5 = 8.25 GB, which is over 8 GB by 0.25 GB. The file alone is under 8 GB, which is the trap. |
| 12B parameters, Q3 | **FITS.** 12 × 3.5 ÷ 8 = 5.25 GB, plus 1.5 = 6.75 GB. |
| 3B parameters, 16 bits per weight | **FITS.** 3 × 16 ÷ 8 = 6.0 GB, plus 1.5 = 7.5 GB. |

### Q9

**S4, S2, S3, S1.** The name gives the parameter count and the level (S4). Those give the file size (S2). Overhead and context memory are added to get the total needed (S3). Only then can the total be compared with the usable memory (S1). Comparing the file size alone with the memory skips S3, which is the usual mistake.

### Q10

**Answer: B**

- **A (Incorrect):** The files differ in parameter count and in precision, not only in size. They are different models.
- **B (Correct):** 8B at 4.5 bits is 4.5 GB, and 3B at 16 bits is 6.0 GB. The smaller file holds more parameters at lower precision. Which answers better is an empirical question for the task, not something file size decides.
- **C (Incorrect):** Nothing is mislabeled. 4.5 GB is the size of 8 billion parameters at 4.5 bits each.
- **D (Incorrect):** Quantization costs some accuracy, but the loss at about 4 bits is usually moderate, and it is a tradeoff to test, not a certainty that the smaller-precision file loses.


### Q11

**1.**

Memory needed = weights + 0.8 GB overhead + 0.125 GB per 1,000 tokens of context. At 32,000 tokens the context memory is 4.0 GB.

| Statement | Answer |
| --- | --- |
| The interface, engine, and weights file all run on the user's own machine. | **TRUE.** The interface was always on the user's device. The change moves the engine and weights onto the machine too. |
| The Q4 file with 32,000 tokens of context fits. | **FALSE.** 4.5 + 0.8 + 4.0 = 9.3 GB, over 8 GB. |
| Switching to a 3.5-bit file lets the 32,000-token setup fit. | **FALSE.** The 3.5-bit file is 8 × 3.5 ÷ 8 = 3.5 GB. 3.5 + 0.8 + 4.0 = 8.3 GB, still over 8 GB. Dropping a full bit saved only 1 GB, and the context term is 4 GB. |
| Switching to a 2.5-bit file lets the 32,000-token setup fit. | **TRUE.** 2.5 + 0.8 + 4.0 = 7.3 GB. It fits, but at the largest accuracy loss of the options, which a task may not tolerate. |
| Keeping the Q4 file and lowering the context to 16,000 tokens fits. | **TRUE.** 4.5 + 0.8 + 2.0 = 7.3 GB. Cutting the oversized term fixed the fit with no accuracy loss. |
| Choosing a lower-bit weights file reduces the context memory in this setup. | **FALSE.** The setup states that context memory is 0.125 GB per 1,000 tokens whichever file is loaded. Fewer bits shrink only the weights term. |
| With a local engine, the text typed into this app can no longer leave the machine through the app. | **FALSE.** Local describes where the engine runs. The app or a tool it enables (a web search, an update check, or synced history) can still send text out. |
| Raising the context from 8,000 to 32,000 tokens changes the weights file's size on disk. | **FALSE.** The file is fixed. Context changes the memory used during a run, not the file. |

**2.** **21 thousand tokens.** (8 − 4.5 − 0.8) ÷ 0.125 = 2.7 ÷ 0.125 = 21.6, so 21 whole thousands (21,000 tokens).

**3.** **29 thousand tokens.** (8 − 3.5 − 0.8) ÷ 0.125 = 3.7 ÷ 0.125 = 29.6, so 29 whole thousands. Going from 4.5 to 3.5 bits bought only 8 thousand tokens, and a 32,000-token context still does not fit.

- **Evidence:** 9.3 GB and 8.3 GB are both over 8 GB; 7.3 GB fits through either a 2.5-bit file or a 16,000-token context.
- **Mechanism and assumptions:** memory needed has three terms. Fewer bits shrink only the weights term. When the context term is the large one, cutting bits barely helps and costs accuracy, while cutting the context fixes the fit directly. This holds under the stated assumption that context memory does not depend on the weights file.
