# Dance Research Assistant Log - 008
Dátum: 2026-06-07
Kezdeti állapot: A MASTER_CONCEPT.md frissítése a Vizuális Disambiguáció (font követelmény) és a dinamikus CamelCase ütközésfeloldás (Tömörítési Skála Elmozdulás) bevezetésével, miközben a Pont (.) operátort szigorúan csak a szülő-gyermek viszonyra (Parent-Child) korlátoztuk.

## 1. Teljes Beszélgetéstörténet (AI_ready)
- **Kutatás fókusza:** Egy struktúrált, redundancia-mentes, didaktikailag és nyelvileg konzisztens táncleíró nyelv és tudásbázis felépítése, valamint a köré épülő pedagógiai és kutatói szoftver-ökoszisztéma.
- **Fázisok és elért mérföldkövek:**
  1. **UAA 1.0 & Dot Case:** L0-L3 absztrakciós szintek; UAA CPC-3 modell; kezdeti pont-szeparáció.
  2. **UAA 2.0 & Hibrid szintaxis:** Kiejthetőségi kivételek (`caminando`->`cami`); CamelCase & Dot Case hibrid bevezetése a pont-burjánzás ellen.
  3. **ASS Level 2:** Rövidítési Skála-Spektrum (ASS) L1-L4; 2-karakteres kiejtés-alapú kódok bevezetése.
  4. **Induktív Felfedezés & Származtatás:** A `>` operátor időbeli szétválasztása az adatbázis-szintű hierarchikus származtatástól.
  5. **Ontológia & Ökoszisztéma:** Lexikalizáció, Szabály-Hierarchia, OntoShift, Scaffolding, Semantic Anchoring, Vizuális Annotáció, 3D Code-to-Motion, Predictive Gap Engine.
  6. **Fuzzy-to-Precise (v2.2.0):** 1. Fuzzy Inbox, 2. Refinement Engine, 3. Dynamic Resolution (Alias rendszer).
  7. **Felhasználási Gradiens & Pedagógia (v2.3.0):** Képesség alapú gradiens (Tanár vs Kutató), Pedagógiai modul (K1-K3 bizonyosság), Shakespeare-szonett probléma (koreográfia vs improvizáció), Atomizáció-vezérelt absztrakció.
  8. **Role-Aware Matrix & Retroactive Skill Distribution (v2.4.0 - v2.5.0):** Role (Leader/Follower) oszlop bevezetése. Visszamenőleges tudás-szétosztás a fuzzy fogalmak dekonstrukciójakor (Bizonyosság-örökítés, Idempotencia).
  9. **Vizuális Disambiguáció & CamelCase Ütközésfeloldás (v2.6.0):** Pont (.) operátor funkciójának korlátozása csak a szülő-gyermek/hierarchikus viszonyra. A CamelCase szóösszetételeknél az ütközésfeloldást a "Tömörítési Skála Elmozdulás" (Compression Shift Rule) kezeli, prioritást adva a Domain-Specifikus főnevek L2-re emelésének (pl. `l` + `a` = `la` ütközik a lady-vel -> `lAr` lesz, nem `leA`). Kötelező UI font szabályok (ligatúra nélküli monospace/sans-serif a kódolvashatóságért).

- **Rendszerszintű Igazságok (MASTER_CONCEPT-be rögzítve):**
  - **Nyelvi mag:** Lexikalizáció, egykarakteres CamelCase, Hanyag defaults.
  - **Szabály-Hierarchia & Compression Shift:** Ütközéskor CamelCase marad, a specifikus főnevet emeljük L2 szintre (`lAr` a `leA` helyett) a vizuális egyértelműségért. Pont csak hierarchiára használható (`body.arm.left`).
  - **Vizuális Disambiguáció (UI):** Szigorú frontend betűtípus követelmények az "l", "I" és "1" összetéveszthetősége miatt.
  - **OntoShift & Retroactive Distribution:** Absztraktabb szülő felfedezésekor a hierarchia frissül. Atomizáláskor a komponensek visszamenőleg jóváíródnak a diákok skill-mátrixában.
  - **Didaktikai Scaffolding & Dynamic Aliasing:** Megértési állapotok (scaffold/precise/deconstructed). Személyre szabott álnevek a tanároknak, Rosetta Stone a háttérben.
  - **Felhasználási Gradiens & Pedagógiai Modul:** T0-T4 jogosultsági rétegek. K1 (Részvétel), K2 (Begyakorlottság), K3 (Minőség) tudásszintek Role megjelöléssel. Improvizációs vs memorizált tudás megkülönböztetése.
  - **Koreográfia-Tervező & Vizuális Annotáció:** 2D térformáció, 3D avatár szimuláció, time-stamped vektor annotációk videókon.

---

## 2. Beszélgetés Archívum
### Beszélgetés #9 (2026-06-07)
- **User kérése:**
  - Vizuális font ütközések elkerülése a rendszer felületein (olyan Arial egyszerűségű betűtípus használata, ahol a kis "l" és a nagy "I" vizuálisan nem redundáns).
  - Javaslatkérés a 4. pont (Hibrid szeparáció) módosítására: a Pont (.) operátort a szeparáció helyett inkább szülő-gyermek viszony jelölésére szeretné használni.
  - Ütközés feloldása: `l` (left) + `a` (arm) -> `la` (ütközik a lady-vel). A `l.a` helyett tisztán CamelCase alapú elmozdulás a preferált. De melyik alak a jobb: `lAr`, `leA`, `leAr`? Milyen logika alapján válasszunk?
- **Tervezés és Megoldások:**
  - A `MASTER_CONCEPT.md`-ben új "Vizuális és UI Követelmények" szekció létrehozása a frontend (3_Agent_Frontend_Developer) felé: kötelező a "disambiguation-friendly" betűtípus használata.
  - A 4. fejezet (Hibrid szeparáció) módosítása: A pont operátort szigorúan korlátoztuk a szülő-gyermek és hierarchikus névterekre (pl. `body.arm.left`). Tilos szavak lineáris elválasztására használni.
  - A 6. fejezet (Dinamikus Ütközésfeloldó Protokoll) frissítése: Bevezetve a **Tömörítési Skála Elmozdulás (Compression Shift Rule)** és a **Domain-Specifikus Szó Prioritása**. Logika: Az univerzális minősítők (`l`, `r`) maximálisan tömör (L1) formában tartandók, mert ezek a legbiztosabb vizuális horgonyok. Ütközéskor a domain-specifikus főnevet emeljük fel L2 szintre. Ez alapján az `lAr` (L1 minősítő + L2 főnév) nyer a `leA` felett.
  - A verziószámot **v2.6.0**-ra emeltük.