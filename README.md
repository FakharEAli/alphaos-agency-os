<div align="center"><img src="cover.png" width="100%"></div>

**[← All 14 systems](https://github.com/FakharEAli/portfolio)** · [VerticalVoice](https://github.com/FakharEAli/verticalvoice-ai-receptionist) · [SeatWise](https://github.com/FakharEAli/seatwise-enrolment-agent) · [CohortPilot](https://github.com/FakharEAli/cohortpilot-admissions-ai)

# AlphaOS

**The operating system for a premium content agency: every client's analytics, every podcast's best clips, every meeting's decisions, in one place.**

🟢 **In production** · **Client:** Alpha Accelerator, a premium content agency · **Live:** private deployment

## The problem

A content agency managing high-profile creators across Instagram, YouTube and TikTok runs on fragmented data. Metrics live in three native analytics tools. Timestampers manually scrub 60-minute podcasts for the best 60-second clips. Editors lack client-specific rules. Meeting decisions evaporate. Strategy is reactive, not proactive. Every one of these was a person doing by hand what software should do.

## What I built

- **Command Center**: agency home with hero KPI cards (total clients, views this week, active alerts, top post), an onboarding pipeline grid with progress bars, and a "needs attention" list of at-risk clients, sorted by health score.
- **Client dashboards** with a multi-tab layout per platform (Instagram / YouTube / TikTok) plus Intelligence and Settings, platform comparison mode, customisable tab names and cross-platform metric normalisation.
- **25 AI intelligence modules** behind a versioned prompt registry with Zod schemas: hook scoring, content and audience analysis, Content DNA profiles, caption scoring, posting cadence with a heatmap and holiday calendar, competitor discovery (7-dimension scoring), trend detection, Script Factory, Monday Reports, Playbooks and production guides.
- **AI Timestamper (TS2)**: long-form episode in, ranked clip candidates out. A two-phase Scene Spotter + Critic pipeline scores 80 to 120 candidates per video on emotion, story value, clarity, energy and shareability, then returns timestamps, rewritten hooks and editor instructions. Backed by a frozen, human-labelled ground-truth benchmark of 1,259 moments across 179 videos with recall, surfaced-recall and hook-match reporting.
- **Transcript & video portal** at `/p/[token]`: signed, shareable per-item pages for reviewers and editors.
- **Pre-sales engine**: prospect analysis (6-stage pipeline: profile → posts → web → classification → golden video → brief), ICP radar with candidate import and queue, proof-asset and case-study campaigns with tracked events.
- **Meeting knowledge**: meeting transcripts synced from the recorder, deep analysis (executive summary, decisions, action items, key quotes), and a pgvector RAG knowledge base with universal and per-client scopes.
- **Operations layer**: 6-phase automated client onboarding (analytics → transcription → web research → DNA → competitors → playbook, about 15 minutes), notifications and inbox, tickets, and admin pages for AI stats, pipeline metrics, dead-letter queue and feature flags.
- **Infrastructure**: Clerk auth, Sentry, multi-layer caching, rate limiting and circuit breakers, Inngest background jobs, webhooks for ingestion, Playwright end-to-end checks.

## Screenshots

<img src="screenshots/01-pipeline-metrics.png" alt="Admin cockpit: pipeline metrics across the agency&#x27;s client roster" width="100%"/>
<sub>Admin cockpit: pipeline metrics across the agency's client roster</sub>

<table>
  <tr>
    <td width="50%" valign="top"><img src="screenshots/02-notifications.png" alt="Notification centre: alerts from analytics, publishing and meetings" width="100%"/><br/><sub>Notification centre: alerts from analytics, publishing and meetings</sub></td>
    <td width="50%" valign="top"><img src="screenshots/03-client-analytics.png" alt="Per-client cross-platform analytics with retention and reach breakdowns" width="100%"/><br/><sub>Per-client cross-platform analytics with retention and reach breakdowns</sub></td>
  </tr>
  <tr>
    <td width="50%" valign="top"><img src="screenshots/04-icp-radar.png" alt="ICP Radar: prospect qualification by tier with cheap and deep analysis scores" width="100%"/><br/><sub>ICP Radar: prospect qualification by tier with cheap and deep analysis scores</sub></td>
    <td width="50%" valign="top"><img src="screenshots/05-public-portal.png" alt="Client-facing public portal with the shareable report view" width="100%"/><br/><sub>Client-facing public portal with the shareable report view</sub></td>
  </tr>
  <tr>
    <td colspan="2" align="center"><img src="screenshots/06-mobile-command-center.png" alt="Mobile: command center" width="45%"/><br/><sub>Mobile: command center</sub></td>
  </tr>
</table>

## Architecture

```mermaid
flowchart LR
    subgraph Ingest["Ingestion"]
        SC["Platform scrapers<br/>IG · YT · TikTok"] --> API
        Fathom["Meeting recorder<br/>webhook"] --> API
        Web["Web research<br/>search + extraction"] --> API
    end
    API["Next.js API routes<br/>(50 intelligence routes)"] --> Inngest["Inngest jobs<br/>onboarding · TS2 · reports"]
    Inngest --> LLM["Gemini · Claude · OpenAI<br/>prompt registry + Zod"]
    LLM --> DB[("Supabase Postgres<br/>+ pgvector RAG")]
    API --> DB
    DB --> UI["Command Center · Client dashboards<br/>AI Timestamper · Meetings · Pre-sales"]
    DB --> Portal["/p/[token] portal"]
    API --> Cache["Upstash Redis<br/>cache + rate limits"]
    API --> Obs["Sentry · pipeline metrics · DLQ"]
    Clerk["Clerk auth"] --> UI
```

## Stack

![Next.js](https://img.shields.io/badge/Next.js_16-000000?style=flat-square&logo=nextdotjs&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=flat-square&logo=typescript&logoColor=white)
![Clerk](https://img.shields.io/badge/Clerk-6C47FF?style=flat-square&logo=clerk&logoColor=white)
![Supabase](https://img.shields.io/badge/Supabase_pgvector-3FCF8E?style=flat-square&logo=supabase&logoColor=white)
![Inngest](https://img.shields.io/badge/Inngest-000000?style=flat-square)
![Upstash](https://img.shields.io/badge/Upstash-00E9A3?style=flat-square&logo=upstash&logoColor=black)
![Sentry](https://img.shields.io/badge/Sentry-362D59?style=flat-square&logo=sentry&logoColor=white)
![Claude](https://img.shields.io/badge/Claude-D97757?style=flat-square&logo=anthropic&logoColor=white)
![Gemini](https://img.shields.io/badge/Gemini-8E75B2?style=flat-square&logo=googlegemini&logoColor=white)
![OpenAI](https://img.shields.io/badge/OpenAI-412991?style=flat-square&logo=openai&logoColor=white)
![Deepgram](https://img.shields.io/badge/Deepgram-13EF93?style=flat-square&logo=deepgram&logoColor=black)
![Resend](https://img.shields.io/badge/Resend-000000?style=flat-square&logo=resend&logoColor=white)
![Playwright](https://img.shields.io/badge/Playwright-2EAD33?style=flat-square&logo=playwright&logoColor=white)

## My role

Architect and lead engineer from the first commit. I built the data ingestion, the intelligence modules and their prompt registry, the TS2 clip pipeline and its ground-truth benchmark harness, the onboarding pipeline, the pre-sales tooling and the infrastructure layer. The agency's lead strategist and ops team are the daily users.

## Outcomes

- 71 catalogued features (57 user-facing + 14 infrastructure), each audited and rated for maturity.
- Client onboarding that used to be manual now runs as a 6-phase pipeline in about 15 minutes.
- Clip selection moved from manual scrubbing to a scored, benchmarked extractor; the benchmark harness gives an honest recall number (baseline 52.4% on the covered subset) instead of a vibe.
- One system replaced scattered analytics tools, Notion pages and spreadsheets as the agency's daily operating surface.

## Status & timeline

Repo created 2026-05-06 · last push 2026-09-13 · 🟢 In production, actively developed.

---
<sub>Part of the <a href="https://github.com/FakharEAli/portfolio">FakharEAli portfolio</a> — real systems, anonymized seeded data, source available under NDA. © 2026 Fakhar E Ali · CC BY-NC-ND 4.0</sub>
