# AI Meetup Talk · 15-Minute Delivery Script

**Title:** *Architecture Over Intelligence*
**Subtitle:** *How I made AI workflows reliable, even on a small model*
**Length:** 15 min talk + 5 min Q&A
**Event:** The AI Fellowship Madrid · 9 October 2026
**Deck:** `architecture-over-intelligence/deck.md`, 16 slides

> Sections use the page number printed bottom right on each slide. The label above each heading (for example "A game") is the kicker. The untrimmed version lives in `script-full.md`.

## The shape of the talk

| Part | Pages | Beat |
|---|---|---|
| Hook | 1-2 | A volunteer plays Opusfived. Every check is paid for. |
| What broke | 3-5 | I moved to a small model to save money. It drifted. Shouting at it didn't work. |
| The turn | 6 | The whole flow on one map. The model thinks. Scripts do. Files remember. |
| Five rules | 7-11 | One slide per rule, each with the evidence from my own logs |
| Proof | 12 | Same meeting, May against today |
| What it unlocks | 13 | Cheaper, safer, portable. Smaller models on bigger jobs. |
| What's next | 14 | A new kind of model, a decider, slots into one box. |
| Take home | 15-16 | Five rules. Start with a checklist; add tested scripts. Make the model a part you can swap. |

**Through-line:** smarter is not the same as correct. Reliability comes from the shape of the workflow, not the size of the model.

---

## Page 1 · Title

**Cue:** Walk on. Ask for a volunteer before you say anything else.

> "Before I say a word about architecture, I need one volunteer. You're going to play a game on my laptop."

**Time:** ~15 sec

---

## Page 2 · "A game": Make one button blue. *Nothing else.*

**On screen:** Opusfived live. The slide shows the loop: task, check the work, check the check, call more agents, explain why checking matters, check again.

**Cue:** The volunteer picks the options. Max 30 seconds of spiral. If the site doesn't load in 10 seconds, use the two-sentence fallback and move on.

> "This is Opusfived, a parody by Milos Novovic. One job: make this button blue. Nothing else."

**(Volunteer plays.)**

> "It checks the work. Then it checks the check. Then it calls more AI agents, and explains why checking matters, and checks again. The button never turns blue.
>
> It's funny because it's true. And it's also where my tokens went. Every one of those checks is paid for."

**Fallback:** "It's a game where you ask an AI to make one button blue. It spirals into checking its own checks and calling more agents, and the button never turns blue."

**Time:** ~1 min 15

---

## Page 3 · "Why I changed models": I hit my limit, halfway through a meeting

**On screen:** Claude in the cloud against Qwen 3.6 on my desk.

> "Some context. I run a lot of my work through AI: a morning briefing, my inbox, and my meetings. A recording goes in. Out come who said what, a summary, the actions, and a note filed in the right place.
>
> Until May, that ran on Claude. Very capable. But my plan has a usage limit, and in May I hit it halfway through a meeting. And every meeting I processed was leaving my house.
>
> So I moved to Qwen 3.6. An open model, running on a 16 GB gaming graphics card on my desk. No limit, no bill, and nothing leaves the house.
>
> The limit was the trigger. Cost, privacy, and not depending on one vendor are why I stayed."

**Cue:** Say the hinge line slowly.

> "Economics started the journey. Drift is what I found along the way."

**Time:** ~1 min

---

## Page 4 · "Drift": Smarter is not the same as *correct*

**On screen:** Two red cards, Run 1 (local) and Run 2 (cloud). Test meeting: "OKR Planning with GitLab Executive Team", public on YouTube.

> "On Claude, this worked. On Qwen, it drifted. Drift is when the AI slowly goes off track, and doesn't notice.
>
> I tested it on a public meeting so I can show you: a GitLab executive team planning their goals, on YouTube.
>
> Run 1, Qwen on my own computer. It wrote the note before it knew who was speaking. It named all four speakers wrong: the CEO became 'head of alliances'. And it deleted the transcript without asking. After 35 minutes I stopped it by hand.
>
> Run 2, the same model in the cloud. It seemed smarter. Better precision. The same kinds of mistakes, just better hidden. Two notes for one meeting. The wrong date. Two of five quotes given to the wrong person. And a Chinese word in the middle of an English sentence.
>
> It knew the right names, and still put the wrong name on the quote.
>
> Smarter is not the same as correct."

**Time:** ~1 min 30

---

## Page 5 · "My first fix: more words": More instructions. Then *SHOUTING*.

**On screen:** Three bar charts across 11 May, 8 Sep and today: prompt lines, capital-letter warnings, scripts. A real prompt line from 8 September.

> "My first fix was the obvious one. More words.
>
> Every time it skipped a step, I added a line. In May the prompt was 73 lines. By 8 September it was 416, with 13 warnings in capitals. MUST. NEVER. STOP.
>
> Here's a real line from that day: 'If preconditions fail, STOP. Report gate failure.'
>
> It helped a little. But an instruction is only a suggestion. The model can still decide something else matters more.
>
> What worked was moving the work out of the prompt and into code. Look at the bottom row: one script in May, four by early September, seven today. And the prompt came back down to about 250 lines, with no capitals at all. The work didn't disappear. It moved out of the prompt.
>
> So if you find yourself writing MUST in capitals, your process is missing a check."

**Time:** ~1 min 15

---

## Page 6 · "The turn": My whole flow, *split three ways*

**On screen:** The whole meeting flow today, one row per step, in columns: Model thinks, Scripts do, You decide, Files remember. Reviews are dashed; the one-way door is solid.

> "So here's the turn. The fix wasn't a smarter model. It was a different shape.
>
> This is my whole meeting flow today, one row per step.
>
> The model does the judgement: who's speaking, what was said, who agreed to do what, how urgent it is. Scripts do the sure things: the transcript, the calendar, spotting duplicates, filing the note. I confirm at a few points, and one step, deleting, is a one-way door. And every step leaves a file.
>
> Qwen is good at two things: calling tools, and writing clean data. So I gave it only the judgement. I worked this shape out with help from Claude and Qwen themselves.
>
> The model thinks. Scripts do. Files remember. The next five slides are the rules behind this map."

**Accuracy note:** 6 model jobs, 7 scripts, 4 quick reviews (speakers, what was said, actions with priorities, follow-up) and 1 one-way door. If asked about slide 11's "only on the doors": the reviews are a glance in the chat; the door is a hard stop the model can't see or get round.

**Time:** ~1 min 30

---

## Page 7 · "Rule 1": Give each step to *the right worker*

**On screen:** Two columns sorted by one question each. Three facts: 5,000 words, wrong date, 1 → 7 scripts.

> "Rule 1: give each step to the right worker. The test is one question. Is there only one right answer? Then it's a script's job.
>
> Who's speaking, the summary, the actions, the priorities, the tone: that's judgement. That's the model. The date, the file name, copying the transcript, linking the project, deleting old files: one right answer. That's a script.
>
> In May, the model retyped the whole transcript. 5,000 words. It ran out of space, and I paid for every word. That's Opusfived again: paid-for work nobody needed. And it got the date wrong, when the date was already in the file name.
>
> Today seven scripts do the sure things, and they get them right every time.
>
> If you don't write code: a script can be a template, a spreadsheet formula, a form. Anything that gives the same answer every time."

**Time:** ~1 min

---

## Page 8 · "Rule 2": Every step leaves *a file*

**On screen:** A chain of work boxes and dashed file boxes: speaker-map.json, summary.json, actions.json, then a script files the note.

> "Rule 2: every step leaves a file. Solid boxes are work, dashed boxes are files.
>
> Map the speakers, write down the result. Summarise, write it down. Find the actions, write them down. Then a script builds the final note from those files. The model never writes the note by hand.
>
> A chat forgets. It gets long, the early details get lost, and you can't check it. A file remembers, and a script can check it. Is it there? Does it have the right shape?"

**Time:** ~45 sec

---

## Page 9 · "Rule 3": Keep progress *outside* the model

**On screen:** The progress file card, and the quit-and-reopen timeline.

> "Rule 3: keep progress outside the model. One small file says which step we're on, what's done, whether the speakers are confirmed, whether I stopped it.
>
> In rehearsal, I closed everything in the middle of step 3. When I opened it again, it read this file and carried on. Nothing repeated. Nothing lost.
>
> The model had forgotten everything. The file hadn't.
>
> And the same file let me run these demos on my local Qwen and on a cloud copy without changing a line."

**Time:** ~1 min

---

## Page 10 · "Rule 4": Check *before* and *after* every step

**On screen:** Before, do the step, after, with STOP under each check. Two cards: what it caught.

> "Rule 4: check before and after every step. Two questions. Before: is it this step's turn? After: did it leave its file, in the right shape? If either answer is no, stop.
>
> And don't patch it by hand. A hand patch is exactly the drift you're trying to catch.
>
> It caught two things. The model still guessed wrong on the speakers: two of four. But now it had to stop and show me first, and I fixed it in one line before anything was written.
>
> And it caught my own bug. A time-zone bug made the script look for a 12 o'clock note. The meeting was at 10. It refused to file, and stopped, instead of filing at the wrong time.
>
> The check is a small script, not a sentence in the prompt. The model cannot talk it round."

**Accuracy note:** only the time-zone bug failed loudly. The speaker mistake was caught because the model had to stop and show it.

**Time:** ~1 min 15

---

## Page 11 · "Rule 5": Mark the *one-way doors*

**On screen:** The approval dialog for step 7. The message the model gets after a Block, and what the shell refuses from then on.

> "Rule 5: mark the one-way doors. A one-way door is a step you can't undo: sending, deleting. Step 7 deletes the transcript, the audio and the progress file. So it asks me, in a dialog the model can't see or click. Every Approve and every Block is logged.
>
> Only on the doors. If I put nine hard gates on every meeting, I'd learn to click yes without reading.
>
> When I click Block, two things happen. The model is told to stop and ask. And the shell makes sure: from then on it can read files and run my pipeline, but it can't delete, move or copy files, edit them in place, run code it wrote itself, or try the same door again.
>
> I needed both layers. In my first tests the model heard 'stop' and went looking for another way. In one early test it even offered to write its own approval. Politely. That's why the model can't touch the progress file at all. After I added the shell layer, I ran it three more times. It stopped and asked every time.
>
> The instruction says stop. The shell makes sure."

**Accuracy notes:** 3 of 3 retests on 24 Sep stopped and asked. The model didn't try another way in those runs, so the refusals are proven by tests, not live. Say "not a sandbox" if asked: the model runs as me.

**Time:** ~1 min 15

---

## Page 12 · "The result": My flow in May vs *my flow today*

**On screen:** Six rows, May against today, same meeting, model and answers.

> "So did it work? Same meeting, same model, same answers to its questions. My flow in May, against my flow today.
>
> It checks the speakers before writing. Quotes go to the right person: five of five, up from three. One note, not two. The right date. It asks before deleting anything. And it carries on after I quit.
>
> To be fair: today's flow also has more steps and better scripts. So treat this as my flow in May against my flow today, not a lab test.
>
> More steps means more places to go wrong. It also means more places to notice.
>
> The old flow went wrong quietly. The new one goes wrong loudly, early, and where I can fix it."

**Time:** ~1 min

---

## Page 13 · "What this unlocks": Smaller models, *bigger jobs*

**On screen:** Three cards: cheaper, safer, portable. Below them, five other workflows the same shape fits.

> "So what does this unlock, beyond my meeting notes?
>
> It's cheaper. A small, local model does the judgement, and scripts do the rest, so you stop paying for work with one right answer.
>
> It's safer. Every step leaves a file, and every approve and block is logged. You can show exactly what happened, and who said yes.
>
> And it's portable. You can swap the model without rewriting the flow, and private data can stay on your own machine.
>
> Sales call follow-ups, invoice processing, onboarding, hiring, month-end close: same steps, same files, same doors. Reliable flows let you use smaller models on bigger jobs."

**Accuracy note:** no numbers on this slide. These are consequences of what the talk has shown, not measurements.

**Time:** ~45 sec

---

## Page 14 · "What's next": A new kind of model arrived *this month*

**On screen:** The model box split into a writer and a decider, the steps that only choose, and a confidence dial.

> "Models change every month, and this month a new kind arrived. TypeSafe AI released Jev, a model that only makes decisions. It doesn't write text. It gives you a choice, and how sure it is.
>
> Look at my flow. Some judgement steps write: the summary, the actions. But many only choose: the sentiment, the priority, the project, whether to send a follow-up, which name goes with which voice.
>
> A decider fits those. The flow offers only the legal options, and a script checks the answer is one of them. And its confidence can decide who answers: sure, carry on; unsure, ask me. I'd be asked less, and only when it matters.
>
> I haven't wired it in yet. But it slots into one box, and nothing else in the flow changes."

**Accuracy note:** Jev early access opened 15 Sep. Say "TypeSafe says" for any claim about accuracy or speed. Be clear it isn't wired in.

**Time:** ~1 min

---

## Page 15 · "Take these home": Five rules. *Start with a checklist.*

**On screen:** The five rules. Leave this slide up through Q&A.

> "Five rules, and you can start without code.
>
> Write the steps down, and give each to the right worker. Make every step leave a file. Keep progress outside the model. Check before and after every step. And mark the one-way doors.
>
> Try one tonight. Take your messiest AI workflow, and write down the steps and what each one should produce. A checklist and a shared folder are enough to start. Without code, you are the check. Then hand the checks with one right answer to a script, and test it. It works just as well for a hiring process, a month-end report, or customer onboarding."

**Time:** ~45 sec

---

## Page 16 · Close: Models change every month.

> "I started this to save money. I ended up with a system I trust more than the frontier model I left, running on a graphics card built for games.
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
| Hook | 1-2 | 1:15 |
| What broke | 3-5 | 3:30 |
| The turn | 6 | 1:30 |
| Five rules | 7-11 | 5:30 |
| Proof | 12 | 1:00 |
| What it unlocks | 13 | 0:45 |
| What's next | 14 | 1:00 |
| Take home | 15-16 | 1:00 |
| **Total** | | **15:30** |

**Running long?** Cut in this order:

1. Page 8 (Rule 2): say the rule and the in-short line, skip the chat against file cards. Saves 30 sec.
2. Page 5: drop the prompt-line numbers, keep the CAPITALS line and the in-short. Saves 30 sec.
3. Page 2: stop the spiral at 15 seconds. Saves 15 sec.

Never cut page 4 or page 11. Page 4 is your evidence that the problem is real. Page 11 is the proof that the fix holds.

---

## Q&A (5 minutes)

**Q: Isn't 95% accuracy per step good enough?**
> "Chain ten of them and you get the right answer about 60% of the time. That's why the checks sit between the steps, not at the end."

**Q: The model offered to edit the state file. What stops it doing that anyway?**
> "Two things now. The model can't write progress files at all; only the pipeline scripts do. And after any Block, its shell can only read files and run the pipeline. What it isn't: a security sandbox. The model runs as me, so a determined attacker is a different problem."

**Q: Why only gate the one-way doors? Why not every step?**
> "Because I'm the weak link. Nine hard gates per meeting would train me to click yes without reading. The checks run on every step. The human only sits at the doors."

**Q: Why not go back to Claude?**
> "I could, and the flow would run on it without changes. That's the point. But the limit, the cost, privacy and not depending on one vendor all still point local."

**Q: How is this different from Temporal or AWS Step Functions?**
> "Same problem, different scale. They give you durable workflows with servers and workers. Mine is a progress file, some small scripts, and one rule: check before and after every step. For one person or a small team that's enough. At company scale, use Temporal."

**Q: Jev is built for decisions. Doesn't it replace all this?**
> "It's a better part, not the structure. It doesn't remember which step you're on, or survive a restart, or hold a door shut. It still needs the loop around it."

**Q: I don't write code. Can I use this?**
> "Yes. A script is anything that gives the same answer every time: a template, a spreadsheet formula, a form. A checklist and a shared folder are enough to start."

**Q: Weren't your scripts written by an LLM too?**
> "Yes, most of them. The difference: a script drifts once, when it's written, and I can test it. A model drifts on every run. Testing for this talk found three bugs in those scripts. All fixed, and they stay fixed."

**Q: Can you share the code?**
> "The shape is the valuable part, and it's all on these slides. My code is tangled up with my own setup: Pi, my notes app, a handful of scripts."
