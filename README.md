# Mijn eigen Tiptoi-audioproject

Een zelfgemaakt geluidsproject voor de Ravensburger Tiptoi-pen, gebouwd met
[tttool](https://github.com/entropia/tip-toi-reveng).

## Structuur

```
mijnproject.yaml     het projectbestand (product-id 900, welkomstgeluid, 3 scripts)
audio/               geluidsbestanden: OGG Vorbis, mono, 22050 Hz
oid-codes/           gegenereerde OID-codes als PNG (1200 dpi)
mijnproject.gme      het bestand dat op de Tiptoi-pen gaat
```

## Stap voor stap

### 1. Audio voorbereiden

De Tiptoi-pen wil **OGG Vorbis, mono, 22050 Hz**. Heb je MP3 of WAV, converteer dan met ffmpeg:

```bash
ffmpeg -i invoer.mp3 -ar 22050 -ac 1 -c:a libvorbis -q:a 3 audio/naam.ogg
```

- `-ar 22050` = samplefrequentie 22050 Hz
- `-ac 1` = mono
- `-c:a libvorbis -q:a 3` = OGG Vorbis met prima kwaliteit voor spraak

Controleer het resultaat:

```bash
ffprobe -v error -show_entries stream=codec_name,sample_rate,channels -of csv=p=0 audio/naam.ogg
# verwacht: vorbis,22050,1
```

### 2. Het projectbestand (`mijnproject.yaml`)

- `product-id: 900` — een ID buiten de reeks van officiële Ravensburger-producten,
  zodat het niet botst met bestaande boeken op je pen.
- `welcome: welkom` — speelt `audio/welkom.ogg` af zodra je het aan-logo van dit
  product aantikt.
- `scripts:` — elke naam (hond, kat, vogel) krijgt een eigen OID-code;
  `P(bestandsnaam)` speelt het bijbehorende geluid af.

### 3. GME-bestand bouwen

```bash
tttool assemble mijnproject.yaml
```

Dit maakt `mijnproject.gme`. Kopieer dat bestand naar de hoofdmap van de
Tiptoi-pen (de pen verschijnt als USB-station op je computer).

Controleren of het bestand geldig is:

```bash
tttool info mijnproject.gme      # toont product-id, aantal scripts, audio
tttool play mijnproject.yaml     # interactieve simulator op je computer
```

### 4. OID-codes genereren en printen

```bash
tttool oid-codes mijnproject.yaml
```

Dit maakt per script een PNG (`oid-900-START.png`, `oid-900-hond.png`, ...).
De START-code is het "aan-logo": eerst aantikken om het product te activeren,
daarna spelen de andere codes hun geluid.

**Printen — belangrijk:**

- De PNG's zijn gemaakt voor **exact 1200 dpi**. Print ze op **100% grootte**
  ("werkelijke grootte", niet "aanpassen aan pagina"), anders klopt het
  puntjespatroon niet meer en herkent de pen niets.
- Gebruik een **laserprinter** als het kan; die drukt het fijne patroon het
  scherpst af. Zet eventuele "toner besparen"-stand uit.
- Print in **zwart-wit op hoogste kwaliteit**.

**Ondersteunt je printer geen 1200 dpi?** Genereer de codes dan opnieuw met een
lagere resolutie én een dikkere puntgrootte, bijvoorbeeld voor 600 dpi:

```bash
tttool --dpi 600 --pixel-size 2 oid-codes mijnproject.yaml
```

- `--dpi 600` maakt het patroon passend voor 600 dpi-printers.
- `--pixel-size 2` maakt elke punt 2×2 pixels, zodat hij groot genoeg blijft.

Niet elke printer/papier-combinatie werkt: test met één velletje voordat je
alles print. Herkent de pen de code niet, probeer dan een andere
`--pixel-size` (2 of 3) of een andere printer.

### 5. Gebruiken

1. Zet `mijnproject.gme` op de pen (USB).
2. Print de OID-codes.
3. Tik het START-logo aan → je hoort het welkomstgeluid.
4. Tik hond/kat/vogel aan → elk speelt zijn eigen geluid.

## Nieuwe geluiden toevoegen

1. Zet een OGG-bestand (mono, 22050 Hz) in `audio/`, bijv. `koe.ogg`.
2. Voeg toe aan `mijnproject.yaml`:
   ```yaml
   koe:
     - P(koe)
   ```
3. Bouw opnieuw: `tttool assemble mijnproject.yaml` en genereer de nieuwe
   OID-code. Kopieer de nieuwe `.gme` naar de pen.
