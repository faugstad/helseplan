# Helseplan 2025 💚

Din personlige helseplan - spor kosttilskudd, mobilitet og vaner.

![Helseplan Icon](icon-512.png)

## Funksjoner

- 💊 **Daglig tilskudd-tracking** - Morgen, lunsj og kveld
- 🧘 **Mobilitetsrutine** - 6 øvelser med lenker til guider
- 🍺 **Alkoholfri uke** - Én enkel vane
- ⏱️ **Bevegelsespause-timer** - 45 min påminnelse
- 📱 **PWA-støtte** - Installer som app på mobilen
- 💾 **Lokal lagring** - Alt lagres i nettleseren

## Deploye til GitHub Pages

### 1. Opprett repository

1. Gå til [github.com/new](https://github.com/new)
2. Navn: `helseplan` (eller hva du vil)
3. Velg **Public**
4. Klikk **Create repository**

### 2. Last opp filer

**Alternativ A: Via GitHub web**
1. Klikk "uploading an existing file"
2. Dra alle filene fra ZIP-filen inn
3. Commit med melding "Initial commit"

**Alternativ B: Via terminal**
```bash
git clone https://github.com/DITT-BRUKERNAVN/helseplan.git
cd helseplan
# Kopier alle filene hit
git add .
git commit -m "Initial commit"
git push
```

### 3. Aktiver GitHub Pages

1. Gå til repository → **Settings**
2. Scroll ned til **Pages** (i venstre meny)
3. Under "Source", velg **Deploy from a branch**
4. Velg **main** branch og **/ (root)**
5. Klikk **Save**

### 4. Ferdig! 🎉

Etter 1-2 minutter er appen live på:
```
https://DITT-BRUKERNAVN.github.io/helseplan/
```

## Installer på mobil

### iPhone (Safari)
1. Åpne lenken i Safari
2. Trykk på Del-knappen (firkant med pil opp)
3. Velg "Legg til på Hjem-skjerm"
4. Trykk "Legg til"

### Android (Chrome)
1. Åpne lenken i Chrome
2. Trykk på menyen (tre prikker)
3. Velg "Legg til på startskjermen"
4. Trykk "Legg til"

## Filstruktur

```
helseplan/
├── index.html      # Hovedappen
├── manifest.json   # PWA-konfigurasjon
├── sw.js          # Service worker (offline-støtte)
├── icon.svg       # Vektor-ikon
├── icon-180.png   # Apple Touch Icon
├── icon-192.png   # Android ikon
├── icon-512.png   # Stort ikon
└── README.md      # Denne filen
```

## Teknologi

- React 18 (CDN)
- Vanilla CSS (ingen frameworks)
- LocalStorage for data
- Service Worker for offline-støtte

## Lisens

MIT - Bruk fritt!
