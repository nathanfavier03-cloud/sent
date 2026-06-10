# Sent — AI Gospel-Conversation Roleplay (design doc)

**Status:** Phase 2 · spec'd, data model seeded · not yet built in the Base44 UI.
**One line:** practice a real gospel conversation in the target language against an AI playing a
realistic local — who raises that culture's actual objections, reacts to your tone, and then coaches you.

> Nobody in the faith-app or language-app space does conversational *evangelism* rehearsal. This is Sent's
> signature Phase-2 differentiator: the bridge from "I learned the phrases" to "I can actually do this."

---

## Why it works
- **Safe reps.** The #1 reason people don't share their faith abroad is fear of freezing up. Roleplay
  gives unlimited low-stakes practice before it's a real person.
- **Compounding.** It exercises everything else in Sent at once — phrases, verses, the gospel steps,
  cultural sensitivity — in one integrated act.
- **Culturally specific.** A French skeptic objects differently than a Thai Buddhist. The persona +
  objection library is per-country, so the practice matches the actual field.

---

## Session flow
```
1. Choose a partner   → pick a RoleplayPersona (Camille easy → Luc/Amir hard) or "surprise me."
2. Choose a mode      → french | bilingual (FR + English gloss) | english (focus on content, not language)
3. Set the goal       → e.g. "have a warm conversation and share at least one part of the gospel."
4. Converse           → persona opens in character; user replies (typed, or voice → STT).
                         Persona stays in character, raises ≤1 objection at a time, reacts to warmth/pushiness.
                         Optional 💡 Hint button → the Coach gives a one-line nudge (doesn't break character).
5. End                → user shares the full gospel · the local "has to go" · or user taps End.
6. Debrief            → the Judge scores the conversation and returns specific, encouraging feedback,
                         objections handled, phrasing fixes, and "practice next" links. Award XP.
```

Each message is a `RoleplayTurn` (role = local | user | coach). The session is a `RoleplaySession`;
the debrief is a `RoleplayFeedback`.

---

## LLM architecture (3 roles, Base44 `InvokeLLM` / core integration)

### A. The Local (in-character partner) — one call per persona reply
**System prompt (assembled at runtime):**
```
You are roleplaying a real person so a Christian visitor can PRACTICE sharing their faith.
Stay fully in character as {persona.name}. Never break character or mention you are an AI.

CHARACTER: {persona.persona_prompt}
WORLDVIEW: {persona.worldview}
OPENNESS: {persona.openness}/100  (higher = more willing to engage and be moved)
COMMON OBJECTIONS you may raise (one at a time, only when natural): {persona.top_objections → Objection.prompt_fr + why_it_comes_up}

RULES:
- Reply in {mode == 'english' ? 'English' : 'natural French'} at a {difficulty}-appropriate level. In
  bilingual mode, add a short English gloss after your French on its own line prefixed "EN:".
- Keep replies to 1–3 sentences. React realistically to the visitor's TONE: warmth and good questions
  raise your openness; preachiness, arguing, or pressure lower it.
- Raise at most ONE objection per reply, and only if it fits the flow. Don't dump all your objections.
- You MAY soften or become curious over a genuinely good, humble conversation — but do NOT "convert"
  cheaply or instantly; a real heart-change is gradual and earned. If the visitor is pushy or robotic,
  get politely guarded.
- NEVER be vulgar, hateful, blasphemous, or disrespectful toward any religion. Skeptical and honest, yes;
  cruel or mocking, no. Portray people of other faiths with full dignity, never as stereotypes.
- Output strict JSON: {"reply_fr": "...", "reply_en": "...", "objection_tag": "<tag|null>",
  "openness_delta": <-15..+15>, "wants_to_end": <bool>}
```
**User message:** the running transcript (last ~12 turns) + the visitor's latest message.
Persist `openness` on the session so it carries across turns.

### B. The Coach (optional hint) — only when the user taps 💡
```
You are a kind evangelism coach. Given the conversation so far and that the visitor is stuck, give ONE
short, concrete next move (≤1 sentence). Don't script a paragraph; nudge. If a learned phrase or verse
fits, name it. Never break the roleplay; this is shown only to the visitor.
Output: {"hint": "..."}
```

### C. The Judge (debrief) — one call at session end
```
You are an encouraging evangelism mentor reviewing a practice conversation in {country}. Be warm,
specific, and honest. Score 0–100 on each axis; never shame.
Axes: clarity (was the gospel made clear?), warmth (listening, relational), objections (how well were
they handled?), cultural (sensitivity to {country} norms, e.g. laïcité/respect), language (French accuracy).
Use the seeded GospelStep/Verse/Phrase the visitor was learning as the bar for "clarity".
Output strict JSON matching RoleplayFeedback: {score_overall, score_clarity, score_warmth,
score_objections, score_cultural, score_language, summary, strengths[], growth[],
phrase_corrections[], practice_next[]}.  practice_next = ids of Phrase/Verse/Lesson to review.
```

---

## Difficulty & adaptive escalation
- **Difficulty** (on the persona) sets starting `openness`, objection depth, and language tolerance.
- **Adaptive within a session:** the Local's `openness_delta` moves a running openness score; high openness
  → warmer, more curious replies; low → guarded, more objections. This makes the same persona feel
  responsive without a separate difficulty knob.
- **Mode** decouples *language* difficulty from *conversation* difficulty so a French beginner can still
  practice the hard conversation (bilingual/english mode) and a fluent user can take Luc in full French.

## Scoring → XP & integration
- Completing a session awards XP (e.g. 40) + bonus for sharing the full gospel.
- `RoleplayFeedback.practice_next` feeds the **SM-2 review queue** (see DAILY_TRACK_LOGIC.md) — weak phrases
  resurface in tomorrow's set.
- A roleplay node can be added to the daily track in Phase 4 (Culture & Readiness) as the capstone rehearsal.

---

## Guardrails (this is a ministry app — non-negotiable)
- **Theology stays anchored.** The AI plays the *non-believer*, never a teacher of doctrine. Gospel content
  shown to the user comes from the seeded `GospelStep`/`Verse` data, not free-form model theology. The Judge
  praises clarity against that fixed gospel; it does not invent doctrine.
- **No cheap conversions.** The persona must not "get saved" in two messages — it trivializes real
  evangelism and trains false expectations. Heart-change is gradual and never guaranteed.
- **Dignity & anti-stereotype.** Personas of other faiths/backgrounds (e.g. Amir) are portrayed with full
  respect; explicit instruction against caricature. No hate, mockery, vulgarity, or blasphemy from any role.
- **Encouraging, never shaming.** Feedback names growth areas kindly; a nervous first-timer should leave
  motivated, not crushed.
- **It's practice, not people.** A persistent reminder in-UI: real people aren't NPCs; this builds courage
  and reps, it doesn't replace love and listening.
- **Privacy.** Sessions/turns/feedback are per-user with RLS (`created_by_id == {{user.id}}`); never shared.
- **Cost/latency.** Cap transcript context (~12 turns), keep replies short, cap session length
  (~15 turns) before nudging toward a wrap; one Judge call per session. Consider a cheaper model for the
  Local turns and a stronger model for the Judge.

---

## Data model (created + seeded in the Base44 app; mirrored in base44/entities/)
- **RoleplayPersona** — name, difficulty, openness, background, worldview, `persona_prompt`, avatar,
  `top_objections[]`, opening lines. *Seeded: Camille (easy), Théo, Sophie (medium), Luc, Amir (hard).*
- **Objection** — tag, label, `prompt_fr/en`, `why_it_comes_up`, `good_response_note`, `related_verse_ref`,
  difficulty. *Seeded: 8 France objections (not_religious, private_faith/laïcité, church_harm, science,
  all_religions_same, good_person, suffering, raised_catholic).*
- **RoleplaySession** — user, persona, mode, difficulty, status, outcome, turn_count, xp, score_overall.
- **RoleplayTurn** — session, order, role (local/user/coach), text_fr, text_en, objection_tag.
- **RoleplayFeedback** — session, per-axis scores, summary, strengths[], growth[], phrase_corrections[],
  practice_next[].

## Build order (when Base44 credits return)
1. Persona picker + mode/goal screen (reads RoleplayPersona).
2. Chat UI + the **Local** `InvokeLLM` loop persisting RoleplayTurn and running openness.
3. The 💡 **Coach** hint call.
4. End → **Judge** call → RoleplayFeedback scorecard screen → XP + practice_next into SM-2.
5. Voice (STT for user input, TTS for the local's French) — optional polish.

## Phase 3+ ideas
- Voice-to-voice (speak and hear the local reply in French).
- "Replay with coaching" — step back through the transcript with per-turn suggestions.
- Team mode — compare scorecards; a leader sees who's ready.
- Persona packs per country as Sent expands beyond France.
