---
marp: true
theme: meetup
paginate: true
size: 16:9
html: true
footer: 'Architecture Over Intelligence · The AI Fellowship Madrid'
---

<!-- _class: lead -->

# Architecture Over Intelligence

<div class="subtitle">Designing reliable AI workflows</div>

<div class="author">Amar Patel · The AI Fellowship Madrid · 9 Oct 2026</div>

<div class="lead-motif"><span class="a"></span><span class="b"></span><span class="c"></span></div>

---

# "Investigate this incident" is one job. *It isn't.*

<div class="body">

<div class="flow">
  <div class="step">Pull the error logs</div>
  <div class="arrow">→</div>
  <div class="step">Identify affected systems</div>
  <div class="arrow">→</div>
  <div class="step">Assess the severity</div>
  <div class="arrow">→</div>
  <div class="step">Draft a response plan</div>
  <div class="arrow">→</div>
  <div class="step">Page the on-call</div>
  <div class="arrow">→</div>
  <div class="step">Write the postmortem</div>
</div>

Six steps. Each one feeds the next.

</div>

---

# The demo vs. production gap

<div class="body">

<div class="split">
  <div class="panel demo">
    <div class="panel-title">Demo</div>
    <ul>
      <li>One uninterrupted session</li>
      <li>Mistakes are annoying</li>
      <li>A human watches the whole time</li>
      <li>A frontier model</li>
    </ul>
  </div>
  <div class="panel prod">
    <div class="panel-title">Production</div>
    <ul>
      <li>Compaction, reset, walk-aways</li>
      <li>Mistakes are data loss</li>
      <li>A human checks hours later</li>
      <li>A cheaper model that drifts</li>
    </ul>
  </div>
</div>

**Reliability fails at the boundaries.**

</div>

---

# A JSON file is the *source of truth*

<div class="body">

<div class="statefile">
  <div class="tab"><span><span class="dot"></span>process-state.json</span><span>meeting_id: 2026-01-15_140000</span></div>
  <div class="fields">
    <div class="field active"><span class="key">current_step</span><span class="val">2.6 → next is 3</span></div>
    <div class="field"><span class="key">completed_steps</span><span class="val">[-1, 0, 1, 2, 2.5, 2.6]</span></div>
    <div class="field"><span class="key">user_confirmed.summary</span><span class="val">true</span></div>
    <div class="field"><span class="key">aborted</span><span class="val">false</span></div>
  </div>
</div>

Before every step, read the file. A step runs only when `current_step` is the one before it.

</div>

---

# A gate is *authority*, not just quality control

<div class="body">

<div class="gates">
  <div class="gate auto">
    <div class="dot"></div>
    <div class="gate-title">Auto</div>
    <p>Safe, predictable steps keep moving.</p>
    <p class="when">File reads, health checks.</p>
  </div>
  <div class="gate human">
    <div class="dot"></div>
    <div class="gate-title">Human</div>
    <p>The agent stops and waits.</p>
    <p class="when">Anything that needs a yes.</p>
  </div>
  <div class="gate blocked">
    <div class="dot"></div>
    <div class="gate-title">Blocked</div>
    <p>Cannot proceed.</p>
    <p class="when">Missing file, corrupted state.</p>
  </div>
</div>

<div class="dial">
  <div class="track"><div class="marker" style="left:28%"></div></div>
  <div class="labels"><span>Reviews every step</span><span>Guards only the irreversible</span></div>
</div>

The human is a **dial, not a switch**.

</div>

---

# No step uses data that doesn't exist yet

<div class="body">

<div class="flow">
  <div class="step">Step 0: Speaker Mapping</div>
  <div class="arrow">→</div>
  <div class="step file">speaker-map.json</div>
  <div class="arrow">→</div>
  <div class="step">Step 1: Context Extraction</div>
</div>

Declare what you produce. Gate on what you consume.

</div>

---

# Interruption is not a bug

<div class="body">

<div class="resume-labels"><span>step 3 completes</span><span>context reset · user walks away</span><span>next session resumes</span></div>
<div class="resume">
  <div class="seg"></div>
  <div class="file">aborted:<br>true</div>
  <div class="gap"></div>
  <div class="file">step: 3<br>→ step 4</div>
  <div class="seg"></div>
</div>

Resume from the same file. It survives compaction, a context reset, and your absence.

</div>

---

# One real pipeline: *11 steps, 8 LLM calls*

<div class="body">

<div class="pipeline">
  <div class="node human"><span class="dot"></span>discovery</div>
  <div class="node human"><span class="dot"></span>speakers</div>
  <div class="node human"><span class="dot"></span>context</div>
  <div class="node human"><span class="dot"></span>summary</div>
  <div class="node auto"><span class="dot"></span>sentiment</div>
  <div class="node human"><span class="dot"></span>personal</div>
  <div class="node human"><span class="dot"></span>actions</div>
  <div class="node human"><span class="dot"></span>triage</div>
  <div class="node human"><span class="dot"></span>follow-up</div>
  <div class="node human"><span class="dot"></span>file</div>
  <div class="node optional"><span class="dot"></span>cleanup</div>
</div>

<div class="stats">
  <div class="stat"><div class="n">9</div><div class="l">Human gates</div></div>
  <div class="stat"><div class="n">4</div><div class="l">Scripts</div></div>
  <div class="stat"><div class="n">1</div><div class="l">State file</div></div>
</div>

</div>

---

# What broke in three months

<div class="body">

<div class="icon-list">
  <div class="row"><div class="badge">✎</div><div class="txt">The state file got edited to the wrong step → <em>validate on load</em></div></div>
  <div class="row"><div class="badge">⏭</div><div class="txt">The LLM skipped a gate → <em>the file decides, not the prompt</em></div></div>
  <div class="row win"><div class="badge">✓</div><div class="txt">Compaction mid-pipeline → <em>the file survived</em> — the win</div></div>
</div>

Never assume a step is done.

</div>

---

# The model is always changing

<div class="body">

<div class="timeline">
  <div class="point"><div class="pt-title">2024</div><p>A bigger model fixed it.</p></div>
  <div class="point"><div class="pt-title">2026</div><p>The gains flattened. The churn didn't.</p></div>
  <div class="point"><div class="pt-title">This month</div><p><strong>Jev</strong> — decisions, not text.</p></div>
</div>

<div class="pullquote">"The State Machine Is the Agent."</div>

</div>

---

# These patterns work everywhere

<div class="body">

<div class="mapping">
  <div class="row"><div class="left">Meeting processing</div><div class="arrow">→</div><div class="right">Customer-call analysis</div></div>
  <div class="row"><div class="left">Inbox routing</div><div class="arrow">→</div><div class="right">Email triage &amp; classification</div></div>
  <div class="row"><div class="left">Task tracking</div><div class="arrow">→</div><div class="right">KPI dashboards</div></div>
  <div class="row"><div class="left">Model-agnostic guardrails</div><div class="arrow">→</div><div class="right">Multi-model fleets</div></div>
</div>

The domain changes. The pattern doesn't.

</div>

---

# Five rules

<div class="body">

<div class="rules">
  <div class="rule"><div class="num">1</div><div class="txt">The <strong>state file</strong> is the source of truth.</div></div>
  <div class="rule"><div class="num">2</div><div class="txt"><strong>Gates</strong> are structural, not optional.</div></div>
  <div class="rule"><div class="num">3</div><div class="txt">Declare <strong>artifacts</strong> before they're needed.</div></div>
  <div class="rule"><div class="num">4</div><div class="txt">Design for <strong>interruption</strong>.</div></div>
  <div class="rule"><div class="num">5</div><div class="txt"><strong>Architecture &gt; intelligence</strong>.</div></div>
</div>

</div>

---

<!-- _class: close -->

# Key takeaways

<div class="body">

- The model is a variable. The structure is the constant.
- The human is a **dial, not a switch** — better models move it up the stakes curve.
- It runs on any model. That part comes free.

<div class="dial">
  <div class="track"><div class="marker" style="left:82%"></div></div>
  <div class="labels"><span>Reviews every step</span><span>Guards only the irreversible</span></div>
</div>

**Reliability at the boundaries.**

</div>
