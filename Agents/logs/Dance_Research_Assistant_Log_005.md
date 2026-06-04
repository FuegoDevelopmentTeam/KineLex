# Dance Research Assistant Log - 005
Dátum: 2026-06-04
Kezdeti állapot: A MASTER_CONCEPT.md beolvasása és kibővítése a fogalom-evolúciós, lexikalizációs, szabály-hierarchia, ontológiai átrendezési és didaktikai scaffolding szabályokkal (10-11. fejezet).

## 1. Teljes Beszélgetéstörténet (AI_ready)
- **Kutatás fókusza:** Egy strukturált, redundancia-mentes, didaktikailag és nyelvileg konzisztens táncleíró nyelv és tudásbázis felépítése.
- **Fázisok és elért mérföldkövek:**
  1. **UAA 1.0 & Dot Case (Log_001):** Definiáltuk az L0-L3 absztrakciós szinteket. Bevezettük az Unified Abbreviation Algorithm (UAA) CPC-3 modellt és a pont-szeparációs (Dot Case) szintaxist a shift-mentes gépeléshez.
  2. **UAA 2.0 & Hibrid szintaxis (Log_002):** Integráltuk a testkondicionáló stretching, Floorwork, Jóga és On2 Rumba legacy kódokat. Kidolgoztuk az UAA 2.0-t (1-2 karakteres Core jelzők és kiejthetőségi kivételek, pl. `long` -> `lon`, `caminando` -> `cami`). Finomítottuk a Hibrid CamelCase & Dot Case rendszert a pont-burjánzás megelőzésére.
  3. **ASS Level 2 (Log_003):** Finomítottuk a Rövidítési Skála-Spektrumot (ASS) egy szigorú, 2-karakteres kiejtés-alapú (syllable/vocalization) szinttel (L1: `l`, L2: `le`, L3: `lft`, L4: `left`), amely optimális, ütközésmentes kódokat eredményez (pl. `leA` a `left arm` kifejezésre).
  4. **Általános-Speciális Tengely & Induktív Fogalom-Felfedezés (Log_004):** Megoldottuk a szintaktikai `>` ("to") operátor és a rendszer-szintű hierarchia szétválasztását, valamint megterveztük az induktív mintázat-felismerő és fogalom-ajánló motort.
  5. **Fogalom-Evolúció, Ontológiai Átrendezések & Didaktikai Scaffolding (Log_005):** Kidolgoztuk a kombinált fogalmak alapszóvá minősítésének szabályait, a 2hxR/2hxL szintaxist, a hanyag evolúciós defaults kezelését, a Szabály-Hierarchiát, a Szeparációs Állapotgépet, az **5s->5a (OntoShift)** ontológiai átrendezések és szemantikai elcsúszások HITL-alapú irányított migrációját, valamint a **Didaktikai Mankók fokozatos pontosításának (Scaffolding & Refinement)** elveit a CBL példa alapján.
- **Rendszerszintű Igazságok (MASTER_CONCEPT-be rögzítve):**
  - **Lexikalizáció:** Ha egy összetett kód (pl. `rHx`) eléri a használati küszöböt és jóváhagyást kap, CamelCase-mentes, tiszta kisbetűs alapszóvá minősül (`rhx`).
  - **Egykarakteres CamelCase szabály:** Egykarakteres alapelemekből álló összetett szavaknál nem kötelező a nagybetűs tagolás, ha a szóhatár egyértelmű (pl. `2hx`).
  - **2hxR és 2hxL Jelölés:** A kétkezes keresztfogásnál a domináns (felül lévő) kezet nagybetűvel jelöljük (`2hxR` = jobb felül, `2hxL` = bal felül). A kis `l` betű használata tilos, mert összetéveszthető az `1`-es számmal.
  - **Hanyag Evolúció (Defaults):** A tanárok használhatnak hanyag/rövidített formákat (pl. `rhxCar` az `rhxUpLaCar` helyett). A parser a háttérben az `is_default_representant` mező segítségével automatikusan feloldja a teljes jelentést.
  - **Szabály-Hierarchia (Rules Priority):** 1. No Collision, 2. DNA Consistency, 3. Lexical Integrity (Szeparációs Állapotgép karakterhossz-vizsgálattal: <=4 char egybeírt, >4 char de-coalesced Dot/CamelCase), 4. Pronounceability, 5. Max Compression.
  - **OntoShift Pipeline:** Ha egy régi bázisfogalomról kiderül, hogy az egy új, absztraktabb fogalom gyama (pl. `5s` -> `5a`), az `5s` nem törlődik, de a `parent_term_id` beállítódik az `5a`-ra. A rendszer felajánlja az irányított curriculum migrációt.
  - **Didaktikai Mankó és Fokozatos Pontosítás (Didactic Scaffolding):** A fogalmakat megértési állapotok szerint tároljuk (`scaffold`, `precise`, `deconstructed`). Kezdő szinten pontatlan, de intuitív fogalmakat tanítunk (pl. `CBL` mint egyszerű helycsere). Haladó szinten a **Refinement Engine** feloldja a tanuló profilján a scaffold fogalmat annak atomi, kinetikai és anatómiai valóságára (pl. `pathOpen`, `codDeact`, `wlk`, `reFace`, `bal.absorb`).

---

## 2. Beszélgetés Archívum
### Beszélgetés #5 (2026-06-04)
- **User kérése:**
  1. Olvassuk be újra a `MASTER_CONCEPT.md` fájlt, mert változott.
  2. Gyakran használt kombinált fogalmak alapelemi helyesírásúvá minősítése: Hogyan és mikor válhat egy összetett kód (pl. `rHx` $\rightarrow$ `rhx`) teljesen kisbetűs alapszóvá? Mi legyen az egykarakteres CamelCase szabálya?
  3. A 2hx (kétkezes keresztfogás) jobb/bal felül dilemmája: Hogyan jelöljük pontosan és vizuálisan ütközésmentesen, ha a jobb vagy a bal kéz van felül? (`2hxl` / `2hxL` vs `2hxr` / `2hxR`).
  4. Alapértelmezéses eset rövidebb (hanyag) megfogalmazásának evolúciója: Hogyan kezeljük szoftveresen, ha egy kezdetben full specifikus kód (`rhxUpLaCar`) a gyakorlatban "alapesetté" válik, és a tanárok elkezdik a hanyagabb, rövidebb `rhxCar` formában írni?
  5. Szabály-Hierarchia és Karakterhossz-konfliktusok: Mi történik, ha egy alapelem hosszváltozása (`h` -> `ha`) miatt az `rhx` -> `rhax` lesz? Hogyan kezeli a rendszer az ebből adódó ütközéseket és a szeparáció visszaállítását? Mikor kötelező a HITL (Human-in-the-loop) kapuőr?
  6. Ontológiai Átrendeződés és Szemantikai Elcsúszás: Hogyan kezeljük, ha egy rögzített kód (`5s` = 5 steps) a gyakorlatban kiderül, hogy egy nagyobb koncepció (`5a` = 5 accents) specifikus része? Hogyan fut le a migráció úgy, hogy a régi jelentés ne sérüljön a meglévő óráknál?
  7. Didaktikai Approximáció és Fokozatos Pontosítás: Hogyan kell kezelni a tanításban szükséges intuitív, de részleges kezdő fogalmakat és azok későbbi anatómiai, fizikai pontosítását a leíró nyelvben és a diákok skill matrixában (a CBL fizikai lebontásának példáján keresztül)?
- **Tervezés és Megoldások:**
  - **MASTER_CONCEPT Frissítés:** Bevezettük a **11. Szemantikai Ontológia, Didaktikai Approximáció és Fokozatos Pontosítás** fejezetet a megértési állapotokkal (`scaffold`, `precise`, `deconstructed`) és a CBL elméleti dekonstrukciójával.
  - **Szoftveres Implementáció:** Kidolgoztuk a **Refinement Engine** működését a didaktikai mankók fokozatos pontosítására és feloldására a diákok profilján és a tantervben.
