# The Complete Developer's Guide to Running LLMs Locally: From Ollama to Production

- **Source:** https://www.sitepoint.com/local-llms-complete-guide/
- **Author/Publisher:** SitePoint

---

Table of Contents

* [Table of Contents](#h-table-of-contents)
* [Why Run LLMs Locally in 2025?](#h-why-run-llms-locally-in-2025)
* [Hardware Requirements Demystified](#h-hardware-requirements-demystified)
* [Understanding Model Formats and Quantization](#h-understanding-model-formats-and-quantization)
* [Local LLM Tools Compared](#h-local-llm-tools-compared)
* [Getting Started with Ollama (Step-by-Step)](#h-getting-started-with-ollama-step-by-step)
* [Building a Local RAG Application](#h-building-a-local-rag-application)
* [Deploying Local LLMs in Production with Docker](#h-deploying-local-llms-in-production-with-docker)
* [Optimization and Troubleshooting](#h-optimization-and-troubleshooting)
* [The Local LLM Ecosystem in Motion](#h-the-local-llm-ecosystem-in-motion)

Table of Contents

* [Table of Contents](#h-table-of-contents)
* [Why Run LLMs Locally in 2025?](#h-why-run-llms-locally-in-2025)
* [Hardware Requirements Demystified](#h-hardware-requirements-demystified)
* [Understanding Model Formats and Quantization](#h-understanding-model-formats-and-quantization)
* [Local LLM Tools Compared](#h-local-llm-tools-compared)
* [Getting Started with Ollama (Step-by-Step)](#h-getting-started-with-ollama-step-by-step)
* [Building a Local RAG Application](#h-building-a-local-rag-application)
* [Deploying Local LLMs in Production with Docker](#h-deploying-local-llms-in-production-with-docker)
* [Optimization and Troubleshooting](#h-optimization-and-troubleshooting)
* [The Local LLM Ecosystem in Motion](#h-the-local-llm-ecosystem-in-motion)

# The Complete Developer's Guide to Running LLMs Locally: From Ollama to Production

[![SitePoint Team](https://uploads.sitepoint.com/wp-content/uploads/2020/03/1584945800Group-5.png)](/author/sitepointteam/)

[SitePoint Team](/author/sitepointteam/)

Published in

[AI](/ai/)·[Computing](/computing/)·[Programming](/programming/)·

February 25, 2026

## The AI briefing for Developers

Stay up to date with AI tools, model releases, and developer workflows that matter.

Weekly. Free. One click to leave.

Share this article

![The Complete Developer's Guide to Running LLMs Locally: From Ollama to Production](https://uploads.sitepoint.com/wp-content/uploads/large_data_243df0993a.png)

Running LLMs locally has shifted from a niche hobby to a legitimate production strategy in 2025. Developers who need to keep proprietary code off third-party servers, eliminate per-token costs, or build AI features that work offline now have a mature local LLM tooling ecosystem to draw from. This guide covers the full stack: hardware sizing, model formats, a comparison of eight tools including Ollama, LM Studio, llama.cpp, and LocalAI, plus step-by-step tutorials for building a local RAG application and deploying it in Docker with GPU passthrough.

## Table of Contents

* [Why Run LLMs Locally in 2025?](#toc-why-run-llms-locally-in-2025)
* [Hardware Requirements Demystified](#toc-hardware-requirements-demystified)
* [Understanding Model Formats and Quantization](#toc-understanding-model-formats-and-quantization)
* [Local LLM Tools Compared](#toc-local-llm-tools-compared)
* [Getting Started with Ollama (Step-by-Step)](#toc-getting-started-with-ollama)
* [Building a Local RAG Application](#toc-building-a-local-rag-application)
* [Deploying Local LLMs in Production with Docker](#toc-deploying-local-llms-in-production-with-docker)
* [Optimization and Troubleshooting](#toc-optimization-and-troubleshooting)
* [The Local LLM Ecosystem in Motion](#toc-the-local-llm-ecosystem-in-motion)

## Why Run LLMs Locally in 2025?

### Privacy, Cost, and Control

The most compelling reason to run inference locally is straightforward: data never leaves the machine. For organizations operating under GDPR, HIPAA, or internal intellectual property policies, local inference eliminates the data-in-transit and third-party processor compliance vectors, though full HIPAA or GDPR compliance requires additional physical, access, and audit controls. No data processing agreements to negotiate, no third-party sub-processors to audit, no ambiguity about whether training data gets retained upstream.

The economics are equally direct. After the upfront hardware investment, the marginal cost per token drops to the electricity bill. For workloads that process millions of tokens daily, such as code completion across a development team or document summarization pipelines, this eliminates API spend entirely. Rate limits, API deprecation notices, vendor lock-in tying application logic to a specific provider's prompt format or model versioning scheme: none of these apply when you own the inference stack.

Offline availability matters more than many teams initially expect. Air-gapped environments, field deployments, and unreliable network conditions all become non-issues when the model runs on the same hardware as the application.

### When Local Beats the Cloud (and When It Doesn't)

Local LLMs excel at specific workload profiles: code completion with full repository context, retrieval-augmented generation over private documents, CI/CD pipeline integration for automated code review, and rapid prototyping where iteration speed matters more than frontier-model quality.

The honest limitations deserve equal weight. A locally-run 8B parameter model will not match GPT-4o or Claude 3.5 Sonnet on complex reasoning tasks. Models above 70B parameters require hardware investments north of $1,200 (see Production tier below) and often well beyond what most individual developers or small teams budget for.

> The decision breaks down along three axes. Latency sensitivity favors local: round-trip to a local server running on the same machine or LAN consistently beats cloud API calls for real-time completion. Data sensitivity favors local: if data cannot leave the network, there is no alternative. Budget favors cloud when usage is sporadic and hardware amortization does not make sense.

## Hardware Requirements Demystified

### GPU Memory Is the Bottleneck

The single most important number for local LLM inference is available VRAM. At Q4\_K\_M quantization, plan for approximately 0.6-0.7 GB per billion parameters as a working estimate. More aggressive quantization levels like Q2 can reach ~0.4 GB per billion parameters, but at significant quality cost. The exact figure depends on context length and quantization level.

| Model Size | Min VRAM (Q4) | Recommended GPU | Notes |
| --- | --- | --- | --- |
| 7-8B | 4-6 GB | RTX 3060 12GB, M1 16GB | Comfortable on consumer hardware |
| 13B | 8-10 GB | RTX 4060 Ti 16GB | Sweet spot for quality vs. cost |
| 34B | 18-22 GB | RTX 4090 24GB | Tight fit on single consumer GPU |
| 70B | 35-40 GB | 2x RTX 4090 or A6000 48GB | Requires multi-GPU or heavy CPU offload |

GPU ecosystem support varies significantly. NVIDIA GPUs with CUDA remain the best-supported option across every tool in this guide. AMD GPUs via ROCm have made real progress, with llama.cpp and Ollama offering functional ROCm support on Linux, though driver setup remains more involved and not all quantization kernels are optimized. Apple Silicon with Metal acceleration is genuinely excellent for the MacBook and Mac Studio form factors, where unified memory means the "VRAM" is shared system memory.

### CPU-Only and Hybrid Inference

CPU-only inference is viable for 7-8B models running batch offline tasks where tokens-per-second is not critical. Expect roughly 5-15 tokens/sec on a modern desktop CPU with AVX2 support, compared to 40-80+ tokens/sec on a midrange GPU. Actual figures depend heavily on the specific CPU, model, and quantization level.

For models that exceed available VRAM, llama.cpp supports hybrid GPU+CPU splitting via the `--n-gpu-layers` flag (or `num_gpu` in Ollama). Layers that do not fit in VRAM spill to system RAM. This works, but every layer on CPU dramatically reduces throughput. System RAM effectively acts as overflow VRAM, so 32GB of system RAM is the practical minimum for hybrid inference with larger models.

### Recommended Configurations

The **budget tier (~$0)** requires no additional hardware. An M1 or M2 MacBook with 16GB unified memory runs 7-8B models at roughly 30-50 tokens/sec via Metal acceleration (varies with model and thermal conditions), making it the best zero-cost entry point.

A **mid-tier setup (~$400)** centers on an RTX 4060 Ti 16GB, which handles 13B models entirely in VRAM and runs 8B models with generous context windows. For most developers, this hits the price-to-performance sweet spot.

At the **production tier (~$1,200+)**, an RTX 4090 with 24GB VRAM handles 34B models and runs 70B models with significant CPU offloading. Dual-GPU setups or datacenter cards like the A6000 48GB open up full 70B inference without layer splitting.

## Understanding Model Formats and Quantization

### GGUF, GPTQ, AWQ, and EXL2 Explained

GGUF is the universal format for local LLM inference. Developed as part of the llama.cpp ecosystem, it is a single-file format that bundles model weights, tokenizer, and metadata. Ollama, LM Studio, GPT4All, Jan, and koboldcpp all consume GGUF files directly. If a tool runs locally, it almost certainly supports GGUF.

GPTQ and AWQ are GPU-centric quantization formats designed for tools like vLLM and Hugging Face's text-generation-inference. They require the full model to fit in GPU VRAM and do not support CPU offloading, but they can deliver higher throughput for pure-GPU deployments. EXL2 is the format used by ExLlamaV2, offering fine-grained per-layer quantization control.

The selection logic is hardware-driven: if the model fits entirely in VRAM and the deployment tool supports it, GPTQ or AWQ may offer better throughput. For everything else, GGUF is the correct default.

### Quantization Levels and Quality Trade-offs

GGUF quantization levels range from Q2\_K (aggressive, lossy) through Q8\_0 (near-lossless). Each step down reduces file size and VRAM requirements while increasing perplexity (the standard measure of quality degradation).

**Q4\_K\_M** is the widely recommended default. It offers roughly 50% size reduction from the full FP16 weights with minimal perplexity increase. Q5\_K\_M provides a marginal quality bump at ~15% more VRAM. Below Q3, quality degradation becomes noticeable in generation coherence.

Pre-quantized models are available on Hugging Face from community quantizers. The user "bartowski" maintains a comprehensive and regularly updated collection of GGUF quantizations for popular models. The "TheBloke" repository, which was previously the go-to source, has been less frequently updated since early 2024 -- verify its activity before relying on it for the latest models.

## Local LLM Tools Compared

### Comparison Table

| Tool | Min VRAM | Model Formats | OpenAI-Compatible API | OS Support | GPU Backends | Quantization | Ease of Setup (1-5) | Tokens/sec single-req (Llama 3.1 8B Q4\_K\_M, RTX 4060 Ti 16GB, estimated) |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| **Ollama** | ~4 GB (8B Q4) | GGUF, safetensors import | Yes | macOS, Linux, Windows | CUDA, ROCm, Metal | Q2-Q8 GGUF | 5 | ~55-65 |
| **LM Studio** | ~4 GB (8B Q4) | GGUF | Yes | macOS, Linux, Windows | CUDA, Metal, Vulkan | Q2-Q8 GGUF | 5 | ~50-60 |
| **llama.cpp** | ~4 GB (8B Q4) | GGUF | Yes (server mode) | macOS, Linux, Windows | CUDA, ROCm, Metal, Vulkan, SYCL | Q2-Q8 GGUF, imatrix | 2 | ~60-70 |
| **LocalAI** | ~4 GB (8B Q4) | GGUF, GPTQ, diffusers | Yes (broad parity) | Linux, macOS (Docker) | CUDA, ROCm, Metal | Multiple | 3 | ~45-55 |
| **GPT4All** | ~4 GB (8B Q4) | GGUF | Limited | macOS, Linux, Windows | CUDA, Metal | Q4-Q8 GGUF | 5 | ~40-50 |
| **vLLM** | ~8 GB (8B FP16) | safetensors, GPTQ, AWQ | Yes | Linux | CUDA, ROCm | GPTQ, AWQ, FP8 | 2 | ~80-100 (batched, concurrent); ~40-60 (single-request) |
| **Jan** | ~4 GB (8B Q4) | GGUF | Yes | macOS, Linux, Windows | CUDA, Metal, Vulkan | Q2-Q8 GGUF | 5 | ~45-55 |
| **koboldcpp** | ~4 GB (8B Q4) | GGUF | Partial (KoboldAI API) | macOS, Linux, Windows | CUDA, ROCm, Vulkan, CLBlast | Q2-Q8 GGUF | 4 | ~55-65 |

*Note: Tokens/sec figures are estimated ranges, not controlled benchmarks. All figures except vLLM reflect single-request inference. vLLM's ~80-100 figure reflects continuous batching with concurrent requests; its single-request latency is in the ~40-60 range, comparable to other tools. Direct comparison between vLLM's batched throughput and other tools' single-request figures is not meaningful for single-user local use.*

### Ollama: The Developer's Default

Ollama provides a single-binary installation with a built-in model registry, automatic GPU detection, and an OpenAI-compatible REST API out of the box. Model management is reduced to `ollama pull` and `ollama run`. It supports multi-model switching, custom Modelfiles for system prompts and parameter tuning, and exposes both `/api/chat` and `/api/generate` endpoints. For most developers starting with local LLMs, Ollama is the right first tool.

### LM Studio: GUI-First with an API

LM Studio offers a visual model browser that searches Hugging Face directly, one-click model downloads with quantization level selection, and a built-in chat interface. It also exposes a local server with an OpenAI-compatible API. LM Studio is the fastest path from "I want to try a model" to actually running it for developers who prefer graphical interfaces or need to evaluate multiple models quickly.

### llama.cpp: The Performance Foundation

llama.cpp is the C/C++ inference engine that Ollama is built directly on top of, and that LM Studio uses as its primary backend. Running it directly provides maximum control: custom compilation flags, fine-grained layer offloading, server mode with concurrent request handling, and access to the newest quantization methods before they propagate to higher-level tools. The trade-off is a steeper setup curve, particularly when compiling with GPU support.

### LocalAI: The Self-Hosted OpenAI Drop-In

LocalAI is a Docker-native project that aims for broad OpenAI API parity, including embeddings, image generation (via Stable Diffusion backends), transcription, and text-to-speech alongside LLM chat completions. It supports GGUF and GPTQ models and allows preloading model configurations. For teams running Docker-based infrastructure that need a single API gateway covering multiple AI modalities, LocalAI fills a gap that Ollama does not.

## Getting Started with Ollama (Step-by-Step)

### Installation Across Platforms

```
# macOS / Linux
# WARNING: Pipe-to-shell installs execute remote code without prior inspection.
# If your security policy prohibits this, download the binary directly from
# https://github.com/ollama/ollama/releases and verify the SHA256 checksum.
curl -fsSL https://ollama.com/install.sh | sh
```

```
# Windows — download installer from https://ollama.com/download
# or via winget:
winget install Ollama.Ollama

# Docker
docker pull ollama/ollama
docker run -d -v ollama:/root/.ollama -p 11434:11434 --name ollama ollama/ollama
```

### Pulling and Running Your First Model

```
# Pull a specific quantization
# Verify available tags with: ollama show llama3.1
# or browse https://ollama.com/library/llama3.1
ollama pull llama3.1:8b-instruct-q4_K_M

# Start an interactive chat session
ollama run llama3.1:8b-instruct-q4_K_M
>>> What is retrieval-augmented generation?
# ... model responds ...
>>> /bye
```

The `pull` command downloads the model to `~/.ollama/models` (or the Docker volume). Subsequent `run` commands load from cache. The first token may take a few seconds (sometimes 10-30 seconds for an 8B model) as the model loads into VRAM; subsequent prompts in the same session are near-instant.

### Using the Ollama REST API

```
# Streaming chat completion via curl
curl http://localhost:11434/api/chat -d '{
  "model": "llama3.1:8b-instruct-q4_K_M",
  "messages": [
    {"role": "user", "content": "Explain GGUF format in two sentences."}
  ],
  "stream": true
}'
```

The response arrives as newline-delimited JSON objects, each containing a `message.content` fragment. For non-streaming responses, set `"stream": false`.

```
# Python: calling Ollama's API
import os
import sys
import requests

OLLAMA_HOST = os.environ.get("OLLAMA_HOST", "http://localhost:11434")

response = requests.post(
    f"{OLLAMA_HOST}/api/chat",
    json={
        "model": "llama3.1:8b-instruct-q4_K_M",
        "messages": [{"role": "user", "content": "What is quantization?"}],
        "stream": False,
    },
    timeout=300,
)

if response.status_code != 200:
    print(f"Ollama API error {response.status_code}: {response.text}", file=sys.stderr)
    sys.exit(1)

payload = response.json()
if "error" in payload:
    print(f"Model error: {payload['error']}", file=sys.stderr)
    sys.exit(1)

print(payload["message"]["content"])
```

### Managing Models and Custom Modelfiles

```
# List downloaded models
ollama list

# Remove a model
ollama rm llama3.1:8b-instruct-q4_K_M

# Copy/rename a model
ollama cp llama3.1:8b-instruct-q4_K_M my-assistant
```

Custom Modelfiles allow setting persistent system prompts and inference parameters:

```
# Modelfile
FROM llama3.1:8b-instruct-q4_K_M
SYSTEM "You are a senior Python developer. Respond with concise, production-ready code. Always include error handling."
PARAMETER temperature 0.3
PARAMETER num_ctx 4096
```

```
# Create and run the custom model
ollama create python-assistant -f Modelfile
ollama run python-assistant
```

This creates a named model variant that persists across sessions with the specified system prompt and parameter overrides baked in.

## Building a Local RAG Application

### Architecture Overview

The RAG pipeline runs entirely locally. Ollama's local embedding model chunks and embeds the documents. ChromaDB, an embedded vector database, stores the resulting vectors. A local LLM running through Ollama answers retrieval-augmented queries. No data leaves the machine at any point.

The flow works like this: the pipeline loads documents and splits them into chunks. `nomic-embed-text`, running in Ollama, embeds each chunk. ChromaDB stores the embeddings. At query time, the pipeline embeds the user's question, retrieves the top-k most similar chunks, injects those chunks into a prompt template, and sends the assembled prompt to Llama 3.1 for generation.

### Prerequisites

* Python 3.10-3.12
* Ollama installed and running (`ollama serve`)
* Both `nomic-embed-text` and `llama3.1:8b-instruct-q4_K_M` pulled before running scripts
* A test PDF at `./internal_docs.pdf`

### Setting Up the Environment

```
# Pin to tested versions to avoid breaking API changes
pip install langchain==0.3.25 langchain-community==0.3.24 \
    langchain-ollama==0.3.3 langchain-chroma==0.1.4 \
    chromadb==0.6.3 pypdf==5.5.0
```

Pull the embedding model separately:

```
ollama pull nomic-embed-text
ollama pull llama3.1:8b-instruct-q4_K_M
```

### Ingesting Documents and Generating Embeddings

```
import os
import sys
from langchain_community.document_loaders import PyPDFLoader
from langchain.text_splitter import RecursiveCharacterTextSplitter
from langchain_ollama import OllamaEmbeddings
from langchain_chroma import Chroma

OLLAMA_HOST = os.environ.get("OLLAMA_HOST", "http://localhost:11434")
PDF_PATH = os.environ.get("PDF_PATH", "internal_docs.pdf")
CHROMA_PERSIST_DIR = os.environ.get("CHROMA_PERSIST_DIR", "./chroma_db")

if __name__ == "__main__":
    # Load and chunk the PDF
    try:
        loader = PyPDFLoader(PDF_PATH)
        documents = loader.load()
    except FileNotFoundError:
        print(f"ERROR: PDF not found at '{PDF_PATH}'", file=sys.stderr)
        sys.exit(1)
    except Exception as e:
        print(f"ERROR: Failed to load PDF: {e}", file=sys.stderr)
        sys.exit(1)

    if not documents:
        print("ERROR: PDF loaded zero pages. Check file integrity.", file=sys.stderr)
        sys.exit(1)

    splitter = RecursiveCharacterTextSplitter(
        chunk_size=1000,
        chunk_overlap=200,
        separators=["

", "
", ". ", " ", ""]
    )
    chunks = splitter.split_documents(documents)
    print(f"Loaded {len(documents)} pages → {len(chunks)} chunks")

    # Embed and store in ChromaDB
    # In chromadb >=0.4, persistence is automatic when persist_directory is set;
    # no separate .persist() call is needed.
    embeddings = OllamaEmbeddings(
        model="nomic-embed-text",
        base_url=OLLAMA_HOST,
    )
    vectorstore = Chroma.from_documents(
        documents=chunks,
        embedding=embeddings,
        persist_directory=CHROMA_PERSIST_DIR,
    )
    print(f"Stored {len(chunks)} chunks in ChromaDB")
```

### Querying with Context-Augmented Generation

```
import os
import sys
import requests
from langchain_ollama import OllamaEmbeddings
from langchain_chroma import Chroma

OLLAMA_HOST = os.environ.get("OLLAMA_HOST", "http://localhost:11434")
CHROMA_PERSIST_DIR = os.environ.get("CHROMA_PERSIST_DIR", "./chroma_db")

# Maximum characters to inject into the prompt context to avoid exceeding the
# model's num_ctx window. Adjust based on your PARAMETER num_ctx setting.
MAX_CONTEXT_CHARS = 3000

if __name__ == "__main__":
    # Reload existing vectorstore (use Chroma() constructor, not from_documents())
    embeddings = OllamaEmbeddings(
        model="nomic-embed-text",
        base_url=OLLAMA_HOST,
    )
    vectorstore = Chroma(
        persist_directory=CHROMA_PERSIST_DIR,
        embedding_function=embeddings,
    )

    # Retrieve relevant chunks
    query = "What is our refund policy for enterprise customers?"
    results = vectorstore.similarity_search(query, k=3)

    # Assemble context with a length guard to stay within the model's context window
    context_parts = []
    total_len = 0
    for doc in results:
        if total_len + len(doc.page_content) > MAX_CONTEXT_CHARS:
            break
        context_parts.append(doc.page_content)
        total_len += len(doc.page_content)
    context = "

".join(context_parts)

    # Generate answer with context
    prompt = f"""Answer the question based only on the following context:

{context}

Question: {query}
Answer:"""

    response = requests.post(
        f"{OLLAMA_HOST}/api/chat",
        json={
            "model": "llama3.1:8b-instruct-q4_K_M",
            "messages": [{"role": "user", "content": prompt}],
            "stream": False,
        },
        timeout=300,
    )

    if response.status_code != 200:
        print(f"Ollama API error {response.status_code}: {response.text}", file=sys.stderr)
        sys.exit(1)

    payload = response.json()
    if "error" in payload:
        print(f"Model error: {payload['error']}", file=sys.stderr)
        sys.exit(1)

    print(payload["message"]["content"])
    print("
--- Sources ---")
    for doc in results:
        print(f"Page {doc.metadata.get('page', 'N/A')}: {doc.page_content[:100]}...")
```

### Performance Tips for Local RAG

Batch embedding calls whenever possible. Embedding documents one at a time incurs per-call overhead from model loading; processing chunks in batches (which LangChain's `from_documents` does by default) reduces wall-clock time by roughly 3-5x for a 500-chunk corpus compared to sequential single-document calls. Measure on your own hardware, but the difference is consistent.

Set `keep_alive` to a longer duration (e.g., `"keep_alive": "30m"` in API calls, or `PARAMETER keep_alive 30m` in a Modelfile) to prevent Ollama from unloading models between requests. The default timeout is 5 minutes; for RAG pipelines processing multiple queries, unload-reload cycles add unnecessary latency.

Use a small, specialized embedding model like `nomic-embed-text` (768 dimensions; verify current download size at https://ollama.com/library/nomic-embed-text) for vector generation rather than using the larger generative model. The embedding model and the generation model can both remain loaded simultaneously if VRAM allows.

## Deploying Local LLMs in Production with Docker

### Why Docker for Local LLM Serving

Docker provides reproducible environments, GPU passthrough via the NVIDIA Container Toolkit, scaling through Docker Compose, and a standardized deployment artifact that works the same on a developer's machine and a production server. For team environments where multiple developers or services consume a shared LLM endpoint, Docker is the natural deployment boundary.

### Prerequisites: NVIDIA Container Toolkit

GPU passthrough in Docker requires the NVIDIA Container Toolkit installed on the host. Without it, containers silently fall back to CPU-only inference. Install it following the official guide at https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html, then verify:

```
# Verify GPU passthrough works
docker run --rm --gpus all nvidia/cuda:12.0-base-ubuntu22.04 nvidia-smi
# Expected: GPU table showing device name and VRAM
```

### Docker Compose with Ollama and NVIDIA GPU Passthrough

This Compose file targets Compose V2 (Docker Engine 23+).

```
# docker-compose.yml
services:
  ollama:
    image: ollama/ollama:latest
    container_name: ollama-server
    ports:
      - "11434:11434"
    volumes:
      - ollama_data:/root/.ollama
    environment:
      - OLLAMA_MAX_LOADED_MODELS=2
      - OLLAMA_NUM_PARALLEL=4  # Reduce to 2 if OOM errors occur; each parallel slot allocates a separate KV-cache
      - OLLAMA_MAX_QUEUE=20    # Caps queued requests to prevent unbounded VRAM exhaustion under load
      - OLLAMA_KEEP_ALIVE=30m
    deploy:
      resources:
        reservations:
          devices:
            - driver: nvidia
              count: 1
              capabilities: [gpu]
    healthcheck:
      test: ["CMD-SHELL", "wget -qO- http://localhost:11434/api/tags || exit 1"]
      interval: 30s
      timeout: 10s
      retries: 5
      start_period: 20s
    restart: unless-stopped

  app:
    build: ./app
    container_name: rag-app
    depends_on:
      ollama:
        condition: service_healthy
    environment:
      - OLLAMA_HOST=http://ollama:11434
    ports:
      - "8000:8000"

volumes:
  ollama_data:
```

> The `ollama/ollama` base image does not include `curl`, so the health check uses `wget` instead, which is available in the image. The `start_period` allows Ollama time to initialize before health check retries begin counting.

The `deploy.resources.reservations` block requires the NVIDIA Container Toolkit to be installed on the host (see prerequisites above). The `OLLAMA_NUM_PARALLEL` setting controls how many concurrent requests Ollama handles. The health check ensures the app service waits until Ollama is actually ready before starting.

After starting the services, pull your models into the running container:

```
docker compose up -d
docker exec ollama-server ollama pull llama3.1:8b-instruct-q4_K_M
docker exec ollama-server ollama pull nomic-embed-text
```

Verify GPU inference is active:

```
docker exec ollama-server ollama ps
# Look for "(GPU)" indicator next to the loaded model
```

### Using LocalAI as an OpenAI-Compatible Gateway

When broader model format support or multi-modal capabilities (image generation, speech-to-text) are required, LocalAI can replace Ollama in the stack. Verify current available image tags at the LocalAI GitHub releases page or container registry before deploying:

```
services:
  localai:
    image: localai/localai:latest-aio-gpu-nvidia-cuda-12
    container_name: localai-server
    ports:
      - "8080:8080"
    volumes:
      - ./models:/build/models
    environment:
      - THREADS=8
      - CONTEXT_SIZE=4096
      - >-
        PRELOAD_MODELS=[{"url":"github:mudler/LocalAI/gallery/llama3.1-8b-instruct.yaml"}]
    deploy:
      resources:
        reservations:
          devices:
            - driver: nvidia
              count: 1
              capabilities: [gpu]
```

**Note:** The `PRELOAD_MODELS` value is a JSON array string. The YAML block scalar (`>-`) ensures the brackets are not misinterpreted by YAML parsers as a YAML sequence. Verify correct parsing with `docker compose config | grep PRELOAD_MODELS`.

### Reverse Proxy, Auth, and Rate Limiting

Exposing a raw LLM API endpoint to a network without authentication is inadvisable. An Nginx reverse proxy provides basic auth and rate limiting:

```
limit_req_zone $binary_remote_addr zone=llm_limit:10m rate=10r/m;

upstream ollama_backend {
    server localhost:11434;
}

# Redirect HTTP to HTTPS to prevent unencrypted credential transmission
server {
    listen 80;
    server_name llm.internal.example.com;
    return 301 https://$host$request_uri;
}

server {
    listen 443 ssl;
    server_name llm.internal.example.com;

    ssl_certificate /etc/ssl/certs/llm.crt;
    ssl_certificate_key /etc/ssl/private/llm.key;

    location /v1/ {
        auth_basic "LLM API";
        auth_basic_user_file /etc/nginx/.htpasswd;

        limit_req zone=llm_limit burst=10 nodelay;

        # Map OpenAI-compatible paths to Ollama's native endpoints.
        # Ollama uses /api/chat (not /api/chat/completions), so a generic
        # rewrite of /v1/* to /api/* would produce 404s for chat requests.
        rewrite ^/v1/chat/completions$ /api/chat break;
        rewrite ^/v1/embeddings$       /api/embeddings break;
        rewrite ^/v1/(.*)$             /api/$1 break;

        # Note: proxy_pass uses plain HTTP on the internal leg. This is
        # acceptable when Ollama runs on localhost. If Ollama is moved to a
        # separate host, switch to HTTPS or use a private network.
        proxy_pass http://ollama_backend;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_buffering off;  # Required for streaming responses
        proxy_read_timeout 300s;
    }
}
```

The `limit_req_zone` directive at the top defines the rate-limiting shared memory zone used inside the `location` block. Without it, Nginx will refuse to start.

The explicit `rewrite` rules map OpenAI-compatible paths to Ollama's native API endpoints. Notably, Ollama's chat endpoint is `/api/chat` (not `/api/chat/completions`), so the specific rewrite for `/v1/chat/completions` is necessary. The fallback rule handles other endpoints generically. Verify routing works after deployment:

```
curl -u user:pass -X POST https://llm.internal.example.com/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{"model":"llama3.1:8b-instruct-q4_K_M","messages":[{"role":"user","content":"ping"}],"stream":false}'
```

Note that `proxy_buffering off` is important for streaming token responses. Without it, Nginx buffers the entire response before forwarding, defeating the purpose of streaming. Be aware that disabling proxy buffering increases per-connection memory use under high concurrency; tune `worker_connections` accordingly.

### Monitoring and Resource Management

Track GPU utilization and VRAM usage with `nvidia-smi` on the host or `ollama ps` to see currently loaded models and their memory footprint. Key environment variables for resource management:

* `OLLAMA_MAX_LOADED_MODELS`: limits how many models stay in VRAM simultaneously (default: 1 on GPU, 1 on CPU)
* `OLLAMA_NUM_PARALLEL`: maximum concurrent request processing per model
* `OLLAMA_MAX_QUEUE`: maximum queued requests before rejecting (default: 512). Set this explicitly alongside `OLLAMA_NUM_PARALLEL` to provide backpressure under load.

For production deployments, pipe Ollama's stdout/stderr to a log aggregation system. Ollama logs model load/unload events, request durations, and token counts, all of which feed into usage monitoring.

## Optimization and Troubleshooting

### Maximizing Inference Speed

Context length directly impacts VRAM usage and speed. The `num_ctx` parameter (default varies by model; verify with `ollama show <model>` -- commonly 4096 for Llama 3.x) allocates a KV-cache proportional to context size. Setting `num_ctx` to 8192 or higher consumes significantly more VRAM and reduces tokens/sec. Set it to the minimum your application actually needs.

Flash Attention reduces the memory overhead of long contexts. llama.cpp supports Flash Attention natively. As of Ollama 0.1.47+, Flash Attention is enabled by default when the backend supports it -- set `OLLAMA_FLASH_ATTENTION=1` explicitly if uncertain. vLLM uses PagedAttention, which achieves similar memory efficiency gains for batched serving.

For high-concurrency scenarios, vLLM's continuous batching architecture processes multiple requests simultaneously with shared KV-cache. Under 10+ concurrent requests, this delivers roughly 3-8x higher aggregate throughput than Ollama's serial processing (compare the table's 80-100 batched figure against the 55-65 single-request range). Note that vLLM's throughput advantage appears primarily under concurrent load; for single-request latency, performance is comparable to other tools. If the deployment serves many concurrent users, vLLM on a CUDA GPU is the throughput-optimized choice, though its setup complexity and Linux-only restriction are real trade-offs.

### Common Issues and Fixes

**"Out of memory" errors:** Reduce `num_ctx`, switch to a smaller quantization (Q4\_K\_M to Q3\_K\_M), or offload more layers to CPU. The error usually means the KV-cache allocation exceeded remaining VRAM after model loading.

**Slow time-to-first-token:** This is typically model loading time. Set `keep_alive` to a longer duration to avoid repeated loads between requests. In Docker Compose, preload the model by running `ollama pull` in an init container or startup script.

**Garbled or incoherent output:** Usually a chat template mismatch. Each model family expects a specific prompt format. If using a custom Modelfile, ensure the `TEMPLATE` block matches the model's expected format. Ollama handles this automatically for models pulled from its registry, but imported GGUF files may need manual template specification.

**GPU not detected:** On Linux, verify that `nvidia-smi` works on the host and that the NVIDIA Container Toolkit is installed for Docker. The most common cause is a CUDA driver version mismatch between the host driver and the toolkit version. For Docker, ensure the `--gpus all` flag or `deploy.resources.reservations` block is present. GPU passthrough silently falls back to CPU if the toolkit is missing -- always verify with `ollama ps` and look for the `(GPU)` indicator.

## The Local LLM Ecosystem in Motion

### Trends to Watch

Speculative decoding, where a small draft model proposes tokens that a larger model verifies in batch, is under active development in llama.cpp. Early benchmarks in the llama.cpp project show 1.5-2x speedups on draft-verify workloads, though results vary by model pair and hardware. Sub-4-bit quantization research continues to advance; BitNet-style 1.58-bit models are an active area of research, though quality at those levels still shows roughly 2-3x higher perplexity than Q4\_K\_M on standard benchmarks, making them unsuitable for coherent multi-paragraph generation today.

On-device fine-tuning with QLoRA has become accessible enough that developers can adapt base models to domain-specific tasks on consumer GPUs with 16GB VRAM. WebGPU inference projects like web-llm and wllama are making browser-based local inference a real possibility, albeit currently limited to smaller models. Multimodal local models, including LLaVA and Qwen2-VL variants, are increasingly available through Ollama's model registry, bringing vision capabilities into the local stack.

> The comparison table at the top of the tooling section provides a starting point for choosing the right tool. For most developers, starting with Ollama, working through the RAG tutorial above, and then iterating toward Docker-based production deployment as requirements solidify will get you from experiment to production with the fewest detours.

Close

[![SitePoint Team](https://uploads.sitepoint.com/wp-content/uploads/2020/03/1584945800Group-5.png)SitePoint Team](/author/sitepointteam/)

Sharing our passion for building incredible internet things.
