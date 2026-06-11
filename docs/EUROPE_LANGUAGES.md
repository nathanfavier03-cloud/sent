# Sent — European Language Coverage

Tracking the build-out of Sent across the languages of Europe. Each language gets, as **data** in the
Base44 app (no UI rebuild required): `VerseTranslation` (Romans Road + John 3:16 in a **public-domain**
Bible), `Language`, `Country`, a `Phrase` starter pack (8 evangelism phrases + pronunciation), and a
culturally-grounded `RoleplayPersona` for the AI roleplay.

> ⚠️ **Verification caveat:** verse text was pulled from public-domain sources by research agents and
> spot-checked, but **multilingual Scripture should get a native-speaker / published-source pass before
> any public launch.** Country briefings for the new languages are stubbed ("BRIEFING PENDING") — verified
> stats + prayer points are a separate dedicated pass.

## Status

| Language | Code | Country | PD Bible | Verses | Phrases | Persona | Briefing |
|---|---|---|---|---|---|---|---|
| French | fr | France | Louis Segond | ✅ | ✅ (15) | ✅ ×5 + objections | ✅ verified |
| English | en | — | KJV | ✅ | (UI lang) | — | — |
| Spanish | es | Spain | Reina-Valera Antigua | ✅ | ✅ | ✅ Mateo | ⏳ pending |
| Portuguese | pt | Portugal | Almeida (JFA) | ✅ | ✅ | ✅ João | ⏳ pending |
| Italian | it | Italy | Diodati 1649 | ✅ | ✅ | ✅ Marco | ⏳ pending |
| Romanian | ro | Romania | Cornilescu | ✅ | ✅ | ✅ Andrei | ⏳ pending |
| Polish | pl | Poland | Biblia Gdańska | ✅ | ✅ | ✅ Katarzyna | ⏳ pending |
| Czech | cs | Czechia | Bible kralická | ✅ | ✅ | ✅ Tomáš | ⏳ pending |
| Croatian | hr | Croatia | Croatian (Šarić)¹ | ✅ | ✅ | ✅ Ivana | ⏳ pending |
| Greek | el | Greece | Vamvas 1850 | ✅ | ✅ | ✅ Dimitris | ⏳ pending |
| Hungarian | hu | Hungary | Károli | ✅ | ✅ | ✅ Eszter | ⏳ pending |

**¹ Croatian:** the cleanest fully-online source is the Šarić-tradition text distributed as public domain by
CCEL/StudyLight. If a verifiably pre-1929 PD Croatian text is required, re-pull from an older Daničić-era source.

## Pending waves (queued)
- **Wave 2:** German, Dutch, Danish, Swedish, Norwegian *(agents hit a session limit mid-run — re-run when reset)* · Finnish, Estonian, Latvian, Lithuanian · Bulgarian, Slovak, Slovene · Ukrainian, Russian, Serbian.
- **Wave 3:** Irish, Maltese, Icelandic, Albanian, Macedonian, Belarusian, Bosnian.

## How verses scale
The `VerseTranslation` store is keyed by **reference × language**, so the same Romans Road framework
renders in any seeded language via a language toggle — no new framework records needed per language.
Currently the Romans Road (Rom 3:23, 6:23, 5:8, 10:9, 10:13 + John 3:16, plus 3:10/5:1/8:1 for EN/FR/ES/PT)
resolves in: **en, fr, es, pt, it, ro, pl, cs, hr, el, hu** (11 languages).

## Translations & sourcing
Public-domain editions only (KJV, Louis Segond, Reina-Valera Antigua, Almeida, Diodati 1649, Cornilescu,
Biblia Gdańska, Bible kralická, Croatian Šarić, Vamvas 1850, Károli). Verse text from Bible Gateway,
bible.com, BibleHub, StudyLight, and equivalent public-domain hosts.
