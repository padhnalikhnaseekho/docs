# Cloud Providers for Production-Scale AI/ML Pipelines

> Last updated: 2026-03-29

This document covers free and cheap cloud providers for running LLM-based pipelines at production scale. Focused on projects that are **API-call-heavy** (call external LLM APIs, don't run models locally), so GPU compute is generally not required.

---

## Part 1: LLM API Providers

### Completely Free (no credit card required)

| Provider | Models | Rate Limit | OpenAI-Compatible | Sign Up |
|----------|--------|------------|-------------------|---------|
| **Groq** | Llama 3.3 70B, Qwen3 32B, Llama 4 Scout | 30 RPM, 14.4k RPD | Yes | https://console.groq.com/keys |
| **Google AI Studio** | Gemini 2.0 Flash, Gemini 2.5 Flash | 2-15 RPM | Yes | https://aistudio.google.com/apikey |
| **OpenRouter** | Gemini Flash, Llama 3.3 70B (free tier) | 20 RPM | Yes | https://openrouter.ai |
| **Cohere** | Command R+, Command R7B | 20 RPM | No (own SDK) | https://dashboard.cohere.com |
| **xAI (Grok)** | Grok-2, Grok-2 Mini, Grok-2 Vision | $25/mo free credits (renewable) | Yes | https://console.x.ai |
| **Mistral** | Mistral Large, Mistral Nemo | 1 req/sec | Yes | https://console.mistral.ai |

### Free Credits (sign-up bonus)

| Provider | Models | Free Credit | OpenAI-Compatible | Sign Up |
|----------|--------|-------------|-------------------|---------|
| **DeepSeek** | DeepSeek-V3.2, DeepSeek-R1 | 10M free tokens | Yes | https://platform.deepseek.com |
| **DeepInfra** | Llama 3.1 405B, Qwen 2.5 72B | $5 signup bonus | Yes | https://deepinfra.com |
| **SambaNova** | Llama 3.1 405B/70B | Generous free credits | Yes | https://cloud.sambanova.ai |

### Cheapest Paid (when free tiers run out)

| Provider | Price per 1M tokens (input/output) | Notes |
|----------|-------------------------------------|-------|
| **DeepSeek V3.2** | $0.14 / $0.28 | 100x cheaper than GPT-5 |
| **SiliconFlow** | Competitive | 2.3x faster inference than competitors |
| **Fireworks AI** | ~$0.20 / $0.80 | Good for open-source models at scale |
| **Google Gemini 2.5 Flash** | ~$0.02 / $0.10 | Cheapest high-quality option |

### Provider Comparison Notes

- **Groq** uses custom LPU hardware for fastest inference (~280-1000 T/sec), but has strict daily token limits per model on the free tier. Each model has an independent quota -- if one is rate-limited, switch to another.
- **DeepSeek** offers the best cost/quality ratio for paid usage. Their R1 model ($6M training cost vs GPT-4's $100M) passes savings to API pricing.
- **xAI Grok** is the hidden gem -- $25/month in free credits renews, and the API is OpenAI-compatible.
- **OpenRouter** aggregates multiple providers and routes to the cheapest available endpoint. Good fallback strategy.

---

## Part 2: Compute Providers (to run the pipeline)

### Free Tier

| Platform | What You Get | Best For | Link |
|----------|-------------|----------|------|
| **GitHub Actions** | 2000 min/month (public repos) | Scheduled runs via CI/CD | https://github.com/features/actions |
| **Google Colab** | Free Jupyter notebooks + occasional GPU | Ad-hoc runs, prototyping | https://colab.research.google.com |
| **Google Cloud Run** | 2M requests/mo, 360k vCPU-sec | Serverless API wrapper | https://cloud.google.com/run |
| **Hugging Face Spaces** | 2 vCPU, 16GB RAM (CPU tier) | Hosting dashboards | https://huggingface.co/spaces |
| **AWS Lambda** | 1M invocations/month | Event-triggered runs | https://aws.amazon.com/lambda |

### Cheapest Paid

| Platform | Price | Best For | Link |
|----------|-------|----------|------|
| **Modal** | Free monthly credits, then pay-per-second | Python-native autoscaling | https://modal.com |
| **DigitalOcean Droplet** | $4-6/month | Always-on VPS for scheduled runs | https://www.digitalocean.com |
| **Vast.ai** | ~$0.15-0.50/hr GPU | Cheapest GPU (if needed for local inference) | https://vast.ai |
| **RunPod** | Pay-per-second, spot instances 70% off | Burst GPU workloads | https://www.runpod.io |

---

## Recommended Setups

### Option A: Zero Cost ($0/month)

```
LLM APIs:  Groq (3 models) + Google AI Studio (Gemini) + xAI Grok ($25 free credits)
Compute:   GitHub Actions (scheduled cron) or Google Colab (manual)
Dashboard: Hugging Face Spaces (Streamlit)
```

- 6+ models across 4 providers, all free
- GitHub Actions can run benchmarks nightly via cron
- Results pushed to repo, dashboard auto-updates

### Option B: Minimal Cost ($5/month)

```
LLM APIs:  All free providers above + DeepSeek (10M tokens free, then $0.14/1M)
Compute:   DigitalOcean Droplet ($5/mo) -- always-on, no sleep interruptions
Dashboard: Hugging Face Spaces (free)
```

- Always-on VPS won't die when your laptop sleeps
- DeepSeek adds high-quality reasoning models (R1)
- Cron job on the droplet runs benchmarks on schedule

### Option C: Production Scale ($20-50/month)

```
LLM APIs:  All free providers + DeepSeek paid + Fireworks AI
Compute:   Modal (autoscaling, parallel runs) or DigitalOcean ($12/mo 2GB)
Dashboard: HF Spaces or Cloud Run
Storage:   S3-compatible (Cloudflare R2 -- free 10GB)
```

- Parallel runs across multiple models
- Cloudflare R2 for persistent result storage (free 10GB)
- Modal auto-scales to zero when idle

---

## Sources

- [Free LLM API Directory (45+ providers)](https://free-llm.com/)
- [LLM API Pricing Comparison (Mar 2026)](https://costgoat.com/compare/llm-api)
- [Top 15 Cloud Platforms for AI/ML in 2026](https://saturncloud.io/blog/top-15-cloud-platforms-for-ai-ml-teams-in-2026/)
- [Cheapest AI Inference Services 2026](https://www.siliconflow.com/articles/en/the-cheapest-ai-inference-service)
- [Top Cloud GPU Providers 2026](https://www.runpod.io/articles/guides/top-cloud-gpu-providers)
- [Free LLM API Resources (GitHub)](https://github.com/cheahjs/free-llm-api-resources)
- [Groq Supported Models](https://console.groq.com/docs/models)
- [Groq Model Deprecations](https://console.groq.com/docs/deprecations)
