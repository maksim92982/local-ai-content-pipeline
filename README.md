# Autonomous Local AI Content Pipeline

> A production-grade AI content pipeline that runs **entirely on local hardware** — no cloud AI, no paid inference. It generates, assembles, and publishes video content end-to-end, autonomously.

---

## What it does

The system orchestrates **multiple local AI models** into a single autonomous workflow:

1. **Content generation** — text (captions, titles, descriptions)
2. **Visual generation** — images from prompts
3. **Media assembly** — video with text overlay, animation, audio
4. **Publishing** — automated distribution to social platforms via official APIs
5. **Cleanup** — lifecycle management of generated media

Everything runs on a **single machine**. No cloud AI services. No paid inference APIs.

---

## Why it exists

Most "AI content" tools depend on cloud APIs. That means:

- **Recurring cost** per generation
- **Rate limits** and latency
- **Vendor lock-in** to a provider's model
- **No control** over model versions or behavior

This pipeline was built to **eliminate all of that**. It runs locally, so the marginal cost per generated piece of content approaches zero — and the entire stack is under my control.

---

## Architecture overview
```
[ Orchestrator ]
│
├──► [ Text Model ] (local LLM)
│         │
│         ▼
├──► [ Image Model ] (local diffusion)
│         │
│         ▼
├──► [ Media Assembly ] (overlay + encode + audio)
│         │
│         ▼
├──► [ Local Serving ] (exposed via secure tunnel)
│         │
│         ▼
└──► [ Publishing Layer ] (official social platform APIs)
          │
          ▼
     [ Cleanup ]
```

Each stage is **independent** and **replaceable**. The orchestrator sequences them, handles failures, and manages resource contention.

---

## Engineering decisions I owned

### 1. Resource-constrained orchestration

Multiple AI models share **one machine**. That means **one GPU** and a **fixed memory budget**. Every model placement is a trade-off:

- What runs on GPU vs CPU
- What stays resident in memory vs what gets loaded on demand
- What gets parallelized vs what must be sequential

This isn't a "just run it" pipeline. It's a **resource scheduling problem**.

### 2. Model interoperability

The models involved were **not designed to talk to each other**. They have different interfaces, different runtimes, and different output formats.

I built a **communication layer** that lets independent local models exchange data as part of a single pipeline. This is the part that most "AI demos" skip entirely.

### 3. Pipeline reliability

Autonomous content generation has **many failure modes**:

- Model output that doesn't match expectations
- Encoding artifacts
- Failed uploads
- Platform API rate limits

I made deliberate choices about **sequencing**, **encoding**, and **fault tolerance** so that output quality stays stable **at scale** — not just for one lucky run.

### 4. Cost architecture

The entire pipeline was designed around **zero paid AI cost**. That constraint drove the architecture — and it's why the system can run **continuously** without burning a budget.

---

## Results

- **Throughput:** ~15 Reels/hour sustained on local hardware
- **50 Reels published per day** to Instagram — daily cap is a platform limit, not the pipeline's ceiling
- **50 posts published per day** to Telegram
- **Fully autonomous** — no manual intervention per item
- **Zero paid AI inference cost** — everything runs locally

The pipeline has moved from prototype to **daily production use**.

---

## What this project demonstrates

- **Local AI orchestration** — running multiple models on constrained hardware
- **Multi-model system design** — making independent models work together
- **Production reliability** — keeping a generative pipeline stable at scale
- **Cost engineering** — designing around zero-marginal-cost inference
- **End-to-end ownership** — from model integration to publishing APIs

---

## Stack (high level)

- **Language:** Python
- **AI:** Local text generation, local image generation
- **Media:** Video assembly, text overlay, audio
- **Infrastructure:** Local serving, secure tunnel
- **Publishing:** Official Instagram Graph API, Telegram Bot API
- **Design:** Sequential orchestration, explicit resource trade-offs

*Specific model choices, parameters, and pipeline details are intentionally omitted.*

---

## Availability

This is a **running system I designed, built, and operate**.

The codebase is **private** — the architecture and decisions above are what I'm sharing publicly.

For a walkthrough or demo, feel free to reach out.
