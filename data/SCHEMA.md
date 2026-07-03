# Schema datasource IRIS

Documentează fișierele de date din `data/` și relațiile dintre ele.
Toate datele derivă dintr-o singură sursă: CSV-ul din `data/source/`, convertit cu
`tools/csv-to-json.html`. Vezi `data/source/SCHEMA.md` pentru schema CSV-ului brut.

## Fișiere

| Fișier | Rol | Formă |
|---|---|---|
| `chapters.json` | Capitole (nivel 1) | listă plată |
| `situations.json` | Situații clinice (nivel 3) | listă plată |
| `recommendations.json` | Recomandări de investigații (nivel 4) | listă plată |
| `guidelines.json` | Aceleași date, arbore complet imbricat | ierarhie |
| `legend.json` | Dicționare de coduri (modalitate, indicație, grad, doză) | dicționar |
| `iris-data.js` | Bundle runtime încărcat de aplicație (`window.IRIS`) | generat |
| `source/` | CSV sursă + schema lui (`SCHEMA.md`) | sursă |
| `CHANGELOG-datasource.md` | Corecții aplicate manual peste datele auto-generate | jurnal |

> `chapters.json`, `situations.json`, `recommendations.json` și `guidelines.json` sunt
> **redundante** — aceleași date în două forme (plată normalizată vs. arbore imbricat).
> Aplicația la runtime citește doar `iris-data.js`; celelalte sunt pentru inspecție și
> re-generare.

## Ierarhia

```
Capitol (chapter)
└── Subcapitol (subchapter)
    └── Situație clinică (situation)
        └── Recomandare (recommendation)
```

Subcapitolele nu au fișier plat propriu — apar în `iris-data.js` (`IRIS.subchapters`)
și în arborele din `guidelines.json`.

## Convenția de ID-uri

ID-urile codează calea completă, cu `__` ca separator de nivel:

```
c-pediatrie                                              (capitol)
c-pediatrie__s-aparat-digestiv                           (subcapitol)
c-pediatrie__s-aparat-digestiv__q-apendicita             (situație)
r-3                                                      (recomandare, id secvențial global)
```

- prefix `c-` capitol · `s-` subcapitol · `q-` situație · `r-` recomandare
- segmentul de situație/subcapitol e un slug diacritice-free al numelui
- fiecare id de nivel inferior conține ca prefix id-ul părintelui (cu excepția `r-*`,
  care leagă în schimb prin `situationId`)

## Câmpuri per entitate

### chapters.json — `[{…}]`
| Câmp | Tip | Note |
|---|---|---|
| `id` | string | `c-*` |
| `name` | string | nume afișat |
| `count` | number | nr. situații în capitol |

### situations.json — `[{…}]`
| Câmp | Tip | Note |
|---|---|---|
| `id` | string | `c-*__s-*__q-*` |
| `name` | string | numele situației clinice |
| `subchapterId` | string | → subcapitol |
| `chapterId` | string | → capitol |
| `count` | number | nr. recomandări |
| `placeholder` | boolean | situație fără conținut real |

### recommendations.json — `[{…}]`
| Câmp | Tip | Note |
|---|---|---|
| `id` | string | `r-*` |
| `situationId` | string | → situație |
| `nr` | number | ordine în cadrul situației |
| `tip` | string | cod modalitate (o cheie din `legend.modality.values`) |
| `exam` | string | denumirea examinării |
| `indication` | string | cheie din `legend.indication.values` |
| `grade` | string | `A` \| `B` \| `C` (cheie din `legend.grade.values`) sau gol |
| `comments` | string | observații clinice |
| `otherInfo` | string | informații suplimentare (poate fi gol) |
| `doseMin` | number | nivel doză minim (0–4, index în `legend.dose.scale`) |
| `doseMax` | number | nivel doză maxim (0–4) |
| `placeholder` | boolean | recomandare fără conținut real |

### guidelines.json — arbore
Capitole → `subchapters[]` → `situations[]` → `recommendations[]`.
Nodurile de situație/recomandare au aceleași câmpuri ca fișierele plate, minus cheile
străine redundante (`chapterId`, `situationId`) — legătura e dată de imbricare.

### legend.json — dicționare de coduri
Patru grupuri, folosite pentru a decoda codurile din recomandări:

- **`modality`** — cheie = `recommendation.tip`; valori cu `{name, short, desc}`
  (ex. `G`=Radiografie, `E`=Ecografie, `T`=CT, `M`=IRM, `N`=Medicină nucleară…).
- **`indication`** — cheie = `recommendation.indication`; valori cu `{tier, tone, desc}`
  (`tone`: `go` \| `caution` \| `stop` \| `info`).
- **`grade`** — cheie = `recommendation.grade`; valori cu `{desc}`.
- **`dose`** — `scale[]` indexat 0–4; `doseMin`/`doseMax` sunt indici în acest tablou;
  fiecare treaptă are `{level, band, note, est}`.

## Relații (chei străine)

```
recommendations.situationId  →  situations.id
situations.subchapterId      →  subchapters.id   (subchapters doar în iris-data.js)
situations.chapterId         →  chapters.id
subchapters.chapterId        →  chapters.id
recommendations.tip          →  legend.modality.values[key]
recommendations.indication   →  legend.indication.values[key]
recommendations.grade        →  legend.grade.values[key]
recommendations.doseMin/Max  →  legend.dose.scale[index]
```

## Regenerare

Nu edita `iris-data.js` sau JSON-urile plate manual. Modifică CSV-ul din `data/source/`
și re-generează cu `tools/csv-to-json.html`. Corecțiile aplicate direct peste ieșire se
consemnează în `CHANGELOG-datasource.md`.
