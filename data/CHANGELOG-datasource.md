# Corecții datasource

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
