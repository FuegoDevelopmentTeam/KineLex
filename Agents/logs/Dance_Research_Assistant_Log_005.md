# Dance Research Assistant Log - 005
Dátum: 2026-06-04
Kezdeti állapot: A MASTER_CONCEPT.md beolvasása és kibővítése a fogalom-evolúciós, lexikalizációs és alapértelmezési szabályokkal (10. fejezet).

## 1. Teljes Beszélgetéstörténet (AI_ready)
- **Kutatás fókusza:** Egy strukturált, redundancia-mentes, didaktikailag és nyelvileg konzisztens táncleíró nyelv és tudásbázis felépítése.
- **Fázisok és elért mérföldkövek:**
  1. **UAA 1.0 & Dot Case (Log_001):** Definiáltuk az L0-L3 absztrakciós szinteket. Bevezettük az Unified Abbreviation Algorithm (UAA) CPC-3 modellt és a pont-szeparációs (Dot Case) szintaxist a shift-mentes gépeléshez.
  2. **UAA 2.0 & Hibrid szintaxis (Log_002):** Integráltuk a testkondicionáló stretching, Floorwork, Jóga és On2 Rumba legacy kódokat. Kidolgoztuk az UAA 2.0-t (1-2 karakteres Core jelzők és kiejthetőségi kivételek, pl. `long` -> `lon`, `caminando` -> `cami`). Finomítottuk a Hibrid CamelCase & Dot Case rendszert a pont-burjánzás megelőzésére.
  3. **ASS Level 2 (Log_003):** Finomítottuk a Rövidítési Skála-Spektrumot (ASS) egy szigorú, 2-karakteres kiejtés-alapú (syllable/vocalization) szinttel (L1: `l`, L2: `le`, L3: `lft`, L4: `left`), amely optimális, ütközésmentes kódokat eredményez (pl. `leA` a `left arm` kifejezésre).
  4. **Általános-Speciális Tengely & Induktív Fogalom-Felfedezés (Log_004):** Megoldottuk a szintaktikai `>` ("to") operátor és a rendszer-szintű hierarchia szétválasztását, valamint megterveztük az induktív mintázat-felismerő és fogalom-ajánló motort.
  5. **Fogalom-Evolúció & Alapértelmezések (Log_005):** Kidolgoztuk a kombinált fogalmak alapszóvá minősítésének (lexikalizáció) szabályait, feloldottuk a 2hx jobb/bal felül dilemmáját, és megterveztük a hanyag/rövidített alapértelmezések szoftveres kezelését.
- **Rendszerszintű Igazságok (MASTER_CONCEPT-be rögzítve):**
  - **Lexikalizáció:** Ha egy összetett kód (pl. `rHx`) eléri a használati küszöböt és jóváhagyást kap, CamelCase-mentes, tiszta kisbetűs alapszóvá minősül (`rhx`).
  - **Egykarakteres CamelCase szabály:** Egykarakteres alapelemekből álló összetett szavaknál nem kötelező a nagybetűs tagolás, ha a szóhatár egyértelmű (pl. `2hx`).
  - **2hxR és 2hxL Jelölés:** A kétkezes keresztfogásnál a domináns (felül lévő) kezet nagybetűvel jelöljük (`2hxR` = jobb felül, `2hxL` = bal felül). A kis `l` betű használata tilos, mert összetéveszthető az `1`-es számmal.
  - **Hanyag Evolúció (Defaults):** A tanárok használhatnak hanyag/rövidített formákat (pl. `rhxCar` az `rhxUpLaCar` helyett). A parser a háttérben az `is_default_representant` mező segítségével automatikusan feloldja a teljes jelentést, de a felületen megőrzi a rövid formát.

---

## 2. Beszélgetés Archívum
### Beszélgetés #5 (2026-06-04)
- **User kérése:**
  1. Olvassuk be újra a `MASTER_CONCEPT.md` fájlt, mert változott.
  2. Gyakran használt kombinált fogalmak alapelemi helyesírásúvá minősítése: Hogyan és mikor válhat egy összetett kód (pl. `rHx` $\rightarrow$ `rhx`) teljesen kisbetűs alapszóvá? Mi legyen az egykarakteres CamelCase szabálya?
  3. A 2hx (kétkezes keresztfogás) jobb/bal felül dilemmája: Hogyan jelöljük pontosan és vizuálisan ütközésmentesen, ha a jobb vagy a bal kéz van felül? (`2hxl` / `2hxL` vs `2hxr` / `2hxR`).
  4. Alapértelmezéses eset rövidebb (hanyag) megfogalmazásának evolúciója: Hogyan kezeljük szoftveresen, ha egy kezdetben full specifikus kód (`rhxUpLaCar`) a gyakorlatban "alapesetté" válik, és a tanárok elkezdik a hanyagabb, rövidebb `rhxCar` formában írni?
- **Tervezés és Megoldások:**
  - **MASTER_CONCEPT Frissítés:** Bevezettük a **10. Fogalom-Evolúció: Kombinált- és Alapértelmezett Szavak** fejezetet, amely részletesen szabályozza a lexikalizációt, a 2hxR/2hxL szintaxist és a kontextus-érzékeny alapértelmezések (hanyag evolúció) működését.
  - **Adatbázis & Parser Támogatás:** A `terms.is_default_representant` logikai mező segítségével a parser képes lesz a hanyag kódokat a háttérben automatikusan kiegészíteni a teljes specifikus jelentésre, így biztosítva a veszteségmentes adatfeldolgozást és a kényelmes tanári gépelést.
