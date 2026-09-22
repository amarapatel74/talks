---
marp: true
theme: meetup
paginate: true
size: 16:9
footer: 'Architecture Over Intelligence · The AI Fellowship Madrid'
---

<!-- _class: lead -->

# Architecture Over Intelligence

<div class="subtitle">Designing reliable AI workflows</div>

<div class="author">Amar Patel · The AI Fellowship Madrid · 9 Oct 2026</div>

---

# "Investigate this incident" is one job. *It isn't.*

- Pull the error logs
- Identify affected systems
- Assess the severity
- Draft a response plan
- Page the on-call
- Write the postmortem

Six steps. Each one feeds the next.

---

# The demo vs. production gap

| Demo | Production |
|---|---|
| One uninterrupted session | Compaction, reset, walk-aways |
| Mistakes are annoying | Mistakes are data loss |
| A human watches the whole time | A human checks hours later |
| A frontier model | A cheaper model that drifts |

**Reliability fails at the boundaries.**

---

# A JSON file is the *source of truth*

```json
{
  "current_step": 2.6,
  "completed_steps": [-1, 0, 1, 2, 2.5, 2.6],
  "user_confirmed": { "summary": true },
  "aborted": false
}
```

Before every step, read the file. A step runs only when `current_step` is the one before it.

---

# A gate is *authority*, not just quality control

- 🟢 **auto** — safe, predictable steps keep moving
- 🟡 **human** — the agent stops and waits
- 🔴 **blocked** — cannot proceed

The human is a **dial, not a switch**.

---

# No step uses data that doesn't exist yet

```
Step 0  →  speaker-map.json
Step 1  →  PRECONDITION: speaker-map.json must exist
```

Declare what you produce. Gate on what you consume.

---

# Interruption is not a bug

```json
{ "aborted": true }
```

Resume from the same file. It survives compaction, a context reset, and your absence.

---

# One real pipeline: *11 steps, 8 LLM calls*

```
discovery · speakers · context · summary · sentiment
· personal · actions · triage · follow-up · file · cleanup
```

9 human gates · 4 scripts · 1 state file.

---

# What broke in three months

- The state file got edited to the wrong step → *validate on load*
- The LLM skipped a gate → *the file decides, not the prompt*
- Compaction mid-pipeline → *the file survived* — the win

Never assume a step is done.

---

# The model is always changing

*2024:* a bigger model fixed it.
*2026:* the gains flattened. The churn didn't.

This month: **Jev** — decisions, not text.

> "The State Machine Is the Agent."

---

# These patterns work everywhere

| Your workflow | Enterprise equivalent |
|---|---|
| Meeting processing | Customer-call analysis |
| Inbox routing | Email triage and classification |
| Task tracking | KPI dashboards |
| Model-agnostic guardrails | Multi-model fleets |

The domain changes. The pattern doesn't.

---

# Five rules

1. The **state file** is the source of truth.
2. **Gates** are structural, not optional.
3. Declare **artifacts** before they're needed.
4. Design for **interruption**.
5. **Architecture > intelligence**.

---

<!-- _class: close -->

# Key takeaways

- The model is a variable. The structure is the constant.
- The human is a **dial, not a switch** — better models move it up the stakes curve.
- It runs on any model. That part comes free.

**Reliability at the boundaries.**