# AthleteQuest AI — On-Device LLM Chat: Requirements Document

**Version:** 1.0 • **Date:** June 12, 2026 • **Status:** Proposed — targeted as a **separate release (v2.0)**

---

## 1. Summary & Decision

Replace (or augment) the current rule-based coach chatbot with a real local LLM — e.g. **Google Gemma 4 E2B** — running entirely on the child's phone/device, so every conversation stays private and never leaves the device.

**Decision: this is NOT a straightforward bundle — ship it as a separate release.**

| Factor | Today's app | With local LLM |
|---|---|---|
| App size | ~115 KB single HTML file | + **0.8–1.5 GB** model download (Gemma 4 E2B quantized) |
| Hardware needed | Any browser | WebGPU + ~4–5 GB RAM device |
| Device coverage | ~100% | ~70–75% of mobile, 90%+ of desktop (WebGPU support) |
| Safety layer | Hand-written, deterministic | Generative model needs prompt + input/output filtering + eval suite |
| Failure modes | None (deterministic) | Download failures, OOM crashes, slow tokens on old devices |

The current rule-based coach already runs 100% on-device and is deterministic-safe for kids, so there is no privacy regression in waiting. The LLM is a quality upgrade, not a privacy fix.

## 2. Background

- Today the "AI coach" is a keyword-routed reply engine + Athlete Twin stats. All data (profile, results, chats, heart checks) lives in `localStorage`; voice-to-text already runs on-device (Whisper tiny via transformers.js, ~40 MB one-time download — a useful precedent for model caching UX).
- Gemma 4 (released April 2, 2026) ships **E2B** (2.3B effective params) and **E4B** (4.5B) sizes designed for on-device use: multimodal, 128K context, runs in ~5 GB RAM at 4-bit; QAT builds push E2B under ~1 GB.

## 3. Goals / Non-Goals

**Goals**
1. Free-form, context-aware coach chat (uses Athlete Twin: sport, goals, trends, recovery) with zero network inference.
2. 100% of conversation content processed and stored on-device.
3. Keep the kid-safe behavior of the current coach (injury → physician, no diet/supplement advice, anti-overtraining).
4. Graceful fallback to the current rule-based coach on unsupported devices.

**Non-Goals**
- Cloud inference of any kind (even "anonymized").
- Accounts, telemetry, analytics.
- Replacing the deterministic safety guardrails (they remain as a hard filter layer).

## 4. Functional Requirements

- **FR-1** Parent-gated opt-in: the model download is initiated only from the Parent dashboard, behind the existing parent gate, with size/required-space disclosure.
- **FR-2** Download manager: resumable, Wi-Fi-recommended warning, progress UI, cancel/delete model (reuse Whisper download UX pattern).
- **FR-3** Model cached locally (Cache API / OPFS for web; app storage for packaged app). No re-download across sessions.
- **FR-4** Coach replies stream token-by-token into the existing chat UI; personality (hype/chill/wise/goofy) maps to system-prompt traits.
- **FR-5** Context injection: system prompt receives Athlete Twin snapshot (sport, age band, goal, consistency, trend, recent sleep/HR) — never the child's full name or free-text injury notes beyond what's needed.
- **FR-6** Offline-first: once downloaded, chat works in airplane mode.
- **FR-7** Kill switch: parent can delete the model and revert to rule-based coach at any time.

## 5. Privacy Requirements

- **P-1** No prompt, reply, or profile data ever transmitted; the only network calls permitted are the one-time model file download from a pinned CDN.
- **P-2** All chat history remains in `localStorage`/on-device storage, included in the existing local backup/restore files.
- **P-3** No third-party SDKs with telemetry; runtime libraries must be self-hosted or telemetry-disabled.
- **P-4** COPPA posture unchanged: no accounts, no PII collection, parent controls the feature.

## 6. Child-Safety Requirements

- **S-1** The existing deterministic guardrails (injury/pain → stop + parent + family physician; diet/supplement refusal; overtraining limits) run as a **pre-filter** on user input. On match, the canned safe response is used and the LLM is bypassed.
- **S-2** System prompt enforces: age-appropriate tone, no medical/diet advice, redirect health questions to parents/physician, no contact-info requests, growth-mindset coaching only.
- **S-3** Output filter: blocklist + lightweight classifier pass on LLM output before display; on violation, fall back to canned response.
- **S-4** Red-team eval suite (≥200 prompts: injury, dieting, body image, overtraining, stranger danger, jailbreak attempts) must pass before release; re-run on every model/prompt change.
- **S-5** Token/length caps and no tool/browsing capabilities.

## 7. Technical Requirements

### Model candidates (in preference order)

| Model | Size on disk (quantized) | Notes |
|---|---|---|
| **Gemma 4 E2B (QAT/int4)** | ~0.8–1.5 GB | Primary target; multimodal, 128K ctx, ~607 MB active RAM in LiteRT-LM mobile builds |
| Gemma 4 E4B (int4) | ~2–3 GB | Higher quality; only for ≥8 GB RAM devices |
| Chrome built-in Gemini Nano (Prompt API) | 0 (browser-managed, 2.7–4 GB) | Zero-download path on Chrome 148+; steep device reqs (16 GB RAM or 4 GB VRAM, 22 GB free disk); Chrome-only — treat as opportunistic tier, not the plan of record |

### Runtime options

| Runtime | Form factor | Pros | Cons |
|---|---|---|---|
| **MediaPipe LLM Inference (Web)** | Stays a web app | Google-supported, Gemma-optimized, WebGPU | WebGPU required; large WASM/asset payload |
| **WebLLM (MLC)** | Stays a web app | Mature, OpenAI-compatible API, ~80% of native speed | Same WebGPU constraint; model conversion pipeline |
| **LiteRT-LM via packaged app (Capacitor/native)** | App-store app | Best perf/battery; NPU use; background download | Biggest lift: app packaging, store review, distribution |

### Minimum device bar (web path)

- WebGPU-capable browser (Chrome 121+ Android, Safari iOS 26+), ~5 GB RAM, ~2.5 GB free storage, mid-2022+ SoC for acceptable tokens/sec. Below the bar → rule-based coach (no download offered).

## 8. UX Requirements

- Parent dashboard "🧠 Smart Coach" card: explain benefit, size, privacy; Download / Pause / Delete.
- First-run capability check (WebGPU, RAM heuristic, storage estimate) before offering download.
- Visible "on-device" badge in chat header when LLM active; typing indicator already exists.
- If generation exceeds ~8 s for first token, show friendly "thinking hard…" hint; never block the UI.

## 9. Acceptance Criteria

1. Airplane-mode chat works end-to-end after initial download.
2. Zero network requests during chat (verified via devtools/network audit).
3. Safety eval suite pass rate 100% on hard-block categories (injury, diet, PII).
4. First token < 4 s, ≥ 6 tokens/s on reference device (e.g., Pixel 8 / iPhone 15 class).
5. Rule-based fallback automatically engages on unsupported devices with no error shown to the child.
6. Parent can delete model; storage is reclaimed; app returns to current behavior.

## 10. Risks

- **Device fragmentation:** ~25–30% of mobile users lack WebGPU → fallback path is mandatory, not optional.
- **Storage/download abandonment:** 1 GB+ on kids' devices is significant; Wi-Fi-only default mitigates.
- **Generative safety:** even small models can produce off-policy text; mitigated by S-1…S-5 layered approach.
- **Browser eviction:** browsers may evict cached models under storage pressure → detect and offer re-download.
- **Single-file architecture ends:** the app becomes multi-asset (runtime WASM + model); PWA service worker required — this is the core of the "big lift."

## 11. Phased Plan

- **Phase 0 (spike, ~1 wk):** WebLLM + Gemma 4 E2B prototype behind a hidden flag; measure tokens/sec on 3 reference devices.
- **Phase 1 (v2.0 web, ~3–4 wks):** MediaPipe or WebLLM integration, PWA + service worker, parent-gated download manager, safety filter layer + eval suite.
- **Phase 2 (optional, ~4–6 wks):** Capacitor-packaged app with LiteRT-LM for app-store distribution and NPU acceleration.
- **Opportunistic:** feature-detect Chrome's built-in Prompt API and use it (still behind the same safety filters) when present — zero download for those users.

## 12. Sources

- [Gemma 4 announcement (Google)](https://blog.google/innovation-and-ai/technology/developers-tools/gemma-4/) • [Gemma 4 model card](https://ai.google.dev/gemma/docs/core/model_card_4)
- [Gemma 4 QAT — E2B under 1 GB](https://byteiota.com/gemma-4-qat-cuts-e2b-to-under-1gb-deploy-it-now/) • [Gemma 4 QAT (Google blog)](https://blog.google/innovation-and-ai/technology/developers-tools/quantization-aware-training-gemma-4/)
- [LiteRT-LM E2B builds (Hugging Face)](https://huggingface.co/litert-community/gemma-4-E2B-it-litert-lm) • [Gemma 4 edge deployment guide](https://www.mindstudio.ai/blog/gemma-4-edge-deployment-e2b-e4b-models)
- [MediaPipe LLM Inference for Web](https://developers.google.com/edge/mediapipe/solutions/genai/llm_inference/web_js) • [WebLLM](https://webllm.mlc.ai/) ([GitHub](https://github.com/mlc-ai/web-llm))
- [WebGPU browser AI availability 2026](https://www.buildmvpfast.com/blog/webgpu-browser-ai-inference-cost-savings-2026)
- [Chrome Prompt API docs](https://developer.chrome.com/docs/ai/prompt-api) • [Gemini Nano in Chrome 148](https://pasqualepillitteri.it/en/news/3145/gemini-nano-chrome-built-in-ai-client-side-en) • [Gemini Nano 4 GB deployment notes](https://earezki.com/ai-news/2026-05-06-googles-prompt-api/)
