# Sent — European Language & Nation Coverage

Build-out of Sent across Europe. Each language/nation is **data** in the Base44 app (no UI rebuild needed):
`VerseTranslation` (Romans Road + John 3:16 in a **public-domain** Bible), `Language`, `Country` (verified
briefing), a `Phrase` starter pack (8 evangelism phrases + pronunciation), and a culturally-grounded
`RoleplayPersona` for the AI roleplay.

> **Where the data lives:** the entity **schemas** live in this repo (`base44/entities/*.jsonc`); the seeded
> **records** are in the Base44 app (system of record) and sync to GitHub via Base44's native "Push to
> GitHub". This doc is the human-readable source of truth for coverage + sourcing.

## Publish status (the `status` field on Language & Country)
The app shows only `status: "published"` records and hides `status: "hold"`. As of the 2026-06-11 publish:
**23 languages + 19 nations published; 7 languages + 11 nations on hold.**

> ⚠️ **Launch caveat (applies to ALL languages):** verse text was assembled by research agents but could
> **not be live-verified** (WebFetch was blocked the entire session). Treat all multilingual Scripture as
> needing a **native-speaker / published-source proof before any public launch.** The "hold" set below are
> the items with *additional* specific problems on top of that.

---

## ✅ PUBLISHED

### Nations (19) — verified briefings (Pew · Operation World · Joshua Project · Open Doors · national census)
🇫🇷 France *(flagship — full daily track)* · 🇪🇸 Spain · 🇵🇹 Portugal · 🇮🇹 Italy · 🇬🇷 Greece · 🇩🇪 Germany ·
🇳🇱 Netherlands · 🇵🇱 Poland · 🇨🇿 Czechia · 🇸🇪 Sweden · 🇳🇴 Norway · 🇩🇰 Denmark · 🇫🇮 Finland · 🇪🇪 Estonia ·
🇱🇻 Latvia · 🇸🇰 Slovakia · 🇭🇺 Hungary · 🇷🇴 Romania · 🇷🇺 Russia *(legal_to_evangelize = false — Yarovaya
anti-missionary law; WWL score 63; briefing shows the restriction warning)*

### Languages (23) — public-domain Bible used
| Lang | Code | Translation | Lang | Code | Translation |
|---|---|---|---|---|---|
| French | fr | Louis Segond | Finnish | fi | Pyhä Raamattu 1933/38 |
| English | en | KJV | Estonian | et | Piibel 1739/1938 |
| Spanish | es | Reina-Valera Antigua | Latvian | lv | Glück |
| Portuguese | pt | João Ferreira de Almeida | Slovak | sk | Roháček 1936 |
| German | de | Luther 1912 | Italian | it | Diodati 1649 |
| Dutch | nl | Statenvertaling | Romanian | ro | Cornilescu |
| Swedish | sv | Svenska 1917 | Polish | pl | Biblia Gdańska |
| Norwegian | no | Norsk 1930 | Czech | cs | Bible kralická |
| Danish | da | Dansk 1931 | Hungarian | hu | Károli |
| Greek | el | Vamvas 1850 | Irish | ga | An Bíobla Naomhtha (Bedell) |
| Icelandic | is | Íslenska Biblían 1912 | Albanian | sq | Kristoforidhi |
| Bosnian | bs | Daničić-Karadžić 1865 (Latin) | | | |

**The Romans Road (Rom 3:23, 6:23, 5:8, 10:9, 10:13 + John 3:16) resolves in all 23 published languages**
via the `VerseTranslation` store + language toggle on the gospel-framework screens.

---

## ⏸️ ON HOLD (hidden in the app; flip live with no rework once resolved)

### Languages (7)
| Lang | Code | Why held |
|---|---|---|
| Croatian | hr | Šarić translation — public-domain status only "probable, not certain" |
| Lithuanian | lt | PD edition uncertain; wording not confirmed |
| Ukrainian | uk | Kulish 1903 — archaic orthography, unconfirmed verbatim |
| Serbian | sr | Karadžić NT 1847 — corroborated via search only, not fetched |
| Slovene | sl | Chráska 1914 — from memory, unconfirmed |
| Bulgarian | bg | Protestant 1940 — from memory, unconfirmed |
| Maltese | mt | **No public-domain verses** — PD Maltese only covers the Gospels, not Romans (full text is copyrighted). Language + phrases + persona seeded; verses intentionally omitted. |

### Nations (11)
Held because their language is on hold **or** their briefing is still pending:
🇭🇷 Croatia · 🇱🇹 Lithuania · 🇺🇦 Ukraine · 🇷🇸 Serbia · 🇸🇮 Slovenia · 🇧🇬 Bulgaria · 🇲🇹 Malta *(language held)* ·
🇮🇪 Ireland · 🇮🇸 Iceland · 🇦🇱 Albania · 🇧🇦 Bosnia & Herzegovina *(briefing still "BRIEFING PENDING")*

### Not seeded — no usable public-domain Bible
- **Macedonian (mk):** all translations are post-1959 and copyrighted; no PD edition exists.
- **Belarusian (be):** a PD option exists (Dziekuć-Malej LDNT 1931) but could not be captured verbatim (web blocked).

---

## To release held items
1. **Verify the held-language verses** against a printed/PD source (native speaker), then set `status: "published"` on the Language + its Country.
2. **Write briefings** for Ireland, Iceland, Albania, Bosnia (currently stubbed), then publish those nations.
3. **Decide Maltese / Macedonian / Belarusian** — accept a copyrighted edition with attribution/permission, or leave verse-less.
4. Country briefings for the 5 Wave-3 nations are still pending.

## Sourcing
Public-domain editions only. Verse text from Bible Gateway, bible.com, BibleHub, StudyLight, ebible.org,
sacred-texts, wordproject, and equivalent PD hosts. Briefing stats from Pew Research, Operation World,
Joshua Project, Open Doors World Watch List, US State Dept IRF reports, and national censuses (cited per
country in each `Country.sources`).
