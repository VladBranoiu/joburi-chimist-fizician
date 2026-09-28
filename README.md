# Joburi chimist / fizician medical: Brașov, Galați, Ilfov, București, Iași, Covasna

Strânge zilnic anunțurile de **chimist** (chimist specialist / principal, inginer chimist, analist chimist) și
**fizician medical** de pe **eJobs, BestJobs, OLX, posturi.gov.ro și ROmedic**, le dă un scor și le afișează
într-o pagină cu filtre (post, județe, salariu, anunțuri noi).

- `./actualizeaza.sh` descarcă anunțurile noi (câteva minute; descrierile se țin în cache).
- `index.html` se deschide în browser. Din Windows: `\\wsl$\Ubuntu\home\vladb\apps\chimist-fizician-joburi\index.html`,
  sau din WSL cu `explorer.exe index.html`.
- `config.json` conține județele (cu localitățile lor și un bonus opțional pe județ), firmele preferate și cursul EUR.

## Surse
| Sursă | Ce are | Cum se aplică |
|---|---|---|
| eJobs, BestJobs | firme private (laboratoare, fabrici, farma) | cont gratuit pe site |
| OLX | anunțuri mici | de obicei se sună direct |
| posturi.gov.ro | concursurile de la stat: spitale, DSP, laboratoare publice (portalul oficial, HG 1336/2022) | dosar la unitate, până la data limită |
| ROmedic | joburi medicale din clinici și laboratoare private | fără cont, butonul „Aplică” |

Joburile din titlu trebuie să ceară chimist sau fizician medical; laborant, tehnician, operator, profesor,
biochimist fără chimist etc. nu intră. La concursurile de la stat se citește din text data limită pentru dosar,
iar după ea anunțul apare ca expirat.

## Cum se calculează scorul (pornește de la 50, limitat la 0–100)
| Criteriu | Puncte |
|---|---|
| Job într-unul din cele 6 județe (bonusul se poate schimba pe județ în `config.json`) | +10 |
| Program de zi, L–V / noapte, weekend | +8 / −12 |
| Contract pe perioadă determinată | −6 |
| Abonament medical / tichete de masă | +4 / +2 |
| Salariu net ≥7000 / ≥6000 / ≥5000 / ≥4000 / <3500 lei | +12 / +9 / +6 / +2 / −6 |
| Firmă preferată / agenție de recrutare | +8 / −6 |
| Nota angajaților pe UndeLucram ≥4 / ≥3,5 / ≥3 / ≥2,5 / mai mică (jumătate dacă are sub 5 păreri) | +8 / +4 / 0 / −6 / −10 |
| Nota clienților pe Google (doar cu cheie API) | ±2 |
| Anunț mai vechi de 45 de zile | −4 |

Verdict: **Foarte potrivit** ≥80, **Potrivit** ≥65, **Merită verificat** ≥50, **Slab** sub 50 (ascuns).
Excluse automat: joburi în străinătate.

## Recenzii firme
- **UndeLucram.ro**: nota angajaților, numărul de păreri. Se caută automat după numele firmei și se reîmprospătează la 30 de zile.
  Comentariile de acolo se văd doar cu cont, așa că pagina dă link spre ele.
  Dacă o firmă e legată greșit, se corectează în `config.json` → `recenzii_potrivire_manuala` (id UndeLucram sau `"nu"`).
- **Google Maps** (opțional): nota clienților și ultimele 5 comentarii prin API-ul oficial Google Places.
  Pune cheia în `config.json` → `google_places_api_key`, apoi rulează `python3 colector.py recenzii --fortat`.

## Comenzi
- `python3 colector.py`: totul (anunțuri + recenzii)
- `python3 colector.py posturi romedic`: doar unele surse (`ejobs`, `bestjobs`, `olx`, `posturi`, `romedic`)
- `python3 colector.py recenzii --fortat`: reface recenziile
- `python3 colector.py reclasifica`: recalculează scorurile după o schimbare de reguli, fără descărcări

Salvatele, aplicările și notițele stau în browser (localStorage); „Setări avansate” → „Salvează notițele” face o copie.
Indeed, Jooble și LinkedIn blochează accesul automat, așa că nu sunt incluse. Viața Medicală preia anunțurile tot de pe
posturi.gov.ro și ms.ro, deci nu aduce nimic în plus.

## Online
- Site: https://vladbranoiu.github.io/joburi-chimist-fizician/ (GitHub Pages, din branch-ul `main`).
- **GitHub Actions** (`.github/workflows/actualizare.yml`) rulează zilnic la 04:00 UTC: toate sursele în afară de OLX, plus recenziile.
  Se poate porni și manual din tabul Actions → „Actualizare anunțuri” → Run workflow.
- **OLX blochează serverele GitHub**, așa că OLX se actualizează de pe PC: sarcina Windows „Joburi chimist - OLX”
  (zilnic la 10:00 și la logare) rulează `sincronizare-olx.sh` într-o copie separată (`~/.local/share/joburi-chimist-sync`).
  Jurnal: `~/.local/share/joburi-chimist-sync.log`. Dacă PC-ul stă oprit, anunțurile OLX dispar treptat în 14 zile, iar restul merge normal.
- Înainte să modifici ceva local: `git pull` (datele se schimbă zilnic pe GitHub).
