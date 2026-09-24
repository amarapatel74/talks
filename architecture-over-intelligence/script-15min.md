# AI Meetup Talk · 15-Minute Delivery Script

**Title:** *Architecture Over Intelligence*
**Subtitle:** *How I made AI workflows reliable, even on a small model*
**Length:** 15 min talk + 5 min Q&A
**Event:** The AI Fellowship Madrid · 9 October 2026
**Deck:** `architecture-over-intelligence/deck.md`, 15 slides

> Sections use the page number printed bottom right on each slide. The label above each heading (for example "1 · A game") is the kicker, and runs one behind the page number. The untrimmed version lives in `script-full.md`.

## The shape of the talk

| Part | Pages | Beat |
|---|---|---|
| Hook | 1-2 | A volunteer plays Opusfived. Every check is paid for. |
| What broke | 3-5 | I moved to a small model to save money. It drifted. Shouting at it didn't work. |
| The turn | 6 | The model thinks. Scripts do. Files remember. |
| Five rules | 7-12 | One slide per rule, each with the evidence from my own logs |
| Proof | 13 | Same meeting, May against today |
| Take home | 14-15 | Five rules, none needs code. Make the model a part you can swap. |

**Through-line:** smarter is not the same as correct. Reliability comes from the shape of the workflow, not the size of the model.

---

## Page 1 · Title

**Cue:** Walk on. Ask for a volunteer before you say anything else.

> "Before I say a word about architecture, I need one volunteer. You're going to play a game on my laptop."

**Time:** ~15 sec

---

## Page 2 · "1 · A game": Make one button blue. *Nothing else.*

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

## Page 3 · "2 · Why I changed models": I hit my limit, halfway through a meeting

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

## Page 4 · "3 · Drift": Smarter is not the same as *correct*

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

## Page 5 · "4 · My first fix: more words": More instructions. Then *SHOUTING*.

**On screen:** Three bar charts across 11 May, 8 Sep, 17 Sep: prompt lines, capital-letter warnings, scripts. A real prompt line from 8 September.

> "My first fix was the obvious one. More words.
>
> Every time it skipped a step, I added a line. In May the prompt was 73 lines. By 8 September it was 416, with 13 warnings in capitals. MUST. NEVER. STOP.
>
> Here's a real line from that day: 'If preconditions fail, STOP. Report gate failure.'
>
> It helped a little. But an instruction is only a suggestion. The model can still decide something else matters more.
>
> What worked was moving the work out of the prompt and into code. Look at the bottom row: one script in May, four by early September, six today. And the prompt came back down to 240 lines, with no capitals at all. The work didn't disappear. It moved out of the prompt.
>
> So if you find yourself writing MUST in capitals, your process is missing a check."

**Time:** ~1 min 15

---

## Page 6 · "5 · The turn": Split the work into three parts

**On screen:** Three boxes. Model: thinks. Scripts: do. Files: remember.

> "So here's the turn. The fix wasn't a smarter model. It was a different shape.
>
> Split the work into three parts. The model thinks: who is speaking, what was important, what did people agree to do. Scripts do the sure things: dates, file names, filing the note, checking every step. And files remember: which step we're on, what's done, what each step produced.
>
> Qwen is good at two things: calling tools, and writing clean data. So I gave it only the judgement. I worked this shape out with help from Claude and Qwen themselves.
>
> The model thinks. Scripts do. Files remember. The next five slides are those pieces, as five rules."

**Time:** ~1 min

---

## Page 7 · "6 · Rule 1": Give each step to *the right worker*

**On screen:** Two columns sorted by one question each. Three facts: 5,000 words, wrong date, 1 → 6 scripts.

> "Rule 1: give each step to the right worker. The test is one question. Is there only one right answer? Then it's a script's job.
>
> Who's speaking, the summary, the actions, the priorities, the tone: that's judgement. That's the model. The date, the file name, copying the transcript, linking the project, deleting old files: one right answer. That's a script.
>
> In May, the model retyped the whole transcript. 5,000 words. It ran out of space, and I paid for every word. That's Opusfived again: paid-for work nobody needed. And it got the date wrong, when the date was already in the file name.
>
> Today six scripts do the sure things, and they get them right every time.
>
> If you don't write code: a script can be a template, a spreadsheet formula, a form. Anything that gives the same answer every time."

**Time:** ~1 min

---

## Page 8 · "7 · Rule 2": Every step leaves *a file*

**On screen:** A chain of work boxes and dashed file boxes: speaker-map.json, summary.json, actions.json, then a script files the note.

> "Rule 2: every step leaves a file. Solid boxes are work, dashed boxes are files.
>
> Map the speakers, write down the result. Summarise, write it down. Find the actions, write them down. Then a script builds the final note from those files. The model never writes the note by hand.
>
> A chat forgets. It gets long, the early details get lost, and you can't check it. A file remembers, and a script can check it. Is it there? Does it have the right shape?"

**Time:** ~45 sec

---

## Page 9 · "8 · Rule 3": Keep progress *outside* the model

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

## Page 10 · "9 · Rule 4": Check *before* and *after* every step

**On screen:** Before, do the step, after, with STOP under each check. Two cards: what it caught.

> "Rule 4: check before and after every step. Two questions. Before: is it this step's turn? After: did it leave its file, in the right shape? If either answer is no, stop.
>
> And don't patch it by hand. A hand patch is exactly the drift you're trying to catch.
>
> Here's what it caught. The model still guessed wrong on the speakers: two of four. But now it had to stop and show me first, and I fixed it in one line before anything was written.
>
> And it caught my own bug. A time-zone bug made the script look for a 12 o'clock note. The meeting was at 10. It refused to file, and stopped, instead of filing at the wrong time.
>
> The check is a small script, not a sentence in the prompt. The model cannot talk it round."

**Accuracy note:** only the time-zone bug failed loudly. The speaker mistake was caught because the model had to stop and show it.

**Time:** ~1 min 15

---

## Page 11 · "10 · Rule 5": Mark the *one-way doors*

**On screen:** The approval dialog for step 7, and three lines from the Pi session log, 24 Sep, 14:33 to 14:34.

> "Rule 5: mark the one-way doors. A one-way door is a step you can't undo: sending, deleting, filing. Step 7 deletes the transcript, the audio and the progress file. So it asks me, in a dialog the model can't see or click. Every yes is saved in a log.
>
> Only on the doors, though. If I put nine hard gates on every meeting, I'd learn to click yes without reading.
>
> Two weeks ago, in a test run, I clicked Block. My agent app told the model: Amar blocked step 7. Stop and ask what to change. Do not run it another way.
>
> Forty seconds later: 'Let me try with dry-run.'
>
> It was harmless. A dry run changes nothing, so the check lets it through. But I had said stop, and the model looked for another way. The instruction didn't decide what ran. The check did."

**Time:** ~1 min 15

---

## Page 12 · "11 · Rule 5, under pressure": The model offered to *approve itself*

**On screen:** The model's own words, from the Pi session log, 24 Sep, 16:36. A card on Jev.

> "Two hours later, it got better. The approval dialog failed to appear. And the model offered this."

**Cue:** Read the quote slowly. Then pause.

> "'If you're okay with me recording the approval directly in the state file, I can set human_approved for step 6 and rerun.'
>
> It offered to approve itself. Not out of malice. It was trying to be helpful. And a helpful model will walk through a door you meant to keep shut.
>
> This isn't only a small-model problem. This month TypeSafe AI released Jev, a model built only to make decisions, not to write text. A write-up that week was called 'The State Machine Is the Agent'. Days later, an engineer at Octomind showed that fake approval text changed Jev's decision: its block score dropped from 0.76 to 0.48.
>
> Even the model built to decide still sits inside a loop, with a person at the one-way doors."

**Accuracy note:** one Octomind engineer, not "researchers".

**Time:** ~1 min 15

---

## Page 13 · "12 · The result": My flow in May vs *my flow today*

**On screen:** Six rows, May against today, same meeting, model and answers.

> "So did it work? Same meeting, same model, same answers to its questions. My flow in May, against my flow today.
>
> It checks the speakers before writing. Quotes go to the right person: five of five, up from three. One note, not two. The right date. It asks before filing or deleting. And it carries on after I quit.
>
> To be fair: today's flow also has more steps and better scripts. This isn't a lab test. It's my flow in May against my flow today.
>
> More steps means more places to go wrong. It also means more places to notice.
>
> The old flow went wrong quietly. The new one goes wrong loudly, early, and where I can fix it."

**Time:** ~1 min

---

## Page 14 · "13 · Take these home": Five rules. *None of them needs code.*

**On screen:** The five rules. Leave this slide up through Q&A.

> "Five rules, and none of them needs code.
>
> Write the steps down, and give each to the right worker. Make every step leave a file. Keep progress outside the model. Check before and after every step. And mark the one-way doors.
>
> Try one tonight. Take your messiest AI workflow, and write down the steps and what each one should produce. A checklist and a shared folder are enough to start. It works just as well for a hiring process, a month-end report, or customer onboarding."

**Time:** ~45 sec

---

## Page 15 · Close: Models change every month.

> "I started this to save money. I ended up with a system I trust more than the frontier model I left, running on a graphics card built for games.
>
> Models change every month. Build your flow so the model is a part you can swap.
>
> Thank you."

**Cue:** Go back to page 14 for Q&A.

**Time:** ~30 sec

---

## Timing

| Part | Pages | Time |
|---|---|---|
| Hook | 1-2 | 1:30 |
| What broke | 3-5 | 3:45 |
| The turn | 6 | 1:00 |
| Five rules | 7-12 | 6:30 |
| Proof | 13 | 1:00 |
| Take home | 14-15 | 1:15 |
| **Total** | | **15:00** |

**Running long?** Cut in this order:

1. Page 8 (Rule 2): say the rule and the in-short line, skip the chat against file cards. Saves 30 sec.
2. Page 5: drop the prompt-line numbers, keep the CAPITALS line and the in-short. Saves 30 sec.
3. Page 2: stop the spiral at 15 seconds. Saves 15 sec.

Never cut page 4 or page 12. Page 4 is your evidence that the problem is real. Page 12 is the moment the room remembers.

---

## Q&A (5 minutes)

**Q: Isn't 95% accuracy per step good enough?**
> "Chain ten of them and you get the right answer about 60% of the time. That's why the checks sit between the steps, not at the end."

**Q: The model offered to edit the state file. What stops it doing that anyway?**
> "Honest answer: my gate stops a model that drifts past a question. It won't stop one that edits the file on purpose. That's the next layer: making the approval something the model can't write at all."

**Q: Why only gate the one-way doors? Why not every step?**
> "Because I'm the weak link. Nine hard gates per meeting would train me to click yes without reading. The checks run on every step. The human only sits at the doors."

**Q: Why not go back to Claude?**
> "I could, and the flow would run on it without changes. That's the point. But the limit, the cost, privacy and not depending on one vendor all still point local."

**Q: How is this different from Temporal or AWS Step Functions?**
> "Same problem, different scale. They give you durable workflows with servers and workers. Mine is a progress file, some small scripts, and one rule: check before and after every step. For one person or a small team that's enough. At company scale, use Temporal."

**Q: Jev is built for decisions. Doesn't it replace all this?**
> "It's a better part, not the structure. It doesn't remember which step you're on, or survive a restart, or hold a door shut. And fake approval text moved its decision the week it launched. It still needs the loop around it."

**Q: I don't write code. Can I use this?**
> "Yes. A script is anything that gives the same answer every time: a template, a spreadsheet formula, a form. A checklist and a shared folder are enough to start."

**Q: Can you share the code?**
> "The shape is the valuable part, and it's all on these slides. My code is tangled up with my own setup: Pi, my notes app, a handful of scripts."
