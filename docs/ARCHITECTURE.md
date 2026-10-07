# Architecture (target state)

> **Overview:** KPIs, privacy framing, and phased checkboxes live in [`README.md`](../README.md). This doc is the technical target state.

## Context

Personal Wellness Intelligence is **health-adjacent coaching software**, not a HIPAA-covered clinical EHR. It still handles sensitive data (progress photos, mood, sleep). Design for **consent, least privilege, and auditability** as if scaling toward regulated environments—even when MVP is just a Sheet.

```
┌─────────────┐     submit      ┌──────────────┐     read      ┌─────────────┐
│ Client      │ ──────────────► │ Google Form  │ ────────────► │ Google Sheet│
│ (browser)   │                 └──────────────┘               │  (SoT v0)   │
└─────────────┘                                                └──────┬──────┘
                                                                      │
                    ┌─────────────────────────────────────────────────┼────────────────────┐
                    │ Phase 1                                         │                    │
                    ▼                                                 ▼                    │
            ┌───────────────┐                                  ┌──────────────┐            │
            │ Looker Studio │                                  │ Web app      │            │
            │ (coach view)  │                                  │ Sheets API   │            │
            └───────────────┘                                  └──────┬───────┘            │
                    │                                                 │                    │
                    └─────────────────────────────┬───────────────────┘                    │
                                                  ▼                                        │
                                          ┌──────────────┐                                 │
                                          │ Coach        │                                 │
                                          │ dashboard    │                                 │
                                          └──────────────┘                                 │
                                                                                           │
 Phase 2: Object storage (Drive / S3 / Supabase) ◄── metadata rows ──────────────────────┘
 Phase 3: LLM gateway + vector store (client-scoped) + audit log
```

---

## Phase 0 components

| Component | Technology | Notes |
|-----------|------------|--------|
| Intake | Google Forms | Version questions; export structure to `docs/form-schema.md` when stable |
| Store | Google Sheets | Backup weekly; restrict sharing |
| Identity | Email or pseudonymous ID | No SSN/clinical MRNs in MVP |

---

## Phase 1 — Read-only web dashboard (recommended pattern)

**Stack option A (portfolio-friendly)**

- **Frontend:** HTML/React in `apps/web/`
- **Data:** Google Sheets API v4, service account with read-only access to one spreadsheet
- **Secrets:** `GOOGLE_SERVICE_ACCOUNT_JSON` in env (never commit); `.env` in `.gitignore`
- **Hosting:** Static host + optional serverless function for API proxy (keeps SA off client)

**Stack option B (no code)**

- Looker Studio connected to Sheet; embed link in portfolio

---

## Phase 2 — Photos

- Store blobs in **private** bucket/folder; URLs time-limited.
- Sheet columns: `client_id`, `captured_at`, `angle`, `storage_key`, `consent_version`.
- Optional: virus scan / file type validation on upload endpoint.

---

## Phase 3 — AI layer

- **Boundary:** Coaching support only; system prompts + policy filters.
- **RAG:** Chunk coach playbooks + client check-ins; **tenant isolation** per client.
- **Evals:** Golden Q&A set; regression before prompt changes.
- **Observability:** Log retrieval sources; no training on client data without consent.

---

## Future: FHIR and care settings

If the product ever integrates with hospital workflows, map observations (sleep hours, mood score) to appropriate FHIR resources **only with legal/clinical review**. MVP intentionally stays in **wellness/coaching** scope to match portfolio positioning (operational KPIs, not diagnosis).

---

## Security checklist (all phases)

- [ ] Written consent for photos and AI assistance (versioned)
- [ ] Coach vs client role separation
- [ ] No secrets in git; use GitHub Actions secrets if CI added
- [ ] Public demo uses synthetic or de-identified data
- [ ] Document retention/deletion process for pilot clients

See also: privacy and KPI notes in [`README.md`](../README.md); coach-facing one-pager (TODO).
