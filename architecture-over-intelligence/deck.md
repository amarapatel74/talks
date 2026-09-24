---
marp: true
theme: meetup
paginate: true
size: 16:9
html: true
lang: en
footer: 'Architecture Over Intelligence · The AI Fellowship Madrid · 9 Oct 2026'
---

<!-- _class: lead -->
<!-- _paginate: false -->

# Architecture Over Intelligence

<div class="subtitle">How I made AI workflows reliable, even on a small model</div>

<div class="author">Amar Patel · The AI Fellowship Madrid · 9 October 2026</div>

<div class="lead-motif"><span class="a"></span><span class="b"></span><span class="c"></span></div>

<!--
Walk on. Ask for a volunteer before saying anything else.
-->

---

<div class="kicker">A game</div>

# Make one button blue. *Nothing else.*

<div class="body">

<div class="loop">
  <div class="task">Task: make this button blue <i></i></div>
  <span class="a">→</span>
  <div class="c">check the work</div><span class="a">→</span>
  <div class="c">check the check</div><span class="a">→</span>
  <div class="c">call more AI agents</div><span class="a">→</span>
  <div class="c">explain why checking matters</div><span class="a">→</span>
  <div class="c">check again…</div>
</div>

<p style="font-size:0.8em">Opusfived, a parody by Milos Novovic: <b>opusfived.dev</b></p>

<p style="font-size:0.74em;color:var(--muted)">The button never turns blue. And every check is paid for. <span class="coin">€</span> <span class="coin">€</span> <span class="coin">€</span></p>

</div>

<div class="inshort"><b>In short</b> A clever AI can waste time and money on a very simple job.</div>

<!--
Volunteer picks the options. Max 30 seconds of spiral.
"It's funny because it's true, and it's also where my tokens went. Every one of those checks is paid for."
If the site doesn't load in 10 seconds: describe it in two sentences, move on.
-->

---

<div class="kicker">Why I changed models</div>

# I hit my limit, halfway through a meeting

<div class="body">

<div class="cols">
  <div class="card soft">
    <div class="sub">Before · until May 2026</div>
    <h3>Claude (cloud)</h3>
    <ul>
      <li>Very capable</li>
      <li>Usage limit on my plan</li>
      <li>My meetings leave my house</li>
    </ul>
  </div>
  <div class="card good">
    <div class="sub">After · from May 2026</div>
    <h3>Qwen 3.6 (open model, on my desk)</h3>
    <ul>
      <li>Runs on a 16 GB gaming graphics card</li>
      <li>No limit, no bill</li>
      <li>Private: nothing leaves the house</li>
    </ul>
  </div>
</div>

<div class="pullquote">Economics started the journey. Drift is what I found along the way.</div>

</div>

<div class="inshort"><b>In short</b> I moved to a smaller, local AI to save money. Then it started to go wrong.</div>

<!--
My system: morning briefing, inbox, meetings (recording → who said what → summary → actions → filed note).
The limit was the trigger. Cost, privacy, not depending on one vendor were the reasons to stay local.
Say the hinge line slowly.
-->

---

<div class="kicker">Drift</div>

# Smarter is not the same as *correct*

<div class="body">

<div class="word"><em>drift</em> = the AI slowly goes off track, and does not notice</div>

<div class="cols" style="margin-top:14px">
  <div class="card bad">
    <div class="sub">Run 1 · Qwen on my own computer</div>
    <ul class="checks">
      <li><span class="no">✗</span><span class="t">Wrote the note <b>before</b> it knew who was speaking</span></li>
      <li><span class="no">✗</span><span class="t">Named <b>4 of 4</b> speakers wrong. The CEO became “head of alliances”</span></li>
      <li><span class="no">✗</span><span class="t">Deleted the transcript <b>without asking</b></span></li>
    </ul>
  </div>
  <div class="card bad">
    <div class="sub">Run 2 · same model, in the cloud · “seemed smarter”</div>
    <ul class="checks">
      <li><span class="no">✗</span><span class="t"><b>Two</b> notes for one meeting</span></li>
      <li><span class="no">✗</span><span class="t">The <b>wrong date</b></span></li>
      <li><span class="no">✗</span><span class="t"><b>2 of 5</b> quotes given to the wrong person</span></li>
      <li><span class="no">✗</span><span class="t">A Chinese word in an English sentence: “metric归属”</span></li>
    </ul>
  </div>
</div>

<p style="font-size:0.64em;color:var(--muted);margin-top:10px">Test meeting: “OKR Planning with GitLab Executive Team”, GitLab Unfiltered, public on YouTube.</p>

</div>

<div class="inshort"><b>In short</b> Same meeting, my May workflow: both runs went wrong. The smarter one hid it better.</div>

<!--
"On Claude it worked. On Qwen it drifted."
Local run: 35 min, stopped by hand. Cloud run: better precision, same kinds of mistakes, better hidden.
Line: "It knew the right names and still put the wrong name on the quote."
-->

---

<div class="kicker">My first fix: more words</div>

# More instructions. Then *SHOUTING*.

<div class="body">

<div class="cols" style="grid-template-columns: 1.35fr 1fr; align-items:start">
<div>

<div class="metric"><div class="mlabel">Lines in the prompt <span>(instructions to the AI)</span></div>
<div class="bars">
  <div class="when">11 May</div><div class="bar ink" style="width:17.5%">73</div>
  <div class="when">8 Sep</div><div class="bar ink" style="width:100%">416</div>
  <div class="when">17 Sep</div><div class="bar ink" style="width:57.7%">240</div>
</div></div>

<div class="metric"><div class="mlabel">Warnings in CAPITALS <span>(MUST, NEVER, STOP…)</span></div>
<div class="bars">
  <div class="when">11 May</div><div class="bar zero" style="width:8%">0</div>
  <div class="when">8 Sep</div><div class="bar red" style="width:100%">13</div>
  <div class="when">17 Sep</div><div class="bar zero" style="width:8%">0</div>
</div></div>

<div class="metric"><div class="mlabel">Scripts doing the sure things <span>(normal code)</span></div>
<div class="bars">
  <div class="when">11 May</div><div class="bar green" style="width:16.7%">1</div>
  <div class="when">8 Sep</div><div class="bar green" style="width:66.7%">4</div>
  <div class="when">17 Sep</div><div class="bar green" style="width:100%">6</div>
</div></div>

</div>
<div>

<div class="terminal"><span class="who">A real line from 8 September</span>If preconditions fail → STOP. Report gate failure.</div>

<div class="card soft" style="margin-top:14px">
<p><b>It helped a little.</b></p>
<p>But an instruction is only a suggestion. The model can still decide something else matters more.</p>
<p>What worked: moving the work <b>out of the prompt</b> and into code.</p>
</div>

</div>
</div>

</div>

<div class="inshort"><b>In short</b> If you write MUST in capitals, your process is missing a check.</div>

<!--
"Every time it skipped a step, I added a line."
"The prompt got shorter as the scripts took over: one in May, four by early September, six today. The work didn't disappear. It moved out of the prompt."
Source: git history of .pi/skills/process-meeting/SKILL.md.
-->

---

<div class="kicker">The turn</div>

# Split the work into three parts

<div class="body">

<div class="trio">
  <div class="box model">
    <svg viewBox="0 0 48 48" fill="none" stroke="#A63E12" stroke-width="3" stroke-linecap="round" stroke-linejoin="round"><path d="M24 6a12 12 0 0 0-7 21.7V33h14v-5.3A12 12 0 0 0 24 6z"/><path d="M19 38h10M21 43h6"/></svg>
    <div class="who">Model</div>
    <div class="verb">thinks · judgement</div>
    <ul><li>Who is speaking?</li><li>What was important?</li><li>What did people agree to do?</li></ul>
  </div>
  <div class="box scripts">
    <svg viewBox="0 0 48 48" fill="none" stroke="#33544A" stroke-width="3" stroke-linecap="round" stroke-linejoin="round"><circle cx="24" cy="24" r="7"/><path d="M24 5v6M24 37v6M5 24h6M37 24h6M10.6 10.6l4.2 4.2M33.2 33.2l4.2 4.2M10.6 37.4l4.2-4.2M33.2 14.8l4.2-4.2"/></svg>
    <div class="who">Scripts</div>
    <div class="verb">do · the sure things</div>
    <ul><li>Dates and file names</li><li>Filing the note</li><li>Checking every step</li></ul>
  </div>
  <div class="box files">
    <svg viewBox="0 0 48 48" fill="none" stroke="#231B12" stroke-width="3" stroke-linecap="round" stroke-linejoin="round"><path d="M13 5h16l8 8v30H13z"/><path d="M29 5v8h8M18 22h14M18 29h14M18 36h9"/></svg>
    <div class="who">Files</div>
    <div class="verb">remember · memory</div>
    <ul><li>Which step we are on</li><li>What is already done</li><li>What each step produced</li></ul>
  </div>
</div>

<p style="font-size:0.74em;margin-top:14px">Qwen is good at two things: <b>calling tools</b> and <b>writing clean data (JSON)</b>. So I gave it only the judgement. I worked this out with help from Claude and Qwen.</p>

</div>

<div class="inshort"><b>In short</b> The model thinks. Scripts do. Files remember.</div>

<!--
"The fix wasn't a smarter model. It was a different shape."
Full script only: every step is safe to run twice (idempotent), so a restart never duplicates.
The next slides are these three pieces.
-->

---

<div class="kicker">Rule 1</div>

# Give each step to *the right worker*

<div class="body">

<div class="sorter">
  <div class="col model">
    <div class="head">Model</div>
    <div class="q">Does this need judgement?</div>
    <div class="chips"><span class="chip">Who is speaking</span><span class="chip">Summary</span><span class="chip">Actions</span><span class="chip">Priorities</span><span class="chip">Tone of the meeting</span></div>
  </div>
  <div class="col script">
    <div class="head">Script</div>
    <div class="q">Is there only one right answer?</div>
    <div class="chips"><span class="chip">Date and time</span><span class="chip">File name and folder</span><span class="chip">Copy the transcript</span><span class="chip">Link to the project</span><span class="chip">Delete old files</span></div>
  </div>
</div>

<div class="evidence">
  <div class="fact"><div class="big">5,000 words</div><div class="small">In May, the model retyped the whole transcript. It ran out of space, and I paid for every word.</div></div>
  <div class="fact"><div class="big">Wrong date</div><div class="small">The date was already in the file name. The model still got it wrong.</div></div>
  <div class="fact green"><div class="big">1 → 6 scripts</div><div class="small">Today, scripts do the sure things. They get them right every time.</div></div>
</div>

</div>

<div class="inshort"><b>In short</b> If the answer never changes, don’t ask a model.</div>

<!--
Eleven steps: discover, speakers, context, summary, sentiment, personal notes, actions, triage, follow-up, file, clean up.
For non-coders: a "script" can be a template, a spreadsheet formula or a form. Anything that gives the same answer every time.
Link back to Opusfived: paid-for work again.
-->

---

<div class="kicker">Rule 2</div>

# Every step leaves *a file*

<div class="body">

<div class="flow chain">
  <div class="step">Map speakers</div><div class="arrow">→</div>
  <div class="step file">speaker-map.json</div><div class="arrow">→</div>
  <div class="step">Summarise</div><div class="arrow">→</div>
  <div class="step file">summary.json</div><div class="arrow">→</div>
  <div class="step">Find actions</div><div class="arrow">→</div>
  <div class="step file">actions.json</div><div class="arrow">→</div>
  <div class="step">Script files the note</div>
</div>

<div class="cols" style="margin-top:10px">
  <div class="card soft"><h3>A chat forgets</h3><p>It gets long. Early details get lost. You cannot check it.</p></div>
  <div class="card good"><h3>A file remembers</h3><p>The next step starts from the file, not the chat. A script can check it: <b>is it there? does it have the right shape?</b></p></div>
</div>

</div>

<div class="inshort"><b>In short</b> Each step writes down its result, so the next step never has to remember.</div>

<!--
Dashed boxes are files. Solid boxes are work.
The final note is built by a script (file-meeting.py) from these files. The model never writes it by hand.
-->

---

<div class="kicker">Rule 3</div>

# Keep progress *outside* the model

<div class="body">

<div class="cols" style="grid-template-columns: 1fr 1.15fr; align-items:center">
<div>
<div class="statefile">
  <div class="tab"><span><span class="dot"></span>progress file</span><span>.process-state.json</span></div>
  <div class="fields">
    <div class="field active"><span class="key">current_step</span><span class="val">2.6</span></div>
    <div class="field"><span class="key">completed_steps</span><span class="val">-1, 0, 1, 2, 2.5, 2.6</span></div>
    <div class="field"><span class="key">speakers confirmed</span><span class="val">yes</span></div>
    <div class="field"><span class="key">aborted</span><span class="val">no</span></div>
  </div>
</div>
</div>
<div>
<div class="resume"><div class="seg" data-label="working"></div><div class="file" data-label="I quit">step 3<br>half done</div><div class="gap"></div><div class="file" data-label="I reopen">reads<br>the file</div><div class="seg end" data-label="carries on"></div></div>
<p style="font-size:0.76em;margin-top:14px">In rehearsal I closed everything in the middle of step 3. When I opened it again, it read this file and carried on. <b>Nothing repeated. Nothing lost.</b></p>
<p style="font-size:0.76em">The same file let me run the demos on my local Qwen and on a cloud copy <b>without changing a line</b>.</p>
</div>
</div>

</div>

<div class="inshort"><b>In short</b> The model forgets everything. The file doesn’t.</div>

<!--
"The model had forgotten everything. The file hadn't."
Also an audit trail: open the file and see what ran and who said yes.
-->

---

<div class="kicker">Rule 4</div>

# Check *before* and *after* every step

<div class="body">

<div class="gatecheck">
  <div class="q"><b>Before</b>Is it this step’s turn?</div>
  <div class="arr">→</div>
  <div class="work">Do the step</div>
  <div class="arr">→</div>
  <div class="q"><b>After</b>Did it leave its file, in the right shape?</div>
</div>
<div class="stopline"><div class="stop">if no<br><span>STOP</span></div><div></div><div></div><div></div><div class="stop">if no<br><span>STOP</span></div></div>

<div class="cols">
  <div class="card">
    <div class="sub">What it caught · speakers</div>
    <p>The model still guessed wrong: <b>2 of 4</b> speakers. But now it had to <b>stop and show me first</b>. I fixed it in one line, before anything was written.</p>
  </div>
  <div class="card">
    <div class="sub">What it caught · my own bug</div>
    <p>A time-zone bug made the script look for a <b>12:00</b> note. The meeting was at <b>10:00</b>. It <b>refused to file</b> and stopped, instead of filing at the wrong time.</p>
  </div>
</div>

</div>

<div class="inshort"><b>In short</b> The check is a small script, not a sentence in the prompt. The model cannot talk it round.</div>

<!--
"If either answer is no, stop. Don't patch it by hand: a hand patch is exactly the drift you're trying to catch."
Script: scripts/meeting-state.py check / complete. Prints GATE BLOCKED.
Don't claim both bugs failed loudly: only the time shift did.
-->

---

<div class="kicker">Rule 5</div>

# Mark the *one-way doors*

<div class="body">

<div class="cols" style="grid-template-columns: 0.9fr 1.3fr; align-items:start">
<div>
<div class="dialog">
  <div class="bar">One-way door · step 7: clean up</div>
  <div class="msg">Delete the transcript, audio and progress file for this meeting. Approve?</div>
  <div class="btns"><span class="btn ok">Approve</span><span class="btn block">Block</span></div>
</div>
<p style="font-size:0.7em;margin-top:12px"><b>Only I can answer.</b> The model can’t see or click it. Every click is logged.</p>
<p style="font-size:0.7em;margin-top:8px"><b style="color:var(--structure)">Retests after Block: 3 of 3</b> stopped and asked.</p>
</div>
<div>
<div class="terminal"><span class="who">After I click Block · Pi → model</span><span class="gate">Amar blocked step 7. Stop and ask what to change. This session can now only read files and run the pipeline scripts.</span></div>
<div class="card" style="margin-top:10px">
  <div class="sub">And the shell makes sure</div>
  <ul class="checks">
    <li><span class="no">✗</span><span class="t">Delete, move or copy files · edit in place</span></li>
    <li><span class="no">✗</span><span class="t">Run code it wrote itself · run any other script</span></li>
    <li><span class="no">✗</span><span class="t">Try the same door again</span></li>
    <li><span class="yes">✓</span><span class="t">Read files · run my pipeline scripts · ask me</span></li>
  </ul>
</div>
</div>
</div>

</div>

<div class="inshort"><b>In short</b> The instruction says stop. The shell makes sure.</div>

<!--
Status quo. Two layers: the message tells the model to stop; the shell refuses anything else.
Backstory, if asked: in the first tests the model heard "stop" and tried --dry-run (harmless), offered to rewrite the script, and once ran it again with bash -x. That is why the shell layer exists.
Retests after the fix: 3 of 3 stopped and asked. The model didn't try another way, so the refusals were proven in tests, not live: 19 workarounds and 8 ways of running the door script, all caught.
Limit: the model runs as me. This stops drift and the usual workarounds. It is not a security sandbox.
Only on the doors: nine hard gates per meeting would train me to rubber-stamp.
-->

---

<div class="kicker">The result</div>

# My flow in May vs *my flow today*

<div class="body">

<div class="score">
  <div class="h">Same meeting, model and answers</div><div class="h old">May</div><div class="h new">Today</div>
  <div class="lab">Checked the speakers before writing</div><div><span class="no">✗</span></div><div><span class="yes">✓</span></div>
  <div class="lab">Quotes given to the right person</div><div><span class="no">✗</span> 3 of 5</div><div><span class="yes">✓</span> 5 of 5</div>
  <div class="lab">One note per meeting</div><div><span class="no">✗</span> 2 notes</div><div><span class="yes">✓</span> 1 note</div>
  <div class="lab">Right date</div><div><span class="no">✗</span></div><div><span class="yes">✓</span></div>
  <div class="lab">Asked before filing or deleting</div><div><span class="no">✗</span></div><div><span class="yes">✓</span></div>
  <div class="lab">Carried on after I quit</div><div><span class="na">–</span> not tested</div><div><span class="yes">✓</span></div>
</div>

<p style="font-size:0.66em;color:var(--muted);margin-top:10px">To be fair: today’s flow also has more steps and better scripts. This is not a lab test. It is my flow in May against my flow today.</p>

</div>

<div class="inshort"><b>In short</b> The old flow went wrong quietly. The new one goes wrong loudly, early, and where I can fix it.</div>

<!--
"More steps means more places to go wrong. It also means more places to notice."
May column = cloud run A (qwen3.6-35b-a3b on OpenRouter). Today = run B, same model.
If asked: ten unchecked steps at 95% each come out right only about 60% of the time.
-->

---

<div class="kicker">What’s next</div>

# A new kind of model arrived *this month*

<div class="body">

<div class="cols three" style="align-items:stretch">
  <div class="card">
    <div class="sub">Split the model box in two</div>
    <p><b>Writer:</b> summary, actions.</p>
    <p><b>Decider:</b> picks one answer from a list.</p>
    <p style="color:var(--muted)">Jev (TypeSafe AI) only decides: a choice, and how sure it is.</p>
  </div>
  <div class="card">
    <div class="sub">Steps that only choose</div>
    <div class="chips" style="display:flex;flex-wrap:wrap;gap:6px">
      <span class="chip" style="background:#fff;border:1px solid var(--ink);border-radius:999px;padding:3px 10px;font-size:0.56em;font-weight:600">Sentiment</span>
      <span class="chip" style="background:#fff;border:1px solid var(--ink);border-radius:999px;padding:3px 10px;font-size:0.56em;font-weight:600">Priority</span>
      <span class="chip" style="background:#fff;border:1px solid var(--ink);border-radius:999px;padding:3px 10px;font-size:0.56em;font-weight:600">Project</span>
      <span class="chip" style="background:#fff;border:1px solid var(--ink);border-radius:999px;padding:3px 10px;font-size:0.56em;font-weight:600">Follow-up? yes / no</span>
      <span class="chip" style="background:#fff;border:1px solid var(--ink);border-radius:999px;padding:3px 10px;font-size:0.56em;font-weight:600">Duplicate? yes / no</span>
      <span class="chip" style="background:#fff;border:1px solid var(--ink);border-radius:999px;padding:3px 10px;font-size:0.56em;font-weight:600">Speaker</span>
    </div>
    <p style="margin-top:8px">The flow offers only legal options. A script checks the answer.</p>
  </div>
  <div class="card good">
    <div class="sub">How sure is it?</div>
    <div class="dial"><div class="track"><div class="marker" style="left:78%"></div></div><div class="labels"><span>ask me</span><span>carry on</span></div></div>
    <p style="margin-top:8px">I am asked only when it is unsure.</p>
    <p style="color:var(--muted)">Not wired in yet. This is where it would go.</p>
  </div>
</div>

</div>

<div class="inshort"><b>In short</b> A new model slots into one box. The flow stays the same.</div>

<!--
"Models change every month. This month, a new kind arrived: a model that only decides."
Jev: TypeSafe AI, early access 15 Sep 2026. Returns typed values (a choice) with probabilities; TypeSafe claims it can't hallucinate because it doesn't write text.
Write-up that week: "Jev at the Branches: The State Machine Is the Agent" (StackToHeap, 21 Sep).
Be clear: not wired in yet.
-->

---

<!-- _class: close -->

<div class="kicker">Take these home</div>

# Five rules. *None of them needs code.*

<div class="body">

<div class="rules">
  <div class="rule"><div class="num">1</div><div class="txt"><b>Write the steps down,</b> and give each one to the right worker. <span style="color:#A79987">Judgement → model. One right answer → script.</span></div></div>
  <div class="rule"><div class="num">2</div><div class="txt"><b>Make every step leave a file.</b> <span style="color:#A79987">The next step starts from the file, not the chat.</span></div></div>
  <div class="rule"><div class="num">3</div><div class="txt"><b>Keep progress outside the model.</b> <span style="color:#A79987">A note says which step you are on.</span></div></div>
  <div class="rule"><div class="num">4</div><div class="txt"><b>Check before and after every step.</b> <span style="color:#A79987">If the answer is no, stop.</span></div></div>
  <div class="rule"><div class="num">5</div><div class="txt"><b>Mark the one-way doors.</b> <span style="color:#A79987">A person says yes before anything is sent, deleted or filed.</span></div></div>
</div>

<p style="font-size:0.8em;margin-top:10px"><b>Try one tonight:</b> take your messiest AI workflow. Write down the steps, and what each one should produce.</p>

</div>

<div class="inshort"><b>In short</b> A checklist and a shared folder are enough to start.</div>

<!--
Leave this slide up through Q&A.
Limit: "my gate stops a model that drifts past a question. It won't stop one that edits the file on purpose. That's the next layer."
Works for a hiring process, a month-end report, customer onboarding.
-->

---

<!-- _class: close lead -->
<!-- _paginate: false -->

# Models change every month.

<div class="pullquote" style="font-size:1.35em">Build your flow so the model is a part you can swap.</div>

<div class="author" style="color:#A79987;margin-top:1.2em">Thank you · Amar Patel · linkedin.com/in/amarapatel</div>

<!--
"I started this to save money. I ended up with a system I trust more than the frontier model I left, running on a graphics card built for games."
Then the take-home line. Thank you. Go back to the five-rules slide for Q&A.
-->
