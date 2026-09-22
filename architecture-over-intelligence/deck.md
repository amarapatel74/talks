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

One request sets off a chain of small jobs.

Each job needs the one before it. If one step drifts, every step after it drifts too.

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

A demo works because nothing interrupts it.

In production, the session ends, the model changes, and a mistake now costs you.

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

Every step reads the same file before it runs, and the file says exactly where you are.

If the session dies, the next run starts from that same file and that same step.

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

You don't remove the human as the model improves. You move it: from checking every step, to guarding only the steps that cannot be undone.

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

Each step says what it creates, and the next step checks that file really exists before it runs.

A step that is missing its input stops and tells you, instead of guessing.

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

A single file remembers where the work stopped.

It survives a crash, a context reset, and you walking away.

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
  <div class="stat"><div class="n">9</div><div class="l">Human checkpoints</div></div>
  <div class="stat"><div class="n">4</div><div class="l">Scripts</div></div>
  <div class="stat"><div class="n">1</div><div class="l">State file</div></div>
</div>

Nine of the 11 steps stop for a human; two do not.

</div>

---

# What broke in three months

<div class="body">

<div class="icon-list">
  <div class="row"><div class="badge">✎</div><div class="txt">The state file got edited to the wrong step → <em>validate on load</em></div></div>
  <div class="row"><div class="badge">⏭</div><div class="txt">The LLM skipped a gate → <em>the file decides, not the prompt</em></div></div>
  <div class="row win"><div class="badge">✓</div><div class="txt">Compaction mid-pipeline → <em>the file survived</em> — the win</div></div>
</div>

Trust the file, and check the artifact itself.

Never trust what the previous step said it did.

</div>

---

# The model is always changing

<div class="body">

<div class="timeline">
  <div class="point"><div class="pt-title">2024</div><p>A bigger model fixed it.</p></div>
  <div class="point"><div class="pt-title">2026</div><p>Bigger models stopped helping. New models kept coming.</p></div>
  <div class="point"><div class="pt-title">This month</div><p><strong>Jev</strong> — decisions, not text.</p></div>
</div>

<div class="pullquote">"The State Machine Is the Agent."</div>

The model changes every month. What stays reliable is the structure around it, not the model itself.

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

The kind of work changes. The pattern stays the same.

One pattern, many jobs: customer calls, email triage, KPI reports.

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

- The model changes. The structure stays.
- Better models do not remove the human; they move it to the irreversible steps.
- The same structure works with any model. That part comes free.

<div class="dial">
  <div class="track"><div class="marker" style="left:82%"></div></div>
  <div class="labels"><span>Reviews every step</span><span>Guards only the irreversible</span></div>
</div>

**Reliability at the boundaries.**

</div>
