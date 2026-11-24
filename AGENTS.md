# AGENTS.md — Atlas Plante Medicinale & Melifere – Parc de la Saussaie

Harta proiectului și regula obligatorie de lucru. Actualizat: 2026-11-24.

## Regulă obligatorie înainte de ORICE modificare

1. **Înainte de a edita orice fișier**, creează/suprascrie backup-ul în `.tmp`:
   - pentru `index.html` → `.tmp/index_backup.html` (copie exactă a stării dinainte de editare);
   - pentru `plante.html` → `.tmp/plante_backup.html`;
   - pentru `AGENTS.md` → `.tmp/agents_backup.md`.
2. După editare, verifică sintaxa JavaScript (`node --check` pe conținutul din `<script>`) și legăturile `index.html ↔ plante.html`.
3. **Actualizează întotdeauna AGENTS.md** cu noile intervale de linii, numărul de înregistrări și data modificării.
4. Nu șterge `.tmp` și nu folosi backup-ul ca sursă de editare; el este doar siguranță pentru revertuire.

## Fișiere

| Fișier | Linii | Rol |
| --- | --- | --- |
| `index.html` | 586 | Aplicația completă, single-file, cu toate cele 4 secțiuni |
| `plante.html` | 293 | Enciclopedie standalone; oglindește CSS-ul și baza de date din `index.html` |
| `.tmp/index_backup.html` | copie | Backup-ul stării lui `index.html` înainte de modificarea curentă |

## Harta `index.html`

### Structură generală

| Interval | Conținut |
| --- | --- |
| 1–7 | `<head>`: meta viewport, theme-color, titlu |
| 8–163 | **CSS complet**: 9–158 componente, 159–162 responsive/print, 163 `</style>` |
| 165–172 | Antet cu logo, titlu și legătura către `plante.html` |
| 173–252 | `<main>` cu cele 4 secțiuni |
| 253–258 | Meniu fix jos, 4 butoane: Plante / Rețetar / Calendar / Jurnal |
| 259 | Modalul fișei plantei |
| 260 | Toast pentru notificări |
| 261–584 | `<script>`: date, stare, randare și logica aplicației |
| 582 | Pornire aplicație (`init()`) |

### Secțiunea 1 — Enciclopedia plantelor

| Interval | Conținut |
| --- | --- |
| 174–193 | Secțiunea `#enciclopedie` |
| 181 | Căutare după nume/francez/latin/afecțiune (`#searchPlants`) |
| 182–187 | Filtre: Toate / Medicinale / Melifere / Ambele |
| 188–190 | Rezultat, grilă `#plantGrid`, stare goală |
| 262–277 | Luni, abrevieri și sfaturile lunilor |
| **278–308** | **Baza de date `PLANTE`: 29 de specii** (denumiri RO/FR/Latin, familie, tip, parte, recoltare, luni, afecțiuni, beneficii, administrare, precauții, fișă apicolă) |
| 332 | Chei `localStorage`: `atlasplante.jurnal.v1`, `atlasplante.favorite.v1`, `atlasplante.retete.v1` |
| 333–346 | Helpers DOM, escape, normalizare diacritice, formatare numere, starea încărcată din `localStorage` |
| 399–421 | `renderPlants()` — filtrare + randare carduri |
| 422–426 | `toggleFavorite()` — favorite în `localStorage` |
| 427–452 | `openPlant()` / `closeModal()` — fișa completă cu secțiunea Fișă Apicolă |

### Secțiunea 2 — Rețetar & apiterapie

| Interval | Conținut |
| --- | --- |
| 194–214 | Secțiunea `#retetar` |
| 197 | Containerul randat `#recipeList` |
| 201–211 | Formular rețetă personală (`#customRecipeForm`) |
| **309–323** | **`RETETE`: 13 rețete prestabilite** (infuzii, siropuri, oximel, tincturi, unguente, hrănire albine) |
| 324–331 | `CATEGORII`: 6 categorii |
| 453–462 | `allRecipes()` + `recipeCard()` — card rețetă cu input de scalare |
| 463–468 | `renderRecipes()` — grupare pe categorii |
| **469–474** | **`updateRecipe()` — calculatorul de cantități** (`factor = porții / bază`) |
| 475–487 | Salvare/ștergere rețete personale în `localStorage` |
| 488–494 | `recipeToJournal()` — transferă rețeta în jurnal |

### Secțiunea 3 — Calendar melifer & de recoltare

| Interval | Conținut |
| --- | --- |
| 215–226 | Secțiunea `#calendar` |
| 218–224 | Selector 12 luni + două coloane: culesuri melifere / recoltă medicinală |
| 495–498 | `initCalendar()` — construiește butoanele lunilor |
| 499–508 | `renderCalendar()` — folosește `luni` pentru nectar și `rand` pentru recoltă, din `PLANTE` |

### Secțiunea 4 — Jurnal, stoc și backup JSON

| Interval | Conținut |
| --- | --- |
| 227–251 | Secțiunea `#jurnal` |
| 229 | Statistici `#journalStats` |
| 232–244 | Formular: tip, plantă, dată, cantitate, unitate, lot, observații |
| 245–249 | Filtrare, listă `#journalList`, butoane Export/Import |
| 509–518 | `makeLot()`, `prefillJournal()`, `resetJournalForm()` |
| 519–531 | Salvare, editare, ștergere înregistrări |
| 532–549 | `renderJournal()` — filtrare, sortare după dată, statistici |
| 550–557 | `exportBackup()` — descarcă JSON cu jurnal + favorite + rețete |
| 558–581 | `importBackup()` — înlocuiește sau completează datele din JSON |

### Navigare și evenimente

| Interval | Conținut |
| --- | --- |
| 347–357 | `init()` — populare datalist, contor specii, categorii, randare inițială |
| 358–364 | `switchView()` — schimbă secțiunea și hash-ul (`history.replaceState`) |
| 365–398 | `bindEvents()` — toate listener-ele pentru nav, căutare, filtre, modal, rețete, calendar, jurnal |

## Harta `plante.html`

| Interval | Conținut |
| --- | --- |
| 1–7 | `<head>` dedicat enciclopediei |
| 8–175 | **CSS**: 9–162 copie din `index.html`, 164–174 stiluri specifice (`full-details`, `honey-mini`, sortare) |
| 177–184 | Antet cu legătura înapoi la `index.html` |
| 185–207 | Pagina enciclopediei: 193 căutare, 194–199 filtre, 200 sortare/tipărire, 201–204 grilă |
| 208 | Buton fix „Deschide rețetarul, calendarul și jurnalul” |
| 209 | Toast |
| 211–212 | Luni și abrevieri |
| **213–243** | **Copie oglindă a bazei `PLANTE` (29 specii)** — trebuie sincronizată cu `index.html:278–308` |
| 244–251 | Cheie `localStorage` `atlasplante.favorite.v1` + helpers + favorite |
| 252–254 | Toast, etichete tip, formatare înflorire |
| 255–267 | `card()` — card cu detalii complete și fișă apicolă mini |
| 268–279 | `render()` — căutare, filtru tip, sortare |
| 280–281 | `toggleFav()` + `openDetail()` |
| 282–290 | Evenimente și inițializare |
| 291–293 | Închidere script/body/html |

## Sincronizare între fișiere

- CSS de bază: `index.html:9–162` = `plante.html:9–162`.
- Baza de date: `index.html:278–308` = `plante.html:213–243`.
- Localele lunilor: `index.html:262–263` = `plante.html:211–212`.
- Favoritele folosesc aceeași cheie: `atlasplante.favorite.v1`.
- La modificarea unei plante, actualizează **ambele** blocuri de date, apoi rulează `node --check` pentru ambele scripturi.
