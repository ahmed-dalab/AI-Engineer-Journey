# Layer 1 Takeaway

Short definitions for the concepts covered in Layer 1. Use this page as a quick reference before building LLM applications.

---

## Contents

1. [Tokens & tokenization](#1-tokens--tokenization)
2. [Context window](#2-context-window)
3. [Transformer](#3-transformer)
4. [Attention](#4-attention)
5. [Parameters, training & inference](#5-parameters-training--inference)
6. [Temperature & top-p](#6-temperature--top-p)
7. [Limitations & hallucinations](#7-limitations--hallucinations)
8. [Structured outputs](#8-structured-outputs)
9. [Streaming](#9-streaming)
10. [How it fits together](#10-how-it-fits-together)

---

## 1. Tokens & tokenization

| Term | Definition |
|------|------------|
| **Token** | A small piece of text the model treats as one unit (a word, part of a word, punctuation, etc.). One word is not always one token. |
| **Tokenization** | Splitting human text into tokens using the model’s tokenizer. |
| **Token ID** | A number assigned to each token; the model processes IDs, not raw text. |

**Generation (core idea):** The model reads input tokens and repeatedly predicts the **next token**, then adds that token and predicts again.

**Why it matters for engineers:** API cost and limits are usually counted in **tokens** (input + output). Long prompts and documents consume more tokens.

---

## 2. Context window

| Term | Definition |
|------|------------|
| **Context window** | The maximum amount of token-based information the model can use in one request (system prompt, history, retrieved docs, tools results, user message, etc.). |
| **Context** | Whatever information you **currently send** to the model in that request. |
| **Context management** | Choosing what to include or exclude so the model gets what it needs—not everything you have. |

**Important distinction:** Context is **not** the same as permanent **memory**. The model only uses what is in the current context (unless your app stores and resends it).

---

## 3. Transformer

| Term | Definition |
|------|------------|
| **Transformer** | A neural-network architecture that processes sequences of tokens using **attention** and stacked layers. Modern LLMs (e.g. GPT-style) are built on it. |
| **Layer** | One block of attention + neural-network computation; models stack many layers to refine token representations. |

**Distinctions:**

- **Transformer** = architecture  
- **LLM** = a language model built with that architecture  
- **Product (e.g. ChatGPT)** = model + UI, safety, tools, and other software  

The model does **not** search the web by default; it uses learned patterns in its weights to produce text.

---

## 4. Attention

| Term | Definition |
|------|------------|
| **Attention** | A mechanism that decides **how much each token should use information from other tokens** when building its representation. |
| **Attention weight** | A score (conceptually) showing how strongly one token relates to another. |
| **Self-attention** | Tokens in the **same** sequence attend to each other (e.g. linking *he* to *Ahmed*). |

**Query, Key, Value (high level):**

- **Query** — what this token is looking for  
- **Key** — what kind of information another token offers  
- **Value** — the information taken from other tokens, weighted by relevance  

Same word (e.g. *bank*) can mean different things depending on surrounding tokens because attention uses context.

---

## 5. Parameters, training & inference

| Term | Definition |
|------|------------|
| **Parameter** | A learned number inside the network (often billions in large models). Knowledge and patterns are **distributed** across parameters—not stored as single facts. |
| **Training** | Process that **updates** parameters using data so the model gets better at predicting language. |
| **Inference** | Using the **fixed** trained model on a prompt to generate output; parameters are not normally updated per request. |
| **Next-token prediction** | At each step, the model outputs probabilities over possible next tokens; one token is chosen and the process repeats. |

| | **Parameters** | **Context** |
|---|----------------|-------------|
| Meaning | What the model **learned** during training | What you **provide right now** in the prompt |

---

## 6. Temperature & top-p

| Term | Definition |
|------|------------|
| **Probability distribution** | After processing the prompt, the model assigns probabilities to candidate next tokens. |
| **Temperature** | Controls how **sharp or flat** that distribution is when sampling. Lower → more predictable; higher → more varied. |
| **Top-p (nucleus sampling)** | Sample only from the smallest set of top tokens whose **combined probability** reaches threshold *p* (e.g. 0.9). |

**Notes:**

- Temperature and top-p **do not add knowledge**; they change **how** a token is picked.  
- Low temperature is common for SQL, JSON, and classification; higher can help brainstorming.  
- **Top-k** (optional): keep only the top *K* tokens by probability (fixed count, unlike top-p).

---

## 7. Limitations & hallucinations

| Term | Definition |
|------|------------|
| **Hallucination** | The model outputs false or unsupported information while sounding confident. |
| **Knowledge cutoff** | Training data ends at a point in time; the model does not automatically know newer events unless your app supplies them. |

**Why hallucinations happen:** The model optimizes for **plausible continuations**, not guaranteed truth. Missing, outdated, or bad context increases risk.

**Engineering responses (preview):** RAG, tool calling, validation, structured outputs, evaluation, guardrails, and clear “say if you don’t know” instructions.

**Principle:** Fluency ≠ correctness. The LLM is usually one **component** inside a larger system (database, APIs, rules, permissions).

---

## 8. Structured outputs

| Term | Definition |
|------|------------|
| **Structured output** | Model response that follows a **schema** (fields, types, enums) so your code can parse it reliably—often JSON. |
| **Schema** | Definition of required fields and types (e.g. Zod, JSON Schema). |

**Distinctions:**

| Output type | Purpose |
|-------------|---------|
| Free text | Humans read it |
| Structured JSON | Your application parses and validates it |
| Tool / function calling | Model asks **your app** to run a function with structured arguments |

**Important:** Valid structure ≠ correct facts. Always **validate** model output like untrusted input.

“Asking for JSON in the prompt” is weaker than API **schema-constrained** generation when available.

---

## 9. Streaming

| Term | Definition |
|------|------------|
| **Streaming** | Sending the model’s output **in chunks** as it is generated, instead of waiting for the full response. |

**Effects:**

- Improves **perceived** speed in chat UIs  
- Does **not** reduce token count, cost, or model capability  
- Partial streamed JSON may be **incomplete** until the stream ends  

Clients can often **cancel** a stream to stop generation early.

---

## 10. How it fits together

```text
User text
    → Tokenization → Token IDs
    → Context (within context window)
    → Transformer (attention + parameters)
    → Next-token probabilities
    → Temperature / top-p → sample token
    → Repeat until done
    → Optional: stream chunks to client
    → Optional: structured JSON for your app
```

**Layer 1 sentence to remember:**

An LLM processes **tokenized context** through a **Transformer** using **parameters** and **attention**, generates text **one token at a time**, and your application controls **context**, **sampling**, **format**, **delivery (streaming)**, and **validation**—often with **RAG** and **tools** for facts and actions.
