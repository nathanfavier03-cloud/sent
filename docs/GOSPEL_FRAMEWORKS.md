# Sent — Gospel Frameworks, Multilingual Verses & Teaching Library

Reusable, **language-agnostic** evangelism content that works across countries (not tied to France).
All Scripture uses **public-domain** translations to avoid copyright issues.

## Data model
- **GospelFramework** — a presentation method (slug, name, summary, origin, `when_to_use`, `audience_tags`, `key_verses`, source).
- **GospelFrameworkStep** — ordered steps per framework (title, teaching_point, `scripture_ref`, key_idea). Verse *text* is not stored here — it's looked up from VerseTranslation by `scripture_ref` + the user's language.
- **VerseTranslation** — one row per **reference × language × translation**: the multilingual verse store. This is what makes "Romans Road in different languages" work. Translations: **KJV** (en), **Louis Segond / LSG** (fr), **Reina-Valera Antigua / RVA** (es), **João Ferreira de Almeida / JFA** (pt) — all public domain.

A presentation screen renders: framework → steps in order → for each step, the verse text in the user's
chosen language (toggle EN/FR/ES/PT), plus the teaching point and a "how to say it" key idea.

---

## The Romans Road (seeded, 4 languages)
Sequence: **3:23 → 6:23 → 5:8 → 10:9 → 10:13 → 8:1** (with 3:10 reinforcing step 1 and 5:1 reinforcing
assurance). Verse text seeded in KJV / LSG / RVA / JFA for: Romans 3:23, 3:10, 6:23, 5:8, 10:9, 10:13, 5:1, 8:1.

| Step | Verse | Point |
|---|---|---|
| 1 · All have sinned | Romans 3:23 (+3:10) | Universal need — no one is righteous on their own |
| 2 · Sin earns death | Romans 6:23 | The wage is death; the gift is eternal life in Jesus |
| 3 · God proved His love | Romans 5:8 | Christ died for us while we were still sinners — grace, not earning |
| 4 · Confess & believe | Romans 10:9 | The response: heart-belief + mouth-confession |
| 5 · Whosoever calls | Romans 10:13 | The invitation is open to anyone |
| 6 · Peace & no condemnation | Romans 8:1 (+5:1) | Assurance for the new believer |

> ⚠️ **Textual-variant note (verify before launch):** Romans 8:1's ending — "...who walk not after the
> flesh, but after the Spirit" — is present in the **Textus Receptus** tradition (KJV, RVA) but omitted in
> many critical-text editions (the LSG/JFA seeded here use the shorter reading). If you want all four
> languages to match exactly, standardize on one textual tradition.

---

## The six frameworks (seeded) — and when to reach for each
| Framework | Best for | Steps |
|---|---|---|
| **Romans Road** | Scripture-anchored, sequential; someone who'll follow verses in order | 6 |
| **Four Spiritual Laws / Knowing God Personally** (Cru) | Literate, intellectual, campus/Western | 4 |
| **The Bridge to Life** (Navigators) | Visual & reproducible — draw it on a napkin; any literacy | 5 |
| **Three Circles** (NAMB) | Conversational; **post-Christian / secular** people who feel brokenness | 3 |
| **The ABCs of Salvation** | Children, new believers, quick/simple; a summary at the invitation | 3 |
| **1-Verse Evangelism** (Romans 6:23) | Oral / low-literacy / restricted-access fields; one verse to carry | 5 |

A "choose your tool" screen can filter by `audience_tags` (oral · simple · intellectual · secular · visual …),
so a missionary picks the right method for the person in front of them. *(France's Luc → Four Spiritual
Laws or honest apologetics; a secular skeptic → Three Circles; a restricted-access field → 1-Verse.)*

---

## Teaching library (12 deeper lessons, seeded as Lesson `track_code: "core"`)
Universal gospel-sharing skills, reusable across every country track:
1. What the Gospel Actually Is (and common distortions)
2. Sharing Your Testimony (Before / How / After)
3. Transitioning a Conversation to the Gospel
4. Asking Good Spiritual Questions
5. Handling Rejection and Fear
6. Prayer and the Holy Spirit in Evangelism
7. Giving an Invitation to Respond
8. Praying With Someone to Receive Christ
9. Follow-up and Basic Discipleship
10. Cross-Cultural Gospel Communication
11. Assurance of Salvation
12. The Readiness of the Witness

### "core" vs country tracks
Lessons with `track_code: "core"` are **universal** and language-agnostic. A user's full track =
**core teaching lessons ∪ the country track** (e.g. `fr-FR`). This keeps the deep teaching reusable as
Sent adds countries, while country tracks hold the language/culture/briefing specifics.

## New memory verses (seeded as Verse, fr/en)
Beyond the existing set: Acts 4:12, Ephesians 2:8-9, 2 Corinthians 5:17, Revelation 3:20, 1 John 1:9,
John 1:12, Romans 10:17 — each with its one-line witnessing use.

## Sources
KJV/LSG/RVA via [Bible Gateway](https://www.biblegateway.com); JFA via [bibliaportugues.com](https://bibliaportugues.com/jfa/);
frameworks: [Cru](https://www.cru.org/us/en/how-to-know-god/would-you-like-to-know-god-personally.html),
[Navigators](https://www.navigators.org/resource/the-bridge-to-life/), [NAMB 3 Circles](https://www.namb.net/evangelism/3circles/).
