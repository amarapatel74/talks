# AI Meetup Talk — 15-Minute Delivery Script

**Title:** *Architecture Over Intelligence: Designing Reliable AI Workflows*
**Subtitle:** *A state-machine approach to reliable, interruptible, auditable AI workflows*
**Length:** 15 min talk + 5 min Q&A
**Audience:** AI practitioners / developers at a meetup
**Deck:** `architecture-over-intelligence/deck.md`, 14 slides in four acts

> Slide numbers match the page numbers printed on the deck. The Opusfived demo runs live before slide 1 and has no slide of its own. The untrimmed version lives in `script-full.md`.

## The shape of the talk

| Act | Slides | Beat |
|---|---|---|
| Hook | live | Opusfived: an agent that never finishes one small job |
| 1 · What broke | 2-4 | My pipeline drifted and never told me. Why the chain hid it. Why the demo hid it. |
| 2 · The rebuild | 5-8 | Four failures, four fixes: the file, the gate, the contract, interruption |
| 3 · In production | 9-10 | What it runs daily. What still broke. |
| 4 · What's next | 11-14 | The model that should have retired this talk. Your turn. |

**The through-line:** AI doesn't fail because a model is stupid. It fails at the boundaries, where a long process meets an interrupted session, a cheaper model, and a real consequence. The fix is not a bigger brain. It's a sturdier structure.

---

## SLIDE 0: Opusfived (live demo, no slide)

**What's on screen:** The Opusfived site, https://opusfived.dev/, by Milos Novovic. A parody of asking a frontier model to make one button blue, which spirals into 23 agents, endless self-verification, and no finished task. It went viral on Hacker News.

**Action:** Open it. Click the button. Let the agent chain spiral for 10 to 15 seconds.

**Talking notes:**

> "I want to start with a game. This is Opusfived, by Milos Novovic.
>
> The task is one line: make this button blue. Change nothing else.
>
> Here's what happens instead."

**(Click. Let it run.)**

> "It verifies. Then it re-verifies. Then it justifies the verification. Then it calls 23 agents to check its own work. And it never does the thing you asked.
>
> This is what happens when the model controls its own loop. It's funny because it's an extreme case.
>
> I'm going to show you the everyday version. It's not funny, because it doesn't look like anything at all."

**Time:** ~1 min

---

## SLIDE 1: Title

**Talking notes:**

> "Architecture over intelligence. I'm Amar. I've been running AI workflows in production daily for months, and this is what I learned the hard way."

**Time:** ~10 sec

---

## SLIDE 2 — Act 1: "It didn't crash. That was the problem."

**What's on the slide:** A pipeline that drifts. Steps 1 and 2 clean, step 3 marked as drifting, everything after it ghosted, still running to the end.

**Talking notes:**

> "Here's what happened to me.
>
> I have a pipeline that processes my meeting transcripts end to end. It worked. I'd been running it for months on a frontier model. Then I moved it onto a cheaper model that I run locally.
>
> It didn't crash. Sit with that for a second, because that's the whole problem.
>
> It ran all the way to the end. It produced output that looked right. It drifted at step 3, and by step 5 it had stopped noticing. No exception, no stack trace, no alert. Plausible output, filed into my notes, wrong.
>
> A crash is a gift. A crash tells you exactly where you are. This told me nothing."

**Time:** ~1.5 min

---

## SLIDE 3 — Act 1: "A chain hides its own failure"

**What's on the slide:** The incident-response chain. Six boxes, five arrows.

**Talking notes:**

> "Why didn't it notice? Because of the shape of the work.
>
> 'Investigate this incident' sounds like one job. It isn't. Pull the logs. Work out which systems were hit. Judge the severity. Draft a response. Page a human. Write the postmortem.
>
> Six steps, and each one takes the previous step's output as fact. Step 4 trusts step 3. Step 5 trusts step 4. Nothing in that chain is checking anything.
>
> So a mistake doesn't stop the chain. It rides it to the end, and it gets more confident on the way."

**Time:** ~1 min

---

## SLIDE 4 — Act 1: "My demo never showed me this"

**What's on the slide:** Demo panel against production panel, four rows each.

**Talking notes:**

> "Fair question: how did I not catch this earlier? Because my demo was lying to me.
>
> When I demo, it's one session, start to finish. I'm watching every step. I'm on the best model I can buy. And if the output is wrong, I delete it and run it again.
>
> Production is the opposite of all four. The session compacts or resets. I've walked away and I check hours later. The model is cheaper. And the output gets filed into my vault, so wrong doesn't mean annoying, it means data loss.
>
> Not one of those four is a model problem. They are all boundaries: where the session ends, where the model changes, where nobody is looking. Reliability fails at the boundaries, and you cannot buy your way out of a boundary with a bigger model."

**Time:** ~1.25 min

---

## SLIDE 5 — Act 2: "Stop letting the model remember"

**Failure on the slide:** It lost its place, and redid work it had already done.

**What's on the slide:** `process-state.json` as a card: `current_step`, `completed_steps`, `user_confirmed.summary`, `aborted`.

**Talking notes:**

> "So I rebuilt it. Four changes, and every one of them came from something that broke.
>
> First: it lost its place. Context compacts, or I re-invoke it, and it redoes work it already did. Notes filed twice.
>
> The fix is that the model doesn't get to remember. One JSON file on disk holds the position.
>
> `current_step` is the last step finished. Here it's 2.6, so the next one is 3. `completed_steps` is everything done, so a restart skips it. `user_confirmed` holds my approvals. `aborted` is the kill switch.
>
> Every step reads that file before it runs and writes it after. A step may only run when `current_step` is the one before it. Otherwise it stops.
>
> The model proposes. The file decides."

**Time:** ~1.5 min

---

## SLIDE 6 — Act 2: "Stop asking the model for permission"

**Failure on the slide:** Told to present the summary and continue, it continued. Nobody had said yes.

**What's on the slide:** Three gate cards (Auto, Human, Blocked) and the dial.

**Talking notes:**

> "Second: I told it to present the summary and then continue. So it continued. Nobody had said yes.
>
> That's when I stopped treating human review as a line in a prompt and made it a property of the step.
>
> Three kinds. Auto: run and keep going, nothing here is irreversible, and a human sees it at a later gate anyway. Human: do the step, show me, stop, wait for my yes. Blocked: something required is missing, so don't guess.
>
> And here's the part that matters more than quality. A gate is not asking 'did the model get this right'. It's asking 'has a human said yes'. That's authority.
>
> Deleting, overwriting, replying to a customer: those should not happen on a model's confidence, no matter how good the model gets."

**Time:** ~1.5 min

---

## SLIDE 7 — Act 2: "Stop trusting the previous step"

**Failure on the slide:** A script failed quietly. The state said it had worked. The next step ran on a file that was never written.

**What's on the slide:** Step 0 produces `speaker-map.json`, required by Step 1.

**Talking notes:**

> "Third: a script failed quietly. The state file said it had succeeded. The next step ran on a file that was never written.
>
> So every step now declares two things: what it produces, and what it needs. Step 0 produces `speaker-map.json`. Step 1 requires it.
>
> Before step 1 runs, it looks on disk. Is the file actually there? Not 'did the last step claim success'. Is it there.
>
> If it's missing, it stops and names the file. It never guesses."

**Time:** ~1 min

---

## SLIDE 8 — Act 2: "Assume you will be interrupted"

**Failure on the slide:** The context compacted mid-pipeline. The model forgot everything it was doing.

**What's on the slide:** The resume timeline: step 3 finishes, a gap, the next run picks up at 4.

**Talking notes:**

> "Fourth, and this is the one that changed how I think about all of it.
>
> The context compacted in the middle of a run. The model forgot everything it was doing.
>
> And nothing happened. The file had been written after step 3, and the file was still on disk. The next run read it and carried on at step 4. Nothing re-ran, nothing was lost.
>
> Interruption is not a bug that a bigger model fixes. It's a fact of any process that outlives a single session. What survives the interruption is not the model. It's the file."

**Time:** ~1 min

---

## SLIDE 9 — Act 3: "What it runs, every working day"

**What's on the slide:** The 11-step pipeline, a legend, and the numbers: 325 meetings, 9 human checkpoints, 8 LLM calls, 1 state file.

**Talking notes:**

> "Here's what that looks like in production. A meeting transcript lands in my inbox.
>
> Eleven steps: discovery, speakers, context, summary, sentiment, personal notes, actions, triage, follow-up, filing, cleanup.
>
> Nine of them stop for me. One runs on its own, that's sentiment, because nothing it does is irreversible. One is optional.
>
> Eight LLM calls, four scripts, one state file. 325 meetings have gone through it.
>
> And the same pattern runs my morning briefing and my inbox. The domain is interchangeable. The shape is not."

**Time:** ~1.25 min

---

## SLIDE 10 — Act 3: "What still broke"

**What's on the slide:** Three failures with their fixes, the third marked as the win.

**Talking notes:**

> "Months of daily use. Three things broke, and each fix moved a decision out of the model and into the file.
>
> The state file got edited to the wrong step. Fix: validate it on load.
>
> The model skipped a gate, because the instruction said present and continue instead of present and stop. Fix: the file decides, not the prompt. It reads `user_confirmed`, and if the flag isn't there, it stops.
>
> Third, the context reset mid-pipeline, the file survived, and it resumed where it left off. That one isn't a failure. That's the thesis paying out.
>
> The rule that survived all of it: trust the file, check the artifact itself, and never trust what the last step claims it did."

**Time:** ~1 min

---

## SLIDE 11 — Act 4: "The model that should have made this obsolete"

**What's on the slide:** 2024, 2026, this month. The Jev pull quote.

**Talking notes:**

> "Now let me argue against myself. The fair challenge is: did I build scaffolding for flaky cheap models, and won't the next model make all of this pointless?
>
> In 2024 the answer to any reliability problem was a bigger model. By 2026 that stopped working. The gains flattened. But the models didn't stop arriving, so more work moved onto the cheaper ones, which is exactly where drift lives. The problem spread, it didn't shrink.
>
> Then this month the industry shipped the punchline. A model called Jev. It isn't an LLM. It returns a decision, not a paragraph. It cannot hallucinate, because it never writes text. Hundreds of times faster and cheaper for the calls it's built to make.
>
> If anything was going to retire this talk, it was this.
>
> Then I read the best write-up of it. The title was 'The State Machine Is the Agent.' The recommendation: put Jev at the branches, and let deterministic code own the plan, the memory, the legal transitions and the waiting.
>
> And days later, researchers showed that prompt injection can bend what Jev decides. Clean, confident, wrong.
>
> So even the model built from scratch to make safe calls sits behind a deterministic loop, with a human on the irreversible branch."

**Time:** ~1.5 min

---

## SLIDE 12 — Act 4: "The same shape fits your work"

**What's on the slide:** Four personal workflows mapped to their enterprise equivalents.

**Talking notes:**

> "None of this is really about meeting notes.
>
> Every row here is the same shape: read, transform, a human approves, write to the system of record. Only the nouns change. Customer call analysis. Email triage. KPI reporting.
>
> And that last row: several models, each running a step, all coordinated by one state file. You get to mix models precisely because the structure doesn't care which one runs a given step."

**Time:** ~45 sec

---

## SLIDE 13 — Act 4: "Run these three over your own agent"

**What's on the slide:** Three questions, and the dial.

**Talking notes:**

> "Three questions. Take them back to whatever you're building.
>
> One: what is your single source of truth? A file, a row, a record. If the answer is 'the conversation', you don't have one.
>
> Two: where does it stop for a human, and can a step skip that stop? If the stop is a sentence in a prompt, it can be skipped.
>
> Three: does each step check that its inputs exist before it runs?
>
> And the dial: as models get better, you don't remove the human. You move it. From checking every step, to guarding the steps that cannot be undone."

**Time:** ~45 sec

---

## SLIDE 14 — Close: "Architecture over intelligence"

**What's on the slide:** Three takeaways, tonight's one-line action, and the closing phrase. Leave it up through Q&A.

**Talking notes:**

> "Three things to take away.
>
> The model changes; the structure stays. A chain breaks wherever nothing supervises it, so put a file in charge.
>
> The human doesn't disappear. Better models move it to the irreversible steps. The agent cannot delete, overwrite or send without a yes.
>
> And the same structure runs any model, so when next month's model lands, your workflow doesn't change. That part comes free.
>
> If you try one thing tonight: add one field, `current_step`. Read it back before every step. Stop if it doesn't match.
>
> Reliability at the boundaries. Thank you."

**Time:** ~45 sec

---

## Running short?

Cut in this order. None of these breaks the arc:

1. Slide 12 (same shape fits your work): fold into one line on slide 13.
2. Slide 3 (chain hides its failure): compress to two sentences over slide 2.
3. Slide 7 (contract): the shortest of the four rebuild beats, and slide 10 restates its lesson.

Never cut slide 2 or slide 11. Slide 2 is the only place the talk earns its authority, and slide 11 is the only place it survives the obvious objection.

---

## Q&A (5 minutes)

**Q: How is this different from Temporal or AWS Step Functions?**
> "Same problem, different scale. They solve durable execution with worker services and orchestration servers. I solve it with a JSON file on disk and one rule: before every step, read the state. For personal workflows and small teams, the file is enough. At enterprise scale, use Temporal."

**Q: The LLM's output is unpredictable even if the state file is predictable. How do you handle that?**
> "The file controls what happens. The model controls how. The file says 'run step 2: generate the summary.' The model writes it. If the summary is bad, the human gate catches it. If the context resets, the next session regenerates it. Predictable structure around unpredictable output."

**Q: The LLM can modify the state file. Isn't that a security hole?**
> "It can, and it's just JSON, so it's readable and git-tracked. Validation on load catches corruption. And the human gates mean it can't take an irreversible action without a yes, whatever the file says."

**Q: Does this need a specific model?**
> "Any model that can read JSON and follow simple logic. In practice I run it on Pi, my own tool, driving a local Qwen 3.6. The reliability comes from the structure, not from the model being clever. That's the point."

**Q: Jev is built for exactly these decisions. Doesn't it replace the state machine?**
> "Jev is a better branch, not the control flow. It doesn't remember which step we're on, enforce which transitions are legal, survive a context reset, or hold a gate at an irreversible action. That's the machine's job. And researchers bent its verdicts with prompt injection the same week it shipped. If anything, Jev makes the case: the machine is the agent, not the model."

**Q: Can you open-source it?**
> "The pattern generalises and I'm happy to share the details. The implementation is tangled up in my setup: Pi, Obsidian, MCP servers, custom scripts. The pattern is the valuable part, not my code."

**Q: Does it scale to multi-agent?**
> "Yes. The state file is shared state. Multiple agents read and write it, and the rule checks stop them treading on each other. I run a single agent, but nothing in the pattern requires that."

**Q: Isn't this just a state machine? What's new?**
> "The state machine isn't new. Applying it to an unpredictable executor is. The insight is that for personal and small-team workflows you don't need Temporal or Airflow. You need a JSON file, a rule that says read it before every step, and the discipline to enforce it."

---

## Demo options, if there's time

- **A. Live:** run the pipeline on a sample transcript, show the state file before and after step 0, kill the session mid-way, resume, show it pick up at the next step.
- **B. Real artifacts:** open a real `.process-state.json` from a recent meeting and walk the artifact chain: speaker-map → context → summary → actions.
- **C. Screenshots:** the state file, the daily note produced, the filed meeting note.
