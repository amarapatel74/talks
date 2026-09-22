# AI Meetup Talk — Full Script with Talking Notes

**Title:** *Architecture Over Intelligence: Designing Reliable AI Workflows*
**Subtitle:** *A state-machine approach to reliable, interruptible, auditable AI workflows*
**Length:** ~18.5 min talk + 5 min Q&A. The 15-minute trim lives in `script-15min.md`.
**Audience:** AI practitioners / developers at a meetup
**Deck:** `architecture-over-intelligence/deck.md`, 14 slides in four acts

> Slide numbers match the page numbers printed on the deck. The Opusfived demo runs live before slide 1 and has no slide of its own.

## The shape of the talk

| Act | Slides | Beat |
|---|---|---|
| Hook | live | Opusfived: an agent that never finishes one small job |
| 1 · What broke | 2-4 | My pipeline drifted and never told me. Why the chain hid it. Why the demo hid it. |
| 2 · The rebuild | 5-8 | Four failures, four fixes: the file, the gate, the contract, interruption |
| 3 · In production | 9-10 | What it runs daily. What still broke. |
| 4 · What's next | 11-14 | The model that should have retired this talk. Your turn. |

---

## Before the talk: what you need to hold

One anchor example carries the whole talk: **meeting processing**. A transcript comes in. The pipeline identifies speakers, extracts summary, actions and personal context, triages priorities, optionally drafts a follow-up, and files the note to your vault. 11 steps, 9 state-enforced human gates, 8 LLM calls, 4 scripts, 2 MCP calls, 325 meetings processed.

The same pattern runs your morning briefing and your inbox. Mention them in one line to prove it generalises, then stay on the meeting pipeline.

All of it rests on one thing: a **state machine stored in a JSON file on disk**. That file tracks the current step, what's completed, what a human approved, which artifacts exist, and whether the run was aborted.

**The insight this talk sells:** this is not personal productivity. It's **reliability at the boundaries**, where a multi-step workflow meets an interruption, a different model, and a consequence. Reliability is the thesis. Portability is one supporting line near the end.

**What changed in this version of the deck:** the talk now opens on the failure rather than the architecture. Your migration story is the inciting incident, each rebuild slide opens on the specific failure it answers, and the Jev beat is staged as the reversal rather than a status update.

---

## SLIDE 0: Opusfived (live demo, no slide)

**What's on screen:** https://opusfived.dev/ by Milos Novovic. A browser parody of asking Claude Opus 5 to make one button blue, which spirals into over-verification, self-justification, 23 mobilised agents, and no finished task. It went viral on Hacker News for exactly that reason.

**Action:** Open the site. Click the button. Show the chain spiralling. Let it play 10 to 15 seconds.

**Talking notes:**

> "I want to start with a game. This is Opusfived, by Milos Novovic. It's a parody of what happens when you give a frontier model a simple instruction.
>
> The task: make one button blue. Change nothing else.
>
> Here's what happens instead."

**(Click. Let it run.)**

> "It decides it needs to verify. Then re-verify. Then justify the verification. Then it calls more agents to check its own work. And by the time it's done all that, there's no room left in the context to do the thing you actually asked.
>
> This is what happens when you let the model control its own loop.
>
> It's funny because it's extreme. The version I'm going to show you isn't extreme, and it isn't funny, because it doesn't look like anything at all. It looks like success."

**Why this works:** funny, visual, universally recognised. It sets up the thesis in 30 seconds and hands off cleanly to your own story.

**Time:** ~1 min

---

## SLIDE 1: Title

**Talking notes:**

> "Architecture over intelligence. I'm Amar. I've been running AI agent workflows in production, daily, for months: meeting transcripts, a morning briefing, my inbox, a knowledge base. Everything I'm about to show you I learned by breaking it."

**Time:** ~15 sec

---

## SLIDE 2 — Act 1: "It didn't crash. That was the problem."

**What's on the slide:** A pipeline drifting. Steps 1 and 2 clean, step 3 marked as drifting, the rest ghosted and still running to the end.

**Talking notes:**

> "Here's what happened to me.
>
> I have a pipeline that takes a meeting transcript and processes it end to end. Names the speakers, writes the summary, pulls out the actions, files it into my notes. It worked. I'd been running it daily for months on a frontier model.
>
> Then I moved it onto a cheaper model that I run locally on my own machine.
>
> It didn't crash.
>
> I want to sit on that, because that's the whole problem. It didn't throw. It didn't stall. It ran all the way to the end and gave me output that looked exactly like the output I'd been getting for months.
>
> It drifted at step 3. By step 5 it had stopped noticing that it had drifted. No exception, no stack trace, no alert. Just plausible, confident, wrong, filed into my vault where I'd find it days later.
>
> A crash is a gift. A crash tells you where you are and what to fix. Silent drift tells you nothing, and it keeps working while it's wrong."

**What this means:** smaller models don't fail loudly. Instructions decay over long contexts, structured output drifts out of shape, and the model commits to plausible-but-wrong plans with full confidence. The failure mode isn't an error. It's unearned confidence.

**Why this slide matters:** this is where the talk earns the right to be given. It's specific, it's yours, and every practitioner in the room has had a version of it. Don't rush it.

**Time:** ~2 min

---

## SLIDE 3 — Act 1: "A chain hides its own failure"

**What's on the slide:** The incident-response chain. Six boxes, five arrows.

**Talking notes:**

> "So why didn't it notice? Because of the shape of the work.
>
> Take a different example, so it's not just my meetings. 'Investigate this incident.' That sounds like one job. It isn't.
>
> Pull the error logs. Work out which systems were affected. Judge how serious it is. Draft a response plan. Page the on-call. Write the postmortem.
>
> Six steps. Some are tool calls, some are model calls. And critically: each one takes the previous step's output as fact. Step 4 trusts step 3. Step 5 trusts step 4. There is nothing in that chain whose job is to check.
>
> So a mistake doesn't stop the chain. It rides it. And because each step is summarising and compressing what came before, the mistake gets smoother and more confident the further it travels."

**Why this slide matters:** it generalises your story into a structural property. The audience stops thinking "Amar picked a bad model" and starts thinking "my pipeline has this shape too."

**Time:** ~1.5 min

---

## SLIDE 4 — Act 1: "My demo never showed me this"

**What's on the slide:** Demo panel against production panel, four rows each.

**Talking notes:**

> "The fair question at this point is: how did you not catch this earlier?
>
> Because my demo was lying to me. Four ways.
>
> A demo is one session, start to finish. Production compacts, resets, and I walk away in the middle.
>
> In a demo, a mistake is annoying and I delete it. In production the output is filed into my vault, so a mistake is data loss.
>
> In a demo I'm watching every step. In production I invoke it, go and do something else, and check hours later.
>
> And in a demo I'm running the best model I can buy. In production I want something cheaper, faster, or self-hosted. That's the one that got me.
>
> Now look at those four. Not one of them is a model problem. They are all boundaries: where the session ends, where the model changes, where nobody is watching, where an action becomes irreversible.
>
> That's the line I want you to leave with. Reliability fails at the boundaries. And you cannot buy your way out of a boundary with a bigger model."

**What this means, plainly:**

- **Session compaction:** models have a context limit. When a conversation gets long the system summarises it, and specific details about where you were in the workflow are exactly what gets dropped. In a demo you finish in one sitting. In production it happens mid-pipeline.
- **Consequences:** a demo writes to a scratch file. Production writes to the system of record.
- **Nobody watching:** you can't validate each step if you're not there.
- **Model size:** cheaper and self-hosted models follow long instructions less faithfully, produce slightly malformed JSON, and commit confidently to wrong plans.

**Why this slide matters:** it reframes the problem away from model selection and onto workflow design, and plants the through-line phrase.

**Time:** ~2 min

---

## SLIDE 5 — Act 2: "Stop letting the model remember"

**Failure on the slide:** It lost its place, and redid work it had already done.

**What's on the slide:** `process-state.json` as a card: `current_step` highlighted, `completed_steps`, `user_confirmed.summary`, `aborted`.

**Talking notes:**

> "So I rebuilt it. Four changes. Each one came from something that broke, and I'll give you the failure first each time.
>
> First failure: it lost its place. The context compacts, or I re-invoke it after walking away, and it starts redoing work it had already finished. Notes filed twice. Actions duplicated.
>
> The fix is blunt: the model doesn't get to remember. One JSON file on disk is the source of truth for the whole run.
>
> `current_step` is the most recently completed step. Here it's 2.6, so the next one is 3.
>
> `completed_steps` is everything that's finished, so a restart skips what's done and nothing runs twice.
>
> `user_confirmed` holds human approvals, one flag per gated step.
>
> `aborted` is the kill switch. Any step that reads it and finds true halts immediately.
>
> And the rule that ties it together: every step reads this file before it runs, and writes it after. A step may only run when `current_step` is the step before it. Anything else, it stops and says so.
>
> The model proposes. The file decides."

**What this means:** the state file is not a log and not a cache. It's the source of truth. The model doesn't choose what happens next, the file does. Read `current_step: 2.6`, run step 3. If the session dies, the next session reads the same file and does the same thing. Deterministic.

**Time:** ~2 min

---

## SLIDE 6 — Act 2: "Stop asking the model for permission"

**Failure on the slide:** Told to present the summary and continue, it continued. Nobody had said yes.

**What's on the slide:** Three gate cards (Auto, Human, Blocked) and the dial.

**Talking notes:**

> "Second failure. My instruction said: present the summary, then continue. So it presented the summary and continued. Nobody had said yes.
>
> That's the moment I stopped treating human review as a sentence in a prompt and made it a property of the step itself.
>
> Every step carries one of three gates.
>
> Auto: run it and keep going. Use it where nothing is irreversible and a human sees the result at a later gate anyway. Reads, health checks, sentiment analysis.
>
> Human: run the step, present the output, and stop. Wait for a yes. This is where the human becomes a step in the workflow instead of a spectator outside it.
>
> Blocked: something required is missing or broken. Don't guess, don't improvise, report it and wait.
>
> Now, the part that matters more than quality control. A gate is not asking 'did the model get this right'. It's asking 'has a human said yes'. That's authority.
>
> Deleting a file, overwriting a record, replying to a customer: those should not happen on a model's confidence alone, no matter how good the model gets.
>
> And that's why the human is a dial, not a switch. As models improve you don't remove the human, you turn the dial down: from reviewing every step, to guarding only the steps that can't be undone. I'll come back to that at the end.
>
> One more time on the mechanism, because it's the whole trick: the gate is enforced by the file, not by the model's judgement. It reads `user_confirmed`, sees the flag isn't set, and knows it must stop. No exceptions."

**Why this slide matters:** this is human-in-the-loop done properly, as authority rather than review. It also plants the dial metaphor you pay off on slide 13.

**Time:** ~2 min

---

## SLIDE 7 — Act 2: "Stop trusting the previous step"

**Failure on the slide:** A script failed quietly. The state said it had worked. The next step ran on a file that was never written.

**What's on the slide:** Step 0 produces `speaker-map.json`, required by Step 1.

**Talking notes:**

> "Third failure, and this one's subtle. A script failed quietly. It returned, the pipeline moved on, and the state file said the artifact had been produced. But it hadn't. The next step ran on a file that was never written.
>
> So now every step declares two things: what it produces, and what it needs.
>
> Step 0 does speaker mapping and produces `speaker-map.json`. Step 1 extracts context, and its precondition is that `speaker-map.json` exists.
>
> Before step 1 runs, it goes and looks on disk. Not 'did the last step claim success'. Is the file there.
>
> If it's missing: gate blocked, missing artifact, name the file, stop.
>
> It's a dependency graph, enforced by checking reality instead of trusting a report. It's simple, and it kills an entire class of bug: using data that doesn't exist yet."

**Why this slide matters:** it's the most concrete, most portable idea in the talk, and it takes 60 seconds to explain.

**Time:** ~1.5 min

---

## SLIDE 8 — Act 2: "Assume you will be interrupted"

**Failure on the slide:** The context compacted mid-pipeline. The model forgot everything it was doing.

**What's on the slide:** The resume timeline: step 3 finishes, the gap, the next run picks up at 4.

**Talking notes:**

> "Fourth, and this is the one that changed how I think about all of it.
>
> The context compacted in the middle of a run. The model forgot everything it was doing. Everything.
>
> And nothing happened.
>
> Because the file had been written after step 3, and the file was still sitting on disk. The next run opened it, saw `current_step: 3`, and carried on at step 4. Nothing re-ran. Nothing was lost. I didn't even notice until I looked at the logs.
>
> Same story if I say stop: `aborted: true`, and everything halts. Same story if I close the laptop and come back tomorrow.
>
> And I want to be precise about why this matters. Interruption is not a bug that a bigger model fixes. It's a fact of any process that outlives a single session. A perfect model still lives inside a process that gets compacted, paused, or abandoned.
>
> What survives the interruption is not the model. It's the file.
>
> This is durable execution, which enterprise engines like Temporal and Step Functions solve with worker services and orchestration servers. Here it's a JSON file and one rule: before every step, read the state."

**Why this slide matters:** it's the emotional payoff of Act 2 and the strongest argument that structure beats intelligence. Land it and slow down.

**Time:** ~1.5 min

---

## SLIDE 9 — Act 3: "What it runs, every working day"

**What's on the slide:** The 11-step pipeline with the gate legend, and the numbers: 325 meetings, 9 human checkpoints, 8 LLM calls, 1 state file.

**Talking notes:**

> "So here's the real thing, running in production.
>
> A meeting transcript lands in my inbox. Eleven steps.
>
> Discovery finds the file and asks which one to process. Speaker mapping turns SPEAKER_0 and SPEAKER_1 into Alice and Bob by cross-referencing the calendar, and asks me to confirm. Context works out which project it belongs to. Summary writes it up with chapters and highlights. Sentiment runs on its own. Personal context picks up the human details, the things worth remembering about the people in the room, and updates their profile. Actions get extracted and deduplicated against what I already have. Triage assigns Now, Next or Someday. Follow-up optionally drafts an email. File writes the note into my vault. Cleanup optionally deletes the working files.
>
> Nine of those eleven stop for me. One runs alone, that's sentiment, because nothing it does is irreversible. One is optional.
>
> Eight LLM calls, four scripts, two MCP calls, one state file. 325 meetings have been through it.
>
> No step is skipped. No artifact is used before it exists. And if I walk away after step 4 and come back tomorrow, it resumes at step 5.
>
> And this isn't really about meetings. The same pattern runs my morning briefing and my inbox. The domain is interchangeable. The shape is not."

**Why this slide matters:** proof at scale. Everything in Act 2 was a claim; this is the receipt.

**Time:** ~2 min

---

## SLIDE 10 — Act 3: "What still broke"

**What's on the slide:** Three failures with their fixes. The third is marked as the win.

**Talking notes:**

> "Months of daily use. Three things broke, and each fix moved a decision out of the model and into the file.
>
> One: the state file got edited to the wrong step, by me or by the model. Everything downstream went out of order. Fix: validate on load. Check the step number is real, check `completed_steps` has no holes, check the confirmation flags are actually booleans.
>
> Two: the model skipped a gate, because the instruction said present and continue instead of present and stop. Fix: the file decides, not the prompt. It reads `user_confirmed.summary`, finds nothing, and stops. The instruction can't be misread if the instruction isn't what's enforcing it.
>
> Three, and this is the win: the context reset mid-pipeline, the file survived, and the next run resumed exactly where it left off. That's not a failure. That's the pattern doing precisely what I built it to do.
>
> So the rule that came out of all of it: trust the file, check the artifact itself, and never trust what the last step claims it did."

**Why this slide matters:** it proves you've actually run this, not just designed it. Real failures with blunt fixes buy more credibility than a clean architecture diagram.

**Time:** ~1.5 min

---

## SLIDE 11 — Act 4: "The model that should have made this obsolete"

**What's on the slide:** 2024, 2026, this month. The Jev pull quote.

**Talking notes:**

> "Now let me argue against myself, because there's a fair challenge coming.
>
> The challenge is: did you just build scaffolding for flaky cheap models? And won't the next model make all of this pointless?
>
> Let me answer the model question straight.
>
> In 2024, the answer to any reliability problem was a bigger model. It usually worked. By 2026 that stopped: the gains flattened, and paying more stopped buying reliability. But the models never stopped arriving. So more work keeps moving onto the cheaper ones, and the cheaper ones are exactly where drift lives. The reliability problem isn't shrinking. It's spreading.
>
> Then this month the industry shipped the punchline. A model called Jev, from TypeSafe AI, founded by one of the people behind ChatGPT.
>
> I'm not going to pretend to be an expert here. In the old sense of the word I'm an amateur: I work on this because I care how it behaves. So here's just the shape of it. Jev is not an LLM. It returns a decision: a choice, a score, a yes or no with a confidence. Not a sentence. It cannot hallucinate, because it never writes text. Reportedly hundreds of times faster and cheaper for the calls it's built to make.
>
> Its natural home is the flood of small classification decisions an agent makes all day, which is exactly where cheap models drift hardest. If it holds up, it puts real pressure on the expensive frontier providers.
>
> And you might think: here comes the model that makes this whole talk pointless.
>
> Then I read the best early write-up of it. The title was, literally, 'The State Machine Is the Agent.' The recommendation: drop Jev in at the decision branches, and let deterministic code own the plan, the memory, the legal transitions, and the waiting. Jev doesn't get to own the loop. The machine does.
>
> And then, days later, security researchers showed that prompt injection can bend what Jev decides. A clean, confident, wrong verdict.
>
> So even the model built from scratch to make safe decisions still sits behind a deterministic loop, with a human on the irreversible branch.
>
> The takeaway isn't 'choose a better model'. The model is the variable. The structure is the constant. And there's a gift buried in that: because the structure doesn't care which model runs a step, the same file moved me off a frontier model onto a local one with no rewrite. That's portability, and it comes free with reliability."

**Why this slide matters:** this is the reversal, and it's the strongest intellectual beat in the talk. It proves your thinking isn't a 2025 snapshot, and it turns the talk from "one person's workflow" into "where the industry is already heading."

**Time:** ~2.5 min

---

## SLIDE 12 — Act 4: "The same shape fits your work"

**What's on the slide:** Four personal workflows mapped to their enterprise equivalents.

**Talking notes:**

> "None of this is about personal productivity. It's about reliable workflow design, and the patterns move.
>
> Meeting processing becomes customer call analysis: transcribe, extract actions, identify risks, draft a follow-up, file to the CRM.
>
> Inbox routing becomes email triage and classification. Task tracking becomes KPI reporting.
>
> And the last row: model-agnostic guardrails become many models with one coordinator. Use a frontier model for the hard reasoning, a cheap one for summarising, something tiny for classification. The state file and the gates work the same regardless of which model runs which step.
>
> Every row here is the same shape: read, transform, a human approves, write to the system of record. Only the nouns change."

**Time:** ~1 min

---

## SLIDE 13 — Act 4: "Run these three over your own agent"

**What's on the slide:** Three diagnostic questions, and the dial.

**Talking notes:**

> "So here's what I'd like you to do with this. Three questions, and you can run them over whatever you're building on the train home.
>
> One: what is your single source of truth? A file, a row, a record. If the honest answer is 'the conversation', then you don't have one, and everything downstream depends on a model's memory.
>
> Two: where does it stop for a human, and can a step skip that stop? If the stop is a sentence in a prompt, it can be skipped, and eventually it will be.
>
> Three: does each step check that its inputs exist before it runs, or does it trust what the last step said?
>
> And then the dial, which I promised I'd come back to. As models get better, you don't remove the human. You move them. From checking every step, to guarding only the steps that cannot be undone. Better models move the dial up the stakes curve. They never take it to zero, because the question at an irreversible action was never 'is the model good enough'. It was 'did a human say yes'."

**Time:** ~1 min

---

## SLIDE 14 — Close: "Architecture over intelligence"

**What's on the slide:** Three takeaways, tonight's one-line action, the closing phrase. Leave it up through Q&A: this is the slide people photograph.

**Talking notes:**

> "Three things to take away.
>
> The model changes; the structure stays. A chain of steps breaks wherever nothing supervises it, so put a file in charge of the chain.
>
> The human doesn't disappear. Better models move them to the irreversible steps. The consequence is concrete: the agent cannot delete, overwrite or send without a yes.
>
> And the same structure runs any model. When next month's model lands, and it will, your workflow doesn't change. That part comes free.
>
> If you try one thing tonight, try this: add one field to your agent's state, `current_step`. Read it back before every LLM call. If the number doesn't match what you expect, stop and say so.
>
> The best AI systems aren't the ones that do the most on their own. They're the ones that know when to ask, and can pick up exactly where they left off.
>
> Reliability at the boundaries. Thank you."

**Time:** ~1 min

---

## Timing

| Section | Slides | Time |
|---|---|---|
| Hook | live + 1 | 1.25 min |
| Act 1 | 2-4 | 5.5 min |
| Act 2 | 5-8 | 7 min |
| Act 3 | 9-10 | 3.5 min |
| Act 4 | 11-14 | 5.5 min |
| **Total** | | **~22.75 min** |

That's the full-fat version, for a longer slot. For the 15-minute slot use `script-15min.md`, which trims Act 1 to 3.75 min, Act 2 to 5 min, and Act 4 to 3.75 min.

**Cut order if you're running long.** None of these breaks the arc:

1. Slide 12: fold into one line on slide 13.
2. Slide 3: compress to two sentences delivered over slide 2.
3. Slide 7: the shortest rebuild beat, and slide 10 restates its lesson.

Never cut slide 2 or slide 11. Slide 2 is where the talk earns its authority. Slide 11 is where it survives the obvious objection.

---

## Q&A (5 minutes)

**Q: How is this different from Temporal or AWS Step Functions?**
> "They solve the same problem, durable interruptible workflows, with real infrastructure: worker services, workflow code, orchestration servers. Mine is a JSON file on disk plus one rule the file enforces. Same pattern, different scale. For personal workflows and small teams a file is enough. At enterprise scale, use Temporal."

**Q: The model's output is unpredictable even if the state file is predictable. How do you handle that gap?**
> "The file controls what happens. The model controls how. The file says 'run step 2: generate the summary'. The model writes the summary. If it's bad, the human gate catches it. If the context resets, the next session regenerates it. Predictable structure wrapped around unpredictable output, and the gates plus artifact checks cover the seam."

**Q: The model can modify the state file. Isn't that a security hole?**
> "It can. But it's plain JSON: human-readable and git-tracked, so I can always see what it wrote. Validation on load stops it corrupting the state in dangerous ways. And the human gates mean it can't take an irreversible action without a yes, whatever the file says."

**Q: Does this need a specific model?**
> "Any model that can read JSON and follow simple logic. In practice I run it on Pi, my own tool, driving a local Qwen 3.6, not Claude Code. The reliability comes from the file and its rules, not from the model being clever. The pattern was designed across model sizes precisely because it doesn't depend on how smart the model is."

**Q: Jev is built for exactly these decisions. Doesn't it replace the state machine?**
> "Jev is a better branch, not the control flow. It can judge a decision well, but it doesn't remember which step we're on, enforce which transitions are legal, survive a context reset, or hold a gate at an irreversible action. Those are the machine's job. And the same week it shipped, researchers showed prompt injection can sway its verdicts. If anything Jev makes my case: the machine is the agent, not the model."

**Q: Can you open-source this?**
> "The pattern generalises and I'm happy to share the details. The core idea, a JSON state file plus gates plus artifact checks, could be packaged. My implementation is tangled up in my own setup: Pi, Obsidian, MCP servers, custom scripts. The pattern is the valuable part, not my code."

**Q: Does it scale to multi-agent systems?**
> "Yes. The state file is just shared state. Multiple agents can read and write it, and the rule checks stop them stepping on each other. In practice I run a single agent, but nothing in the pattern requires that."

**Q: Isn't this just a fancy state machine? What's new?**
> "State machines aren't new. Putting one in charge of an unpredictable executor is the part worth talking about, and the insight that for personal and small-team workflows you don't need Temporal or Airflow. You need a JSON file, a rule that says read it before every step, and the discipline to enforce it."

---

## Demo options, if a live demo is possible

**Option A: the state file in action**
- Run the pipeline on a sample transcript
- Show the state file before step 0, after step 0, after step 2
- Kill the session mid-way, show the state persists
- Resume, show it picking up at the next step

**Option B: real artifacts from production**
- Open `.process-state.json` from a recent meeting
- Walk through what each field means
- Show the artifact chain: speaker-map → context → summary → actions

**Option C: screenshots**
- Real state files, the daily note produced, the filed meeting note

---

## References

### Jev / TypeSafe AI (September 2026)

- **"Introducing System One Models & Jev"** (TypeSafe AI) — the launch; not an LLM, calibrated decisions, 200x faster / 400x cheaper. https://typesafe.ai/blog/introducing-system-one-models-and-jev
- **"Jev at the Branches: The State Machine Is the Agent"** (StackToHeap, Sep 21) — deterministic code owns the machine, Jev supplies judgement at the branches. https://stacktoheap.com/blog/2026/09/21/the-state-machine-is-the-agent/
- **"What Is Jev? A Guide to TypeSafe AI's System One Model"** (LangChain) — how Jev fits an agent loop. https://www.langchain.com/blog/building-a-harness-with-jev
- **"A new kind of AI model from a ChatGPT inventor…"** (TechCrunch) — no text output, cannot hallucinate. https://techcrunch.com/2026/09/18/a-new-kind-of-ai-model-from-a-chatgpt-inventor-is-thrilling-developers/
- **"Companies are putting Jev in charge of AI agent decisions, and prompt injection can influence the verdict"** (VentureBeat) — the counter-evidence. https://venturebeat.com/security/companies-are-putting-jev-in-charge-of-ai-agent-decisions-and-prompt-injection-can-influence-the-verdict

### Enterprise workflow engines

- **Temporal** — durable execution, distributed orchestration. https://temporal.io/
- **AWS Step Functions** — serverless workflow orchestration. https://aws.amazon.com/step-functions/
- **CrewAI** — multi-agent orchestration patterns. https://docs.crewai.com/
- **LangGraph** — stateful multi-agent orchestration. https://langchain-ai.github.io/langgraph/

### Academic / research

- **AgentHallu: Benchmarking Automated Hallucination Attribution** (arXiv, Jan 2026) — open models do worse at step localisation. https://www.alphaxiv.org/overview/2601.06818
- **When Small Models Are Right for Wrong Reasons** (arXiv, Jan 2026) — 50-69% of correct answers from 7-9B models contain flawed reasoning. https://arxiv.org/html/2601.00513
- **Operational Hallucination and Safety Drift** (arXiv, Jul 2026) — alignment degrades over extended interactions. https://www.alphaxiv.org/abs/2607.18366
- **Reason Less, Verify More: Deterministic Gates Recover a Silent Policy-Violation Failure Mode in Tool-Using LLM Agents** (arXiv, Jul 2026). https://arxiv.org/html/2607.07405v1
- **LLM-based Agents Suffer from Hallucinations: A Survey** (arXiv, Sep 2025). https://arxiv.org/html/2509.18970v1

### Open-source frameworks solving the same problem

- **Overseer** — reliable multi-agent workflows with validation and recovery. https://github.com/nikitavivat/Overseer
- **Forge** (ACM CAIS 2026) — reliability layer that lifts an 8B model from 53% to 99% on agentic workflows. https://dev.to/monuminu/llm-agent-guardrails-the-engineering-playbook-for-taking-an-8b-local-model-from-53-to-99-on-18c
- **Nexum** — durable execution engine for LLM agents. https://github.com/kuro6061/nexum
- **GraphBit** (alphaXiv, May 2026) — graph-based agentic framework, deterministic DAG execution. https://www.alphaxiv.org/abs/2605.13848
- **TENET** — multi-judge hallucination verifier, pre-tool-call governance. https://github.com/zakky8/TENET
- **Heddle** — actor-based framework, deterministic routing, self-checkpointing. https://github.com/getheddle/heddle
- **SpecOps AI** — framework-agnostic toolkit for self-healing agents. https://github.com/kripikroli/specops-ai

### Blogs / articles

- **"Why Cheap Models Fail Silently in Long Agent Loops"** — instructions decay, JSON drifts, confident wrong plans. https://dreaming.press/posts/why-cheap-models-fail-silently-in-long-agent-loops.html
- **"AI Agents Coded for 16 Days Straight. The Model Didn't Do That, the Harness Did."** https://dreaming.press/posts/agents-that-run-for-days-durable-harness-not-model.html
- **"Before You Switch Your Agent's Model, Run This 20-Minute Test"** — compare completed-task cost, not sticker price. https://dreaming.press/posts/before-you-switch-agent-models-completed-task-cost-test.html
- **"LLM Agent Guardrails: The Engineering Playbook"** (DEV) — the 53% to 99% result. https://dev.to/monuminu/llm-agent-guardrails-the-engineering-playbook-for-taking-an-8b-local-model-from-53-to-99-on-18c
- **"Designing Reliable LLM Agents With Deterministic Control Flow"** (Hackernoon). https://hackernoon.com/designing-reliable-llm-agents-with-deterministic-control-flow
- **"Why Tool Calling Failed in llama.cpp (Qwen 2.5)"** (iunera). https://www.iunera.com/kraken/enterprise-ai/why-tool-calling-failed-in-llama-cpp-qwen-2-5/

### Conference talks on similar ground

- **Adam Terlson (Best Buy), AI Engineer** — finite state machines for multi-agent systems. https://ai.engineer/talks/building-multi-agent-systems-with-finite-state-machines
- **QCon AI Boston, "Beyond Sandboxes"** — durable agent runtimes. https://boston.qcon.ai/presentation/boston2026/beyond-sandboxes-architecting-durable-runtimes-ai-agents
- **AWS re:Invent, "Human-in-the-Loop"** — HITL patterns for multi-agent systems. https://www.youtube.com/watch?v=SC3pHo-CycI
- **Maksym Prokopov, "My AI Runs My Life"** — personal AI at scale. https://prokopov.me/talks/personal-ai-use-cases
- **Radek Sienkiewicz, "I Gave an AI Agent the Keys to My Life"** — Obsidian + agent integration. https://ai.engineer/talks/i-gave-an-ai-agent-the-keys-to-my-life-here-s-what-happened
