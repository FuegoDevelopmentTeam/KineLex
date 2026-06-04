# Dance Research Assistant Log - 004
Dátum: 2026-06-04
Kezdeti állapot: A Hibrid (Inbox / Asszisztens) modell elfogadása; a szintaktikai `>` ("to") és a rendszer-szintű deriváció szétválasztása, valamint az Induktív Fogalom-Felfedezés szoftverarchitekturális megtervezése.

## 1. Teljes Beszélgetéstörténet (AI_ready)
- **Kutatás fókusza:** Egy szigorú, redundancia-mentes, didaktikailag strukturált és gépileg láncolható táncleíró nyelv és tudásbázis felépítése (Salsa, Bachata stílusokra).
- **Fázisok és elért mérföldkövek:**
  1. **UAA 1.0 & Dot Case (Log_001):** Definiáltuk az L0-L3 absztrakciós szinteket. Bevezettük az Unified Abbreviation Algorithm (UAA) CPC-3 modellt és a pont-szeparációs (Dot Case) szintaxist a shift-mentes gépeléshez.
  2. **UAA 2.0 & Hibrid szintaxis (Log_002):** Integráltuk a testkondicionáló stretching, Floorwork, Jóga és On2 Rumba legacy kódokat. Kidolgoztuk az UAA 2.0-t (1-2 karakteres Core jelzők és kiejthetőségi kivételek, pl. `long` -> `lon`, `caminando` -> `cami`). Finomítottuk a Hibrid CamelCase & Dot Case rendszert a pont-burjánzás megelőzésére.
  3. **ASS Level 2 (Log_003):** Finomítottuk a Rövidítési Skála-Spektrumot (ASS) egy szigorú, 2-karakteres kiejtés-alapú (syllable/vocalization) szinttel (L1: `l`, L2: `le`, L3: `lft`, L4: `left`), amely optimális, ütközésmentes kódokat eredményez (pl. `leA` a `left arm` kifejezésre).
  4. **Általános-Speciális Tengely & Induktív Fogalom-Felfedezés (Log_004):** Megoldottuk a szintaktikai `>` ("to") operátor és a rendszer-szintű hierarchia szétválasztását, valamint megterveztük az induktív mintázat-felismerő és fogalom-ajánló motort.
- **Rendszerszintű Igazságok (MASTER_CONCEPT-be rögzítve):**
  - **A `>` operátor szintaxisa:** Kizárólag időbeli átmenetet, célpozíciót vagy állapotváltozást ("TO" - pl. `laRt > Hlock`, `t > h`) jelöl a leírásokban.
  - **Rendszerszintű Deriváció:** Nem a nyers kódokban jelenik meg, hanem egy háttér- és adatbázisszintű meta-hierarchia (`parent_term_id` hivatkozás az alapesetre).
  - **Induktív Fogalom-Felfedezés (Pattern Recognition):** Ha több óravázlatban ismétlődnek azonos mozgássémák (pl. jobboldali nem-forgós áthaladások), a motor javasolja egy új absztrakt bázisszó bevezetését (pl. `rsp` = Right Side Pass) a szintaxis egyszerűsítésére.
  - **Combinatorial Variation Engine (CVE):** Új bázisszavak esetén a rendszer a megadott dimenziók mentén automatikusan legenerálja a logikailag lehetséges, de még nem létező variációkat, és `experimental` státusszal a "KutatóLab" felületre küldi őket.
  - **Core Inputok Életciklusa:** Külső stilisztikai minták `raw_input` $\rightarrow$ `deconstructed` (szilánkokra bontott) $\rightarrow$ `generalizing` (más környezetben tanított) $\rightarrow$ `fully_integrated` (standard szótárelem) életciklus-kezelése.
  - **Lineage Tracking:** A dekonstruált szilánkok a `term_lineage` táblán keresztül összeköttetésben maradnak az eredeti Core Input videós rekordjával.

---

## 2. Beszélgetés Archívum
### Beszélgetés #4 (2026-06-04)
- **User kérése:** 
  1. Döntés a Hibrid (Inbox / Asszisztens) modell mellett a meglévő 621 soros legacy adathalmaz kurálására.
  2. Az Általános - Speciális Tengely pontosítása: A `>` jel a leíró nyelvben nem származtatást, hanem **"TO"**-t, azaz egy pozícióhoz/állapothoz való átmenetet/folyamatot jelöl (pl. `2hLaLt > wrpCudPos`). A származtatás (alapeset vs. speciális verziók) tisztán egy háttér-fejlesztési és rendszerhierarchiás dolog.
  3. Induktív Absztrakció: A rendszer ne csak deduktív variációkat generáljon, hanem a sok szerteágazó, rögzített tananyagmintát észlelve javasoljon új bázis-fogalmakat, amelyek egyszerűsíthetik a szintaktikát (pl. ha sok jobboldali áthaladás van, javasolja a "jobboldali átvezetés" - `rsp` = Right Side Pass fogalom rögzítését).
  4. A "Core Inputok" (Challenge Sentences) kezelése: a videókból másolt, újszerű mozdulatsorok "szókincsbővítő" bemenetként érkeznek, amelyeket később darabokra (szilánkokra) bontva, más kontextusban gyakoroltatva kell generalizálni, de az eredetüket (honnan származnak) és az életciklusukat nyomon kell követni.
- **Tervezés és Megoldások:**
  - **MASTER_CONCEPT Frissítés:** Átdolgoztuk a **7. Általános - Speciális Tengely** fejezetet a szintaktikai `>` ("to") és a rendszerszintű deriváció szétválasztásával, valamint az **Induktív Fogalom-Felfedezés** és a **9. Didaktikai Hasonlatok, Történetek és Ciklikus Lengés-Dinamika** fejezetek beépítésével.
  - **Szoftverarchitektúra & Adatbázis sémák:**
    - Megterveztük a PostgreSQL sémát, amely tartalmazza a globális szótárat (ASS szintekkel, szülő-gyermek hierarchiával), a SaaS több-bérlős stúdió-szeparációt (`tenants`, `users` SSO-val), a tanterveket (`lessons` szintaktikai linterrel), és a diákok egyéni kompetencia-mátrixát (`student_skills`).
    - Bevezettük a `term_lineage` származási fát a Core Inputok és dekonstruált szilánkjaik összekapcsolására.
    - Definiáltuk a Kombinatórikus Kísérleti Motor (CVE) dimenzióit az új mozgások automatikus generálására és a kutatómunka támogatására.
    - Kiegészítettük a sémát a videó-mélylinkeléssel, didaktikai történetekkel és a kinetikus lengési tulajdonságokkal (`swg.dir`, `swg.trans`).
