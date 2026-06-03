# Dance Research Assistant Log - 001
Dátum: 2026-06-03
Kezdeti állapot: Tabula Rasa kutatás megkezdése.

## 1. Teljes Beszélgetéstörténet (AI_ready)
- **Kutatás fókusza:** A meglévő emberi rövidítések (`Abbreviations.json`) feldolgozása, a mögöttük rejlő tökéletlen algoritmus azonosítása, valamint egy új, skálázható, determinisztikus rövidítési algoritmus (UAA) és összetett szavakhoz használható szemantikus szeparációs módszer javaslata.
- **Megállapított elvek (MASTER_CONCEPT-be rögzítve):**
  - **L0-L3 absztrakciós szintek** bevezetése az atomi szinttől a komplex koreográfiákig.
  - **UAA (Unified Abbreviation Algorithm):** Core fogalmak fixen 1-2 betűsek (`t`, `r`, `l`, `f`, `b`, `u`, `d`, `a`, `h`, `lg`, `ft`, `ch`). Standard fogalmakra CPC-3 algoritmus (mássalhangzó-elsődlegesség: mássalhangzóval induló szavaknál első betű + következő két mássalhangzó, pl. `glide` -> `gld`, `slide` -> `sld`). Magánhangzós kezdetnél első 3 betű csonkolás (`angled` -> `ang`).
  - **Semantic Case Separation (Dot Case):** A CamelCase helyett pont (`.`) karakterrel választjuk el a fogalmakat (pl. `land.arc`, `tck.sit`). Ez kiküszöböli a Shift billentyű használatát, tisztán tokenizálható és hierarchikus névtereket alkot.
  - **Operátorok:** `-` a szekvencialitáshoz, `&`/`+` a szimultán akciókhoz, `:` a specifikációhoz.

---

## 2. Beszélgetés Archívum
### Beszélgetés #1 (2026-06-03)
- **User kérése:** Kezdjük el a kutatást. Elemezzük az `Abbreviations.json` és a többi 14 JSON fájl fogalmait, keressük meg a meglévő rövidítések mögötti algoritmust, tegyünk javaslatot egy egységesebb képzésre és szeparációra.
- **Elemzés menete:**
  - Letöltöttük és beolvastuk a legfontosabb JSON fájlok tartalmát (pl. `Abbreviations.json`, `Categories.json`, `Dances.json`, `Movement Types.json`, `Movements.json`, `Difficulty of Movement.json`, `Framework TAGs.json`, `Topic TAGs.json`).
  - Azonosítottuk a meglévő rövidítések 4 fő mintáját: (1) mássalhangzók kivonása (`clp`, `spr`), (2) prefix csonkolás (`lat`, `bal`), (3) ultra-rövid báziselemek (`t`, `ch`), (4) CamelCase-alapú összetétel (`landArc`, `tckSit`).
  - Rámutattunk a meglévő rendszer következetlenségeire (pl. `turn` egyszerre `t`, `tou` és `tour`; a `long` -> `lon` és `short` -> `sho` prefixes, míg a `spear` -> `spr` mássalhangzós; a CamelCase-nél nem egyértelmű a tokenhatár szótár nélkül, és fárasztó a gépelése).
- **Megoldási javaslatok:**
  - Bemutattuk az **UAA CPC-3** modellt, ami ~90%-ban megegyező, de immár 100%-ban determinisztikus és szigorú 3-betűs kódokat ad.
  - Bemutattuk a **Dot Case (`.`)** összetételt, ami Shift-mentes gépelést, gépileg azonnali tokenizálhatóságot és objektum-orientált struktúrát ad.
  - Frissítettük a `MASTER_CONCEPT.md` fájlt a véglegesített architektúrával.
