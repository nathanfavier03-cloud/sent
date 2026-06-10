# Daily Track Logic

How the fixed curriculum (the France pack) maps onto however many days a user has before departure.
The journey always moves through the same **5 phases**; they stretch or compress to fit the runway.

```
Foundations → Language Core → Heart & Verses → Culture & Readiness → Sending
   ~25%            ~35%             ~20%              ~15%             last day
```

## Phases
| Phase | Unlocks | Job |
|---|---|---|
| 1 · Foundations | Country briefing, "the gospel you carry," first word *Bonjour*, first prayer | "I understand where I'm going." |
| 2 · Language Core | Evangelism phrases in usefulness-rank order + audio + review | "I can open my mouth." |
| 3 · Heart & Verses | Choose-your-own French verses, memorization, deeper prayer | "I carry the Word in their tongue." |
| 4 · Culture & Readiness | Cultural Knowings, teaching module, checklist push | "I won't offend; I'm packed." |
| 5 · Sending | Final review + commissioning | "I'm ready. Go." |

## Generation algorithm
```
daysLeft   = departureDate − today            (or "open" if no trip)
trackLen   = min(daysLeft, 30)                // cap so 6-month trips aren't 180 lessons
pool       = lessons sorted by priority(1→3), then phase order
dailyCap   = 3 content lessons + 1 prayer prompt   // Duolingo-bite-sized
perDay     = clamp(ceil(pool.length / max(1, min(daysLeft || 21, 30))), 1, 4)

if daysLeft >= pool.length:        SPREAD   → 1–2 lessons/day + review + daily prayer
if daysLeft <  essentials.length:  COMPRESS → drop priority-3, double essentials, "tight runway" banner

todaySet = first `perDay` lessons not yet completed   // progress-driven, not calendar-locked
Day 1 leads with the briefing; final day is the commission.
```

## Runway modes
- **Express (< 7 days):** essentials only (priority 1) — briefing, top phrases, key verses, core do's/don'ts, commissioning.
- **Standard (7–30 days):** full arc, ~2 lessons/day, spaced verse review, daily prayer.
- **No trip planned:** self-paced, no countdown pressure; suggested 21-day default.

## XP & streak
Phrase 10 · Verse 15 · Culture 15 · Teaching 20 · Briefing 25 · Prayer 5 · Commission 50.
Completing a full day → streak +1. Miss a day → streak resets, track persists, overdue lessons surface.

## Departure back-planning (the differentiator)
The trip date drives everything. Set a **target ready date** a few days before departure so the user is
prepared with margin, then pace toward it:
```
targetReady = departureDate − 3 days          // finish with buffer, not at the gate
plannedPerDay = ceil(remainingLessons / max(1, businessDaysUntil(targetReady)))
expectedDoneByToday = lessonsScheduledUpToToday(targetReady)
paceStatus = completed >= expectedDoneByToday ? "on_pace"
           : completed >= expectedDoneByToday - plannedPerDay ? "slightly_behind"
           : "behind"
```
Home screen surfaces it: *"12 days left · you're on pace"* or *"behind — 3 lessons to catch up."*
Stored on `TripPlan` as `target_ready_date`, `daily_minutes_cap` (default 5), `pace_status`.

## Spaced repetition (SM-2) for phrases & verses
Each phrase/verse the user has learned becomes a review item with an SM-2 record on `Progress`.
On review, the user self-rates recall **q (0–5)**; update:
```
if q < 3:                      // forgotten — relearn
  repetitions = 0
  interval = 1
  lapses += 1
else:
  if repetitions == 0: interval = 1
  elif repetitions == 1: interval = 6
  else: interval = round(interval * ef)
  repetitions += 1
ef = max(1.3, ef + (0.1 − (5 − q) * (0.08 + (5 − q) * 0.02)))   // ease factor
due_date = today + interval days
```
Items whose `due_date <= today` are folded into the day's set as **review nodes** (not a separate screen),
capped so the daily session still fits `daily_minutes_cap`. New material is throttled when reviews pile up.

## Retention mechanics
- **Streak** +1 on completing the day's set. **Streak-freeze** (`User.streak_freezes`, default 2) auto-spends
  to protect the streak on one missed day before it resets (Duolingo/Drops loss-aversion).
- **~5-min cap** (`User.daily_goal_minutes`) keeps each day finishable — scarcity makes it a treat, not a chore.

## Daily home screen also shows
- The **pace status** (above) and the countdown.
- One **daily "Pray for France" card** (`PrayerPrompt`, rotated by `order`).

## Completion
Finishing every lesson lights up France on the globe and unlocks the
**"Préparé(e) et envoyé(e) en France"** commissioning (Jean 20:21).
