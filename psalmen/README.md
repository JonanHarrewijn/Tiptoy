# Psalmen die kinderen kennen (Tiptoi)

Zes bekende "schoolpsalmen": 42, 84, 100, 116, 134 en 150.
Product-ID **901** — kan samen met `mijnproject.gme` (900) op één pen.

## Gebruik

1. Kopieer `psalmen.gme` naar de pen (USB).
2. Print de codes uit `oid-codes/` op 100% grootte (1200 dpi), of gebruik
   `oid-codes/600dpi/` voor printers die geen 1200 dpi aankunnen.
3. Tik het START-logo aan (welkomstboodschap), daarna een psalm-code om
   die psalm te horen.

## Audio vervangen door echte zang

De huidige geluiden zijn gesproken plaatshouders (computerstem, eerste regels
uit de berijming van 1773 — publiek domein). Zo vervang je ze door echte
opnames, bijvoorbeeld zelf ingezongen of met de klas:

```bash
ffmpeg -i opname-psalm42.mp3 -ar 22050 -ac 1 -c:a libvorbis -q:a 3 audio/psalm42.ogg
tttool assemble psalmen/psalmen.yaml   # vanuit de map boven psalmen/
```

De OID-codes blijven hetzelfde (vastgelegd in `psalmen.codes.yaml`), dus
opnieuw printen is niet nodig — alleen de nieuwe `psalmen.gme` op de pen
zetten.

**Let op auteursrecht:** de Geneefse melodieën en de berijming van 1773 zijn
publiek domein, maar bestaande opnames (cd's, YouTube) en nieuwere
berijmingen zijn dat meestal niet. Eigen opnames zijn altijd veilig voor
eigen gebruik.

## Psalm toevoegen

1. `audio/psalm121.ogg` toevoegen (mono, 22050 Hz).
2. In `psalmen.yaml` onder `scripts:` toevoegen:
   ```yaml
   psalm121:
     - P(psalm121)
   ```
3. Opnieuw bouwen en alleen de nieuwe code printen.
