# MVP plan — phased delivery

## Executive recommendation

Ship **Phase 0 in days**, not months. Validate that coaches and clients will actually use a **weekly Form** before building auth, photo pipelines, or AI. Each phase adds one risk layer: visualization → media → generative AI.

---

## Phase 0 — Google Forms + Google Sheets (now)

**Goal:** Single source of truth for weekly check-ins with zero custom backend.

| Activity | Owner | Output |
|----------|--------|--------|
| Design Form (sleep, mood, split, consent) | Juan + pilot coach | Live Form URL |
| Link to Sheet | Juan | `responses` tab auto-fill |
| Coach playbook | Coach | “Review every Monday”; filter by client |
| Portfolio | Juan | Screenshot + KPI definitions in README |

**Why Forms/Sheets:** Novice-friendly, free tier, familiar to non-engineers. Coaches can tweak questions without deploys.

**Limits:** No role-based app UI; photos awkward in Forms (use Phase 2); no real-time chat.

---

## Phase 1 — Read-only dashboards

**Goal:** Coach sees roster health; client sees **their** trends only—still no in-app editing (Form remains intake).

**Options (pick one)**

1. **Looker Studio** connected to Sheet — fastest, no code; fine for portfolio demo.
2. **Lightweight web app** — Next.js or static site + **Google Sheets API** (service account, read-only scope); host on Vercel/Cloudflare Pages.
3. **No-code bridge** — Zapier/Make sync Sheet → Airtable if you need relational views (extra cost/complexity).

**Portfolio angle:** Document data freshness, API quota, and how you handle **PII** (hash client IDs in public demos).

---

## Phase 2 — Check-ins + progress photos

**Goal:** Timeline of front/side/back photos; tie to Sheet rows by `client_id` + `week`.

**Approach**

- **Simple:** Google Drive folder per client; Form file upload or dedicated “photo week” Form.
- **App:** Supabase/Firebase storage + signed URLs; metadata row in Sheet or Postgres.

**Analytics:** Cadence adherence, not automated “body fat estimation” in MVP (avoid overclaiming).

---

## Phase 3 — AI chat with guardrails

**Goal:** Assist coach–client **communication** (summaries, reminder drafts, FAQ)—not medical advice.

| Requirement | Implementation sketch |
|-------------|------------------------|
| Human in the loop | Coach approves outbound messages |
| Scope | RAG over coach’s own docs + client’s **consented** check-in history |
| Safety | Refuse diagnosis/prescription; log prompts; periodic eval set |
| Portfolio tie-in | Responsible AI, eval metrics, escalation—aligns with hospital analyst narrative |

Defer until Phase 0–1 prove retention.

---

## Tech stack comparison (Phase 1+)

| Path | Pros | Cons |
|------|------|------|
| **Sheets API + static/SPA** | Full control, portfolio-grade code | OAuth/service account setup |
| **Looker Studio only** | Zero code | Less “engineering” story |
| **Retool / Glide on Sheet** | Fast internal tools | Vendor lock-in |
| **Full stack (Postgres + auth)** | Scalable | Overkill before pilot n |

**Recommendation:** Looker Studio or Sheets API read-only app for portfolio **Phase 1 demo**; migrate to Postgres when &gt;20 clients or photos land.

---

## Success metrics (pilot)

- ≥70% weekly Form completion over 4 weeks (pilot cohort)
- Coach time to weekly review &lt;30 minutes
- At least one documented insight (sleep ↔ adherence) with explicit bias caveats
