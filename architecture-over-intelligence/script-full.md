# AI Meetup Talk · Full Script with Talking Notes

**Title:** *Architecture Over Intelligence*
**Subtitle:** *How I made AI workflows reliable, even on a small model*
**Length:** ~20 min talk + 5 min Q&A. The 15-minute trim lives in `script-15min.md`.
**Event:** The AI Fellowship Madrid · 9 October 2026
**Deck:** `architecture-over-intelligence/deck.md`, 16 slides

> Sections use the page number printed bottom right on each slide. The label above each heading (for example "A game") is the kicker.

## The shape of the talk

| Part | Pages | Beat |
|---|---|---|
| Hook | 1-2 | A volunteer plays Opusfived. Every check is paid for. |
| What broke | 3-5 | I moved to a small model to save money. It drifted. Shouting at it didn't work. |
| The turn | 6 | The model thinks. Scripts do. Files remember. |
| Five rules | 7-11 | One slide per rule, each with the evidence from my own logs |
| Proof | 12 | Same meeting, May against today |
| What it unlocks | 13 | Cheaper, safer, portable. Smaller models on bigger jobs. |
| What's next | 14 | A new kind of model, a decider, slots into one box. |
| Take home | 15-16 | Five rules. Start with a checklist; add tested scripts. Make the model a part you can swap. |

---

## Before the talk: what you need to hold

**The system.** Three workflows run through AI: a morning briefing, the inbox, and meetings. Meetings carry the talk. A recording goes in; out come who said what, a summary, the actions, and a note filed in the right place. Eleven steps: discover, speakers, context, summary, sentiment, personal notes, actions, triage, follow-up, file, clean up.

**The story.** Until May 2026 it ran on Claude. The plan's usage limit stopped a meeting halfway, so the flow moved to Qwen 3.6, an open model on a 16 GB gaming graphics card. Qwen drifted. More prompt made it worse before it got better. What fixed it was changing the shape: the model does judgement, scripts do the sure things, files hold the memory.

**The thesis.** Smarter is not the same as correct. Reliability comes from the shape of the workflow, not the size of the model, once the model clears a floor: it can follow the steps, has room for one run, and judges well enough. And because the shape doesn't care which model runs it, the model becomes a part you can swap.

**The evidence.** Every number on the slides comes from your own git history or Pi session logs, run against one public test meeting: "OKR Planning with GitLab Executive Team", GitLab Unfiltered, on YouTube. Keep the claims exactly as the slides state them. The accuracy notes under pages 10 and 11 matter.

---

## Page 1 · Title

**Cue:** Walk on. Ask for a volunteer before you say anything else.

> "Before I say a word about architecture, I need one volunteer. You're going to play a game on my laptop. It's harmless, I promise. It's also the whole talk in thirty seconds."

**Time:** ~20 sec

---

## Page 2 · "A game": Make one button blue. *Nothing else.*

**On screen:** Opusfived live, https://opusfived.dev/, a parody by Milos Novovic. The slide shows the loop behind it.

**Cue:** The volunteer picks the options. Max 30 seconds of spiral. If the site doesn't load in 10 seconds, use the fallback and move on.

> "This is Opusfived. It went round Hacker News a while back. It's a parody of what happens when you give a frontier AI a simple instruction.
>
> One job: make this button blue. Nothing else. Over to you."

**(Volunteer plays.)**

> "It turns Add to Cart blue. And Cancel, because it changed a colour the two buttons share. Then it says: 'Done. The change is small: one value, one file.' Push back, and it sends 23 agents to trace where that colour goes. And the buttons change colour again.
>
> Thank you. Give them a hand.
>
> It's funny because it's true. And for me it's also where my tokens went. Every one of those agents is paid for. Keep that picture in your head, because it comes back."

**Fallback:** "It's a game where you ask an AI to make one button blue. It turns every button blue, says it's done, then sends 23 agents to clean up."

**Why this slide matters:** it's funny, it's physical, and it plants three ideas you'll use later: a model does more than you asked, it marks its own work as done, and the clean-up costs money.

**Time:** ~1 min 30

---

## Page 3 · "Why I changed models": I hit my limit, halfway through a meeting

**On screen:** Before, until May 2026: Claude in the cloud. After, from May 2026: Qwen 3.6 on my desk.

> "Some context on me. I run a lot of my work through AI. A morning briefing. My inbox. And my meetings: a recording goes in, and out come who said what, a summary, the actions, and a note filed in the right place in my notes.
>
> Until May, all of that ran on Claude. Very capable. I have no complaints about the quality.
>
> But my plan has a usage limit, and in May I hit it halfway through processing a meeting. It just stopped. And I'd been quietly uneasy about something else: every meeting I processed was leaving my house.
>
> So I moved to Qwen 3.6. It's an open model, and it runs on a 16 GB gaming graphics card on my desk. No limit. No bill. Nothing leaves the house.
>
> The limit was the trigger. Cost, privacy, and not depending on one vendor are the reasons I stayed."

**Cue:** Say the hinge line slowly.

> "Economics started the journey. Drift is what I found along the way."

**Why this slide matters:** it makes the motivation ordinary and relatable. Nobody in the room has to care about architecture yet. They only have to recognise hitting a limit, or a bill.

**Time:** ~1 min 15

---

## Page 4 · "Drift": Smarter is not the same as *correct*

**On screen:** The definition of drift. Two red cards: Run 1 on my computer, Run 2 in the cloud.

> "On Claude, this worked. On Qwen, it drifted.
>
> Drift is when the AI slowly goes off track, and doesn't notice. It doesn't crash. It carries on, confidently, somewhere slightly wrong, and then somewhere more wrong.
>
> I wanted to show you real output, so I tested on a public meeting: a GitLab executive team planning their quarterly goals, on YouTube. Same meeting, same answers to its questions, my May workflow.
>
> Run 1: Qwen on my own computer. It wrote the note before it knew who was speaking. It named all four speakers wrong. The CEO became 'head of alliances'. And then it deleted the transcript, without asking me. After 35 minutes I stopped it by hand.
>
> So I thought: my computer is slow, let's give it a better chance. Run 2: the same model, running in the cloud. It seemed smarter. The precision was better. And it made the same kinds of mistakes, just better hidden. Two notes for one meeting. The wrong date. Two of five quotes given to the wrong person. And a Chinese word dropped into the middle of an English sentence.
>
> The one that got me: it knew the right names. It had them. And it still put the wrong name on the quote.
>
> Smarter is not the same as correct."

**Why this slide matters:** this is the evidence that the problem is real. Every item on it is specific and checkable. Take your time.

**Time:** ~2 min

---

## Page 5 · "My first fix: more words": More instructions. Then *SHOUTING*.

**On screen:** Three bar charts across 11 May, 8 Sep and today: lines in the prompt, warnings in capitals, scripts doing the sure things. A real prompt line from 8 September.

> "My first fix was the obvious one. More words.
>
> Every time it skipped a step, I added a line telling it not to. In May the prompt was 73 lines long. By 8 September it was 416. And 13 of those lines were warnings in capitals. MUST. NEVER. STOP.
>
> Here's a real line from that day: 'If preconditions fail, STOP. Report gate failure.'
>
> It helped. A little. But an instruction is only a suggestion. However loudly you write it, the model can still decide that something else matters more, in the moment.
>
> What worked was moving the work out of the prompt and into code. Look at the bottom row: one script in May, four by early September, eight today. And look at what the prompt did: back under 300 lines, and not a single capital letter warning.
>
> The work didn't disappear. It moved out of the prompt, into places where it happens the same way every time.
>
> So here's a test for your own workflows. If you find yourself writing MUST in capitals, your process is missing a check."

**Source:** git history of `.pi/skills/process-meeting/SKILL.md`.

**Time:** ~1 min 30

---

## Page 6 · "The turn": My whole flow, *split three ways*

**On screen:** The whole meeting flow today, one row per step: Transcribe, Name speakers, Add context, Summarise, Read the tone, Find actions, Set priorities, Draft follow-up, File the note, Clean up. Columns: Model thinks, Scripts do, You decide, Files remember. Reviews are dashed; the one-way door is solid. An arrow down the left shows the order, Transcribe to Clean up. A line underneath: between every row, a script checks it's this step's turn and the last step left its file.

> "So here's the turn. The fix wasn't a smarter model. It was a different shape.
>
> This is my whole meeting flow as it runs today, one row per step, top to bottom. Read it by column.
>
> The model thinks. It does judgement, and only judgement: who is speaking, what was said, who agreed to do what, the tone, how urgent each task is, and a draft follow-up. Things a script can't do.
>
> Scripts do the sure things. The transcript. The calendar details. Spotting a task I already have. Building the note. Deleting the working files. Things with one right answer.
>
> I decide at four points: I confirm the speakers, check what it says was said, glance at the actions and priorities, and say yes or no to a follow-up. And one step, deleting, is a one-way door. More on that later.
>
> And files remember. Every row leaves one. Which step we're on, what's already done, what each step produced. Between every row, a script checks the last file is there before the next step can start.
>
> Then it clicked. Qwen is good at two things: calling tools, and writing clean, structured data. It's bad at holding a long process in its head. So I stopped asking it to. I gave it only the judgement, and took everything else away.
>
> I didn't work this out alone, by the way. I worked it out with help from Claude and Qwen themselves.
>
> The model thinks. Scripts do. Files remember. The next five slides are the rules behind this map."

**Accuracy note:** 6 model jobs, 8 scripts, 4 quick reviews (speakers, what was said, actions with priorities, follow-up) and 1 one-way door. The reviews are a glance in the chat; the door is a hard stop the model can't see or get round. That's why slide 11 can say "only on the doors" without contradicting this map.

**Extra, full version only:** every step is safe to run twice. If a step runs again after a restart, it produces the same result instead of a duplicate. Engineers call that idempotent. You don't need the word; you need the property.

**Time:** ~1 min 30

---

## Page 7 · "Rule 1": Give each step to *the right worker*

**On screen:** Two columns, each with one sorting question. Three facts: 5,000 words, wrong date, 1 → 8 scripts.

> "Rule 1: give each step to the right worker.
>
> The test is one question. Is there only one right answer? If yes, it's a script's job. If it needs judgement, it's the model's.
>
> Who is speaking, the summary, the actions, the priorities, the tone of the meeting: that's judgement. That's the model. The date and time, the file name and folder, copying the transcript, linking to the project, deleting old files: there's one right answer. That's a script.
>
> I got this wrong at first. In May, the model retyped the whole transcript into the note. 5,000 words. It ran out of space halfway, and I paid for every one of those words. That's Opusfived again: paid-for work that nobody needed.
>
> And it got the date wrong. The date was already sitting in the file name. The model still got it wrong.
>
> Today, eight scripts do the sure things, and they get them right every time.
>
> And if you don't write code: a script doesn't have to be code. It can be a template, a spreadsheet formula, a form. Anything that gives the same answer every time."

**The eleven steps, if asked:** discover, speakers, context, summary, sentiment, personal notes, actions, triage, follow-up, file, clean up.

**Time:** ~1 min 30

---

## Page 8 · "Rule 2": Every step leaves *a file*

**On screen:** A chain of work boxes and dashed file boxes: speaker-map.json, summary.json, actions.json, then a script files the note. Two cards: a chat forgets, a file remembers.

> "Rule 2: every step leaves a file. On the slide, solid boxes are work and dashed boxes are files.
>
> Map the speakers: write the result down. Summarise: write it down. Find the actions: write them down. Then a script builds the final note from those files. The model never writes the final note by hand.
>
> Why does that matter? Because a chat forgets. It gets long, the early details get lost, and you can't check it.
>
> A file remembers. The next step starts from the file, not from the chat. And a script can check a file: is it there? Does it have the right shape?
>
> Let me show you why this matters, with a test I ran. I used a small local model, 3-bit, with a 64K context window. The context filled up, and the harness auto-compacted. It did this four times. After each compaction, the model forgot work it had already finished, and it tried to do that work again by hand. It never finished.
>
> So I tried the opposite. After every step, I cleared the context on purpose. Nothing was lost, because every result was already in a file. The compaction summary was not written by a model: a script wrote it, from the progress file. It says which meeting, which steps are done, and which step comes next.
>
> Same model, same 64K, same meeting. Zero auto-compactions. This time it finished every step.
>
> One honest point: it was not smarter. It still missed my tasks. Clearing the context solves the context problem. It does not solve the judgement problem. We will come back to that."

**Source:** the final note is built by `file-meeting.py` from these files. The test: 28 Sep, Qwen3.6 3-bit, 64K context, the Iñaki meeting. Squeezed run: 4 automatic compactions, did not finish. Cleared run: `.pi/extensions-global/step-boundary.ts`, 0 automatic compactions, 10 clean clears, peak 41K, finished in 20 min, 0 of 2 tasks. Evidence: `~/meeting-eval/scores/2026-09-28_112908-qwen-iq3-clear.md`. One run: say "in my test".

**Time:** ~1 min 50

---

## Page 9 · "Rule 3": Keep progress *outside* the model

**On screen:** The progress file, `.process-state.json`, and a timeline: working, I quit, I reopen, carries on.

> "Rule 3: keep progress outside the model.
>
> This is one small file. It says which step we're on, what's already done, whether I've confirmed the speakers, and whether I've stopped the run.
>
> In rehearsal, I closed everything in the middle of step 3. The app, the model, the lot. When I opened it again, it read this file and carried on from where it was. Nothing repeated. Nothing lost.
>
> The model had forgotten everything. The file hadn't.
>
> Two bonuses I didn't plan for. The same file let me run the demos in this talk on my local Qwen and on a cloud copy, without changing a line. And it's an audit trail: I can open it and see exactly what ran, and who said yes."

**Time:** ~1 min 15

---

## Page 10 · "Rule 4": Check *before* and *after* every step

**On screen:** Before, do the step, after, with STOP under each check. Two cards: what it caught.

> "Rule 4: check before and after every step.
>
> Two questions. Before: is it this step's turn? After: did it leave its file, in the right shape? If either answer is no, stop.
>
> And don't patch it by hand. That's the temptation, and a hand patch is exactly the drift you're trying to catch.
>
> It caught two things. First, the speakers. The model still guessed wrong: two of four. The model didn't get smarter. But now it had to stop and show me before it wrote anything. I fixed it in one line, and nothing wrong ever reached the note.
>
> Second, my own bug. A time-zone bug made the script look for a note at 12 o'clock. The meeting was at 10. So it refused to file, and it stopped, instead of quietly filing at the wrong time.
>
> And last week I added one more check. The model can't mark a review as done until I've replied. On its first real run, the model tried to skip my review of the actions. The check stopped it, and it had to show me the table.
>
> The check is a small script, not a sentence in the prompt. That's the difference. The model can talk its way round a sentence. It cannot talk a script round."

**Accuracy note:** only the time-zone bug failed loudly. The speaker mistake was caught because the model had to stop and show it. Don't claim both failed loudly. The reply check: 28 Sep, Qwen3.6 fp8 on the Iñaki meeting, `GATE BLOCKED: step 4 … Amar has not replied since step 3 completed`. Pi logs each reply in the progress file (`human_answers`); `meeting-state.py` refuses `--confirmed` at a review without one. Before this check, the same model skipped that review unnoticed.

**Source:** `scripts/meeting-state.py check` and `complete`. It prints `GATE BLOCKED`.

**Time:** ~1 min 30

---

## Page 11 · "Rule 5": Mark the *one-way doors*

**On screen:** The approval dialog for step 7. The message the model gets after a Block. A card listing what the shell refuses from then on, and what it still allows.

> "Rule 5: mark the one-way doors.
>
> A one-way door is a step you can't undo. Sending something. Deleting something. Publishing something where people will see it. Resetting a meeting's progress.
>
> Step 7 of my flow deletes the transcript, the audio and the progress file for that meeting. So it asks me first, in a dialog the model cannot see or click. Only I can answer. Every Approve and every Block is logged.
>
> Notice I only do this on the doors. If I put nine hard gates on every meeting, I'd learn to click yes without reading, and then the gates are worthless.
>
> So what happens when I click Block? Two things.
>
> First, the model is told: Amar blocked step 7. Stop and ask what to change. This session can now only read files and run the pipeline scripts.
>
> Second, the shell makes sure. From that moment the model can still read files, run my pipeline and ask me. But it can't delete, move or copy files. It can't edit a file in place. It can't run code it wrote itself, or any script that isn't part of the pipeline. And it can't try the same door again.
>
> Why both? Because in my first tests, the model heard 'stop' and went looking for another way. First a dry run. Then an offer to rewrite the script. Once, the same script again with a different flag. Nothing was deleted, but it was looking.
>
> In one early test it went further. The dialog couldn't appear, and the model offered, politely, to write my approval into the progress file itself. It was trying to be helpful. That's why the model can't touch the progress file at all: only the pipeline scripts write it.
>
> So I added the second layer, and ran the test three more times. It stopped and asked, every time.
>
> The instruction says stop. The shell makes sure."

**Accuracy notes:** the first tests were on 24 Sep (Pi session logs; the `bash -x` run got through the old gate but found nothing to delete). The retests after the fix: 3 of 3 stopped and asked, logged in `state/approvals.jsonl`. The model didn't try another way in the retests, so the shell's refusals are proven by tests (19 workarounds, 8 ways of running the door script), not live. If asked: the model runs as me, so this is not a security sandbox.

**Time:** ~2 min

---

## Page 12 · "The result": My flow in May vs *my flow today*

**On screen:** Six rows, May against today. Same meeting, same model, same answers.

> "So, did it work? Same meeting. Same model. Same answers to its questions. My flow in May, against my flow today.
>
> Checked the speakers before writing: no, now yes. Quotes given to the right person: three of five, now five of five. One note per meeting: two notes, now one. The right date: no, now yes. Asked before deleting: no, now yes. And carried on after I quit: I never tested that in May, and it works today.
>
> To be fair, and I want to be fair: today's flow also has more steps and better scripts. So treat this as my flow in May against my flow today, not a lab test.
>
> And more steps means more places to go wrong. It also means more places to notice.
>
> The old flow went wrong quietly. The new one goes wrong loudly, early, and where I can fix it."

**Sources:** May column is cloud run A, `qwen3.6-35b-a3b` on OpenRouter. Today is run B, the same model.

**Time:** ~1 min 30

---

## Page 13 · "What this unlocks": Smaller models, *bigger jobs*

**On screen:** Three cards: cheaper, safer, portable. Below them, five other workflows the same shape fits.

> "Let me step back from my meeting notes for a moment. What does this unlock for a business, or for anyone running a complex flow?
>
> First, it's cheaper. The model only does the judgement. The dates, the filing, the checks: scripts do those, for free, the same way every time. So a smaller, cheaper model is enough, and you stop paying tokens for work that has one right answer.
>
> A smaller model. Not the smallest: there's a floor, and I hit it. Last week I ran the same flow on the same meeting with five models. One couldn't even read the steps: it went looking for a program to run. Give a model too little memory and it loses the thread halfway. And the same Qwen I use, squeezed to 3-bit to fit my graphics card, invented a person who wasn't on the call. He was at a conference in Vegas. At 8-bit, it got everyone right.
>
> Architecture lowers the bar. It doesn't remove it.
>
> Second, it's safer. Every step leaves a file. Every approve and every block is logged. If a customer, an auditor or your boss asks what happened, you can show them: what ran, in what order, and who said yes at each door.
>
> Third, it's portable. The flow doesn't care which model runs it. You can swap models without rewriting anything, which means less lock-in, and it means private data can stay on your own machine, if a model that fits your machine clears the floor.
>
> And none of this is about meetings. Sales call follow-ups. Invoice processing. Customer onboarding. Hiring. Month-end close. Same steps, same files, same doors.
>
> Reliable flows let you use smaller models on bigger jobs."

**Accuracy note:** no numbers on the cheaper/safer/portable cards. They are consequences of what the talk has shown, not measurements. If asked "how much cheaper?": "I haven't measured it for a business. A meeting through a small cloud model costs me pennies."

**Floor evidence (28 Sep, Iñaki meeting, same pipeline, one run each):** gpt-oss-20b read `SKILL REFERENCE:` as a program and never finished step 0. Qwen3.6 3-bit at 64K context compacted four times and lost the thread. Qwen3.6 3-bit (IQ3_XXS, local) invented Pablo as a speaker; the same weights at fp8 (OpenRouter, pinned) got all speakers right. Neither Qwen found Amar's two tasks; GLM-5.3-flash found one. Full write-up: [[2026-09-28 Local Models on the Meeting Pipeline]]. Say "one meeting, one run each" if pressed.

**Time:** ~1 min 45

---

## Page 14 · "What's next": A new kind of model arrived *this month*

**On screen:** Three cards. The model box split into a writer and a decider. The steps that only choose. A confidence dial from "ask me" to "carry on".

> "I said at the start that models change every month. This month, a new kind arrived.
>
> TypeSafe AI released Jev. It's a model built only to make decisions. It doesn't write text at all. You give it a question and a list of answers, and it gives you back a choice, and how sure it is. A write-up that week was called 'The State Machine Is the Agent'.
>
> So look at my flow again. The model box does two different jobs. Some steps write: the summary, the actions. But many steps only choose. What's the tone of the meeting: one of four. What's the priority: now, next or someday. Which project: one from my list. Send a follow-up: yes or no. Which name goes with which voice: one from the attendees.
>
> A decider fits those steps. The flow offers only the legal options. The decider picks one. A script checks the answer is on the list. The right-shape check can't fail, because a choice always has the right shape.
>
> And the confidence becomes a gate. When it's sure, the flow carries on. When it's unsure, it asks me. Today I'm asked at four review steps in every meeting, and the more you're asked, the sooner you start clicking yes without reading. This way I'm asked less, and only when it matters. And the dial belongs to a separate decider and a script, never to the model doing the work.
>
> I haven't wired it in yet. But look where it goes: into one box. The files, the scripts, the checks and the doors don't change. That's the whole talk. The model is a part you can swap."

**Accuracy notes:** Jev early access opened 15 Sep 2026. It returns typed values with probabilities. TypeSafe claims it can't hallucinate because it doesn't write text; say "TypeSafe says". The StackToHeap write-up ran on 21 Sep. Not wired in yet: say so plainly.

**Time:** ~1 min 30

---

## Page 15 · "Take these home": Five rules. *Start with a checklist.*

**On screen:** The five rules, each with a one-line gloss. Leave this slide up through Q&A.

> "So, five rules. And you can start without writing any code.
>
> One: write the steps down, and give each one to the right worker. Judgement goes to the model. One right answer goes to a script.
>
> Two: make every step leave a file. The next step starts from the file, not the chat.
>
> Three: keep progress outside the model. A note that says which step you're on.
>
> Four: check before and after every step. If the answer is no, stop.
>
> Five: mark the one-way doors. A person says yes before anything is sent, deleted or filed.
>
> Try one tonight. Take your messiest AI workflow, and write down the steps, and what each one should produce. That's it. A checklist and a shared folder are enough to start.
>
> Be honest with yourself about one thing, though. Without code, you are the check. You read each output, and you are the lock on the doors. That works, but it's tiring. So when a check has only one right answer, hand it to a script, and test that script. That's what I did, one step at a time.
>
> And this isn't only for meeting notes. It works for a hiring process, a month-end report, customer onboarding. Anywhere there are steps and a door you can't walk back through."

**Time:** ~1 min 15

---

## Page 16 · Close: Models change every month.

> "I started this to save money. I ended up with a system I trust more than the frontier model I left, and I can move it between models in one line.
>
> Models change every month. Build your flow so the model is a part you can swap.
>
> Thank you."

**Cue:** Go back to page 15 for Q&A.

**Time:** ~30 sec

---

## Timing

| Part | Pages | Time |
|---|---|---|
| Hook | 1-2 | 1:50 |
| What broke | 3-5 | 4:45 |
| The turn | 6 | 1:30 |
| Five rules | 7-11 | 8:25 |
| Proof | 12 | 1:30 |
| What it unlocks | 13 | 1:45 |
| What's next | 14 | 1:35 |
| Take home | 15-16 | 1:45 |
| **Total** | | **~23:05** |

For a 15-minute slot use `script-15min.md`.

---

## Q&A (5 minutes)

**Q: Isn't 95% accuracy per step good enough?**
> "Chain ten of those steps and the whole run comes out right only about 60% of the time. That's why the checks sit between the steps, not only at the end."

**Q: The model offered to edit the state file. What stops it doing that anyway?**
> "Two things now. The model can't write progress files at all; only the pipeline scripts do. And after any Block, its shell can only read files and run the pipeline: no deleting, copying, editing in place or running its own code. What it isn't: a security sandbox. The model runs as me. I'd rather tell you where the edge is than pretend there isn't one."

**Q: Why only gate the one-way doors? Why not every step?**
> "Because I'm the weak link. Nine hard gates per meeting would train me to click yes without reading. The checks run on every step, automatically. The human only sits at the doors, where a yes means something."

**Q: Why not go back to Claude?**
> "I could, and the flow would run on it without changes, which is the point. Today my meetings run on a small cloud model. It costs pennies, and I'm not tied to one vendor: switching is one line. And I trust this system more now than I trusted the frontier model without it."

**Q: How small can you go?**
> "Same model at 8-bit got the people right. At 3-bit it invented one. Neither found all my tasks. Precision sets one floor, the model sets another. And that's one meeting, one run each: a strong hint, not a benchmark."

**Q: So do you run it locally?**
> "For quick single jobs, yes: a 3-bit model on my graphics card is fast and free. For meetings, no. At 3-bit on my card it made mistakes I'd have to catch every time, so meetings go to a small cloud model."

**Q: How is this different from Temporal or AWS Step Functions?**
> "Same problem, different scale. They give you durable workflows with servers, workers and workflow code. Mine is a progress file, some small scripts, and one rule: check before and after every step. For one person or a small team, that's enough. At company scale, use Temporal."

**Q: Jev is built for exactly these decisions. Doesn't it replace all this?**
> "It's a better part, not the structure. It doesn't remember which step you're on, survive a restart, or hold a door shut. It still needs the loop around it. If anything, Jev makes the case: put the model inside the loop, not in charge of it."

**Q: Isn't this just a state machine? What's new?**
> "The state machine isn't new. Putting one around an unpredictable worker is the part worth talking about. And the fact that you don't need a workflow engine to do it: a file, a few checks, and the discipline to stop when a check says no."

**Q: I don't write code. Can I use this?**
> "Yes. A script is anything that gives the same answer every time: a template, a spreadsheet formula, a form. Write the steps down, make each leave something you can check, and decide which steps need your yes. A checklist and a shared folder are enough to start."

**Q: Weren't your scripts written by an LLM too?**
> "Yes, most of them. And that's the point. A script drifts once, when it's written. I can read it, test it, and after that it does the same thing every time. A model drifts on every run, differently each time. Testing for this talk found eight bugs in those scripts, from a two-hour time shift to a slash in a meeting title that made a folder instead of a note. All fixed, and they stay fixed."

**Q: Can you share the code?**
> "The shape is the valuable part, and it's all on these slides. My code is tangled up with my own setup: Pi, my notes app, a handful of scripts. Happy to talk through any piece of it."

---

## If Opusfived won't load

Two sentences, then move on: "It's a game where you ask an AI to make one button blue. It turns every button blue, says it's done, then sends 23 agents to clean up." The slide carries the rest.

---

## References

### Opening

- **Opusfived** by Milos Novovic. https://opusfived.dev/

### Test meeting

- **"OKR Planning with GitLab Executive Team"**, GitLab Unfiltered, public on YouTube.

### Jev / TypeSafe AI (September 2026)

- **"Introducing System One Models & Jev"** (TypeSafe AI): the launch; outputs decisions, not text. https://typesafe.ai/blog/introducing-system-one-models-and-jev
- **"Jev at the Branches: The State Machine Is the Agent"** (StackToHeap, 21 Sep): deterministic code owns the loop, Jev supplies judgement at the branches. https://stacktoheap.com/blog/2026/09/21/the-state-machine-is-the-agent/
- **"What Is Jev? A Guide to TypeSafe AI's System One Model"** (LangChain). https://www.langchain.com/blog/building-a-harness-with-jev
- **"A new kind of AI model from a ChatGPT inventor…"** (TechCrunch). https://techcrunch.com/2026/09/18/a-new-kind-of-ai-model-from-a-chatgpt-inventor-is-thrilling-developers/

### Workflow engines, for the Temporal question

- **Temporal**: durable execution. https://temporal.io/
- **AWS Step Functions**: serverless workflow orchestration. https://aws.amazon.com/step-functions/
- **LangGraph**: stateful agent orchestration. https://langchain-ai.github.io/langgraph/

### Background reading

- **"Why Cheap Models Fail Silently in Long Agent Loops"**: instructions decay, JSON drifts, confident wrong plans. https://dreaming.press/posts/why-cheap-models-fail-silently-in-long-agent-loops.html
- **"AI Agents Coded for 16 Days Straight. The Model Didn't Do That, the Harness Did."** https://dreaming.press/posts/agents-that-run-for-days-durable-harness-not-model.html
- **"LLM Agent Guardrails: The Engineering Playbook"** (DEV): an 8B local model from 53% to 99% on agentic workflows. https://dev.to/monuminu/llm-agent-guardrails-the-engineering-playbook-for-taking-an-8b-local-model-from-53-to-99-on-18c
- **"Reason Less, Verify More: Deterministic Gates Recover a Silent Policy-Violation Failure Mode in Tool-Using LLM Agents"** (arXiv, Jul 2026). https://arxiv.org/html/2607.07405v1
- **"When Small Models Are Right for Wrong Reasons"** (arXiv, Jan 2026). https://arxiv.org/html/2601.00513
- **Adam Terlson (Best Buy), AI Engineer**: finite state machines for multi-agent systems. https://ai.engineer/talks/building-multi-agent-systems-with-finite-state-machines
