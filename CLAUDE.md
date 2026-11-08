# Scorekort – Discgolf

Enkel scorekort-app for discgolf (ren html/css/js, ingen byggesystem). Fungerer for alle baner og hulltall; alt settes opp i appen.
Brukere: spillere på iPhone, ofte lagt til på Hjem-skjermen som app. Ingen servere eller kontoer, alt lagres lokalt på enheten.

## Struktur
- `discgolf-scorekort.html` – hele appen (html, css og js i én fil). Ikonet ligger inline som base64 PNG.
- `README.md` – brukerdokumentasjon. Oppdater den når funksjoner endres.
- Ingen byggesteg: filen som ligger i repoet er filen som publiseres.

## Kjøre lokalt
`python3 -m http.server 8000` i prosjektmappen, åpne http://localhost:8000/discgolf-scorekort.html.
Filen kan også åpnes direkte (`open discgolf-scorekort.html`), men bruk serveren ved test så localStorage oppfører seg som på nett.
For test på iPhone må siden ligge bak https (f.eks. Netlify Drop eller GitHub Pages).

## Arbeidsflyt (hver oppgave)
Repo: github.com/RuneSor/discgolf-scorecard-app. Aldri commit eller push rett til `main`, og aldri merge PR-er.
1. Motta oppgaven og bli enige om hva som skal gjøres. Spør hvis noe er uklart.
2. Lag en ny branch fra oppdatert `main` (`feature/…`, `fix/…`, `docs/…`).
3. Gjør endringene i branchen.
4. Start `python3 -m http.server 8000` i prosjektmappen og si fra at det er klart til test.
5. Brukeren tester og sier OK eller ber om endringer. Ved endringer: bare lagre filen og be om ny test (serveren kan stå).
6. Etter OK: commit, push, lag PR med `gh pr create` og kjør `/code-review`. Rett funnene. Endrer rettelsene oppførselen, be om ny test.
7. Brukeren merger på GitHub (squash). Etterpå: `git switch main && git pull`, slett den lokale branchen.

## Arkitektur
- All tilstand ligger i ett `state`-objekt (`phase`: `setup` | `play` | `summary`). Endringer skjer i små funksjoner som så kaller `render()`, som tegner hele skjermen på nytt (`renderSetup`, `renderPlay`, `renderSummary`, `renderModal`).
- `modal` er midlertidig UI-tilstand og lagres ikke.
- Lagring i localStorage: runden under `discgolf_scorekort_v1`, banemaler under `discgolf_baner_v1`, valgt kartlag under `discgolf_kartlag_v1`. Endres formatet på en måte som ikke er bakoverkompatibel, bump versjonen i nøkkelen eller migrer gamle data.
- Brukerinput som vises i HTML skal gjennom `escapeHtml`.
- Kart: kartboksen (`#mapOverlay`) ligger utenfor `#root` slik at `render()` ikke river ned Leaflet-kartet. Den oppdateres med egne funksjoner (`openMap`, `updateMapFoot` osv.).
- Utkast per hull ligger i `state.tees[hull] = {lat, lng}` (eller `null`) og i malen som `tees`. Les alltid via `teeFor()`, siden eldre runder og maler mangler feltet. `state.templateId` er malen runden bruker; `setTee()` oppdaterer den.

## Konvensjoner
- Kommentarer i kode skrives på engelsk (noen eldre kommentarer er på norsk; oversett dem når du likevel endrer koden der).
- UI-tekst er på norsk (bokmål).
- Alle kall til localStorage pakkes i try/catch (lagring kan være blokkert, f.eks. i sandkasse-forhåndsvisninger).
- Mobil først: maks bredde 480 px, store trykkflater, ingen tastatur nødvendig under spilling. Respekter safe-area (`env(safe-area-inset-*)`) for iPhone.
- Farger ligger som CSS-variabler i `:root`; bruk dem i stedet for nye hardkodede farger.
- Ingen eksterne avhengigheter utover Google Fonts (Barlow Condensed, Inter) og Leaflet 1.9.4 fra cdnjs (med SRI). Leaflet lastes først når kartet åpnes, så resten av appen virker uten nett. Ingen rammeverk, ingen byggesteg.
- Kartlag: OpenStreetMap (standard) og Esri World Imagery (flyfoto). URL-er og kreditering ligger i `MAP_LAYERS`. Krediteringen må alltid synes i kartet.
- GPS krever HTTPS (localhost er unntatt).

## Test
Ingen automatiske tester ennå. Verifiser manuelt i nettleseren: sett opp runde, registrer poeng, se scorekort, lagre/bruke bane, spill på nytt, og at runden overlever omlasting av siden. Test gjerne også på iPhone.
For kart: lagre utkast, dra markøren, bytt kartlag, og sjekk at malen i bruk blir oppdatert. I Chrome kan posisjonen simuleres i DevTools (Sensors → Location).
