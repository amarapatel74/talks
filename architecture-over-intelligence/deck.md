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

<div class="eyebrow">1 of 4 · What broke</div>

# It didn't crash. *That was the problem.*

<div class="body">

<div class="pipeline">
  <div class="node auto"><span class="dot"></span>step 1</div>
  <div class="node auto"><span class="dot"></span>step 2</div>
  <div class="node bad"><span class="dot"></span>step 3 · drifts</div>
  <div class="node ghost"><span class="dot"></span>step 4</div>
  <div class="node ghost"><span class="dot"></span>step 5 · stops noticing</div>
  <div class="node ghost"><span class="dot"></span>runs to the end</div>
</div>

I moved my meeting pipeline off a frontier model onto a cheaper one. It ran all the way through.

It drifted at step 3. By step 5 it had stopped noticing. No exception, no crash, no alert: just plausible output, filed.

</div>

---

<div class="eyebrow">1 of 4 · What broke</div>

# A chain hides its own failure

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

"Investigate this incident" sounds like one job. It is a chain of small jobs, and each one treats the last one's output as fact.

Step 4 trusts step 3. Step 5 trusts step 4. A mistake doesn't stop the chain. It rides it to the end.

</div>

---

<div class="eyebrow">1 of 4 · What broke</div>

# My demo never showed me this

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

A demo works because nothing interrupts it, I'm watching, and I'm running the best model I can buy.

None of those hold on a Tuesday. **Reliability fails at the boundaries**, and no model is big enough to fix a boundary.

</div>

---

<div class="eyebrow">2 of 4 · The rebuild</div>

# Stop letting the model *remember*

<div class="body">

<div class="failure"><span class="lbl">Failure</span><span class="t">It lost its place, and redid work it had already done.</span></div>

<div class="statefile">
  <div class="tab"><span><span class="dot"></span>process-state.json</span><span>meeting_id: 2026-01-15_140000</span></div>
  <div class="fields">
    <div class="field active"><span class="key">current_step</span><span class="val">2.6</span></div>
    <div class="field"><span class="key">completed_steps</span><span class="val">[-1, 0, 1, 2, 2.5, 2.6]</span></div>
    <div class="field"><span class="key">user_confirmed.summary</span><span class="val">true</span></div>
    <div class="field"><span class="key">aborted</span><span class="val">false</span></div>
  </div>
</div>

One file on disk holds the position: read before every step, written after.

The model proposes. The file decides.

</div>

---

<div class="eyebrow">2 of 4 · The rebuild</div>

# Stop asking the model for *permission*

<div class="body">

<div class="failure"><span class="lbl">Failure</span><span class="t">Told to present the summary and continue, it continued. Nobody had said yes.</span></div>

<div class="gates">
  <div class="gate auto">
    <div class="dot"></div>
    <div class="gate-title">Auto</div>
    <p>Runs without stopping. Nothing here is irreversible.</p>
    <p class="when">Reads, health checks, sentiment.</p>
  </div>
  <div class="gate human">
    <div class="dot"></div>
    <div class="gate-title">Human</div>
    <p>Stops and waits for your yes.</p>
    <p class="when">Anything that changes data.</p>
  </div>
  <div class="gate blocked">
    <div class="dot"></div>
    <div class="gate-title">Blocked</div>
    <p>Cannot continue. Something required is missing.</p>
    <p class="when">Missing file, corrupted state.</p>
  </div>
</div>

A gate isn't asking whether the model got it right. It's asking whether a human said yes. That's authority, not quality control.

</div>

---

<div class="eyebrow">2 of 4 · The rebuild</div>

# Stop trusting the *previous step*

<div class="body">

<div class="failure"><span class="lbl">Failure</span><span class="t">A script failed quietly. The state said it had worked. The next step ran on a file that was never written.</span></div>

<div class="flow">
  <div class="step">Step 0<br>Speaker Mapping</div>
  <div class="arrow note"><b>→</b>produces</div>
  <div class="step file">speaker-map.json</div>
  <div class="arrow note"><b>→</b>required by</div>
  <div class="step">Step 1<br>Context Extraction</div>
</div>

Every step declares two things: what it produces, and what it needs.

Before it runs, it looks on disk for its inputs. Missing? It stops and says which file. It never guesses.

</div>

---

<div class="eyebrow">2 of 4 · The rebuild</div>

# Assume you will be *interrupted*

<div class="body">

<div class="failure"><span class="lbl">Failure</span><span class="t">The context compacted mid-pipeline. The model forgot everything it was doing.</span></div>

<div class="resume-labels"><span>step 3 finishes</span><span>context reset · you walk away</span><span>next run resumes at 4</span></div>
<div class="resume">
  <div class="seg"></div>
  <div class="file">aborted:<br>true</div>
  <div class="gap"></div>
  <div class="file">step: 3<br>→ step 4</div>
  <div class="seg"></div>
</div>

The file is written after every step, so nothing depends on memory. The reset wiped the model. The file was still on disk.

The next run read it and carried on at step 4. This is the one that made me trust the whole thing.

</div>

---

<div class="eyebrow">3 of 4 · In production</div>

# What it runs, every working day

<div class="body">

Transcript in. Speakers named, summary written, actions pulled and triaged, note filed to my vault.

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
  <div class="stat"><div class="n">325</div><div class="l">Meetings processed</div></div>
  <div class="stat"><div class="n">9</div><div class="l">Human checkpoints</div></div>
  <div class="stat"><div class="n">8</div><div class="l">LLM calls</div></div>
  <div class="stat"><div class="n">1</div><div class="l">State file</div></div>
</div>

</div>

---

<div class="eyebrow">3 of 4 · In production</div>

# What still broke

<div class="body">

<div class="icon-list">
  <div class="row"><div class="badge">✎</div><div class="txt">The state file got edited to the wrong step → <em>validate on load</em></div></div>
  <div class="row"><div class="badge">⏭</div><div class="txt">The model skipped a gate → <em>the file decides, not the prompt</em></div></div>
  <div class="row win"><div class="badge">✓</div><div class="txt">The context reset mid-pipeline → <em>the file survived, and it resumed where it left off</em></div></div>
</div>

Months of daily use, three failures. Each fix moved a decision out of the model and into the file.

Trust the file. Check the artifact itself. Never trust what the last step claims it did.

</div>

---

<div class="eyebrow">4 of 4 · What's next</div>

# The model that should have made this *obsolete*

<div class="body">

<div class="timeline">
  <div class="point"><div class="pt-title">2024</div><p>A bigger model fixed the problem.</p></div>
  <div class="point"><div class="pt-title">2026</div><p>Bigger stopped helping. New models kept coming.</p></div>
  <div class="point"><div class="pt-title">This month</div><p><strong>Jev</strong>: it returns a decision, not a paragraph.</p></div>
</div>

Jev cannot hallucinate, because it never writes text. If anything was going to retire this talk, it was this. Then I read the best write-up of it, titled:

<div class="pullquote">The State Machine Is the Agent.</div>

Days later researchers bent its decisions with prompt injection. Even a model built to be safe sits behind a deterministic loop, with a human on the irreversible branch.

</div>

---

<div class="eyebrow">4 of 4 · What's next</div>

# The same shape fits your work

<div class="body">

<div class="mapping">
  <div class="row"><div class="left">Meeting processing</div><div class="arrow">→</div><div class="right">Customer-call analysis</div></div>
  <div class="row"><div class="left">Inbox routing</div><div class="arrow">→</div><div class="right">Email triage &amp; classification</div></div>
  <div class="row"><div class="left">Task tracking</div><div class="arrow">→</div><div class="right">KPI dashboards</div></div>
  <div class="row"><div class="left">Model-agnostic guardrails</div><div class="arrow">→</div><div class="right">Many models, one coordinator</div></div>
</div>

Every row is the same shape: read, transform, a human approves, write to the system of record. Only the nouns change.

</div>

---

<div class="eyebrow">4 of 4 · What's next</div>

# Run these three over your own agent

<div class="body">

<div class="rules">
  <div class="rule"><div class="num">1</div><div class="txt">What is your single source of truth: a file, a row, a record?</div></div>
  <div class="rule"><div class="num">2</div><div class="txt">Where does it stop for a human, and can a step skip that stop?</div></div>
  <div class="rule"><div class="num">3</div><div class="txt">Does each step check its inputs exist before it runs?</div></div>
</div>

<div class="dial">
  <div class="track"><div class="marker" style="left:28%"></div></div>
  <div class="labels"><span>Reviews every step</span><span>Guards only the irreversible</span></div>
</div>

You don't remove the human as models improve. You move the dial: from checking every step, to guarding the steps that cannot be undone.

</div>

---

<!-- _class: close -->

<div class="eyebrow">Key takeaways</div>

# Architecture over intelligence

<div class="body">

- **The model changes. The structure stays.** A chain breaks wherever nothing supervises it. Put a file in charge.
- **The human doesn't disappear.** Better models move it to the irreversible steps. It cannot delete, overwrite or send without a yes.
- **The same structure runs any model.** When next month's model lands, your workflow doesn't change.

Tonight: add one field, `current_step`. Read it back before every step. Stop if it doesn't match.

**Reliability at the boundaries.**

</div>
