# Teamwork Exercise — "Architect the Agent"

> **Companion to:** `agentic-turn-seminar.html` + `agentic-turn-speaker-notes.md` · Course **CTT526 – Kiến trúc phần mềm**
> **Format:** in-class design studio, run **right after** the seminar · **~45–60 min** · teams of **4–5**
> **One-line pitch to the class:** *"You just heard the theory. Now your team is the architecture team — take an ordinary system, make it agentic, and defend your boundaries against a rival team trying to break it."*

---

## Why this exercise

It forces students to *use* the four lenses from the talk instead of just recognizing them:

1. **A new style** → choose the agent patterns.
2. **A new middleware** → decide what the agent connects to (MCP / tools & data).
3. **Quality attributes** → say how they bound non-determinism.
4. **The architect's role** → design the guardrails and human-in-the-loop gates.

The final **red-team round** turns the seminar's punchline into an experience: the durable part of a design is the *boundaries*, and the way you discover whether a boundary holds is to have someone attack it.

### Learning objectives

By the end, each student can:
- Map a real system onto agent patterns and justify the choice.
- Identify where non-determinism creates architectural risk and propose a way to bound it.
- Place human-in-the-loop gates at the *irreversible / high-blast-radius* actions and nowhere wasteful.
- Critique another team's architecture using the vocabulary of quality attributes and guardrails.

---

## Logistics

|                     |                                                                                                                  |
| ------------------- | ---------------------------------------------------------------------------------------------------------------- |
| **Team size**       | 4–5 (assign roles below, or let teams self-organize)                                                             |
| **Number of teams** | Any; the exercise scales — see *Scaling* at the end                                                              |
| **Time**            | 45 min minimum, 60 min comfortable                                                                               |
| **Materials**       | One printed **Architecture Canvas** per team (template below), or a shared slide/whiteboard; markers or a laptop |
| **Deliverable**     | One filled canvas + a 3-minute pitch                                                                             |

**Optional team roles** (rotate the vocabulary onto people):
- **Lead Architect** — owns the canvas, makes the final call on trade-offs.
- **Integrations** — owns the tools/data (MCP) column.
- **Quality & Evals** — owns non-determinism and testing.
- **Safety / Guardrails** — owns the human-in-the-loop gates and limits.
- **(5th) Skeptic** — pre-attacks the team's own design before the red-team round.

---

## Facilitator timeline (45-min version)

| Time      | Phase                  | What happens                                                                                             |
| --------- | ---------------------- | -------------------------------------------------------------------------------------------------------- |
| 0–5 min   | **Brief & form teams** | Hand out a scenario per team + a blank canvas. Read the task aloud.                                      |
| 5–25 min  | **Design sprint**      | Teams fill the canvas. Circulate; nudge with the prompts in *Facilitator prompts*.                       |
| 25–30 min | **Pitch prep**         | Each team distills to a 3-minute pitch: system, patterns, guardrails, "what breaks first."               |
| 30–40 min | **Pitches + red team** | Teams present in pairs; the paired team runs the **red-team checklist** and asks one attacking question. |
| 40–45 min | **Debrief**            | Whole-class discussion using the debrief questions.                                                      |

*(60-min version: give the design sprint 30 min and let every team present, not just pairs.)*

---

## The task (read this to the class)

> "Your team owns a system that today is run entirely by people. Leadership wants to make it **agentic** — an AI agent doing the routine work. Your job is not to build it; it's to **architect** it responsibly on one page.
>
> Fill the Architecture Canvas. You have 20 minutes. Then you'll pitch it in 3 minutes, and a rival team will try to find the boundary you forgot. Design assuming your agent is a *capable but unreliable actor* — because it is."

---

## Scenarios (assign one per team)

Each scenario deliberately contains at least one **irreversible or high-stakes action** so the guardrail thinking has teeth. Reuse the same scenario for two teams if you want a head-to-head at pitch time.

1. **Course-registration assistant (university).** Helps students pick courses, resolve clashes, and *submit their enrollment*. Stakes: enrollment deadlines, prerequisite rules, limited seats.
2. **Clinic triage assistant (healthcare).** Reads a patient's described symptoms and history, suggests urgency and next steps. Stakes: safety, wrong triage, privacy.
3. **Refund & support agent (e-commerce).** Answers customers and *issues refunds* up to some amount. Stakes: money out the door, fraud, tone.
4. **Incident-response agent (DevOps).** Watches alerts, diagnoses outages, and *can restart services or roll back a deploy*. Stakes: prod downtime, cascading failure.
5. **Loan / fraud pre-review agent (banking).** Screens applications or flags suspicious transactions for a human officer. Stakes: money, bias/fairness, false positives.
6. **Research/library assistant (your own domain).** Searches internal papers and drafts literature summaries with citations. Stakes: hallucinated sources, plagiarism.

*(Feel free to swap in a scenario from students' own semester projects — it makes the debrief land harder.)*

---

## The Architecture Canvas (one page per team)

> Teams copy this and fill every box. Keep answers to bullet points — this is a napkin architecture, not a document.

```
┌────────────────────────────────────────────────────────────────────┐
│  TEAM: __________________     SCENARIO: ____________________________ │
├────────────────────────────────────────────────────────────────────┤
│ 1. SYSTEM & GOAL                                                     │
│    What does the agent do, and what "done" looks like:              │
│    ________________________________________________________________ │
│                                                                      │
│ 2. AGENT PATTERNS  (pick 2–4, say WHY each)                          │
│    ☐ Reflection  ☐ ReAct  ☐ Plan-and-Execute  ☐ Tool Use            │
│    ☐ Multi-Agent ☐ Memory ☐ Human-in-the-Loop                       │
│    Why these: ____________________________________________________  │
│                                                                      │
│ 3. TOOLS & DATA  (the MCP layer — what it connects to)              │
│    Tools/APIs it calls: __________________________________________  │
│    Data it reads/writes: _________________________________________  │
│    What is READ-ONLY vs WRITE: ___________________________________  │
│                                                                      │
│ 4. BOUNDING NON-DETERMINISM  (it will be wrong sometimes)           │
│    How you test it (semantic? rollouts? threshold?): _____________  │
│    What you pin / bound (version, temperature): __________________  │
│    New quality attribute you care about most: ____________________  │
│                                                                      │
│ 5. GUARDRAILS & HUMAN-IN-THE-LOOP                                    │
│    Actions the agent may do UNATTENDED: __________________________  │
│    Actions that REQUIRE a human click: ___________________________  │
│    Hard limits (scope, spend cap, permissions): __________________  │
│                                                                      │
│ 6. FAILURE MODE                                                      │
│    "What breaks FIRST, and who gets hurt?": ______________________  │
│                                                                      │
│ 7. THE DURABLE PART                                                  │
│    One sentence: if the agent's code were thrown away tomorrow,     │
│    what boundary in this design must survive? ____________________  │
└────────────────────────────────────────────────────────────────────┘
```

Boxes **5, 6, 7** are the ones that connect straight back to the seminar's thesis — reward teams that take them seriously, not just the pattern-picking in box 2.

---

## The 3-minute pitch (what each team presents)

Keep it disciplined — one slide of the canvas on screen or the paper held up:

1. **The system** in one sentence. *(20s)*
2. **Patterns & why** — the two or three that shape the design. *(40s)*
3. **The dangerous action** and the **guardrail** you put around it. *(60s)*
4. **What breaks first** — name the failure you're most worried about. *(40s)*
5. **The durable boundary** — the one line from box 7. *(20s)*

---

## Red-team round (the payoff)

After each pitch, the **paired rival team** has 90 seconds to attack, using this checklist. They must land **one** concrete question the presenting team answers on the spot.

**Red-team checklist — try to find the missing boundary:**
- [ ] Name an **irreversible action** the agent can do with no human gate.
- [ ] Describe an input where the agent is **confidently wrong** — what happens next?
- [ ] Find a tool/permission that should be **read-only** but isn't.
- [ ] Ask: *how would you even know* it's failing in production? (observability)
- [ ] Point at a guardrail that is **theatre** — friction with no real protection.
- [ ] If the model version changed overnight, **what silently breaks?**

**Rule for presenters:** you may fix your design *out loud* — "good catch, we'd add a gate there." Improving under fire is the point, not defending a perfect answer. Award a small bonus to the sharpest attack, not just the best design — it rewards the guardrail mindset the seminar argued for.

---

## Debrief (5 min, whole class)

Run 2–3 of these:

1. **Every team's dangerous action needed a human gate — did anyone gate something that *didn't* need it?** (Surfaces the "gate the irreversible, free the reversible" principle from slide 9.)
2. **Whose "what breaks first" was the scariest — and was it a code bug or an architecture gap?** (Almost always the latter — that's the thesis.)
3. **Did any two teams with the same scenario draw different boundaries? Who was right?** (There's rarely one answer — architecture is trade-offs.)
4. **Fill in the blank as a class:** *"When the agent's code is disposable, the thing worth arguing over in a review is ______."* (Steer to: the boundaries in box 7.)

Close by pointing back at the mapping table from the seminar: *"You just did design patterns, middleware, quality attributes, and governance — the whole course — for a kind of system that didn't exist when the textbook was written."*

---

## Assessment rubric (optional, if graded)

| Criterion                | 0 — missing               | 1 — present                    | 2 — strong                                            |
| ------------------------ | ------------------------- | ------------------------------ | ----------------------------------------------------- |
| **Pattern fit**          | Patterns picked at random | Reasonable patterns            | Patterns clearly justified by the task                |
| **Integration clarity**  | Tools/data vague          | Tools & read/write named       | Least-privilege reasoning (read-only by default)      |
| **Non-determinism plan** | Ignored                   | A testing/bounding idea        | Statistical acceptance + version pinning + a named QA |
| **Guardrails**           | None, or all theatre      | Human gate on the risky action | Gates *only* where irreversible; hard limits stated   |
| **Failure insight**      | "It won't fail"           | Names a plausible failure      | Failure traced to a specific boundary + who's harmed  |
| **Red-team performance** | Silent                    | One valid attack               | Attack that exposed a real missing boundary           |

Total /12. The exercise rewards *judgment about boundaries* over technical polish — which is exactly the skill the seminar claimed becomes scarce and valuable.

---

## Facilitator prompts (to unstick teams during the sprint)

- Stuck on patterns? *"Does the agent need to check its own work? That's Reflection. Does it need live data? That's Tool Use."*
- Hand-waving on quality? *"Say the same request runs twice and gives two answers — is that OK here? If not, what do you do about it?"*
- Over-gating everything? *"Which of these actions can you always undo? Those probably don't need a human."*
- Under-gating? *"Point at the action that spends money or can't be taken back. Who approves that?"*
- Empty box 7? *"If we deleted all the agent's code tonight and rebuilt it, what rule must the new version still obey?"*

---

## Scaling & variants

- **Large class / short time:** skip live pitches; do a **gallery walk** — canvases on the wall, each team leaves one red-team sticky note on a neighbor's, then a 5-minute debrief.
- **Two-scenario head-to-head:** give the *same* scenario to two teams and let the class vote on whose boundaries they'd trust with their own money/data.
- **Take-home / individual:** the canvas works solo as a one-page written assignment; drop the red-team round and require a written "how a rival would attack this" paragraph instead.
- **Follow-on lab:** the strongest canvas becomes the spec for a small build later in the course — the architecture is the durable artifact, the code comes after.

---

### Quick-print checklist for you before class

- [ ] Print **N ÷ 2** … actually **N** copies of the Architecture Canvas (one per team) + a couple spare.
- [ ] Decide scenario assignment (or let teams draw one at random).
- [ ] Pre-pair teams for the red-team round (Team 1↔2, 3↔4, …).
- [ ] Put the **red-team checklist** on a slide or the board so attackers can see it.
- [ ] Keep the seminar's slide 9 (guardrails) and slide 11 (thesis) reachable — you'll point back to them in the debrief.