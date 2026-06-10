# Sent — Base44 Build Brief (Phase 1 MVP)

**Purpose:** capture *all the ideas* — features, flows, data, content — so Base44 can build the
Phase 1 app. **The UI/UX is greenfield in Base44** — this brief is functional, not a visual spec.
The `standalone/` PWA is a *reference implementation of the ideas and flows only*; do not treat its
styling or layout as prescriptive.

> Brand intent stays the same: **Favier's Lighthouse**, "light to the nations." Warm, hopeful,
> mobile-first, iPhone-optimized (iOS safe areas), light + dark mode. Exact look is Base44's call.

---

## The product in one line
A "Duolingo for missionaries" — short, daily, gamified pre-field training so a short-term team member
is **equipped enough to go** (not seminary) before a trip. Starter track: 🇫🇷 France → French.

## Hero user
**Short-term team member** with a known destination + departure date. The app's spine is the
**departure countdown** that generates a back-counted daily training track.

---

## Screens / features (build all; UI is open)
1. **Onboarding (trip-first):** choose language (French) → destination (France) → departure date OR
   "no trip planned yet" → generate the daily track ("You leave for France in 21 days").
2. **Today / Home:** countdown, today's lesson set (sized to the runway), XP + streak.
3. **Country briefing — "Know Before You Go":** spiritual stats, legality, persecution, spiritual
   history, framing, and prayer points (daily prayer prompts).
4. **Language lessons:** evangelism-ranked **phrase** flashcards (English + French + pronunciation +
   audio) and choose-your-own **verses** (French + English + audio).
5. **Cultural training — "Cultural Knowings":** do's & don'ts, dress, greetings, etiquette.
6. **Teaching modules:** short Gospel-sharing lessons the user picks from.
7. **Gamification:** XP per lesson, daily streaks, progress tracking.
8. **Interactive globe:** completing the track drops a pin / lights up the nation.
9. **Pre-departure checklist:** vaccines, visa, packing, emergency contacts, embassy — checkable.
10. **Light commissioning:** finishing the track → "Préparé(e) et envoyé(e) en France" (Jean 20:21).

## Daily track logic (see DAILY_TRACK_LOGIC.md)
5 phases — Foundations → Language Core → Heart & Verses → Culture & Readiness → Sending — mapped
across days-until-departure. `perDay = clamp(ceil(total / min(daysLeft||21, 30)), 1, 4)`.
`<7 days` = "tight runway," priority-1 essentials only. Today's set = next `perDay` uncompleted
lessons. Day 1 leads with the briefing; final day is the commission.

## Audio
Phrases and verses need audio playback. The standalone uses browser `SpeechSynthesis` (`fr-FR`) as a
zero-cost stand-in; Base44 can use TTS or recorded native audio (the `audio_url` field exists on
Phrase/Verse for recorded files).

---

## Data model (already created + seeded in the Base44 app)
Entities live in the app and mirror `base44/entities/*.jsonc`:
**Country, Language, Phrase, Verse, Lesson, ChecklistItem, TripPlan, Progress, User.**
France content is seeded: 1 Country (verified stats), 1 Language, 15 Phrases, 5 Verses,
10 ChecklistItems, 8 narrative Lessons (`track_code: fr-FR`). See **FRANCE_CONTENT_PACK.md** for the
full, sourced content and **the verified stats + sources**.

> ⚠️ Spiritual/legal/persecution data is trust-critical — keep it sourced and human-verified before launch.

---

## Greenlit enhancements (research-driven, 2026-06-09)
From a competitive scan (GodTools, YouVersion, Hallow, Duolingo, Memrise, Pimsleur, Joshua Project,
Open Doors, CultureMee, IMB/YWAM). The differentiator: **Sent is pre-field and deadline-driven** —
no competitor combines a trip deadline with a back-planned curriculum.

**Build into Phase 1:**
1. **Departure back-planning** *(core differentiator)* — from the trip date, set `TripPlan.target_ready_date`
   ≈ departure − 3 days and pace the track to finish by then. Home shows a **pace status** ("on pace" /
   "behind") so the countdown drives the whole loop. (`TripPlan.target_ready_date`, `pace_status`, `daily_minutes_cap`.)
2. **Spaced repetition (SM-2)** — phrases/verses get review nodes surfaced *just before forgetting*, baked
   into the daily path (not a separate chore). Fields on `Progress`: `ef` (ease, default 2.5),
   `interval_days`, `repetitions`, `due_date`, `lapses`, `quality_last`. Algorithm in DAILY_TRACK_LOGIC.md.
3. **Interactive Gospel presentation** (GodTools-style) — a swipeable French/English **parallel** gospel
   walk-through the user can actually present. Entity `GospelStep` (5 seeded steps for France: God's love →
   sin → Jesus → receive → prayer), each with `scripture_fr/en`, `body_fr/en`, `guiding_question_fr/en`.
4. **Native audio + "say it out loud"** — prefer recorded native-speaker audio (`audio_native_url` on
   Phrase/Verse; TTS `fr-FR` as fallback). Add a Pimsleur-style active-recall mode: prompt in English →
   user says the French aloud (mic optional) → reveal.
5. **Streak-freeze + ~5-min cap** — `User.streak_freezes` (default 2) protects the streak on a missed day;
   `User.daily_goal_minutes` (default 5) keeps each session finishable (Drops/Duolingo loss-aversion).
6. **Culture as scenario cards** — `Scenario` entity (5 seeded for France: greeting, dining, faith-talk,
   public, conversation), each a situation + do/don't + tip. Replaces bullet-list culture tips.
7. **Daily "Pray for France" card** — `PrayerPrompt` entity (7 seeded), one dated prayer surfaced on the
   home screen each day (Unreached-of-the-Day ritual).
8. **Joshua Project–sourced briefing** — the Country briefing should cite **Joshua Project / Operation World**
   for France's % evangelical, primary religion, and top unreached groups (sourced, not hand-wavy).

## Deep teaching content (research-driven, 2026-06-10) — see GOSPEL_FRAMEWORKS.md
Reusable, language-agnostic evangelism content, all in **public-domain** translations:
- **6 gospel-presentation frameworks** (Romans Road, Four Spiritual Laws, Bridge to Life, Three Circles,
  ABCs, 1-Verse) as `GospelFramework` + `GospelFrameworkStep`, each tagged with *when to use it* so a
  missionary picks the right tool for the person (oral/simple vs intellectual vs secular/broken).
- **Multilingual verse store** `VerseTranslation` — the "Romans Road in different languages": one row per
  reference × language (KJV/en, LSG/fr, RVA/es, JFA/pt). Framework steps reference verses by `scripture_ref`
  and render text in the user's chosen language. *(Note the Romans 8:1 textual-variant flag in the doc.)*
- **12 deeper teaching lessons** seeded as `Lesson` with **`track_code: "core"`** (testimony, transitions,
  asking questions, handling rejection, the Spirit's role, invitation, follow-up, cross-cultural comms, …).
- **Track composition:** a user's full track = **core teaching lessons ∪ the country track** (`fr-FR`).
  `core` is universal/reusable; country tracks hold language/culture/briefing specifics. Build the app to
  merge them.

## Monetization (decision: free / donation)
**Sent is free for everyone, ministry/donor-funded** (the YouVersion model) — maximizes reach and fits the
reality that team members are already fundraising. No paywalls on the France track or any core feature.
Optional later: a non-blocking "support this ministry" / donate prompt, and faith-tech grants/hackathons
(Gloo, Indigitous "AI & the Church") for build-out funding. *(Per-trip church licensing was considered and
declined in favor of free access.)*

## Out of scope for Phase 1 (deferred)
- **Phase 2:** **AI gospel-conversation roleplay** — fully spec'd in **AI_ROLEPLAY.md** (data model +
  France personas/objections already seeded; build deferred to Phase 2). Plus: pronunciation feedback,
  teams/leaderboard, field journal, shareable commissioning certificate.
- **Phase 3:** full offline, multi-church/org admin accounts, live data feeds
  (Joshua Project / Open Doors / Operation World).

## Pickup
When credits return: build this in Base44 against the existing seeded entities, then connect the app
to the GitHub repo (Push to GitHub) — same flow as nlc-outreach. UI/UX designed fresh in Base44.
