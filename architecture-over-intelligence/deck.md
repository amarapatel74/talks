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
    <div class="panel-title">In a demo</div>
    <ul>
      <li>One uninterrupted session</li>
      <li>Mistakes are annoying</li>
      <li>A human watches the whole time</li>
      <li>A frontier model</li>
    </ul>
  </div>
  <div class="panel prod">
    <div class="panel-title">In production</div>
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
    <div class="field active"><span class="key">current_step</span><span class="val">2.6</span></div>
    <div class="field"><span class="key">completed_steps</span><span class="val">[-1, 0, 1, 2, 2.5, 2.6]</span></div>
    <div class="field"><span class="key">user_confirmed.summary</span><span class="val">true</span></div>
    <div class="field"><span class="key">aborted</span><span class="val">false</span></div>
  </div>
</div>

Every step reads the same file before it runs. It shows the last step done — here 2.6, so the next is 3.

If the session dies, the next run starts from that same file and that same step, and redoes nothing.

</div>

---

# A gate is *authority*, not just quality control

<div class="body">

<div class="gates">
  <div class="gate auto">
    <div class="dot"></div>
    <div class="gate-title">Auto</div>
    <p>Runs without stopping. Nothing irreversible here, and a human checks it at a later gate.</p>
    <p class="when">Reads, health checks, sentiment.</p>
  </div>
  <div class="gate human">
    <div class="dot"></div>
    <div class="gate-title">Human</div>
    <p>Stops and waits for your yes before continuing.</p>
    <p class="when">Anything that changes data.</p>
  </div>
  <div class="gate blocked">
    <div class="dot"></div>
    <div class="gate-title">Blocked</div>
    <p>Cannot continue. Something required is missing or broken.</p>
    <p class="when">Missing file, corrupted state.</p>
  </div>
</div>

"Auto" does not mean the model got it right. It means a mistake here is cheap and always reviewed later.

<div class="dial">
  <div class="track"><div class="marker" style="left:28%"></div></div>
  <div class="labels"><span>Reviews every step</span><span>Guards only the irreversible</span></div>
</div>

You don't remove the human as the model improves. You move it: from checking every step, to guarding only the steps that cannot be undone.

</div>

---

# Each step declares its *contract*

<div class="body">

<div class="flow">
  <div class="step">Step 0<br>Speaker Mapping</div>
  <div class="arrow">→ produces</div>
  <div class="step file">speaker-map.json</div>
  <div class="arrow">→ required by</div>
  <div class="step">Step 1<br>Context Extraction</div>
</div>

Every step declares two things: what it produces, and what it needs.

Before a step runs, it checks its inputs really exist on disk. Missing input? It stops and says so — never guesses.

</div>

---

# Interruption is not a bug

<div class="body">

<div class="resume-labels"><span>step 3 finishes</span><span>context reset · you walk away</span><span>next run resumes at 4</span></div>
<div class="resume">
  <div class="seg"></div>
  <div class="file">aborted:<br>true</div>
  <div class="gap"></div>
  <div class="file">step: 3<br>→ step 4</div>
  <div class="seg"></div>
</div>

The file is written after every step, so nothing depends on memory.

Step 3 finishes → the file is on disk → the session dies (context reset, or you leave) → the next run reads the file → resumes at 4, redoing nothing.

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

<div class="legend">
  <span><i class="d-human"></i>stops for a human</span>
  <span><i class="d-auto"></i>runs alone</span>
  <span><i class="d-opt"></i>optional</span>
</div>

<div class="stats">
  <div class="stat"><div class="n">9</div><div class="l">Human checkpoints</div></div>
  <div class="stat"><div class="n">4</div><div class="l">Scripts</div></div>
  <div class="stat"><div class="n">1</div><div class="l">State file</div></div>
</div>

Nine of the 11 steps stop for a human; two do not (sentiment runs alone, cleanup is optional).

</div>

---

# What broke after months of daily use

<div class="body">

I ran this every working day for months — processing my meetings end to end. Three things broke, and each fix pushed the decision into the file, not into the model.

<div class="icon-list">
  <div class="row"><div class="badge">✎</div><div class="txt">The state file got edited to the wrong step → <em>validate on load</em></div></div>
  <div class="row"><div class="badge">⏭</div><div class="txt">The LLM skipped a gate → <em>the file decides, not the prompt</em></div></div>
  <div class="row win"><div class="badge">✓</div><div class="txt">The context reset mid-pipeline → <em>the file survived, and it resumed where it left off</em> — the win</div></div>
</div>

Trust the file, and check the artifact itself.

Never trust what the previous step said it did.

</div>

---

# The model is always changing

<div class="body">

<div class="timeline">
  <div class="point"><div class="pt-title">2024</div><p>A bigger model fixed the problem.</p></div>
  <div class="point"><div class="pt-title">2026</div><p>Bigger models stopped helping. New models kept coming.</p></div>
  <div class="point"><div class="pt-title">This month</div><p><strong>Jev</strong> — it returns a decision, not a paragraph.</p></div>
</div>

Scaling hit a wall: paying for a bigger model stopped buying reliability. Yet a new model still lands every month, so the pressure moves onto the cheaper ones — which is exactly where the drift lives.

This month it was Jev. It cannot hallucinate, because it does not write text. But the first good write-up of it was titled:

<div class="pullquote">"The State Machine Is the Agent."</div>

And days later, researchers showed prompt injection can still bend its decisions. Even a model built to be safe sits behind a deterministic loop and a human.

The model changes every month. What stays reliable is the structure around it, not the model.

</div>

---

# The same pattern fits many jobs

<div class="body">

<div class="mapping">
  <div class="row"><div class="left">Meeting processing</div><div class="arrow">→</div><div class="right">Customer-call analysis</div></div>
  <div class="row"><div class="left">Inbox routing</div><div class="arrow">→</div><div class="right">Email triage &amp; classification</div></div>
  <div class="row"><div class="left">Task tracking</div><div class="arrow">→</div><div class="right">KPI dashboards</div></div>
  <div class="row"><div class="left">Model-agnostic guardrails</div><div class="arrow">→</div><div class="right">Many models, one coordinator</div></div>
</div>

Every row is the same shape: read → transform → a human approves → write to the system of record. Only the names change.

"Many models, one coordinator" just means several different models, each doing a step, all managed by the same state file.

</div>

---

# Five rules — and how to check your own workflow

<div class="body">

<div class="rules">
  <div class="rule"><div class="num">1</div><div class="txt">The <strong>state file</strong> is the source of truth.</div></div>
  <div class="rule"><div class="num">2</div><div class="txt"><strong>Gates</strong> are structural, not optional.</div></div>
  <div class="rule"><div class="num">3</div><div class="txt">Declare <strong>artifacts</strong> before they're needed.</div></div>
  <div class="rule"><div class="num">4</div><div class="txt">Design for <strong>interruption</strong>.</div></div>
  <div class="rule"><div class="num">5</div><div class="txt"><strong>Architecture &gt; intelligence</strong>.</div></div>
</div>

Run these three questions over your own agent:

<div class="rules">
  <div class="rule"><div class="num">a</div><div class="txt">What is your single source of truth — a file, a row, a record?</div></div>
  <div class="rule"><div class="num">b</div><div class="txt">Where do you stop for a human, and can a step be skipped without one?</div></div>
  <div class="rule"><div class="num">c</div><div class="txt">Does each step check its inputs exist before it runs?</div></div>
</div>

Tonight: add one field — current_step — read it back before each step, and stop if it doesn't match.

</div>

---

<!-- _class: close -->

# Key takeaways

<div class="body">

- **The model changes. The structure stays.** A chain of steps breaks wherever it isn't supervised. Put a state file in charge.
- **The human does not disappear.** Better models move it to the irreversible steps. Consequence: the model cannot delete, overwrite, or send without a yes.
- **The same structure runs any model.** When next month's model arrives, your workflow does not change.

**Reliability at the boundaries.**

</div>