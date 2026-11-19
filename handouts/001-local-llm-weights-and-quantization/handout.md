---
title: "Handout 001: Local LLMs, Weights, and Quantization"
---

# 001: Local LLMs, Weights, and Quantization

A **large language model (LLM)** is a very large set of learned numbers which, when a program runs them, turn text into a prediction of what text comes next. A **local** LLM runs on hardware you control instead of on a provider's machines. Whether it can run on your hardware depends on one practical question: how many bytes does the model occupy? This handout builds the answer from the bottom. First, what a model is made of and which part of a chat setup lives where. Then why a model is a file of a particular size, how **quantization** shrinks that file, and how to check whether a file fits on a machine.

Only arithmetic and the idea of a byte are assumed. Every other term is defined where it first matters.

## Terms to keep straight

| Term | What it means | Closest familiar idea | Where the analogy breaks |
| --- | --- | --- | --- |
| **Token** | A piece of text the model reads and writes: a whole word, part of a word, or punctuation. | A syllable | Pieces come from a fixed list (the vocabulary), so one word can be one piece or several, and counts differ between tokenizers. |
| **Parameter** (also **weight**) | One learned number inside the model. "8B" means 8 billion of them. | A dial setting | Nobody sets them by hand. Training adjusts them, and inference only reads them. |
| **Training** | The process that adjusts the weights from text examples. | Studying for an exam | Done once, by the model's maker. Your chat never does it. |
| **Inference** | Running the finished model to produce text. | Taking the exam | The weights stay fixed throughout. |
| **Weights file** | The file holding the weights, plus the vocabulary and settings that go with them. | A saved game | The file is only data. Nothing runs until a program reads it. |
| **Inference engine** | The program that loads a weights file and does the arithmetic to generate text. | A media player | It supports particular file formats and hardware. It is not the model. |
| **Interface** (front end) | What you type into: a chat window, a terminal, or a web page. | A remote control | It can sit in front of a local engine or a remote service. |
| **Local** | The engine and weights run on hardware you control. | A program installed on your laptop | It says where the arithmetic happens, not that nothing else touches the network. |
| **Quantization** | Storing each weight with fewer bits, trading some accuracy for a smaller file. | Rounding prices to the nearest dollar | Weights are stored in groups, and each group carries extra numbers, so the saving is a little smaller than the bit count suggests. |
| **Format** | The container layout of a weights file (for example GGUF). | A file type such as PDF | The format is not the quantization level. One format holds many levels. |

## How a model produces text

A language model does one job: given the text so far, assign a probability to each possible next piece. **Generation** repeats that job. Pick one piece, add it to the text, and ask again with the longer text.

The text is first cut into tokens by a fixed vocabulary. This toy tokenizer lets you predict the cut exactly. Its vocabulary is `the`, `cat`, `sat`, `un`, `break`, `able`, `s`. For each word, take the longest vocabulary piece that matches the start of what is left. If no piece matches, take one letter. Real tokenizers have tens of thousands of pieces, but the idea is the same.

### Worked example: tokens are not words

Cut `the unbreakable cats sat` with the toy tokenizer. Predict the token count and compare it with the word count.

Predict before reading the result:

#### Exact result

```text
the        ->  the                  1 token
unbreakable ->  un | break | able   3 tokens
cats       ->  cat | s              2 tokens
sat        ->  sat                  1 token
Total: 7 tokens for 4 words
```

Why:

- `unbreakable`: the longest piece matching the start is `un` (`unbreak` is not in the vocabulary). Then `breakable` starts with `break`. What is left, `able`, is a piece.
- `cats`: `cat` is the longest match, and the leftover `s` is a piece.
- A belief that "a token is a word" predicts 4. The answer is 7, and a different vocabulary would give a different count. Token counts belong to a tokenizer, not to the text.

### Worked example: one piece at a time

Suppose a model assigns these next-piece probabilities. In this example the program always takes the most probable piece. Other programs may sample instead.

| Text so far | Next piece: probability |
| --- | --- |
| `the cat` | `sat` 0.5, `ran` 0.3, `is` 0.2 |
| `the cat sat` | `on` 0.6, `down` 0.3, `.` 0.1 |
| `the cat sat on` | `the` 0.7, `a` 0.2, `my` 0.1 |

Predict before reading the result: after three steps, what text exists?

#### Exact result

```text
Step 1: pick sat  (0.5)  ->  the cat sat
Step 2: pick on   (0.6)  ->  the cat sat on
Step 3: pick the  (0.7)  ->  the cat sat on the
```

Why:

- Each row of the table is computed from **all** the text so far. The table for step 2 does not exist until step 1 has chosen `sat`. Had step 1 chosen `ran`, step 2 would use a different table.
- The probabilities in each row sum to 1. The weights produced them, and the weights did not change between steps. Inference reads weights and never rewrites them.
- A belief that "the model looks up a finished sentence" predicts one step. Generation takes one step per piece, so a long reply costs many repeats of the same computation.

## Where each part lives

A chat setup has three parts that people often call "the model": the **weights file**, the **inference engine** that reads it, and the **interface** you type into. The same question can travel two ways.

### Worked example: the same question, two setups

You type `Summarize this email` into a chat window. Setup H sends the text to a hosted service. Setup L uses a local engine and weights file.

| Part | Setup H (hosted) | Setup L (local) |
| --- | --- | --- |
| Interface | Your device | Your device |
| Inference engine | The provider's machines | Your machine |
| Weights file | The provider's storage | Your disk |
| Your email text | Sent over the network to the provider | Stays on your machine, unless something else sends it |

Predict before reading the result: if you replace only one part, which replacement changes the answers you get?

#### Exact result

```text
Replace the interface       -> same answers, different window
Replace the engine          -> same weights, so essentially the same answers; speed and supported file formats may change
Replace the weights file    -> a different model, so different answers
```

Why:

- The weights hold everything the model learned. The engine only does arithmetic with them, and the interface only collects your text and shows the reply.
- "Local" changes the location of the engine and weights. It does not stop an app or a tool the app enables (such as a web search) from sending text out. Whether data leaves depends on what each part calls.
- **Open weights** means the weights file is published for you to download and run, under a license. It does not mean you receive the training data or the code that trained it.

## Why a model is a file of a given size

The weights are the whole model, so file size follows from two numbers.

```text
file size = parameters × bytes per parameter
```

A byte is 8 bits. **Precision** is how many bits store one weight: 16, 8, or 4. This handout uses 1 GB = 10^9 bytes and ignores the small difference from other conventions.

### Worked example: one model at three precisions

Take a model with 8 billion parameters.

Predict before reading the result: how many GB is the file at 16, 8, and 4 bits?

#### Exact result

```text
16 bits = 2 bytes:    8,000,000,000 × 2   = 16,000,000,000 bytes = 16 GB
 8 bits = 1 byte:     8,000,000,000 × 1   =  8,000,000,000 bytes =  8 GB
 4 bits = 0.5 byte:   8,000,000,000 × 0.5 =  4,000,000,000 bytes =  4 GB
```

Why:

- The parameter count never changed. Only the bytes stored per parameter did. "8B" describes how many numbers there are, not how big the file is.
- Halving the bits halves the file. A belief that "an 8B model is an 8 GB file" is right only at 8 bits.
- This is the ideal size. The next section shows why a 4-bit file comes out a little larger.

## Quantization: fewer bits, with bookkeeping

**Quantization** rounds each weight onto a short list of allowed values. A 4-bit weight chooses among 16 values. A single list for the whole model would fit badly, so weights are stored in small **blocks**, and each block keeps its own **scale** (a number that stretches the 16 allowed values to the range of that block's weights). Those scale numbers are extra bits.

Take one simple scheme: blocks of 32 weights, 4 bits per weight, and one 16-bit scale per block.

```text
bits per block   = 32 × 4 + 16 = 144
bits per weight  = 144 ÷ 32   = 4.5
```

So "4-bit" averages 4.5 bits per weight here. Real schemes differ in block size and extras. This handout uses **4.5 bits** for a typical 4-bit file and **8.5 bits** for a typical 8-bit file (8 + 16 ÷ 32), and every exercise states the figure it needs.

### Worked example: why 4.5 GB and not 4 GB

Predict before reading the result: the file size of the 8-billion-parameter model at 4.5 bits per weight.

#### Exact result

```text
8,000,000,000 × 4.5 ÷ 8 = 4,500,000,000 bytes = 4.5 GB
```

Why:

- The 0.5 extra bit per weight is the scale stored for each block of 32. Ideal 4-bit math says 4 GB, and the real file is 12.5% larger.
- A 16-bit file has no scale to store, so its size is exactly parameters × 2 bytes.

### What lower precision costs

Fewer bits means a coarser rounding of every weight, so answers drift from the full-precision model. Typical reports for a mainstream model: 8-bit loses very little, about 4-bit loses a moderate amount that is often acceptable, and below about 3 bits the loss becomes noticeable and keeps growing. These are rules of thumb. How much a particular task suffers has to be tested on that task (a later handout teaches how). A smaller file also reads faster from memory, which a later handout covers.

| Choice | What you gain | What you give up |
| --- | --- | --- |
| Higher precision (16 or 8 bits) | Closest to the original model's behavior. | Larger file, more memory needed. |
| About 4 bits | A file roughly a quarter the size of 16-bit. | Some accuracy loss, usually small enough to accept. |
| Below 3 bits | A very small file. | Noticeable accuracy loss that a task may not tolerate. |

### Reading a file name

A file name such as `tern-8B-instruct-Q4.gguf` carries four facts.

| Piece | Tells you |
| --- | --- |
| `tern` | The model family (a made-up name here). |
| `8B` | The parameter count: 8 billion. |
| `instruct` | The kind: tuned to follow requests. A `base` model only continues text. |
| `Q4` | The quantization level: about 4 bits per weight. |
| `.gguf` | The **format**, the container layout. It tells you which programs can read the file, not the precision. |

Two files of one model in the same format can have different sizes (different levels). Two files at one level in different formats can have nearly the same size (different containers). The format and the level are separate choices.

## Does it fit?

The file size is not the memory a run needs. The engine also needs working memory, and the text being processed needs space (the cache of context, taught in a later handout). A fit check adds them and compares the total with the memory you can actually use.

```text
memory needed = file size + overhead + context memory
fits when memory needed ≤ usable memory
```

### Worked example: one model, two machines

**Assume that** this engine needs 1.5 GB for overhead and context together, and that Machine A has 8 GB of usable GPU memory while Machine B has 6 GB. The model has 8 billion parameters.

Predict before reading the result: which of the 16-bit (16 GB), 8.5-bit (8.5 GB), and 4.5-bit (4.5 GB) files fit on each machine?

#### Exact result

```text
File        Needed                 Machine A (8 GB)   Machine B (6 GB)
16-bit      16.0 + 1.5 = 17.5 GB   no                 no
8.5-bit      8.5 + 1.5 = 10.0 GB   no                 no
4.5-bit      4.5 + 1.5 =  6.0 GB   yes                yes (exactly 6.0 ≤ 6.0, no headroom)
```

Why:

- The 8-bit file is 8.5 GB, so it does not fit even on Machine A. A belief that "8-bit means an 8 GB file" would wrongly say it fits.
- On Machine B the 4.5-bit file fits with nothing to spare. Any extra context memory would break the fit, so real plans leave headroom.
- Lowering the bits is not free: the file that fits is the one with the largest accuracy loss. The model that fits is not automatically the model you should pick.

### Worked example: the smaller file is the bigger model

Compare `tern-3B-instruct-F16.gguf` (3 billion parameters at 16 bits) with `tern-8B-instruct-Q4.gguf` (8 billion at 4.5 bits).

Predict before reading the result: which file is bigger, and which model has more parameters?

#### Exact result

```text
3B at 16 bits  : 3,000,000,000 × 16  ÷ 8 = 6.0 GB
8B at 4.5 bits : 8,000,000,000 × 4.5 ÷ 8 = 4.5 GB
```

Why:

- The 3B file is bigger (6.0 GB) and the 8B file has more parameters. File size reflects parameters times bits, and neither alone.
- Which one answers a task better is not settled by file size. Larger models at about 4 bits are often competitive with smaller models at full precision, but the only reliable way to know is to test on the task.

## Mistakes, edge cases & strategies

| Trap | Correct reasoning | Strategy |
| --- | --- | --- |
| "A 7B model is a 7 GB file." | Size is parameters × bytes per parameter. | Always multiply by the bytes per weight of the specific file. |
| "4-bit means exactly 4 bits per weight." | Block scales add extra bits, so 4-bit averages about 4.5. | Use the stated bits per weight, not the label. |
| "GGUF (or any format) tells me the precision." | The format is a container. The level is a separate part of the name. | Read the quantization label for precision and the extension for the format. |
| "The smaller file is the smaller model." | Fewer bits shrink the file without removing parameters. | Compare parameters and bits separately. |
| "If the file fits, the model runs." | Overhead and context memory add to the file size. | Add file + overhead + context and keep headroom. |
| "Fewer bits fixes any memory problem." | Fewer bits only shrinks the weights. Context memory stays, and accuracy falls. | Find which term of the sum is too big before changing it. |
| "Local means my text never leaves." | Local describes where the engine runs. Apps and tools can still call out. | Ask what each part of the setup sends. |
| "Tokens are words." | The tokenizer's vocabulary decides the cut. | Count with the tokenizer, never by words. |
| "The model writes the whole reply at once." | Generation picks one piece per step from all the text so far. | Think of the reply as many repeated steps. |

## Exercises

### Q1

A download page lists two files for one 8-billion-parameter model: one is 16 GB and the other is 4.5 GB. Which statement explains the difference?

- A. The 16 GB file also contains the chat window.
- B. The 4.5 GB file was trained on less text.
- C. Both files hold the same parameters, stored with different numbers of bits each.
- D. The 4.5 GB file contains fewer parameters.

### Q2

Use the toy tokenizer from this handout. Its vocabulary is `the`, `cat`, `sat`, `un`, `break`, `able`, `s`. For each word, take the longest vocabulary piece that matches the start of what is left, and take one letter when no piece matches.

**1.** How many tokens does `sat on the cats` become? Write the count.

**2.** How many tokens does `breakable cats` become, and how many words does it contain? Write both numbers.

### Q3

A model gives these next-piece probabilities. The program always picks the most probable piece.

| Text so far | Next piece: probability |
| --- | --- |
| `my dog` | `barks` 0.45, `runs` 0.35, `is` 0.20 |
| `my dog barks` | `at` 0.50, `loudly` 0.30, `.` 0.20 |

Mark each statement.

| Statement | Answer |
| --- | --- |
| After two steps the generated text is `my dog barks at`. | ```options
○ TRUE
○ FALSE
``` |
| The second row is computed using the piece chosen at step 1. | ```options
○ TRUE
○ FALSE
``` |
| Running these two steps changed the model's weights. | ```options
○ TRUE
○ FALSE
``` |
| The model produced the whole reply in a single step. | ```options
○ TRUE
○ FALSE
``` |

### Q4

An app has a chat window (interface), an inference engine, and a weights file. For each change, mark the one part that must be replaced.

| Change wanted | Part to replace |
| --- | --- |
| Support for a GPU brand the current program cannot use | ```options
○ WEIGHTS FILE
○ ENGINE
○ INTERFACE
``` |
| A voice-input button in the window | ```options
○ WEIGHTS FILE
○ ENGINE
○ INTERFACE
``` |
| A smaller file of the same model, so it fits a smaller machine | ```options
○ WEIGHTS FILE
○ ENGINE
○ INTERFACE
``` |

### Q5

For each claim, mark whether it holds ALWAYS, SOMETIMES, or NEVER in the stated situation.

| Claim | Holds |
| --- | --- |
| An app whose engine runs on your own machine sends the text you type to another computer. | ```options
○ ALWAYS
○ SOMETIMES
○ NEVER
``` |
| Ordinary inference rewrites the weights file. | ```options
○ ALWAYS
○ SOMETIMES
○ NEVER
``` |
| A weights file's size on disk changes with the length of the prompt you send. | ```options
○ ALWAYS
○ SOMETIMES
○ NEVER
``` |
| A hosted chat service runs its engine on hardware you control. | ```options
○ ALWAYS
○ SOMETIMES
○ NEVER
``` |
| The number of tokens in a text equals its number of words. | ```options
○ ALWAYS
○ SOMETIMES
○ NEVER
``` |

### Q6

A model has 12 billion parameters. Use 1 GB = 10^9 bytes and write each answer in GB.

**1.** What is the file size at 16 bits per weight?

**2.** What is the file size at an average of 4.5 bits per weight?

**3.** Another scheme stores blocks of 64 weights, 4 bits per weight, and one 16-bit scale per block. What is its average bits per weight, and what is the 12-billion-parameter file size in GB?

### Q7

Four files come from one model family. Their sizes follow from parameters and bits. **Assume that** F16 is 16 bits per weight, Q8 is 8.5, and Q4 is 4.5.

| Label | File name |
| --- | --- |
| F1 | `tern-3B-instruct-F16.gguf` |
| F2 | `tern-8B-instruct-Q4.gguf` |
| F3 | `tern-8B-instruct-Q8.gguf` |

**1.** Write the labels in order from smallest file to largest file.

**2.** Mark each statement.

| Statement | Answer |
| --- | --- |
| Files F2 and F3 hold the same number of parameters. | ```options
○ TRUE
○ FALSE
``` |
| The `.gguf` ending tells you each file's quantization level. | ```options
○ TRUE
○ FALSE
``` |

### Q8

**Assume that** this engine needs 1.5 GB for overhead and context together, and the machine has 8 GB of usable GPU memory. A file fits when its size plus 1.5 GB is at most 8 GB. Use 4.5 bits per weight for Q4 and 3.5 bits per weight for Q3.

| File | Fits? |
| --- | --- |
| 8B parameters, Q4 | ```options
○ FITS
○ DOES NOT FIT
``` |
| 8B parameters, 8.5 bits per weight | ```options
○ FITS
○ DOES NOT FIT
``` |
| 12B parameters, Q4 | ```options
○ FITS
○ DOES NOT FIT
``` |
| 12B parameters, Q3 | ```options
○ FITS
○ DOES NOT FIT
``` |
| 3B parameters, 16 bits per weight | ```options
○ FITS
○ DOES NOT FIT
``` |

### Q9

Put the steps of a fit check in order, from first to last. Write the labels.

| Label | Step |
| --- | --- |
| S1 | Compare the total with the usable memory. |
| S2 | Compute the file size from the parameter count and the bits per weight. |
| S3 | Add the overhead and the context memory to the file size. |
| S4 | Read the parameter count and the quantization level from the file name. |

### Q10

A teammate says: "`tern-8B-instruct-Q4.gguf` is 4.5 GB and `tern-3B-instruct-F16.gguf` is 6.0 GB, so the 4.5 GB file is the smaller model and must answer worse." Which correction is best?

- A. The two files must answer identically, since only their sizes differ.
- B. The 4.5 GB file has more parameters (8B against 3B) at fewer bits each, so test both on the task.
- C. The 4.5 GB file is really a 3B model whose name was mislabeled.
- D. Quantization ruins a model, so the 6.0 GB file is certain to be better.

### Q11

A chat app currently sends every request to a hosted service. A teammate changes it to run an 8-billion-parameter model on a local machine with 8 GB of usable GPU memory, and also raises the context to 32,000 tokens.

**Assume that** the weights are stored at an average of 4.5 bits per weight (Q4), runtime overhead is 0.8 GB, and context memory is 0.125 GB per 1,000 tokens whichever weights file is loaded. A setup fits when weights + overhead + context memory is at most 8 GB.

**1.** Mark each statement for the changed app.

| Statement | Answer |
| --- | --- |
| The interface, engine, and weights file all run on the user's own machine. | ```options
○ TRUE
○ FALSE
``` |
| The Q4 file with 32,000 tokens of context fits. | ```options
○ TRUE
○ FALSE
``` |
| Switching to a 3.5-bit file lets the 32,000-token setup fit. | ```options
○ TRUE
○ FALSE
``` |
| Switching to a 2.5-bit file lets the 32,000-token setup fit. | ```options
○ TRUE
○ FALSE
``` |
| Keeping the Q4 file and lowering the context to 16,000 tokens fits. | ```options
○ TRUE
○ FALSE
``` |
| Choosing a lower-bit weights file reduces the context memory in this setup. | ```options
○ TRUE
○ FALSE
``` |
| With a local engine, the text typed into this app can no longer leave the machine through the app. | ```options
○ TRUE
○ FALSE
``` |
| Raising the context from 8,000 to 32,000 tokens changes the weights file's size on disk. | ```options
○ TRUE
○ FALSE
``` |

**2.** With the Q4 file, what is the largest context, in whole thousands of tokens, that fits?

**3.** With the 3.5-bit file, what is the largest context, in whole thousands of tokens, that fits?

#### Hints

- Split the memory total into the part that depends on the bits per weight and the part that depends on the context length, and compute each total before deciding.
