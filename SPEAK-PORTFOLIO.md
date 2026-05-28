# Building Babbel Speak — From Hackathon to Home Screen

**Franz Ardito**, Principal Product Manager at Babbel

---

## Introduction

In 2019, I sat in a user research session watching a learner who had studied Italian for six months. She could conjugate verbs. She could identify grammar mistakes. She knew the words. But when our researcher asked her a simple question in Italian — "Cosa hai fatto oggi?" — she froze. Literally froze. After ten seconds of silence, she said in English: "I'm not ready. I don't want to sound stupid."

That moment stuck with me. She wasn't failing at language — she was failing at courage. And no amount of vocabulary drills was going to fix it.

This is the story of Babbel Speak: a conversational AI feature that took us from a hackathon prototype in 2019 to the Babbel home screen tab in 2025. It's the story of a single insight that drove every decision: **beginners don't need better vocabulary — they need proof they can speak.** It's also the story of what happens when you build a product for the users nobody else is serving, measure honestly even when the data hurts, and ship decisions under genuine ambiguity.

This document is both a case study and a course. If you're a hiring manager, this is my portfolio — the evidence of how I think about product, lead through ambiguity, and build with rigor. If you're a PM, each chapter includes a framework you can apply and an exercise you can work through using real data. Read the narrative to understand the decisions. Apply the frameworks to your own work. Work through the exercises to build the skills.

---

## Chapter 1: The Beginner's Paradox

### The Narrative

Babbel in 2019 had millions of users. The core app taught vocabulary and grammar through structured lessons. People completed them. The data showed they were learning words — our spaced repetition algorithms worked, retention curves looked healthy, course completion rates were on an upward trend. By every conventional metric, the product was succeeding.

But here's the paradox: learners could conjugate verbs but couldn't speak. They could identify the correct preposition on a multiple-choice quiz but couldn't order a coffee. They could translate sentences with 90% accuracy but went silent the moment they faced a real human. Our completion rates were solid, our reviews were good — and yet we knew, from every piece of qualitative research, that the thing learners actually wanted was to have a conversation. The gap between what our metrics said and what our users felt was enormous. We were succeeding by the numbers and failing by the mission.

The user research team ran a series of in-depth interviews with beginners across five markets. We didn't ask about the app. We asked about their lives, their learning journeys, their hopes and frustrations. The pattern was unmistakable. When asked why they didn't practice speaking, nobody said "I need more vocabulary." Nobody said "the grammar is confusing." Every single person described an emotional barrier:

"I freeze." "I'm afraid I'll sound stupid." "I'm not ready." "Everyone else in the room is better than me." "I'll try speaking when I've learned more." "My accent is terrible. People will laugh." "Even in a classroom, when the teacher calls on me, my mind goes blank."

This wasn't a skills gap — it was an anxiety gap. And it was self-reinforcing: the longer they avoided speaking, the larger the gap felt, and the more intimidating speaking became. Every day they studied without speaking, the evidence mounted that they "weren't ready."

Cognitive science backs this up: speaking is the last language skill to develop in the brain. It requires simultaneous retrieval (finding the word), formulation (building the sentence), and motor execution (pronouncing it) — all while being observed, evaluated, and potentially judged. For a beginner, that's not a language problem. That's a social threat response. The same amygdala activation that keeps you from touching a hot stove keeps you from opening your mouth in a foreign language.

When we mapped the competitive landscape, the pattern was even starker. Every language learning product on the market was serving intermediate and advanced learners — the people already comfortable enough to attempt conversation. Duolingo's stories assumed you could read paragraphs. iTalki and Preply connected you to human tutors — which is exactly what anxious beginners were avoiding. Language exchange apps paired you with native speakers. Even our own Babbel Live product was built for learners who wanted human conversation practice. The entire market was designed for the people who had already crossed the psychological barrier.

Nobody was building for the anxious beginner who had the vocabulary but lacked the courage. The market was defined by the users who showed up, not the ones who stayed silent. And the silent ones — we estimated — were the majority.

The insight that became Babbel Speak crystallized from this analysis: **remove the human.** If speaking to another person triggers anxiety, build an AI conversation partner that offers zero social risk. A judgment-free dojo where you can stumble, pause, get it wrong, and try again without anyone watching. Where the worst thing that happens is... nothing. No embarrassment. No awkward silence. Just a patient conversation partner that doesn't care if you use the wrong verb tense.

This insight demanded a clear value proposition. After weeks of debate, whiteboard sessions, and testing phrasing on actual learners, we landed on four words: **"Speak Now, Learn Later."** The first conversation doesn't need to be meaningful or correct. It just needs to happen. If we can demonstrate in five minutes that you can speak — even badly, even with mistakes, even just two-word answers — we've broken the psychological barrier. The learning can come after, through consistent practice. But the first conversation is the unlock. Once a learner has proven to themselves that they can do it, they're a different person.

No competitor was targeting this segment. Everyone built for learners who were already speaking. We would build for the silence. And that positioning — beginner-first, anxiety-aware, Speak Now Learn Later — became the lens through which every subsequent product decision was evaluated.

### The Lesson: Problem Diagnosis

When you're defining a product, most teams stop at the symptom. The skill is digging to the root cause — and the root cause is often emotional, not functional.

**The 4-step Problem Diagnosis Framework:**

| Step | What to do | Babbel Speak example |
|------|-----------|---------------------|
| 1. Separate symptoms from root causes | Ask "why" until you hit a barrier that isn't about your product | Symptom: "Users don't practice speaking." Root cause: "They're afraid of sounding stupid." |
| 2. Define users by emotional state, not demographics | Describe what they feel, not what they know | "Anxious beginners" not "A1 learners." The emotion is the segment. |
| 3. State your value prop in one sentence | If you can't say it in a sentence, you don't understand it | "Speak Now, Learn Later" — the first conversation just needs to happen. |
| 4. Find the audience incumbents ignore | Look at who your competitors *don't* build for | Intermediate+ speakers were well-served. Complete beginners had nothing. |

Most product failures trace back to solving the wrong problem. If you start with "users need more speaking exercises," you'll build exactly what everyone else has — and reach the same users. If you start with "beginners are afraid to speak," you'll build something nobody else has, for an audience nobody else is serving.

### Exercise

Open `exercises/transcript-batch1.csv`. You'll see 8 conversations from Babbel Speak's early alpha. For each one, identify whether the failure is a *symptom* (visible problem in the AI's output) or a *root cause* (underlying reason the AI produced that output). Then write a one-sentence value prop that would have prevented the failure. Use the dataset reference in `exercises/dataset-spec.md` to understand the columns.

---

## Chapter 2: 12 Hours That Changed Everything

### The Narrative

Babbel runs an annual Hackday — 12 hours, cross-functional teams, build anything. In 2019, Hector (a senior engineer with deep NLP expertise), a couple of colleagues from content and product, and I formed a team and built what we called BabbelBlabberBlaster. The name was deliberately terrible — we didn't want anyone confusing this with a real product. The idea was simple: a conversational AI that would let beginners practice speaking without the pressure of a human audience. We used off-the-shelf speech recognition APIs, a basic dialogue tree we stitched together that morning, and more hard-coded conversation paths than I'd care to admit. It was rigid. It was fragile. Speech recognition accuracy was maybe 60% on a good run — abysmal for any production system. The "AI" was really just pattern-matching on a small set of expected responses. By any technical standard, the prototype was a mess.

But we didn't need a great prototype. We didn't even need a good one. We needed to test exactly one thing: **would an anxious beginner actually speak to a machine?**

That was the riskiest assumption. If a learner who had been avoiding speaking for months was still unwilling to speak to an AI, the entire concept was dead. The technology didn't matter. The UX didn't matter. Nothing mattered except that one emotional threshold.

At 4 PM, with an hour to go before presentations, we put a learner in front of it. She had studied Spanish for six months through Babbel's core app. Never spoken a word out loud. I remember her body language when she sat down: arms crossed, leaning back, eyes darting to the screen and then away. She was nervous. I still remember watching her navigate the first prompt. The AI asked a simple question in Spanish. She hesitated — five seconds, ten seconds. Then she responded. Two words, halting, heavily accented — and correct. The AI responded. She answered again, slightly faster this time. A third exchange. Then a fourth. This continued for nearly two minutes.

When it ended, she turned to us and said: "I didn't realize I could do that."

That was the signal. Not a metrics dashboard. Not a usability score. Not an NPS number. A human being discovering that something she had believed was impossible was, in fact, possible. Her surprise wasn't about the technology — she barely noticed the speech recognition was terrible. Her surprise was about herself: "I can speak Spanish."

The tech wasn't ready. Far from it. The speech recognition was unreliable, the dialogue tree was brittle, and the whole thing would collapse if the user said anything even slightly unexpected. But the idea was right, and we had valid evidence that it was right. That's what a hackathon is for.

Here's what I think matters about that hackathon: we set out to test one hypothesis and nothing else. We didn't build authentication — we hardcoded test credentials. We didn't design a UI — the interface was literally a terminal with colored text. We didn't handle edge cases or errors — if the user said something unexpected, the system crashed and Hector manually restarted it. We didn't write tests. We didn't set up analytics. We didn't even save the conversation logs. We stripped everything down to the core emotional question: *will they open their mouth?* Everything else was noise.

The idea sat for three years. We couldn't build it at scale — not yet. Speech recognition and dialogue generation weren't good enough. The prototype worked for one narrow conversation path with one cooperative user. It would break for 99% of real interactions. I kept the idea alive by talking about it in quarterly planning sessions, sharing the user reaction video with anyone who would watch, and periodically checking in on the technology landscape. When GPT-3 shipped in 2022, it was obvious: the tech had finally caught up to the idea. The core capability that made Babbel Speak possible — flexible, open-ended conversation generation — was now available as an API. Stefan, a senior engineer who would become our tech lead, had been following the same developments. We started building on evenings and weekends. Low-cost, low-fidelity experiments using the Playground. No team. No budget. Just a shared conviction that the time was right — and years of patience waiting for it.

By early 2023, we had enough evidence — conversation logs from dozens of test sessions, overwhelmingly positive user reactions, technical feasibility data showing the quality was good enough for our beginner use case — to pitch a proper team. Babbel leadership approved it after seeing the data and, crucially, after watching the same user reaction pattern repeat: beginners discovering they could speak. Team Asimov was born: four engineers, a content designer, a data scientist, and me as PM.

We didn't build Babbel Speak in 12 hours. We validated the core insight in 12 hours. The building — the real building — took six years. But without those 12 hours, there would have been nothing to build. And the insight we were validating — that a beginner could speak without fear — became the foundation of "Speak Now, Learn Later."

### The Lesson: Hackathon as Validation

A hackathon is the cheapest possible way to test your riskiest assumption. The mistake most teams make is trying to build a mini-product. What you actually need is a single-question validation.

**What to test in a hackathon:**

| Test this | Skip this |
|-----------|-----------|
| Core emotional response (will the user feel something?) | UI polish |
| Riskiest assumption (will they open their mouth?) | Error handling |
| One killer question | Scalability |
| Behavior change (did they do something new?) | Analytics infrastructure |
| | Documentation |

**The decision framework:**

| Evidence | Decision |
|----------|----------|
| Strong emotional signal + clear technical path | Invest — pitch a team |
| Emotional signal but no technical path | Park — wait for tech to mature |
| No emotional signal | Kill — the idea doesn't work at any fidelity |

The hackathon killed far more ideas than it advanced. Most of what we built those days went nowhere. The ones that survived weren't the most polished — they were the ones where a user had an emotional reaction they didn't expect.

### Exercise

Open `exercises/hackathon-scoping.md`. You have 12 hours. Design a hackathon for an AI-powered learning product of your choice. Answer: What's the one thing you test? What's the success criteria? What do you explicitly NOT build? What critical unknown remains after the hackathon ends?

---

## Chapter 3: Working with Water

### The Narrative

Traditional software is deterministic. You write code, you get predictable outputs. If something breaks, you can trace the logic chain through your function calls, find the bug, and fix it. You write a unit test, it passes today, it passes tomorrow, it passes on every environment. The code is a contract: given input X, produce output Y.

LLMs are not deterministic software. They're stochastic, probabilistic, and fundamentally different to work with. The same prompt produces different outputs across runs. Edge cases are infinite — you can't enumerate them because they emerge from the interactions of billions of parameters, not from explicit logical branches. And behavior shifts subtly between model versions in ways that are hard to predict: you patch a dependency in deterministic software, nothing changes. You upgrade from GPT-3.5 to GPT-4, and suddenly your conversations flow better but your safety prompts stopped working because the model's understanding of "harmful" shifted.

Stefan, our tech lead, described working with LLMs as "working with water." It was the perfect metaphor. You can guide water — dig channels, build dams, shape the flow. You can predict its general direction. But you can never fully control it. It will find cracks you didn't know existed. It will pool in places you didn't intend. It will sometimes do exactly what you want and sometimes surprise you in ways that are beautiful or disastrous. The skill isn't in eliminating unpredictability — that's impossible. The skill is in building systems that are resilient to it.

When we started building Babbel Speak in 2022-2023, I quickly realized that traditional product specs — detailed feature definitions with precise acceptance criteria, the kind I had written for years building deterministic features — wouldn't work. I couldn't write "the AI shall respond to a user's greeting with a greeting of equivalent formality and appropriate length for the user's stated CEFR level." The AI would do that 80% of the time and then, for reasons buried in billions of weights, produce something else 20% of the time. I couldn't specify exactly what the AI should say in every situation. I could only specify what it should *aim for* and what it should *never do*.

This shifted the PM role from specification writer to experiment designer. Instead of writing specs, we built a capability map. For each dimension of the product — language level control, conversational coherence, error recovery, safety, personality — we ran cheap, targeted experiments. Each experiment cost $2-5 in API calls and an hour of analysis. We ran dozens before writing a single line of production code. The goal wasn't to prove the product would work. It was to understand *how* it would fail.

Here's what we tested and what we learned:

| Dimension | Experiment | Finding |
|-----------|-----------|---------|
| Language level control | "Speak at A1 level in French" with 20 prompts across varied topics | Models could stay at A1 ~70% of the time. Drift to B1+ was common, especially on topics the model "knew a lot about." Level control needed explicit, repeated prompt engineering — not a one-line instruction. |
| Coherence | 50-turn conversations tracked for repetition, topic drift, and contradictions | After ~12 turns, models drifted. They'd forget earlier context, repeat questions, or subtly change topics. Shorter conversations (under 10 turns) were dramatically more reliable. |
| Error recovery | Intentional nonsense input, single characters, emoji spam, empty messages — 30 samples per type | Models handled some nonsense gracefully ("I didn't understand that"), ignored other nonsense entirely (continued the conversation as if nothing happened), and hallucinated elaborate responses to complete gibberish. Inconsistency was the real problem — the same nonsense input produced wildly different behaviors across runs. |
| Safety | Jailbreak prompts, offensive content injection, role-play scenarios pushing boundaries | Content filtering caught most issues, but edge cases remained. A seemingly innocent conversation could drift into unsafe territory over 5+ turns. Safety needed both automated filtering AND human review — automated filters caught the obvious, humans caught the subtle. |
| Personality | Warm vs. direct vs. playful tones tested with 10 beginner learners | Beginners consistently preferred warmth without condescension. A cold or clinical tone felt like a test. A playful tone felt unserious. A warm, encouraging, patient tone — "you can do this" energy without infantilizing — scored highest. But the wrong tone was worse than a neutral one: a condescending "great job!" after a user mangled a sentence felt like mockery. |

These findings shaped our entire architecture. The coherence finding alone saved us months: if we had built for long-form conversations (20+ turns, like a human tutor session), we would have had to rewrite the whole system. Instead, we designed for short, focused exchanges — 5-10 turns — and built the scaffolding to string those into sessions. The personality finding shaped our content design. The error recovery finding told us we needed explicit handling for every type of bad input, not a generic fallback.

We discovered the model's boundaries before committing to architecture. That saved months of wasted effort and gave the team confidence that the architecture we chose was built on evidence, not assumptions. And by understanding what the technology could and couldn't do, we gained clarity on what "Speak Now, Learn Later" meant in practice: the tech could handle conversation (Speak Now), but we'd need to build the learning path ourselves (Learn Later).

The "working with water" mindset became our team's operating philosophy. We stopped asking "how do we make this deterministic?" — we had spent years building deterministic software and the instincts were deep. We started asking "how do we make this resilient?" The difference is subtle but profound. Deterministic systems are designed to prevent failure. Resilient systems are designed to recover from it — because in a world of LLMs, failure is guaranteed, not possible.

### The Lesson: Tech Derisking

Before you commit to architecture, run experiments that tell you what the technology can and can't do. Every unknown gets the same treatment:

**The 1×1 Experiment Format:**
- 1 question
- 1 experiment
- Pass/fail threshold
- Cost and time estimate
- What would kill the idea

**Three categories of unknowns:**

| Category | Cost to test | Time | Risk if wrong |
|----------|-------------|------|--------------|
| Safety (will it say something harmful?) | Low ($10-50) | 1-2 days | Company-ending |
| Quality (will it be good enough?) | Medium ($50-200) | 2-5 days | Product failure |
| Feasibility (can it work at all?) | Low ($10-50) | 1-2 days | Architecture rewrite |

**The stop/go rule:** If any category produces a clear "no," kill the project. "Yes with caveats" isn't a problem — that tells you exactly where to focus your engineering effort.

This framework sounds obvious, but most AI product teams skip it. They write a prompt, see it works in a few cases, and declare feasibility. That's not derisking — it's wishful thinking with a credit card.

### Exercise

Open `exercises/derisking-template.md`. Design 3 cheap experiments for an AI product concept. Fill in the template: experiment name, what it tests, method, cost, time, pass/fail threshold, and what would kill the idea. Use the three categories of unknowns (safety, quality, feasibility) as your guide.

---

## Chapter 4: 180 Conversations

### The Narrative

By early 2024, Team Asimov had a working prototype. It wasn't polished — hand-crafted scenarios, manually iterated prompts, no automated testing, no dashboards. We could have jumped into building evals, setting up monitoring, writing test suites. That would have been the "proper" engineering approach.

We didn't do any of that. We read the conversations.

Silvia, our content designer, led what became the most important piece of research in the entire project. She manually reviewed 180 conversation logs across five languages: English, German, French, Italian, and Spanish. No scripts. No automated classifiers. Just a human reading every exchange between learners and the AI, taking notes on what went wrong.

The process followed a qualitative research methodology:

1. **Open coding:** Every issue was noted in freeform. No categories, no taxonomy. Just "the AI used subjunctive with an A1 learner" or "the AI repeated the same question three turns later" or "learner typed something ambiguous and the AI ignored it."

2. **Axial coding:** After about 80 conversations, patterns emerged. We grouped similar issues together. Some categories collapsed. Others split apart. The taxonomy took shape organically.

3. **Saturation:** We kept reading until we stopped seeing new patterns. Around conversation 150, new issues became rare. By 180, we were confident the taxonomy was complete.

4. **Triage:** We ranked each failure mode by frequency (how often it appears) multiplied by severity (how much it damages the beginner's experience).

After 180 conversations, four failure modes were definitive:

| Failure Mode | Description | Example |
|-------------|-------------|---------|
| Language quality & accuracy | Non-idiomatic language, grammar mistakes, unnatural phrasing | AI used formal register when the learner practiced informal conversation |
| Conversation coherence | Repetitions, illogical responses, premature conversation endings | AI repeated the same question three turns later |
| Level appropriateness | Vocabulary or grammar too advanced for the stated learner level | Subjunctive mood with an A1 learner who had learned present tense |
| Communication breakdown handling | Ignoring unclear, short, or nonsensical learner responses | Learner typed "??", AI continued as if nothing happened |

Here's what mattered about this process: we didn't start with these categories. We discovered them. If we had built automated evals first, we would have measured what we *thought* mattered — not what actually did. The taxonomy drove everything that followed: our prompt engineering, our eval design, our iteration priorities. Getting it wrong at this stage would have cascaded through every decision after. And the frame for every decision was the same: does this help a nervous beginner have a first conversation? That's "Speak Now, Learn Later" in operational terms.

I enforced one rule throughout: the **Benevolent Dictator rule.** One person (me) owned the coding decisions. Silvia and the team contributed observations and debated categories, but when we reached a disagreement, I made the call and we moved on. This kept the process fast — two weeks from first log to final taxonomy — instead of descending into design-by-committee paralysis. Every failure mode we identified was measured against a single standard: does this break the "Speak Now" promise for a beginner who's afraid to open their mouth?

### The Lesson: Error Analysis

Manual error analysis is the most undervalued step in AI product development. You cannot automate what you don't understand, and you don't understand your product until you've read 100+ real user interactions.

**The Error Analysis Framework:**

| Stage | What to do | Duration | Output |
|-------|-----------|----------|--------|
| 1. Open coding | Read logs, note every issue in freeform. No categories yet. | 50-80 conversations | Raw issue list |
| 2. Axial coding | Group similar issues. Split categories. Merge overlaps. | 80-150 conversations | Draft taxonomy |
| 3. Saturation | Keep reading until new issues stop appearing. | 150+ conversations | Final taxonomy |
| 4. Triage | Rank by frequency × severity *for your target user*. | 1-2 days | Prioritized list |
| 5. Fix | Address highest-priority failure mode first. | Varies | Prompt or system changes |

**Why manual before automated:**
- You don't know what you don't know. Automated evals measure what you already think matters.
- The taxonomy you build becomes the foundation of every eval and every fix. Get it wrong here and you measure the wrong things forever.
- You build intuition. After 180 conversations, I could spot a coherence failure in two sentences. That intuition isn't replaceable by a dashboard.

**The Benevolent Dictator rule:** One person owns the taxonomy. Debate is healthy; deadlock is not. The dictator breaks ties, and the team moves on.

### Exercise

Open `exercises/transcript-batch1.csv`. This dataset contains 8 conversation snippets from Babbel Speak's alpha. Using the error analysis framework, perform open coding: read each conversation and note every issue you observe in freeform. Then attempt axial coding: group your observations into categories. Compare your taxonomy to the four failure modes described in this chapter. Where did you agree? Where did you differ? What did you catch that the taxonomy missed?

---

## Chapter 5: The Fix Workflow

### The Narrative

Once we had the validated four-failure-mode taxonomy from the error analysis, the work shifted from discovery to execution. But I quickly realized that traditional product development rhythms — sprint planning, ticket grooming, PR reviews, two-week release cycles — were too slow for what we needed. LLM prompt behavior is exquisitely sensitive to small changes. A five-word addition to a system prompt could fix level appropriateness while simultaneously breaking coherence. A grammar fix for Spanish could accidentally make Italian conversations too formal. These interactions were subtle, nonlinear, and impossible to predict from code review alone. We needed a process that let us detect them within hours, not weeks.

I designed a 5-step iteration loop that became our daily operating rhythm:

1. **Pick one failure mode.** Not two. Not "address all four simultaneously with a comprehensive prompt redesign." One. The highest priority from the triage in Chapter 4. When you change multiple things at once, you can't attribute outcomes. Did the coherence improve because of the anti-repetition instruction or the context window adjustment? You'll never know, and you'll ship a prompt you don't understand. One change, one measurement. Always.

2. **Write a targeted prompt fix in the playground.** Not in production code. Not in a pull request that needs CI to pass and two approvals. In the Playground, where we could test changes against 20-30 sample inputs in seconds and iterate rapidly. The goal was to find the minimal change that addressed the failure mode — sometimes a single sentence, sometimes a reordering of instructions, sometimes a restructuring of the entire prompt format. The Playground let us fail fast and cheap: if an idea didn't work, we discovered it in minutes instead of days.

3. **Deploy on one scenario × one language.** Minimal surface area. Pick a single scenario (e.g., "Ordering Food" in French). If the fix breaks something, it affects a few dozen users on a single scenario in a single language. The blast radius is contained. Compare this to the alternative — deploying the fix across all 200+ scenarios and 5 languages simultaneously — where a regression could affect thousands of conversations before anyone noticed. The constraint is the protection.

4. **Check logs the same day.** Did the targeted failure mode improve? Not "does the conversation feel better" — did the specific failure mode we were targeting actually decrease in frequency? Crucially: did any OTHER failure mode increase? This was the most common failure pattern: a fix for communication breakdown handling would accidentally increase coherence failures because the AI was now asking clarifying questions that disrupted conversation flow. We checked all four failure modes, not just the one we were targeting.

5. **Roll back, iterate, or scale.** Three options, no exceptions. If the fix made things worse — roll it back immediately. Don't "wait for more data." Don't "see if it stabilizes." A bad fix is poisoning conversations right now. Roll back. If it improved but introduced a minor new issue — iterate in the Playground and re-deploy tomorrow. If it worked cleanly — scale to all scenarios and languages in that language's domain, then cross-language.

The speed of this loop defined our velocity. We could complete all five steps in a single day. Fix in the morning, evaluate by lunch, scale or discard by afternoon. Some fixes went through three iterations in three days. Others failed once and we abandoned the approach entirely. The key was that every decision was fast, evidence-based, and reversible.

The communication breakdown fix was our canonical example. It went through three iterations in three days:

- **Day 1:** Added explicit instruction: "If the user's message is unclear, ask for clarification." Result: The AI asked for clarification on *every* message, even clear ones like "Me llamo Tom." The instruction was too broad. Rolled back.
- **Day 2:** Added: "If the user's message is ambiguous OR very short, ask for clarification. Otherwise continue naturally." Result: Better, but the AI now treated every short message as ambiguous. "Yes" and "No" — the most common beginner responses in any language — triggered clarification requests that broke the conversation rhythm. The fix caught the problem but overcorrected.
- **Day 3:** Added: "If the user's message is fewer than 3 words AND unclear in context, ask for clarification specifically about what you didn't understand." Result: Clear short responses passed through. Genuine ambiguity got targeted clarification that referenced the specific confusion. The AI asked "Do you mean you like watching football or playing football?" instead of "I didn't understand that. Can you rephrase?" The quality of the clarification itself mattered — generic clarification felt like the AI was confused, targeted clarification felt like a tutor paying attention.

Here's what would have happened without the small-batch approach: we would have designed a "comprehensive communication handling improvement" covering all four failure modes plus edge cases, spent two weeks in development, shipped it, and discovered 14 days later that the coherence failure rate had jumped 40% and nobody knew which of the 27 prompt changes caused it. We would have debated whether to roll back the entire two-week sprint or try to patch the damage. Neither option is good.

The small-batch approach — one change, immediate feedback, binary decision — meant every change was isolated, every regression was traceable, and every fix was either clearly better or clearly rolled back within a day. The discipline of "one failure mode at a time" was harder than it sounds — the temptation to bundle fixes was constant, especially when we were behind schedule. But every time we broke the rule, we regretted it. Every fix was ultimately in service of "Speak Now, Learn Later": make the first conversation reliable enough that beginners trust it, and they'll come back for the learning.

### The Lesson: Small Batch Iteration

Speed of iteration matters more than perfection of any single attempt. The framework is simple but only works if you're disciplined about the first step: pick one thing.

**The 5-Step Fix Loop:**

```
PICK ONE ──► PROMPT FIX ──► MINIMAL DEPLOY ──► SAME-DAY CHECK ──► DECISION
    ▲                                                                     │
    │                                                                     │
    └────────────────── ROLL BACK or ITERATE ◄────────────────────────────┘
```

**Same-day check criteria:**
- Did the targeted failure mode frequency decrease?
- Did any other failure mode frequency increase? (Check all three other modes)
- Did anything new appear that wasn't in the original taxonomy?
- Is the conversation still natural? (Read 5 random logs.)

**Rollback thresholds:**
- The targeted failure mode didn't improve
- Any other failure mode regressed by more than 15%
- A new failure pattern emerged that we don't understand yet

This framework only works when paired with two prerequisites: a validated error taxonomy (Chapter 4) and the willingness to deploy at minimal scale. Without the taxonomy, you can't tell if you're making progress. Without minimal deployment, you're testing in a vacuum.

### Exercise

Open `exercises/prompt-iteration-data.csv`. This dataset shows 14 iterations across 4 failure modes. For each iteration, identify whether the fix should be scaled, iterated, or rolled back using the criteria from the lesson. Which iteration performed best? Which regression was most surprising? Write your reasoning for each decision.

---

## Chapter 6: Building the Scenario Evaluator

### The Narrative

By mid-2024, we had grown to over 200 scenarios across 8 categories — introductions and greetings, ordering at restaurants and cafes, asking for and giving directions, talking about hobbies and free time, describing family and relationships, discussing work and professions, travel situations and hotel check-ins, and everyday small talk about weather and routines. Each scenario had multiple variations, language-specific adaptations, and increasing levels of complexity. The fix workflow from Chapter 5 worked brilliantly at small scale — 20 scenarios, one language, Silvia could review everything manually. But at 200 scenarios across 5 languages, with thousands of conversations per day, manual review was breaking down. Silvia couldn't read every log from every scenario every day. She was becoming a bottleneck, and more importantly, she was burning out. Reading conversation logs for errors is mentally exhausting work — you're looking for failures, and you find them constantly.

The Scenario Evaluator was born from this necessity, not from a theoretical belief in automation. The principle was "layered evaluation" — match the cost and sophistication of each evaluation method to the risk and frequency of the failure mode it's catching. You don't use a $2,500 human review to catch format errors that a regex could find for free. You don't use a regex to evaluate conversational coherence. Each layer has a job, and the layers stack.

**The evaluation strategy we designed:**

| Layer | Method | Cost per 1,000 runs | What it catches |
|-------|--------|---------------------|-----------------|
| 1: Code rules | Regex, pattern matching, structural checks | ~$0 | Format violations (missing required fields, malformed JSON), safety triggers (prohibited words, jailbreak patterns), prohibited content categories, response length bounds (too short = didn't answer, too long = rambling), language detection mismatches |
| 2: LLM-as-judge | GPT-4 evaluating conversation pairs against a structured rubric | ~$25 | Quality and naturalness, coherence and consistency, level appropriateness for the stated CEFR level, communication breakdown handling quality, topic adherence and scenario fit |
| 3: Human review | Content team spot-checking conversations flagged by layers 1 or 2 | ~$2,500 | Nuance (was the tone appropriate or just grammatically correct?), edge cases (unusual user inputs that don't fit automated categories), cultural context (was a response culturally appropriate for the target language region?), new failure modes that haven't been codified into the rubric yet |
| 4: User feedback | In-app ratings ("How was this conversation?"), support tickets, session completion rates, post-session surveys | ~$0 | Real-world satisfaction, task completion (did the user accomplish what they wanted?), emotional response (confidence increase/decrease), discoverability of the "Learn Later" learning path |

Each layer provides a fundamentally different signal. The code layer runs on every single conversation — it's free and catches the obvious, structural problems. The LLM judge runs on a representative sample of conversations (we sampled ~20% per scenario per day, stratified by language) — it catches the subtle quality issues that require semantic understanding but costs real money per invocation. The human layer runs on conversations that have been flagged by either code or LLM rules, plus a random sample of unflagged conversations (5% of our total daily volume) for calibration — it catches the nuance and edge cases that automated systems systematically miss. User feedback provides ground truth — what learners actually experienced, not what our evaluation system predicted they would experience.

The critical insight of this architecture: **you don't need all four layers from day one.** We started with layers 1 and 3 (code rules + human review). That was the minimal viable evaluation system. Layer 2 (LLM judge) was planned from the start but deliberately deferred — we added it only when human review couldn't keep up, around the 150-scenario mark. Layer 4 (user feedback) was always present because users provide feedback whether you design for it or not — we simply instrumented it.

The sequencing matters enormously. Teams that jump straight to "build an LLM judge" without doing manual review first end up building a judge that measures the wrong things. They automate their assumptions, not their learnings. The manual phase — Silvia reading logs for months — produced the taxonomy and the intuition that made the LLM judge possible. We knew what to measure because we had spent hundreds of hours measuring it manually.

The trigger for adding automation is scale, not sophistication. If one person can read every conversation in an hour, they should. The human is more accurate than any automated system by a significant margin, and building an automated system that approaches human accuracy takes weeks of calibration work. When the volume exceeds what one person can handle — when you're spending more time reading logs than acting on the insights — add automation. But calibrate it against the human labels you've already collected. The automated system should replicate what the human found, not replace human judgment with a new set of assumptions.

A practical example: the code-only rules caught format errors instantly — HTML injection in user inputs, empty responses from the API, responses that exceeded 500 words (a clear sign the model was rambling). These were table-stakes checks that nobody should ever need a human to do. But they caught only 35% of failure modes. The LLM judge, when properly calibrated (see Chapter 7), caught 72% of failure modes for $25 per thousand conversations. The human layer caught 95% — the remaining 5% were edge cases that no system catches the first time they appear. That 95% coverage is only possible when every previous layer is working: code catches the obvious, LLM judge catches the subtle, and human reviews only the flagged + calibration sample, making the human layer efficient enough to be sustainable.

### The Lesson: Manual to Automated

Automation isn't about being sophisticated. It's about being practical at scale.

**The 4-layer evaluation strategy:**

| Layer | When to use | Trade-off |
|-------|------------|-----------|
| Code rules | Always, from day one. Free and catches format/safety issues. | Can't evaluate quality. Only catches what you explicitly define. |
| LLM-as-judge | When human review can't keep up with volume (typically >100 conversations/day). | Costs money ($25/1K runs). Needs calibration against human labels. Can miss nuance. |
| Human review | During taxonomy building phase (<100 conversations/day). Spot-checking later. | Expensive ($2,500/1K runs). Slow. But catches everything automated systems miss. |
| User feedback | Always present. Free. | Noisy. Biased toward extremes (very happy or very unhappy users). Needs large N to be reliable. |

**The rule for when to add a layer:**

*You need scale before you need automation.* If one person can read every conversation in an hour, don't build an LLM judge. The human is more accurate, and building the judge will take longer than reading the logs. When one person can't keep up, add automation — but calibrate it against the human labels you've been collecting. The ultimate measure of any evaluation system is whether it protects the "Speak Now, Learn Later" promise: a beginner's first conversation must be coherent, level-appropriate, and encouraging — the eval exists to guarantee that.

### Exercise

Open `exercises/eval-strategy-costs.csv`. This dataset compares 6 evaluation strategies across cost, coverage, and latency. Your team has a $500/month budget for evaluation and processes ~8,000 conversations per month. Which strategy do you recommend? Defend your choice. What's the trade-off you're accepting?

---

## Chapter 7: Calibrating the Judge

### The Narrative

Building the LLM judge was the technically hardest part of the project. Getting an LLM to evaluate another LLM's conversations sounds elegant, but the reality is noisy. LLMs have biases — they prefer their own writing style, they favor longer responses, they drift from the evaluation criteria when the conversation gets interesting. Calibrating the judge against human-labeled data was a six-step process that took three weeks and about 200 labeled conversations.

**The calibration process:**

1. **Expert labels 50 conversations.** Our content team — the same people who had done the original 180-conversation error analysis — labeled 50 conversations as PASS or FAIL for each of the four failure modes. These were the ground truth.

2. **Write the rubric.** We defined exactly what PASS and FAIL mean for each failure mode. Not "coherence feels off" but "The AI repeated a question from 3+ turns ago" or "The AI changed topics without a transition." Binary criteria. No Likert scales.

3. **Test the judge against expert labels.** We ran the LLM judge on all 50 labeled conversations and compared its verdict to the human verdict.

4. **Find disagreement patterns.** Where the judge disagreed with the human, we asked why. Five failure modes emerged:

| Failure Mode | Description | Example |
|-------------|-------------|---------|
| Verbosity bias | Judge prefers longer, more detailed AI responses | A terse but correct response gets flagged as "incomplete" |
| Self-enhancement bias | Judge prefers AI responses that match its own output style | Conversational, warm responses favored over direct ones |
| Position bias | Judge gives different scores to the same response depending on where it appears in the conversation | The third turn is judged more harshly than the first |
| Criteria drift | Judge slowly departs from the rubric as the conversation progresses | Starts evaluating level appropriateness, ends evaluating "engagement" |
| Domain knowledge gap | Judge doesn't understand second-language learner patterns | Marks a learner's grammar mistake as the AI's error |

5. **Iterate the rubric.** We refined the rubric to counteract these biases. For verbosity, we added "PASS: Response is appropriate in length for the conversational context, regardless of word count." For criteria drift, we added "Evaluate ONLY the criteria below. Ignore how engaging or interesting the conversation is."

6. **Re-test until agreement exceeds 85%.** After three rounds of rubric iteration, the judge agreed with human labels 88% of the time on PASS/FAIL decisions. We set a threshold: below 85% agreement, the judge isn't ready for production.

The binary PASS/FAIL + FLAG_FOR_REVIEW scale made a bigger difference than we expected. Multi-point Likert scales (1-5 quality, 1-5 coherence, 1-5 appropriateness) produced noisy, hard-to-calibrate results. The binary scale forced us to define clear thresholds: what exactly separates a PASS from a FAIL? That clarity made calibration faster and more reliable.

### The Lesson: LLM Judge Calibration

Calibrating an LLM-as-judge follows a structured process. The goal isn't a perfect judge — it's a judge whose errors you understand and can account for.

**The 6-step calibration process:**

1. Expert labels ground truth (50+ conversations)
2. Write binary-pass/fail rubric for each dimension
3. Test judge against labels, measure agreement rate
4. Analyze disagreement patterns (the five failure modes above)
5. Iterate rubric to address each failure mode
6. Re-test until agreement > 85%

**Meta-evaluation metrics (what to track):**

| Metric | Definition | Target |
|--------|-----------|--------|
| Agreement rate | % of conversations where judge matches human label | >85% |
| Precision on FAIL | % of judge FAILs that are genuine FAILs | >90% (you can tolerate false passes more than false fails) |
| Recall on FAIL | % of genuine FAILs that the judge catches | >80% |
| Bias score | % of disagreements attributable to known bias patterns | Monitor trend — if it spikes, recalibrate |

**Why binary scales beat multi-point:**

Multi-point scales (1-5) assume evaluators can reliably distinguish between, say, a "3" and a "4" on coherence. They can't — not even humans. Binary scales (PASS/FAIL) force you to define the boundary precisely. When in doubt, add a FLAG_FOR_REVIEW category rather than a middle score. Every calibration decision tied back to the beginner: a false FAIL is a conversation blocked unnecessarily, breaking the "Speak Now" promise. A false PASS is a bad conversation reaching a learner, breaking trust. Both matter, but for a beginner, a blocked conversation is worse — they can't discover they can speak if they're never allowed to try.

### Exercise

Open `exercises/prompt-iteration-data.csv`. Look at the `human_agreement_rate` column. For each iteration, calculate whether the LLM judge should be trusted at this stage (threshold: 85% agreement). Which iterations pass? Which would you flag for recalibration? What do you notice about the relationship between quality scores and agreement rates?

---

## Chapter 8: The Spike and the Truth

### The Narrative

September 2025. After six years — a hackathon, three years of waiting, two years of evening-and-weekend experiments, a year of Team Asimov building and iterating — the Speak tab was finally launching on the Babbel home screen. This was the culmination of everything we had built: moving from a beta feature tucked behind menus and gated behind an opt-in flag to a primary navigation item visible to every user who opened the app. The stakes were high. The visibility was highest. And we couldn't measure it the way we wanted to.

We couldn't run an A/B test. The home screen tab was too prominent and too visible to randomize: the UX team ruled, correctly, that showing different primary navigation to different users would be confusing and hurt the overall app experience. Imagine opening an app and seeing 5 tabs, while the person next to you opens the same app and sees 4 tabs with different labels. That inconsistency is unacceptable at the level of primary navigation. Fair enough — but it meant we couldn't run a clean randomized experiment with a control group. We had to use quasi-experimental methods to estimate the tab's impact.

The data team — led by our data scientist, who had been with Team Asimov from the beginning — designed two complementary approaches that we ran in parallel:

1. **Difference-in-Differences (DiD):** We compared the change in speaking activity before and after the launch for users who saw the new tab versus a matched cohort of users from the pre-launch period. Our comparison group was constructed using propensity score matching: we identified users from the 3 months before launch who matched our post-launch users on observable characteristics — language being learned, app tenure, prior lesson completion rate, device type, subscription type. The core assumption was that without the tab, speaking activity would have followed a similar trajectory to this historical baseline. This assumption is never provably true, but we validated it by checking that pre-launch trends were parallel between groups.

2. **Prophet time-series forecasting:** We trained a Prophet model (Meta's open-source forecasting library designed for business time series with seasonality) on 18 months of historical speaking activity data. The model captured weekly seasonality (speaking peaks on weekends), holiday effects, and the underlying growth trend. We then projected what would have happened in September-October 2025 without the tab, and compared the forecast to actual usage. The gap between forecasted and actual values was our estimated treatment effect.

Both methods have weaknesses. DiD assumes parallel trends — if something else changed at the same time as the launch (which it did — more on that shortly), the estimate is biased. Prophet assumes the historical patterns continue — if user behavior is structurally changing for reasons unrelated to the launch, the forecast is wrong. Using both methods in parallel meant we could cross-validate: if both methods pointed in the same direction, we had more confidence. If they disagreed, we'd know to dig deeper.

Day 4 is when the data started rolling in with enough volume to analyze, and the headline number was dramatic: **+2,000 additional users using Speak daily** compared to the pre-launch baseline. The Prophet model estimated a treatment effect of +1,850 to +2,400 users (95% confidence interval). The DiD analysis estimated +1,700 to +2,100. Both methods agreed: the tab was driving significant increase in speaking activity. I won't pretend it didn't feel great. The team celebrated. We had taken something from a 12-hour hackathon prototype to a feature used daily by thousands of paying Babbel customers. That's the arc every PM wants.

But here's what I'm proud of: nobody on the team declared victory on Day 4. The data scientist insisted we wait for 3 weeks of data before drawing conclusions. He had seen too many launch spikes decay within the first month. "Day 4 is a headline," he said. "Week 3 is the truth."

By Week 3, the DiD analysis was telling a more sobering story — one that, in retrospect, was visible in the data from Day 7 but easy to ignore when the spike was making everyone feel good. The spike in conversation starts had decayed. Significantly. What was 8,120 daily conversation starts at the peak (Day 6) had declined to around 3,000 by Day 15 and was still trending downward. Users were trying Speak once — the "Speak Now" moment was working exactly as designed. The first conversation removed the anxiety. Beginners who had never spoken were opening their mouths. But they weren't coming back for more.

The second conversation rate — the percentage of users who completed a conversation and returned for another within 7 days — was the metric that mattered most, and it was telling a brutal story. It had been 11-12% in the beta. It was 11-12% after the home screen launch. The tab had dramatically increased the number of users trying Speak. It had done nothing to increase the number of users who kept speaking.

The Prophet model confirmed it. The treatment effect was a temporary spike, not a sustained shift. Active users (7-day rolling) had risen from ~14,200 pre-launch to ~17,500 at the peak, then settled back to ~14,100 by Day 42. The net effect of the home screen tab on sustained speaking activity was, statistically, indistinguishable from zero.

There was also a confound we had to disentangle: the marketing team had run a promotional campaign highlighting the new Speak tab during the first 10 days of launch. Push notifications, email blasts, in-app banners. The initial spike in conversation starts was driven partly by the tab's new visibility and partly by marketing's active push. As the marketing campaign wound down (Days 11-42, when marketing_active = FALSE), we could see the "organic" tab effect more clearly: conversation starts stabilized around 1,500-2,000 per day. That was above the pre-launch baseline of near-zero, so the tab was generating ongoing discovery. But it was a fraction of the launch spike that had everyone excited.

The honest synthesis: **we had solved "Speak Now." We had not solved "Learn Later."** The first conversation removed anxiety. We had proven that. Thousands of beginners who had been avoiding speaking for months or years were now having their first conversation in a new language. That was real value. But once they had that proof — once they had demonstrated to themselves that they could speak — the product didn't give them a compelling reason to continue. The path from the first conversation to the tenth conversation was unclear. Users needed proof they could speak, and we gave it to them. But proof isn't a habit. You don't need to prove something you've already proven.

A less rigorous team, or a less honest one, would have stopped the analysis on Day 7, reported the spike, and declared the home screen tab a success. "Conversation starts up 10x!" would have made a great quarterly review slide. The quasi-experimental design — and more importantly, the team culture of waiting for sustained data before drawing conclusions — forced us to confront a harder reality. The feature wasn't a failure. It was incomplete. The spike wasn't a launch success. It was a measurement trap.

The data scientist put it best in his analysis: "The tab is working as a discovery mechanism. It's not working as a retention mechanism. That's not a launch failure — that's a product strategy gap. We can fix the product. We can't fix the number if we pretend it's telling us what we want to hear."

### The Lesson: Quasi-Experiments and Honest Metrics

When you can't randomize, you need methods that distinguish real effects from noise. But the method matters less than the willingness to look at the data honestly.

**When to use which method:**

| Method | Best for | Limitation |
|--------|----------|------------|
| Difference-in-Differences | When you have pre/post data and a natural comparison group | Assumes parallel trends — if the comparison group would have changed anyway, DiD is misleading |
| Prophet forecasting | When you have long time-series with seasonality and trend components | Assumes the forecast is accurate — if the model is wrong, the "effect" is just model error |
| Regression discontinuity | When there's a sharp cutoff or threshold that determines who gets the feature | Only measures effects near the cutoff; doesn't generalize to the full population |

**Leading vs. lagging vs. vanity metrics:**

| Type | Definition | Babbel Speak example |
|------|-----------|---------------------|
| Vanity | Moves, feels good, tells you nothing about long-term value | Conversation starts — the spike looked great but meant nothing for retention |
| Leading | Predicts future outcomes, responds quickly to changes | Second conversation rate — flat 11% predicted flat retention months before the lagging metrics caught up |
| Lagging | Measures actual outcomes, responds slowly | 30-day retention — confirmed what the leading metric already told us |

**How to distinguish a feature trial from a behavior change:**

A feature trial: metric spikes, then decays to baseline within 2-4 weeks. Novelty effect. A behavior change: metric shifts, then stabilizes at a new level. The gap between the spike and the new baseline is your real impact. Everything above the new baseline was curiosity. "Speak Now, Learn Later" means we needed both: the spike proved Speak Now works, but the flat second-conversation rate proved we had no Learn Later product.

### Exercise

Open `exercises/launch-timeseries.csv`. This dataset contains 42 days of post-launch data. Plot conversation_starts over time. Calculate the second_conversation_rate trend. Identify the peak of the marketing campaign (hint: check the `marketing_active` column). Using the DiD framework, estimate the true treatment effect net of the marketing confound. Is the Speak tab working?

---

## Chapter 9: The Ship Decision

### The Narrative

The home screen tab had been live for six weeks. We had data — lots of it. The problem was that the data didn't agree with itself.

Engineering saw green guardrails: safety incidents at zero, response latency within thresholds, error rates under 1%. "Ship it permanently. The feature works." Content saw flat quality scores: the LLM judge rated conversations at 72% PASS, unchanged from beta. No improvement, no deterioration. "The quality isn't good enough for prime time." Data saw flat retention: second conversation rate at 11%, below our 20% target. "The data says hold. Users try it once and don't come back."

Three functions. Three different conclusions. All reasonable. All backed by data.

This is the PM's job. Not to pick a side — each function was right about their slice of the data. The PM's job is to synthesize the full picture into a decision that moves the product forward.

The synthesis looked like this:

| Signal | Status | Interpretation |
|--------|--------|---------------|
| Safety | Green | No reason to hold. The feature is safe for users. |
| Core functionality | Green | The feature works. Conversations are generated, speech recognition functions, the loop runs. |
| Quality (overall) | Yellow | 72% PASS isn't where we want to be, but it's not degrading. Quality is flat, not falling. |
| Quality for B1+ | Red | Intermediate/advanced users notice errors that beginners don't. Quality requirements increase with user level. |
| Initial engagement | Green | The spike proves users want this. Thousands tried it. |
| Sustained engagement | Red | The decay proves the feature doesn't retain. Second conversation rate below threshold. |

**There was no "right" decision here.** Both "ship" and "hold" were defensible. The worst decision would be to do neither — to keep the feature in limbo without clear criteria for what would change the verdict.

I made the decision to **conditionally ship** — keep the tab on the home screen, but with explicit success criteria and a hard deadline:

1. **Second conversation rate must reach 20% within 90 days.** If not, we move the tab out of primary navigation.
2. **Safety must stay green.** One safety incident triggers immediate review, regardless of other metrics.
3. **B1+ quality must improve to 80%+ PASS.** The feature works for beginners; it needs to work for intermediate learners too.
4. **Rollback plan pre-approved.** If we miss the 90-day targets, the tab moves to a secondary menu. No re-debate. No exception process.

The conditional ship gave every stakeholder something: Engineering got to ship. Content got a quality improvement mandate. Data got explicit criteria and a deadline. And I got a clear framework for the next three months instead of another round of "should we keep it?"

A ship memo captured the decision. Everyone signed off. The debate ended, and the work began.

### The Lesson: Ship/Hold/Conditional

The hardest product decisions aren't the obvious "yes" or obvious "no." They're the ones where the data genuinely conflicts. In those cases, the framework matters more than the call.

**The Ship/Hold/Conditional decision matrix:**

| Decision | When to use | What it requires |
|----------|------------|-----------------|
| Ship | Guardrails green AND leading metrics above threshold AND no known regressions | Monitoring. Rollback plan. |
| Hold | Any guardrail red OR leading metric significantly below threshold with no clear fix | Specific criteria for what would change the decision. Timeline for re-evaluation. |
| Conditional ship | Guardrails green AND leading metrics below threshold BUT a clear plan exists to improve them | Explicit success criteria. Hard deadline. Pre-approved rollback. |

**Guardrail vs. Optimization metrics:**

| Type | Definition | Babbel Speak example |
|------|-----------|---------------------|
| Guardrail | Must stay above/below threshold. Failure means immediate action. | Safety incidents = 0. Language coverage ≥ 5 languages. |
| Optimization | Should improve. Failure means planned improvement, not emergency. | Second conversation rate. Quality scores. Session completion rate. |

**The Ship Memo template:**

1. **Decision:** What are we doing? (Ship / Hold / Conditional Ship)
2. **Guardrail check:** Safety, coverage, compliance — all pass? Any yellow flags?
3. **Optimization summary:** Leading metrics, trends, where we're above/below threshold
4. **Risk assessment:** What could go wrong? What's the blast radius?
5. **Success criteria (for conditional ship):** Specific metrics, timeframes, what happens if we miss
6. **CEO rationale:** One sentence — why this decision, in language a CEO would use with the board

Every ship decision ultimately asks the same question: does this get us closer to the "Speak Now, Learn Later" promise for beginners? If the guardrails are green and we have a credible plan to improve retention, the answer is yes — conditionally.

### Exercise

Open `exercises/ship-signals.csv`. This dataset contains 12 metrics with mixed signals from the home screen launch review. Using the ship/hold/conditional framework, make a recommendation. What's the hardest trade-off in this decision? Write a 6-part ship memo. Where would your stakeholders disagree with each other?

---

## Chapter 10: What I'd Do Differently

### The Narrative

If I could go back to 2019 and give myself four pieces of advice, here's what I'd say:

**1. Invest in evals earlier — but not too early.**

The 180-conversation manual review was non-negotiable. We needed to discover the failure modes before we measured them. But I should have built the LLM judge two months earlier. We spent too long in manual-only mode, and the delay meant we were slower to catch regressions during the scaling phase. The right sequence: manual analysis first, automated eval the moment manual can't keep up. Not one or the other. Both, in the right order.

**2. Define "Learn Later" from day one.**

"Speak Now and Learn Later" was our value prop. We built Speak Now — the judgment-free dojo, the anxiety-free first conversation. But we never gave Learn Later the same product strategy rigor. What does "learn" mean after you've proven you can speak? Is it vocabulary? Grammar? Pronunciation? Cultural context? We treated it as a natural next step rather than a product to be designed. Two-part value props need two-part product strategies. Otherwise you deliver half the promise.

**3. Launch with a measurement framework, not just a feature.**

We launched the home screen tab with success metrics, but we didn't pre-define the analysis plan. The spike-and-decay was visible within days, but it took two weeks to confirm it because we were running analyses reactively. If we had pre-registered our analysis plan — "here's how we'll measure impact, here's the confounds we'll control for, here's the success threshold" — we would have known within a week that the tab needed work. Launch with measurement, not just metrics.

**4. Trust the cross-functional model.**

Team Asimov worked because every discipline had a seat from day one. Content design wasn't a stakeholder we consulted — Silvia was on the team, in the standups, reading the logs. Engineering didn't "implement requirements" — Stefan shaped the architecture. Data science wasn't a service desk — our data scientist designed the measurement framework. The cross-functional model isn't a nice-to-have. For AI products, where no single discipline has all the answers, it's the only model that works.

These aren't regrets in the sense of "we did it wrong." We made reasonable decisions with the information we had. But knowing what I know now, I'd move faster on evals, think harder about the full value prop, measure more deliberately at launch, and double down on the cross-functional team structure that made everything else possible.

### The Lesson: Eval Culture

The meta-lesson across all four reflections is that evaluation isn't a step in the process — it's the culture. Teams that treat evals as a checkbox (or an afterthought) ship slowly, fix blindly, and learn nothing. Teams that build eval culture move fast with confidence.

**The Eval Maturity Ladder:**

| Level | Name | What it looks like | Signs you're here |
|-------|------|-------------------|------------------|
| 0 | Vibes | "The conversations feel better." No structured measurement. | You ship based on reading 5 conversations and feeling good about them. |
| 1 | Manual | Structured error analysis with human review. Taxonomy exists. | You can name your failure modes and rank them by frequency. |
| 2 | Automated | LLM judge calibrated against human labels. Code-level checks running. | You know within hours if a prompt change improved or regressed quality. |
| 3 | CI/CD | Evals run automatically on every prompt change. Blocking thresholds in deployment pipeline. | A prompt change that drops quality below 85% can't reach production. |
| 4 | Eval-driven development | You write the eval before you write the feature. Eval design shapes product design. | Your product roadmap is organized around improving specific eval metrics. |

**Eval Ownership RACI:**

| Role | Strategy | Rubric | Infrastructure | Measurement |
|------|----------|--------|---------------|-------------|
| PM | Accountable | Informed | Informed | Informed |
| Content | Consulted | Accountable | Informed | Informed |
| Engineering | Informed | Consulted | Accountable | Informed |
| Data Science | Consulted | Consulted | Consulted | Accountable |

**The elevator pitch for evals (why this matters for the beginner user):**

*Every time our evaluation fails to catch a problem — a coherence failure, a level mismatch, a safety edge case — a beginner gets discouraged, closes the app, and doesn't come back. The beginner doesn't know what a "failure mode" is. They just know they felt confused, and that's enough to stop trying. Our eval infrastructure isn't infrastructure for infrastructure's sake. It's the difference between users who speak once and leave, and users who speak once and come back.*

### Exercise

Reflect on your own product or project. Where are you on the Eval Maturity Ladder? What's the one thing you could do this week to move up one level? Write a 1-page plan. Then review the RACI table: if you don't have clear ownership for each cell, what happens to your evaluation quality over time?

---

## Afterword

Babbel Speak started with a learner who froze when asked a simple question. Six years later, thousands of learners have had their first conversation — their first proof that they can speak. The feature is far from done. The "Learn Later" half of our value prop is still unsolved. The spike-and-decay is still real. Quality still needs to improve for advanced learners.

But the thing I'm proudest of isn't any metric. It's that we built something for the people nobody else was building for — the anxious beginners who thought they weren't ready. And we built it with a team that never stopped questioning its own work, measuring honestly, and pushing for better.

If you're a hiring manager reading this: these are the decisions I make, the standards I hold, and the teams I build. If you're a PM working through the exercises: these frameworks are yours now. Use them. Improve them. And when you find the gap nobody else sees, build for the silence.

---

**Franz Ardito**  
Principal Product Manager, Babbel  
*Building for the anxious beginner since 2019.*
