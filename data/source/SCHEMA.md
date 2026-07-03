# Ghid indicații radioimagistice — schema datelor

Sursa: `data/source/Ghid indicatii radioimagistice.csv` (1655 rânduri de date, 12 coloane).

## Coloane CSV → câmpuri
| CSV | Câmp JSON | Note |
|---|---|---|
| NR.CRT | `nr` | număr de ordine în ghid |
| Capitol | → `chapters` | 14 capitole |
| Subcapitol | → `subchapters` | grupate pe capitol (deduplicate) |
| Situația Clinică | → `situations` | 796 situații (701 reale + 95 marcaje) |
| Tip | `tip` | cod modalitate (vezi `legend.modality`) |
| Examen | `exam` | denumirea examinării |
| Indicație | `indication` | statut recomandare (7 valori, vezi `legend.indication`) |
| Grad Indicație | `grade` | A / B / C nivel dovezi (poate fi gol) |
| Comentarii | `comments` | ~92% completate |
| Alte informații | `otherInfo` | ~29% completate |
| Doza Min / Doza Max | `doseMin` / `doseMax` | scală 0–4 (vezi `legend.dose`) |

## Model relațional (normalizat)
```
chapters (1) ──< subchapters (n) ──< situations (n) ──< recommendations (n)
```

**chapters.json** — `[{ id, name, count }]`
**subchapters.json** — `[{ id, name, chapterId, count }]`
**situations.json** — `[{ id, name, subchapterId, chapterId, placeholder }]`
**recommendations.json** — `[{ id, situationId, nr, tip, exam, indication, grade, comments, otherInfo, doseMin, doseMax, placeholder }]`

**guidelines.json** — aceleași date în formă imbricată (chapter → subchapters → situations → recommendations), pentru încărcare offline într-un singur fetch.

**legend.json** — dicționar de coduri: `modality`, `indication` (cu `tier` 0–5 + `tone`), `grade`, `dose` (bandă + comparație cu fondul natural de radiație, scală 0–4 conform ghidului).

**synonyms.json** — dicționar de intenție: `[{ terms:[...], expand:[...] }]`. Termeni lay / EN / latină → cuvinte-cheie românești pentru căutare non-specifică (ex. „cranium perimeter" → „perimetru cranian").

## Curățare aplicată la conversie
- **Diacritice unificate**: `ş→ș`, `ţ→ț` (cedilă → virgulă-jos).
- **Deduplicare subcapitole** prin cheie canonică (fără diacritice, `si/și` unificat, punctuație colapsată). Ex. `Bazin şi sacru` = `Bazin și sacru`, `Tumori cerebrale si medulare` = `… și medulare`.
- **Marcaje non-conținut** (`ALTĂ SITUAȚIE CLINICĂ`, `Refaceti selectia…`, `Situație clinică neîncadrabilă`) → `placeholder: true`, excluse din numărători și navigare.
- ID-uri stabile derivate din slug (ex. `c-cancer__s-limfom__q-...`).

## ID-uri
- Capitol: `c-<slug>`
- Subcapitol: `<chapterId>__s-<slug>`
- Situație: `<subchapterId>__q-<slug>`
- Recomandare: `r-<n>`

## Reconversie
Deschide `tools/csv-to-json.html` în browser, încarcă un CSV cu aceleași coloane și descarcă pachetul JSON regenerat.
