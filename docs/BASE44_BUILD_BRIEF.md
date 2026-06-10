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

## Out of scope for Phase 1 (deferred)
- **Phase 2:** conversation simulator (LLM roleplay), pronunciation feedback, teams/leaderboard,
  field journal, shareable commissioning certificate.
- **Phase 3:** full offline, multi-church/org admin accounts, live data feeds
  (Joshua Project / Open Doors / Operation World).

## Pickup
When credits return: build this in Base44 against the existing seeded entities, then connect the app
to the GitHub repo (Push to GitHub) — same flow as nlc-outreach. UI/UX designed fresh in Base44.
