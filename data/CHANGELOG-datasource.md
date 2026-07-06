# Corecții datasource

> **Status (2026-07-03):** corecțiile documentate mai jos au fost aplicate **direct în CSV-ul sursă** (`data/source/Ghid indicatii radioimagistice.csv`), care este de acum baza canonică. Acest fișier rămâne ca **jurnal de audit** — nu se re-aplică nimic peste CSV. Vezi secțiunea „Merge în CSV-ul sursă" la final.

Modificări aplicate manual peste datele auto-generate din CSV (`data/source/Ghid indicatii radioimagistice.csv`). Se aplică în `iris-data.js` (runtime) și în artefactele derivate (`chapters/subchapters/situations/recommendations/guidelines.json`).

## 2026-07 — capitalizare, diacritice, typo-uri, deduplicare

### Capitalizare
- Subcapitole cu inițială mică: „cancer anal" → „Cancer anal"; „cancer de rect" → „Cancer de rect".
- Situații clinice cu inițială mică: „diagnostic" → „Diagnostic"; „stadializare" → „Stadializare"; „urmărire" → „Urmărire" (în subcapitolele Cancer anal și Cancer de rect).

### Diacritice — „si" → „și" (în titluri de capitole/subcapitole/situații)
- Subcapitole: „Cancer laringian si al hipofaringelui", „Uter si anexe".
- Situații: toate titlurile cu „si" izolat (ex. „v. si capitolul Pediatrie", „diagnostic si bilanț de extensie", „Tumori cerebrale si medulare", „Colangiocarcinom si cancerul…").

### Typo-uri
- „stadialilzare" → „stadializare".
- „restadialilzare" → „restadializare".
- „vasculariazație" → „vascularizație".
- „aripice" → „atipice" (în „Cefalee cronice aripice").
- Spațiere: „(AVC)-" → „(AVC) -".

### Uniformizare
- Subcapitol: „Ficat, colecist, pancreas" → „Ficat, colecist și pancreas" (formă unică).

### Deduplicare situații (typo-uri care scindaseră o situație în două)
Recomandările au fost mutate pe situația canonică, iar duplicatul a fost eliminat:
- „Cefalee cronice aripice" + „Cefalee cronice atipice" → o singură situație „Cefalee cronice atipice (v. și capitolul Pediatrie)".
- AVC „vasculariazație extracraniană" → fuzionat în „AVC - vascularizație extracraniană".
- AVC „vasculariazație intracraniană" → fuzionat în „AVC - vascularizație intracraniană".
- Cancer de rect: „urmărire si restadialilzare" → fuzionat în „Urmărire și restadializare".

### Deduplicare subcapitole
- Capitolul Cancer avea două subcapitole „Ficat, colecist și pancreas" (unul cu 1 situație, unul cu 7) — fuzionate într-unul singur. Subcapitole 93 → 92. (Subcapitolul omonim din capitolul Aparat digestiv este distinct și rămâne separat.)
- După fuziune, situația „Colangiocarcinom și cancerul veziculei biliare – Diagnostic" apărea în ambele subcapitole Ficat → fuzionată. Situații 696 → 695.

## 2026-07 — corecții typo-uri ortografice

Greșeli de tastare corectate în titluri, comentarii și denumiri de examene:
- „penntru" → „pentru"
- „anevrsim" → „anevrism"
- „Hirschprung" → „Hirschsprung"
- „radiațiii" → „radiații"
- „paratitoide" → „paratiroide"

## 2026-07 — cratime și liniuțe în titluri

### Cuvinte compuse cu spațiu greșit (spațiul eliminat)
- „Osgood- Schlatter" → „Osgood-Schlatter"
- „vezico- ureteral" → „vezico-ureteral"
- „farmaco- rezistentă" → „farmacorezistentă"
- „Lombo -radiculalgie" → „Lombo-radiculalgie"

### Separatori de segment uniformizați la liniuță (en dash) „ – "
Toți separatorii din titluri (indiferent de spațierea inițială: „x- y", „x -y", „x - y") au fost normalizați la liniuță cu spații de o parte și de alta: „ – ". 87 de titluri afectate (ex. „Neoplasm bronhopulmonar – stadializare", „AVC – vascularizație extracraniană", „Cancerul de stomac – diagnostic și bilanț de extensie"). Cratimele din cuvinte compuse legitime („nou-născut", „cranio-cerebral", „arterio-venoasă", „gastro-intestinală", „HIV-SIDA", „40-49 ani" etc.) au fost păstrate.

Efect asupra numărătorilor: situații clinice 700 → 696 (4 duplicate eliminate); recomandări 1559 (neschimbat — doar reasignate). Numărătorile pe capitole/subcapitole/situații au fost recalculate.

### Note / rămase de verificat
- „si" izolat mai apare în textul comentariilor (nu în titluri) — nemodificat în această rundă.
- Există în continuare situații cu spațiere/cratimă inconsecventă în titluri (ex. „Cancerul de stomac- diagnostic…") — necorectate, fiind ambigue.

## 2026-07-03 — merge în CSV-ul sursă (audit)

Toate corecțiile documentate mai sus au fost scrise **direct în celulele CSV-ului sursă**, ca operații țintite (nu regenerare din datele derivate — datele derivate conțineau și transformări nedocumentate: restaurare de diacritice „pulmonara→pulmonară", expandare de abrevieri „inf.→inferior", „sdr.→sindrom", „v.→vezi", și chiar corecții de conținut „hepatice→renale" — **acestea NU au fost preluate**, fiind în afara acestui changelog).

**Impact:** 337 celule modificate pe 294 rânduri. Structura, ordinea coloanelor, ghilimelele și numărul de rânduri ale CSV-ului au fost păstrate identic (round-trip exact verificat).
- Subcapitole (col. Subcapitol): 83 celule — capitalizare (Cancer anal, Cancer de rect), „si→și" (Cancer laringian…, Uter…, Tumori cerebrale…), uniformizare (Ficat, colecist și pancreas), typo (Tiroidă, paratiroide).
- Situații (col. Situația Clinică): 246 celule / 90 titluri distincte — „si→și", capitalizare inițială (în Cancer anal / de rect), cratime în cuvinte compuse (Osgood-Schlatter, vezico-ureteral, farmacorezistentă, Lombo-radiculalgie, (AVC) -), separatori de segment normalizați la „ – " (79 titluri distincte, cratimele compuse legitime păstrate), typo-uri (stadializare, restadializare, vascularizație, atipice, anevrism, radiații).
- Comentarii: 7 celule — typo-uri (Hirschsprung, pentru etc.). Alte informații: 1 celulă.

**Regula de normalizare a liniuțelor** (aplicată doar coloanei Situația Clinică): o cratimă/liniuță cu spațiu pe cel puțin o parte („x- y", „x -y", „x - y") → „ – "; cratimele fără spații („nou-născut", „cranio-cerebral", „40-49") sunt păstrate. Corecțiile de cuvinte compuse s-au aplicat înainte, ca să nu fie confundate cu separatori.

**Divergență cunoscută:** datele derivate livrate (`iris-data.js` + `*.json`) conțin transformări suplimentare nedocumentate (diacritice, abrevieri, câteva corecții de conținut) care NU există în CSV. O regenerare din CSV via `tools/csv-to-json.html` nu le-ar reproduce. De verificat/documentat separat dacă se dorește sincronizarea completă.
