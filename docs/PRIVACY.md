# Privacy & compliance notes (health-adjacent)

## What this is / is not

| In scope (MVP) | Out of scope (unless explicitly expanded) |
|----------------|-------------------------------------------|
| Coaching check-ins, wellness self-report | Clinical diagnosis or treatment |
| Progress photos with consent | HIPAA BAAs as a covered entity EHR |
| Operational KPIs for adherence | Medical device claims |

This is **not** a substitute for licensed medical care. Copy and UI should say so clearly.

---

## Data categories

- **Self-reported:** sleep, mood, training, notes  
- **Media:** progress photos (special category—treat with care)  
- **Derived:** adherence %, correlations, coach response times  
- **Future AI:** prompts, retrieved chunks—audit and minimize retention  

---

## Controls (minimum viable)

1. **Consent:** Checkbox on Form; store `consent_version` + timestamp in Sheet.  
2. **Access:** Coach-only Sheet; clients use Form + (later) client dashboard with auth.  
3. **Sharing:** No public Sheet links; portfolio demos use fake data or aggregates.  
4. **Deletion:** Process for pilot client offboarding (delete Drive folders + Sheet rows).  
5. **Breach mindset:** If using Gmail/Workspace, use 2FA; avoid forwarding exports to personal email.

---

## Responsible AI (Phase 3 preview)

- No autonomous medical recommendations.  
- Escalation: “Talk to your coach or a licensed provider.”  
- Evaluations before changing models/prompts; document failure modes for portfolio “decision” section.

---

## Portfolio framing for analysts

Emphasize **measurement quality**: missing data, self-report bias, selection bias (only motivated clients respond), and distinguishing **signal vs noise** in sleep–mood–training relationships—the same statistical humility expected in hospital operations and quality analytics.
