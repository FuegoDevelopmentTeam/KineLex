# MASTER_CONCEPT (Dance Systems Architecture)
Version: 2.1.0 (Hybrid Pronounceable System with ASS)
Date: 2026-06-04

## 1. Rendszer-architektúra és Filozófia
Ez a dokumentum a "DANCE" projekt szoftverarchitektúrájának, fogalomterének és formális leíró nyelvének egyetlen igazságforrása (Source of Truth - SoT).
A rendszer fő célja, hogy egy rendkívül tömör, szótárral visszakövethető (dictionary-backed), de gépileg és emberileg egyaránt könnyen írható és olvasható hibrid kódrendszert biztosítson a solo és social páros táncok (különösen Salsa, Bachata) leírására.

---

## 2. Didaktikai Szintek (Abstraction Layers)
A mozgásokat négy absztrakciós szinten modellezzük:
1. **L0: Atomi / Geometriai és Anatómiai szint (Atoms & Primitives):** A testrészek (`occi`, `scr`), tér felosztás, síkok, eltolások, súlypont haladás, lépések és típusai, irányok és kéz-koordináták (`6h`, `12h`).
2. **L1: Alap Lépéssorozatok és Forgások (Solo Basic Step Patterns):** Alaplépések és forgások (`b`, `cng`, `baB`, `dblFrx`, `ai`,`cuca`,`tri`,`cami`) 
3. **L1: Kinetikus Simított Átmenetek (Smooth Transients - SM00-SM21):** Test- és vállhullámok, fej- és csípőkörzések rögzített listája.
4. **L2: Solo Sorok és Koreográfiák:** egyszemélyes mozdulatsorok.
5. **L3: Páros Alapfigurák:** vezetés-követéses rendszerű egyszerű alapelemek (pl. `CblLhxLaLTo`, `CBL`, `LaLT`, `maRT`,)
6. **L4: Páros Sorok és Koreográfiák (Choreographies):** Időben láncolt, összetett figurák és koreográfiák. 
---

## 3. Egységesített Rövidítési Algoritmus 2.0 (UAA 2.0)
A kódok hosszúságának radikális csökkentése érdekében egy kiterjesztett 1-2 karakteres bázist és egy kiejtés-barát (pronounceable) tömörítési szabályt alkalmazunk.

### A. Kiterjesztett Determinánsok és Jelzők (1-2 Karakteres zárt készlet)
Minden olyan fontos fogalom, amely gyakran szerepel jelzőként vagy összetett szavak tagjaként, fixen 1-2 karakteres rövidítést kap:

| Kategória | Fogalom (Eng) | UAA 2.0 kód | Legacy / Példa | Jelentés |
| :--- | :--- | :---: | :--- | :--- |
| **Szerepek** | Leader (Man)<br>Follower (Lady) | **`M`** (vagy `ma`) <br> **`F`** (vagy `la`) | `maLt`<br>`laLt` | Vezető szerep (Man)<br>Követő szerep (Lady) |
| **Irányok** | Right<br>Left<br>Front<br>Back<br>Up<br>Down<br>Diagonal | **`R`**<br>**`L`**<br>**`F`**<br>**`B`**<br>**`U`**<br>**`D`**<br>**`dg`** | `RT`<br>`LT`<br>`Fr`<br>`Ba`<br>`Up`<br>`Dwn`<br>`Diag` | Jobb<br>Bal<br>Elöl / Elülső<br>Hátul / Hátulsó<br>Fent / Felső<br>Lent / Alsó<br>Diagonál (45 fok) |
| **Anatómia** | Arm<br>Hand<br>Leg<br>Foot<br>Shoulder<br>Hip<br>Knee<br>Head<br>Torso | **`a`**<br>**`h`**<br>**`lg`**<br>**`ft`**<br>**`sh`**<br>**`hp`**<br>**`k`**<br>**`hd`**<br>**`to`** | `arm`<br>`hand`<br>`leg`<br>`foot`<br>`sh`<br>`hip`<br>`knee`<br>`head`<br>`torso` | Kar / Karív<br>Kéz<br>Láb / Lábszár<br>Lábfej<br>Vállöv<br>Csípő<br>Térd<br>Fej<br>Törzs |
| **Jelzők** | Elevated / Elevation<br>Deep / Double<br>Single<br>Triple | **`el`**<br>**`dp`**<br>**`sgl`**<br>**`trpl`** | `elRT` / `elevated`<br>`dpCbl` / `dbl`<br>`sgl`<br>`trpl` | Emelt / Emelés (Váll/Kar)<br>Mély / Dupla<br>Egyszerű / Szóló<br>Tripla |
| **Ritmus** | Basic 1st Half<br>Basic 2nd Half | **`b1`**<br>**`b2`** | `b1`<br>`b2` | Alaplépés 1-4. üteme<br>Alaplépés 5-8. üteme |

### B. Standard Fogalmak: CPC-3 + Kimondhatósági Kivétel (Pronouncability Exception)
Minden egyéb szót alapesetben a 3-betűs mássalhangzó-kiemeléssel (CPC-3) rövidítünk, **DE** ha az így kapott kód nehezen kimondható vagy nem elég intuitív az összetett szavakban, akkor a **kimondhatóság és a hagyomány kedvéért prefix/fonetikus csonkolást** használunk:
- `long` -> **`lon`** (Pronounceable - pl. `lon.a` = long arm, sokkal jobban kimondható és jobban hangzik, mint az `lng.a` / `lngA`)
- `short` -> **`sho`** (Pronounceable - pl. `sho.a` = short arm, a nehezen kiejthető `shr` helyett)
- `double` -> **`dbl`** (CPC-3 kivétel)
- `triple` -> **`trpl`** (CPC-3 kivétel)
- `caminando` -> **`cami`** (Megtartott hagyaték)
- `cucaracha` -> **`cuca`** (Megtartott hagyaték)
- `mambo` -> **`mambo`** (Rövid bázisszó, változatlan)
- `conga` -> **`conga`** (Rövid bázisszó, változatlan)
- `aida` -> **`aida`** (Rövid bázisszó, változatlan)

---

## 4. Hibrid CamelCase és Pont (Dot) Szeparációs Nyelvtan
A kódok láncolatának minimalizálása érdekében egy **hibrid rendszert** alkalmazunk: ahol a CamelCase egyértelmű és kompakt, ott azt használjuk; ahol viszont kétértelműség vagy strukturális tagolás szükséges, ott kötelezően bevezetjük a pont (`.`) vagy space (` `) karakterek használatát.

### A. Mikor használunk CamelCase-t? (Takarékos láncolás)
A CamelCase-t használjuk a bázisszavak és standard jelzőik közvetlen összekapcsolására, amennyiben a szóhatár nagybetűvel egyértelműen elkülönül:
- `bwdCami` (Backward Caminando - nem kell pont, mert a `C` nagybetű jelzi a határt).
- `elRT` (Elevated Right Turn - az `el` kisbetűs jelzőt követi a nagybetűs `RT` turn bázisszó).
- `frxMam` (Front Cross Mambo).
- `dblFrx` (Double Front Cross).
- `rhx` / `lhx` (Right/Left Hand Crossed - a kis `x` jelzi a keresztezést).

### B. Mikor kötelező a Pont (`.`) használata? (Ambivalencia-szűrés)
A pont szeparátor használata kizárólag az alábbi esetekben kötelező:
1. **Egybetűs Core kódok összeolvadásának megakadályozására (Collision Prevention):**
   - *Példa:* Amikor nem akarunk CamelCase-t használni, de meg kell előznünk a kétszavas ütközést (pl. `l` (left) + `a` (arm) egybeírva `la` lenne, ami ütközik a `la` = lady/follower bázisszóval. Ha nem akarunk CamelCase-t (pl. `lAr`), akkor kötelező a pont: **`l.a`**).
2. **Hierarchikus és anatómiai névterek elhatárolására (Anatomical Namespaces):**
   - *Példa:* A gerinc végpontjainak és eltolásainak leírásakor: **`occi.shift.lat`** (occiput lateral shift) vagy **`scr.circle.hor`** (sacrum horizontal circle). Ez azonnal tokenizálható és tisztán olvasható.
3. **Állapotváltozások és Specifikációk (Properties & Types):**
   - *Példa:* Forgások és kivezetéseik specifikálásakor: **`LLT.TO`** (Lady Left Turn with Turnout) vagy **`MRT.TB`** (Man Right Turn with Turnback).
4. **Két azonos típusú CPC-3 kód közvetlen találkozásakor:**
   - *Példa:* Ha két standard 3-betűs vagy kiejtés-barát kód áll egymás mellett, és egybeírásuk nehezen olvasható szótömböt alkotna: **`gld.sld`** (glide and slide).

### C. Egyéb elválasztó operátorok
- ` - ` (Dash spaces): Időbeli szekvencia (egymás utániság), pl. `b1 - CBL - LLT`.
- ` & ` vagy ` + `: Egyidejű (szimultán) akciók, pl. `M:cbl & F:spin` vagy `lh.b & rh.b`.
- `:` (Colon): Szerepek vagy bázis-időzítések megjelölésére, pl. `1:step` (az első ütésre történő lépés).

---

## 5. Ritmikai Szintaktika (Megtartandó Hagyaték)
A 8-ütéses (8/4) ritmusú rendszerek zenei szintaktikáját szigorúan megőrizzük:
- **Ritmikai képlet:** **`12 + 34 & 56 + 78`**
- **Időzítési marker (`X:`):** A számérték és a kettőspont jelzi a konkrét zenei ütésre történő mozgásokat.
  - **`1:`** = Első ütésre történő mozgás (pl. `1:bwdCami` - lépés hátra az 1-re).
  - **`5:`** = Ötödik ütésre történő mozgás.
  - **`5&6:`** = Siettetett, sűrített ütemek (hastening).

---

## 6. Rövidítési Skála-Spektrum (ASS - Abbreviation Scale Spectrum)
A pontok felesleges burjánzásának elkerülésére és a kódok hosszának minimalizálására bevezetjük a **Rövidítési Skála-Spektrum (ASS)** elvét. Minden fogalomhoz a szótárban több tömörítési szintet rendelünk hozzá, amelyek között a rendszer szükség esetén lépked.

### A. Minimalizált Tömörítési Szintek (Spectrum Layers):
1. **Level 1 (Core - 1 karakter):** A leggyakoribb báziselem kód (pl. `left` -> **`l`**, `right` -> **`r`**, `arm` -> **`a`**).
2. **Level 2 (Strictly Minimal 2-char - 2 karakter):** Kétkarakteres kiejtés-barát (syllable-based) vagy mássalhangzós kód. **Elsőbbséget élvez az L3/L4 előtt.**
   - *Példa:* `left` $\rightarrow$ Kétkarakteres opciók: `le` (vocalized) és `lf` (consonant). **A kiejthető `le` az abszolút győztes!**
   - *Példa:* `arm` $\rightarrow$ **`ar`**.
3. **Level 3 (Standard / CPC-3 - 3 karakter):** CPC-3 alapú mássalhangzós kód vagy pronouncable szótag (pl. `left` -> **`lft`**, `glide` -> **`gld`**).
4. **Level 4 (Full Word):** A fogalom teljes angol kiírása (pl. `left`, `arm`).

### B. Dinamikus Ütközésfeloldó Protokoll (Collision Resolution):
Amikor összetett szavakat képzünk (pl. *Left Arm*), a szoftveres motor megpróbálja az L1 + L1 formát alkalmazni. Ha ütközést észlel egy bázisfogalommal, **fokozatosan, a minimális karakterhosszúság elvét szem előtt tartva** lépteti fel az egyik szót a 2-karakteres szintre (Level 2), megkeresve a legjobban kimondható opciót:

- **Kísérlet #1 (L1 + L1):** `l` + `a` $\rightarrow$ **`la`**
  - *Státusz:* **ÜTKÖZIK** a `la` (lady / follower) bázisszóval. -> **REJECTED**
- **Kísérlet #2 (L2-Left + L1-Arm):** `left` L2 formája (`le`) + `arm` L1 formája (`a`) $\rightarrow$ **`leA`**
  - *Státusz:* **NINCS ÜTKÖZÉS!** Rendkívül tömör, mindössze 3 karakteres, kiválóan kimondható és egyértelmű kód. -> **ACCEPTED (WINNER)**

Ez a megközelítés garantálja, hogy a kódok mérete mindig a lehető legkisebb maradjon, de 100%-ban ütközésmentes és jól verbalizálható formát öltsön.

---

## 7. Általános - Speciális Tengely és Induktív Fogalom-Felfedezés (General-Specific Axis & Inductive Discovery)
A rendszer támogatja a mozgások általános-speciális hierarchiájának kezelését, elkerülve a redundanciát, és támogatva az oktatást mind deduktív (kísérleti variációk ajánlása), mind induktív (új alapfogalmak javaslása a gyakori sémák alapján) módon.

### A. Szintaktika és a `>` Operátor (Syntactic vs. System Derivation)
Szigorúan elhatároljuk a leíró nyelv szintaktikai operátorait a rendszer-szintű hierarchiától:
1. **A `>` operátor jelentése a nyelvtanban: "TO" (Valahová / Valamivé tartó folyamat):**
   - A leírásokban a `>` nem származtatást jelent, hanem **időbeli átmenetet, pozíció- vagy állapotváltozást**:
     - *Példa:* `2hLaLt > wrpCudPos` $\rightarrow$ kétkezes lady bal forgás **cuddle pozícióba** (transition to cuddle position).
     - *Példa:* `laRt > Hlock` $\rightarrow$ lady jobb forgás **hand-lock tartásba**.
     - *Példa:* `t > h` $\rightarrow$ tail **to** head (faroktól a fejig).
2. **A származtatás egy rendszer- és adatbázis-szintű meta-kapcsolat:**
   - A tanár az órai leírásban nem ír származtatást. A háttérrendszer az, amely metaadatként összeköti a speciális kifejezéseket a hozzájuk tartozó didaktikai alapesettel (Base Case):
     - *Példa:* A `2hCruz_with_collector_and_rebound` nevű éles szótári fogalom a háttérben (az adatbázis `parent_term_id` mezőjén keresztül) származik a `2hCruz` (kétkezes párhuzamos Cruzados) alapesetből. Ez a hierarchia a tanárok felületén és a diákok fejlődési útvonalán jelenik meg, nem a nyers kódokban.

### B. Induktív Absztrakció / Új Fogalom Ajánló Motor (Inductive Concept Discovery)
Ha a tanárok sok különböző, részletes és szerteágazó variációt visznek fel az óravázlatokba, a rendszer háttérelemzője (pattern-recognition engine) elkezdi vizsgálni a mintázatokat a "szövegkörnyezetben":
1. **Mintázat-felismerés:** Ha több óravázlatban is szerepelnek olyan összetett leírások, amelyekben azonos anatómiai és geometriai lépések ismétlődnek (pl. a jobb oldalon keresztüli, irányváltás nélküli egyszerű áthaladások és helycserék).
2. **Fogalom-ajánlás (Conceptual Abstraction):** A rendszer észleli ezt a közös strukturális karaktert, amelynek még nincs önálló, tömör kódja a szótárban, és **felajánlja a tanárnak egy új bázis-fogalom bevezetését**:
   - *Példa:* *"Észleltük, hogy az elmúlt 5 órán 8 olyan variációt rögzített, amelyek a jobb oldalon keresztüli, nem irányváltós áthaladást tartalmazzák. Javasoljuk a 'jobboldali átvezetés' (rsp = Right Side Pass) fogalmának bevezetését. Ezzel az Ön leírásainak szintaktikai hossza átlagosan 35%-kal csökkenthető (pl. a bonyolult 'laWlk.r.side.noTurn' helyett egyszerűen az 'rsp' használható)."*

### C. Kombinatórikus Kísérleti Motor (Combinatorial Variation Engine - CVE)
A kutatás-fejlesztés felgyorsítására a rendszer tartalmaz een logikai generátort, amely az alábbi dimenziók mentén állít elő új, még nem létező variációkat (kísérleti feladatokat):
*   **Variációs Dimenziók:**
    1.  *Kapcsolódás (Hold):* `2h`, `1h`, `rhx`, `lhx`, `noHand`
    2.  *Pálya (Path/Trajectory):* `colUp`, `colDw`, `straight`, `loop`, `wave`, `zigzag`
    3.  *Dinamika (Dynamics/Technique):* `rebd` (rebound), `sld` (slide), `gld` (glide), `cod` (change of direction)
    4.  *Kivezetés (Ending):* `TO` (turnout), `TB` (turnback), `End` (alt end)
*   **Kísérleti Generáció:** A CVE egy bázis kifejezéshez (pl. `Cruz`) kiszámítja az összes logikailag lehetséges, de az adatbázisban még **NEM** szereplő variációt.
*   **Status: Experimental:** Ezeket a generált kódokat a rendszer `status = 'experimental'` címkével menti el, és a tanárok felületén a **"KutatóLab" / "Research Lab"** panelen jeleníti meg, mint kikísérletezendő, új didaktikai kihívásokat.

---

## 8. Core Inputok és Dekonstrukciós Életciklus (Core Inputs & Deconstruction)
A tánctanárok gyakran külső, karakteres mintákat (pl. Instagram videók, fesztivál workshopok anyagai) használnak a tananyag fejlesztéséhez. Ezeket a rendszer nem szimpla variációként, hanem **"Core Inputként" (Challenge Sentences)** kezeli, amelyek célja a stúdió saját "szókincsének" (vocabulary) bővítése és későbbi dekonstrukciója.

### A. A Core Input Életciklus (Core Input Lifecycle)
Minden új, külső forrásból származó komplex szekvencia az alábbi fázisokon megy keresztül:
1.  **Raw Input (Akvizíció / Beviteli fázis):**
    *   A tanár rögzíti a külső forrást (videó link, időzítés, stílus, eredeti előadó) és felviszi a komplex, egybefüggő leírást.
    *   *Státusz:* `raw_input`
2.  **Deconstructed (Dekonstrukció / Felbontás):**
    *   A tanár a szerkesztőfelületen darabokra, atomi mozdulatokra (L0/L1 szilánkokra) bontja a Core Inputot.
    *   *Státusz:* `deconstructed`
3.  **Generalizing (Generalizáció / Integráció):**
    *   A dekonstruált szilánkokat önállóan, más kontextusban, más alapfigurákhoz csatolva is tanítják a kurzusokon (pl. az eredeti core mintamondatból kiemelt egyedi vállhullám-irányváltást ráépítik egy standard `CBL`-re).
    *   *Státusz:* `generalizing`
4.  **Fully Integrated (Beépült tudás):**
    *   A szilánkok önálló szócikként beépülnek a standard szótárba. A tanulók tudásmátrixában már nem a teljes core mondat, hanem a különálló elemek szerepelnek mint elsajátított improvizációs készségek.
    *   *Státusz:* `fully_integrated`

### B. Származási Fa és Visszakövethetőség (Lineage Tracking)
A rendszer egy származtatási táblán (`term_lineage`) keresztül összeköti a dekonstruált szilánkokat az eredeti Core Input rekorddal:
```
[Core Input: "Viral On2 Rumba Challenge 2026"]
       │
       ├───> [Fragment A: "rsh.sh.wave"] (Vállhullám rumba stílusban)
       ├───> [Fragment B: "cod.rebd.step"] (Irányváltós rebound lépés)
       └───> [Fragment C: "lhx.colDw"] (Keresztezett bal kezes collector lefelé)
```
Ezáltal az órai videóösszefoglalók naplózásakor és a tanulók tudásmátrixának elemzésekor a rendszer pontosan ki tudja mutatni:
- Milyen új **Core Inputokkal** bővült az iskola szókincse.
- Egy-egy órán tanított figura egyszerű variációs ismétlés-e, vagy egy Core Input dekonstruált szilánkja.
- Melyik eredeti videós forrásból származik az a stílusjegy, amit a diák éppen sikeresen elsajátított.

---

## 9. Didaktikai Hasonlatok, Történetek és Ciklikus Lengés-Dinamika (Didactics & Cyclical Swing Dynamics)
A tánctanítás hatékonyságát a száraz kódok mellett a mögöttük rejlő történetek, vizuális hasonlatok és mély fizikai igazságok adják. A rendszer ezeket elsőrendű entitásként kezeli, összekötve a leíró nyelv szintaxisával.

### A. Didaktikai Hasonlatok és Történetek (Metaphors & Stories)
Minden fogalomhoz (`terms`) és mozgássorhoz hozzárendelhető egy vagy több didaktikai hasonlat (metaphor), sztori (story) és magyarázó kép, amelyek segítik a megértést:
- *Példa (Ciklikusság és az Autó-motor hasonlat):*
  - **Koncepció:** A Salsa és a Bachata ciklikus táncok. Rendelkeznek önmagukba visszatérő alaplépéssel (self-returning basic step).
  - **Hasonlat:** Az alaplépés olyan, mintha egy autó motorját indítanánk be, ahol a Follower (nő) az autó, a Leader (férfi) pedig a vezető. A motor beindul, de az autó egy helyben jár (ciklikus üzemmód), amíg el nem kezdjük irányítani.

### B. Inga- és Lengőmozgások Dinamikája (Pendulum Swing Dynamics - `swg.dir`)
A Salsa és Bachata alaplépései **inga- vagy lengőmozgások** (swinging/pendulum-type movements). Az inga egy adott zenei ütemre/lépésre vált irányt.
A leíró nyelvben ezt a **`swg.dir`** (swinging direction) tulajdonsággal és a **`chg`** (change) időzítési markerrel fejezzük ki:
1. **Salsa On1:** Az első lépésre/ütemre vált irányt az inga:
   - **`swg.dir.chg:1`** (Swing direction changes on beat 1).
2. **Salsa On2:** A második lépésre/ütemre vált irányt az inga:
   - **`swg.dir.chg:2`** (Swing direction changes on beat 2).
3. **Bachata:** A harmadik lépésre/ütemre vált irányt az inga:
   - **`swg.dir.chg:3`** (Swing direction changes on beat 3).

### C. Dinamikus Lengés-Átmenetek (Swing Transitions - `swgDir2` / `swg.trans`)
A felhasználó által felvetett `swgDir2` problémája arra az átmenetre utal, amikor az éppen táncolt alaplépés lengésiránya (és annak fázisa) átvált a következő lépés vagy figura lengésirányára.
Ezt a rendszerszinten **`swg.trans`** (swing transition) metaadattal modellezzük, amely leírja:
1. Az aktuális alaplépés lengési állapotát (Current Swing Phase).
2. A soron következő mozdulat elvárt induló lengési állapotát (Target Swing Phase).
3. A kettő közötti kinetikus fázisillesztést (Phase Alignment / Transition Mode), biztosítva a folyamatos, zökkenőmentes és természetes mozgásáramlást (flow).

---

## 10. Fogalom-Evolúció: Kombinált- és Alapértelmezett Szavak (Concept Evolution & Defaults)
A kutatási fázisban lévő leíró nyelv természetes tulajdonsága, hogy használat közben a fogalmak sűrűsödnek (tömörödnek), és bizonyos specifikus esetek alapértelmezetté (default) válnak. A rendszernek ezt a kognitív és nyelvészeti evolúciót szoftveresen is támogatnia kell.

### A. Kombinált Fogalmak Alapelemivé Minősítése (Lexicalization of Compounds)
Amikor több egykarakteres vagy kétkarakteres alapelemből (pl. `r` = right, `h` = hand, `x` = cross) gyakran ismétlődő kombinációt hozunk létre, a szigorú CamelCase szabályok szerinti írásmód (pl. `rHx`) nehezebbé teszi a gyors olvasást (madártávlat / high-level view).
1. **A Lexikalizáció feltételei:** Egy összetett kód akkor minősíthető át CamelCase-mentes, tiszta kisbetűs önálló alapszóvá (pl. `rhx`), ha:
   - A kifejezés a tananyagokban és óravázlatokban elér egy bizonyos használati gyakoriságot (Frequency Threshold, pl. legalább 15-ször előfordul).
   - Az emberi kontroll (Human Control) jóváhagyja az átminősítést.
2. **Az egykarakteres CamelCase szabály (Single-Character Edge Case):**
   - Ha egy összetett szó egykarakteres alapelemekből áll, **NEM kötelező** minden egyes tagot nagybetűvel kezdeni.
   - Elegendő csak a legfőbb strukturális határt nagybetűvel jelölni, vagy ha a szó már lexikalizálódott, teljesen kisbetűvel írni:
     - *Példa:* `rhx` (Right Hand Cross) és `lhx` (Left Hand Cross) $\rightarrow$ teljesen kisbetűs, mert bevált, önálló atomi fogalmakká váltak.
     - *Példa:* `2hx` (Two Hands Crossed) $\rightarrow$ teljesen kisbetűs, mert a szám (`2`) és a betűk határa nagybetű nélkül is egyértelmű.

### B. A 2hx (Kétkezes Keresztfogás) Jobb/Bal Felül Dilemmája
A kétkezes keresztfogásnak (`2hx`) didaktikailag két fajtája van attól függően, hogy melyik kéz helyezkedik el felül. Ennek pontos és ergonomikus jelölése:
1. **2hxR (Right over Left):** Kétkezes keresztfogás, ahol a **jobb kéz van felül**.
   - *Szintaxis:* **`2hxR`** (A nagy `R` jelzi a domináns, felül lévő kezet. Ergonomikus, mert a nagybetű azonnal felhívja magára a figyelmet a madártávlati olvasáskor).
2. **2hxL (Left over Right):** Kétkezes keresztfogás, ahol a **bal kéz van felül**.
   - *Szintaxis:* **`2hxL`** (A nagy `L` jelzi a felül lévő kezet).
3. **Miért hibás a csupa kisbetűs forma (`2hxl` / `2hxr`)?**
   - A kis `l` betű könnyen összetéveszthető az `1`-es számmal (pl. `2hx1`), ami ritmikai vagy darabszámbeli zavart okozhat a kódértelmezőben és az emberi szemnek is. A nagy `L` és `R` használata ezt a vizuális ütközést 100%-ban kiküszöböli.

### C. Alapértelmezett Esetek Hanyag/Rövidített Evolúciója (Defaulting & Contextual Shorthands)
A kutatás kezdetén a tanárok mindent a legmagasabb részletességgel (max detail zoom) írnak le. Később a leggyakoribb variációk "alapesetté" válnak, és a nyelvben ráragad a "sima" jelző, elhagyva a felesleges specifikációkat.
1. **A Hanyag Evolúció (Shorthand Evolution) működése:**
   - *Kezdeti fázis (Full Specification):* `rhxUpLaCar` (Right hand cross, upper lady's caress).
   - *Evolúciós fázis (Shorthand):* Ha nincs más caress megadva, az `rhxCar` automatikusan a leggyakoribb `rhxUpLaCar`-t jelenti.
2. **Szoftveres támogatás (Context-Aware Defaults):**
   - A szótárban minden fogalomhoz hozzárendelhető egy **`is_default_representant`** (alapeset reprezentáns) logikai mező.
   - Ha a tanár az óravázlatban a rövidebb `rhxCar` kódot használja, a parser a háttérben automatikusan feloldja azt a teljes `rhxUpLaCar` jelentésre, de a képernyőn meghagyja a tanár által preferált rövid, kényelmes formát.
   - Ha később egy új, alternatív caress jelenik meg (pl. `rhxLoLaCar` - lower caress), azt kötelezően ki kell írni, de a régi alapeset továbbra is megmaradhat a hanyag `rhxCar` alakban.



