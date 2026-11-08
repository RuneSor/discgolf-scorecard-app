# 🥏 Scorekort – Discgolf

En superenkel, dynamisk scorekort-app for discgolf. Fungerer for enhver bane og alle hulltall – ingen oppsett i kode, alt konfigureres i appen. Kjører som én selvstendig HTML-fil, direkte i nettleseren, og kan legges til på hjemskjermen på iPhone som en app.

## Innhold

- [Funksjoner](#funksjoner)
- [Kom i gang](#kom-i-gang)
- [Bruk](#bruk)
  - [1. Sett opp runden](#1-sett-opp-runden)
  - [2. Registrer poeng](#2-registrer-poeng)
  - [3. Se scorekort](#3-se-scorekort)
- [Kart og utkast](#kart-og-utkast)
- [Lagrede baner (maler)](#lagrede-baner-maler)
- [Spill på nytt med samme spillere](#spill-på-nytt-med-samme-spillere)
- [Bruk som app på iPhone](#bruk-som-app-på-iphone)
- [Teknisk](#teknisk)

## Funksjoner

- 🎯 **Dynamisk oppsett** – velg antall hull og par per hull, passer enhver bane
- 👥 **Flere spillere** – legg til og fjern spillere fritt
- ➕➖ **Enkel poengregistrering** – stepper-knapper per hull, ingen tastatur nødvendig
- 📊 **Automatisk +/-** – viser birdie/bogey osv. live, fargekodet
- 📋 **Fullt scorekort** – oversikt over alle hull, totalt antall kast og total +/- per spiller
- 💾 **Lagrede baner** – lagre en bane (antall hull + par) som en mal og gjenbruk den senere
- 🗺️ **Kart med utkast** – se hvor du er på kart eller flyfoto, og lagre utkastet for hvert hull
- 🔁 **Spill på nytt** – nullstill poengene og spill samme bane om igjen med samme spillere
- 📱 **Husker runden** – runden lagres automatisk på enheten og overlever at du lukker appen, helt til du starter en ny runde
- 🚫 **Ingen servere, ingen kontoer** – alt kjører lokalt i nettleseren

## Kom i gang

Appen er én fil: `discgolf-scorekort.html`. Den kan åpnes direkte i en hvilken som helst moderne nettleser (Safari, Chrome osv.) – ingen installasjon eller bygg-steg nødvendig.

```bash
# Klon repoet
git clone <repo-url>
cd <repo-mappe>

# Åpne filen i nettleseren din
open discgolf-scorekort.html   # macOS
# eller bare dobbeltklikk filen i Finder/Utforsker
```

## Bruk

### 1. Sett opp runden

- Angi **antall hull** på banen
- Legg til **spillere** ved navn (du kan legge til/fjerne så mange du vil)
- Par settes automatisk til 3 på alle hull som standard – du justerer dette per hull underveis i runden
- Trykk **Start runde**

### 2. Registrer poeng

- Naviger mellom hull med pilene eller «Forrige»/«Neste»
- Juster **par** for det aktuelle hullet med +/− om nødvendig
- Registrer **antall kast** per spiller med +/− steppere
- +/- i forhold til par vises automatisk og fargekodes:
  - 🟡 Gull = under par (birdie/eagle)
  - ⚪ Grå = par
  - 🔴 Rødt = over par (bogey+)

### 3. Se scorekort

Trykk **«Vis fullt scorekort»** for å se alle hull, totalt antall kast og total +/- per spiller. Lederen markeres tydelig.

## Kart og utkast

Trykk **📍 Kart** øverst under spilling for å åpne kartet. Det viser hvor du er (blå prikk) og utkastet for hullet du står på, hvis det er lagret (gul markør med hullnummeret). En gul prikk på Kart-knappen betyr at hullet har et lagret utkast.

- **Lagre utkast her** lagrer GPS-posisjonen din som utkast for hullet. Finnes det allerede et utkast, blir du spurt før det erstattes.
- Dra markøren for å finjustere plasseringen.
- Knappen øverst bytter mellom **kart** og **flyfoto**.
- Når utkastet er lagret, vises avstanden fra deg til utkastet.

Utkastene lagres i runden. Er runden startet fra en lagret bane, oppdateres banen også, og de blir med når du lagrer banen som ny mal.

> Kartet krever nett og at siden åpnes over `https://` (GPS virker ikke ellers). Resten av appen virker uten nett.

## Lagrede baner (maler)

Har du spilt en bane før? Lagre den som en mal så slipper du å taste inn par på nytt hver gang:

1. Under spilling eller på scorekort-siden, trykk **«Lagre bane som mal»**
2. Gi banen et navn
3. Neste gang du setter opp en ny runde, velger du banen fra listen **«Lagrede baner»** og trykker **«Bruk»** – antall hull og par fylles inn automatisk

Maler kan slettes med ✕ ved siden av navnet.

## Spill på nytt med samme spillere

Fra scorekort-siden kan du trykke **«Spill på nytt – samme spillere»** for å:

- Nullstille alle poeng
- Beholde de samme spillerne og samme bane (hull/par)
- Starte rett på hull 1 igjen

Nyttig for flere runder på samme bane med samme gjeng, uten å taste inn spillere på nytt.

## Bruk som app på iPhone

Appen kan legges til på hjemskjermen og oppføre seg som en vanlig app – eget ikon, fullskjerm, ingen adressefelt:

1. Host filen et sted tilgjengelig fra telefonen. Enklest uten konto: dra `discgolf-scorekort.html` inn på [netlify.com/drop](https://app.netlify.com/drop) – du får en live-lenke med en gang. Alternativt: host via GitHub Pages.
2. Åpne lenken i **Safari** på iPhonen
3. Trykk **Del-knappen** → **«Legg til på Hjem-skjerm»**

Runden lagres automatisk på enheten mellom økter, helt til du trykker **«Start ny runde»**.

> **Merk:** Lagring (runde-data og baner) krever at siden åpnes som en vanlig nettside (`https://...` eller lokalt i Safari) – i sandkasse-forhåndsvisninger (f.eks. i chat-verktøy) kan lagring være blokkert av nettleseren.

## Teknisk

- Ren HTML/CSS/JavaScript – ingen rammeverk, ingen bygg-steg
- Kart med [Leaflet](https://leafletjs.com) (lastes først når kartet åpnes), kartdata fra © OpenStreetMap og flyfoto fra Esri
- All state holdes i én JSON-struktur og lagres i `localStorage`
- Ingen data sendes til noen server – alt skjer lokalt på enheten (unntak: når kartet er åpent, hentes kartbilder for området fra OpenStreetMap eller Esri)
