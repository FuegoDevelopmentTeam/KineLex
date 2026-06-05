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

### D. Komponens-Öröklődés és Szemantikus Refaktorizációs Pipeline (Dependency DNA & Refactoring)
A legnagyobb kockázat a lexikalizált szavaknál (pl. `rhx`), hogy ha a háttérben megváltoztatunk egy alapelemet (pl. a `h` = hand helyett bevezetjük a `ha` kódot, hogy a `h` felszabaduljon a `hip` részére), a sima szöveges keresés és csere (`Search & Replace`) működésképtelen vagy veszélyes lesz, mert nem látja a kapcsolatot az `rhx` és a `h` között.

Ennek feloldására egy szigorú **szemantikus függőségi gráfot** és egy **refaktorizációs fordítómotort (Compiler)** alkalmazunk:

1. **A Szemantikai "DNA" (Összetételi Képlet) tárolása:**
   Minden olyan kifejezés, amely más alapelemekből áll össze (legyen az CamelCase vagy már teljesen lexikalizált, egybeírt `rhx`), az adatbázisban **soha nem veszíti el a szülő-gyermek kapcsolatát**. 
   A szótárban minden összetett kifejezés mögött kötelezően tároljuk annak **strukturális képletét (DNA Recipe)** egy relációs táblán keresztül (`term_components`):
   ```
   [Compound Term: rhx] (id: UUID_rhx)
          │
          ├───> [Index 1]: UUID_r (r = right)
          ├───> [Index 2]: UUID_h (h = hand)
          └───> [Index 3]: UUID_x (x = cross)
   ```
   Az `rhx` nem egy statikus karakterlánc, hanem a háttérben az **`r`**, **`h`** és **`x`** fizikai fogalmak UUID-fókusza.

2. **Absztrakt Szintaxisfa (AST) Alapú Értelmezés:**
   A leíró nyelven írt óravázlatokat a rendszer nem sima szövegként kezeli, hanem egy **AST Parser** segítségével lefordítja egy objektum-orientált fává, ahol minden egyes elem és karakter a mögöttes adatbázis-beli UUID-jára mutat.

3. **Az Automatikus Refaktorizációs Pipeline (The Migrator):**
   259|   Amikor a fejlesztő vagy a tanár megváltoztat egy alapelemet (pl. `h` $\rightarrow$ `ha` szabály):
   260|   - **Step 1 (Impact Analysis):** A rendszer a `term_components` tábla segítségével azonnal kilistázza az összes érintett összetett és lexikalizált szót (pl. kimutatja, hogy a `h` módosítása érinti a `rhx`, `lhx`, `2hx` kifejezéseket).
   261|   - **Step 2 (Auto-Regeneration):** A refaktorizációs motor a képletek alapján automatikusan újragenerálja a lexikalizált szavak új karaktersorozatát: `r` + `ha` + `x` $\rightarrow$ **`rhax`**.
   262|   - **Step 3 (Semantic Migration):** A rendszer végigfut az összes adatbázis-rekordon (óravázlatok, videó-időbélyegek leírásai). Minden szöveget lefordít AST fává, elvégzi az UUID-alapú token-cseréket (pl. `rhx` helyett `rhax`), majd visszamenti a frissített szöveget.
   263|   - **Step 4 (Safety Validation):** A linter ellenőrzi az új nevek biztonságát (lásd 10. E. szabály-hierarchiát az ütközések és stílusváltások kezelésére).

Ez a háttér-architektúra garantálja a **100%-os skálázhatóságot és biztonságot**: a nyelvtan és a kódok szabadon és hanyagul fejlődhetnek a felszínen, miközben a motor mélyén az adatintegritás és a visszakövethetőség matematikai precizitással megmarad.

### E. Komponens-Módosulási Konfliktusok és Szabály-Hierarchia (Conflict Resolution & Rules Priority)
Ha egy alapelem módosítása (pl. `h` $\rightarrow$ `ha`) megváltoztatja egy lexikalizált vagy összetett szó hosszát, ez közvetlenül ütközhet a tömörségi, helyesírási és szeparációs szabályokkal. Ennek feloldására egy szigorú, prioritás-alapú döntési hierarchiát és egy "Human-in-the-loop" (HITL) kapuőrzési folyamatot alkalmazunk.

#### 1. Rendszerszintű Szabály-Hierarchia (Rules Priority)
Amikor a refaktorizációs compiler újragenerál egy kódot, az alábbi prioritási sorrend szerint dönti el a végső helyesírási és összevonási állapotot (fentről lefelé haladva):

| Prioritás | Szabály Neve | Leírás / Megkötés | Konfliktus Kezelés |
| :---: | :--- | :--- | :--- |
| **1.** | **Ütközésmentesség (No Collision)** | Az új kód semmilyen körülmények között nem ütközhet éles bázisszóval. | **Átléphetetlen korlát.** Ütközés esetén azonnali HITL leállítás történik. |
| **2.** | **Szemantikai DNA konzisztencia** | A kódnak felépíthetőnek kell lennie a szülő UUID-k sorrendjéből. | Ha a képlet sérül, a kód érvénytelen. |
| **3.** | **Vizuális Érthetőség (Lexical Integrity)** | A kód hosszától függően a compiler újraértékeli a szeparációt (pl. ha a `h` $\rightarrow$ `ha` miatt `rhx`-ből `rhax` lesz, ez még egybefüggően olvasható, de ha `lhdshldx` lenne belőle, az már olvashatatlan). | **Szeparációs Állapotgép (Transition):**<br>- Ha a kód $\le$ 4 karakter: csupa kisbetűs összeolvadás engedélyezett (pl. `rhax`).<br>- Ha a kód > 4 karakter: Kötelező visszalépés CamelCase-re (pl. `rHaX`) vagy Dot Case-re (pl. `r.ha.x`). |
| **4.** | **Pronounceability (Kimondhatóság)** | A kódnak verbalizálhatónak kell lennie (ASS Level 2 elv alapján). | Ha nehezen kiejthető (pl. `rhtx`), a linter figyelmeztetést (Warning) küld a tanárnak jóváhagyásra. |
| **5.** | **Minimális Hossz (ASS Max Compression)** | Törekedni kell a lehető legrövidebb reprezentációra. | Alárendelt az 1-4. pontoknak. |

#### 2. Hogyan fut át a változtatás a rendszeren? (The Refactoring Flow)
A megváltozott karakterhosszból adódó speciális helyesírási és összevonási állapotváltozásokat a compiler az alábbi folyamat szerint menedzseli:

```
[ Alapelem Módosítás: h ──> ha ]
               │
               ▼
┌──────────────────────────────────────────────┐
│  Módosult DNS Képletek Újragenerálása        │ 
│  (r + ha + x ──> rhax)                       │
└──────────────────────────────────────────────┘
               │
               ▼
┌──────────────────────────────────────────────┐
│  Szeparációs Állapotgép Értékelése           │
│  - Karakterhossz vizsgálat                   │
│  - Szükséges-e CamelCase / Dot Case vissza-   │
│    lépés? (pl. rhax <= 4 char ──> OK)        │
└──────────────────────────────────────────────┘
               │
               ▼
┌──────────────────────────────────────────────┐
│  Biztonsági Linter & Ütközésvizsgálat        │
│  - Ütközik létező szóval?                    │
│  - Kimondható a kód?                         │
└──────────────────────────────────────────────┘
       │                              │
       │ (NINCS ütközés / OK)         │ (ÜTKÖZÉS vagy bizonytalanság)
       ▼                              ▼
┌──────────────────────────────┐ ┌──────────────────────────────────────────┐
│  Automatizált Végrehajtás    │ │  Human-in-the-Loop (HITL) Gateway       │
│  - Az összes óravázlat       │ │  - A compiler leáll.                     │
│    AST-alapú átírása.        │ │  - Felületen mutatja a konfliktust:      │
│  - Push a GitHubra.          │ │    "A h->ha miatt az rhx->rhax lett, de  │
│  - 100% autonóm lefutás.     │ │     ez ütközik a 'rhax' standard szóval. │
│                              │ │     Válasszon alternatívát!"             │
└──────────────────────────────┘ └──────────────────────────────────────────┘
```

#### 3. Mikor KÖTELEZŐ a Human-in-the-Loop (HITL) beavatkozás?
Az automatizációt szigorú védőkorlátok (guardrails) szabályozzák. Az AI/compiler nem dönthet önállóan az alábbi esetekben:
1. **Név-ütközés (Name Collision):** Ha az újonnan generált hosszabb vagy rövidebb kód megegyezik egy már létező, más UUID-re mutató éles fogalommal (pl. ha az új `rhax` ütközne egy létező `rhax` nevű speciális forgással).
2. **Kiejthetetlenség (Aesthetic / Pronounceability Alert):** Ha a kód hossza megnő, és a magánhangzó-mássalhangzó arány szerint a szó kimondhatatlanná válik az órán (pl. `lhx` $\rightarrow$ `lshldbx`), a rendszer megáll, és felajánlja a Dot Case szeparációra való visszalépést (`l.shld.b.x`).
3. **Didaktikai Jelentőség-csökkenés:** Ha a változás miatt az alapeset (Default Representant) vizuálisan teljesen felismerhetetlenná válik a tanároknak a madártávlati nézetben.

### F. Ontológiai Átrendezés és Szemantikai Elcsúszás (Ontological Reclassification & Semantic Drift)
Gyakori módszertani hiba a tananyag-fejlesztés kezdetén, hogy egy kategóriát vagy fogalmat túl korán, nem elég absztrakt szinten nevezünk el. Használat közben kiderül, hogy a bevezetett fogalom valójában egy nagyobb, általánosabb igazság specifikus részhalmaza.

#### 1. A "5s" (5 steps) ──> "5a" (5 accents) Példa
- **A kezdeti koncepció (Beginner Level):** A zenei kérdésre (1-4. ütem) a táncos az alap 3 lépés helyett 5 lépést tesz. Elnevezzük **`5s`**-nek (5 steps), ami kisebb, sűrűbb lépéseket és gyorsabb érzetet ad.
- **A mélyebb didaktikai igazság (Advanced Level):** Magasabb szinten a táncos takarékoskodik a súlyvonal-váltásokkal (flow optimalizáció). Nem tesz 5 teljes lépést, hanem 5 zenei történést / hangsúlyt táncol le (pl. testizoláció, vállpattintás, tap súlyáthelyezés nélkül).
- **A szemantikai elcsúszás (Semantic Drift):** Kiderül, hogy az általános, felsőbb kategória a **`5a`** (5 accents - 5 hangsúly). Az **`5s`** (5 steps) valójában a **`5a`** egy specifikus, kezdő szintű megvalósulása (ahol minden hangsúlyhoz teljes lépés/súlyáthelyezés társul).
- **A logikai reláció:** `5s` is-a `5a` (where `weight_transfer = true`).

#### 2. Az Ontológiai Elcsúszás Kezelésének Folyamata (OntoShift Pipeline)
Amikor egy korábbi bázisfogalomról kiderül, hogy az egy új, absztraktabb fogalom gyermeke (hyponymja), a rendszer az alábbiak szerint hajtja végre a struktúra átrendezését:

1. **A Kapcsolat Átírása (Taxonomical Re-mapping):**
   - Nem töröljük az `5s` kódot, mert az a kezdő órák videóiban és leírásaiban fizikailag pontos (ott tényleg léptek).
   - Az adatbázisban az `5s` rekord `parent_term_id` mezőjét beállítjuk az újonnan létrehozott **`5a`** UUID-jára.
   - Az `5s` tulajdonságaként rögzítjük a specifikus megszorítást: `{ weight_transfer: true }`.

2. **AST-Alapú Szemantikai Fordítás (Semantic Expansion):**
   - A parser felkészül arra, hogy a jövőben az `5s` kódot a háttérben **`5a.step`** formában is képes legyen értelmezni, biztosítva a szigorú logikai ekvivalenciát.

3. **Irányított Curriculum Migráció (Guided Migration Toolkit):**
   - A rendszer átvizsgálja az összes történelmi óravázlatot, ahol a régi `5s` szerepelt.
   - Nem hajt végre vak szöveges cserét, hanem egy **HITL (Human-in-the-Loop) Refaktorizációs Panel** elé tárja a döntést a tanárnak:

> 🔄 **Ontológiai Refaktorizációs Asszisztens:**
>
> *"Észleltük, hogy az **`5s`** (5 steps) fogalmat átminősítette az **`5a`** (5 accents) gyermekévé.*
> *Az adatbázisban 42 történelmi óravázlat használja az `5s` kódot. Hogyan szeretné ezeket frissíteni?*
>
> 1. **Megtartás (Keep Specific):** *"Hagyja változatlanul `5s`-ként az érintett órákon, mert a didaktikai szint alapján (Salsa Cat 01) ott valóban fizikai lépéseket végeztek a tanulók."*
> 2. **Általánosítás (Generalize to `5a`):** *"Cserélje le `5a`-ra az órákon, mert a kontextus alapján a zenei hangsúlyok elhelyezése volt a cél, és a lépés másodlagos."*
> 3. **Dekonstruált Csere (Deconstruct to `5a.step`):** *"Cserélje le az explicit `5a.step` Dot Case formára a maximális elméleti precizitás érdekében."*
>
> *[Gomb: Végrehajtás kiválasztott órákon]*

---

## 11. Szemantikai Ontológia, Didaktikai Approximáció és Fokozatos Pontosítás (Didactic Scaffolding & Epistemological Refinement)
A tánctanítás hatékonysága megköveteli, hogy a kezdő tanulóknak ne a teljes fizikai valóságot (L0 szintű anatómia és kinetika) tanítsuk meg azonnal, hanem "didaktikai mankókat" (scaffolds) alkalmazzunk. Ezek a fogalmak kezdő szinten szándékosan pontatlanok, részlegesek és intuitívak, de később, a tanulás előrehaladtával pontosítani, szűkíteni és dekonstruálni kell őket.

### A. Didaktikai Szintezés és Approximációs Életciklus (Refinement Pathway)
Minden didaktikai fogalomhoz az adatbázisban hozzárendelünk egy **pontossági/didaktikai állapotot** (Epistemological State) és egy finomítási útvonalat (Refinement Pathway):

```
[Didaktikai Mankó / Kezdő Fogalom: "CBL"] (L3: Első intuitív benyomás)
                       │
                       ▼  (Középszint: Döntési pontok és pályasíkok tisztázása)
[Kinetikai Pontosítás: "CBL.pathOpen & CBL.codDeact"] (L1: Viselkedési szabályok)
                       │
                       ▼  (Haladó szint: Teljes anatómiai és kinetikai dekonstrukció)
[Atomi Fizikai Képlet: "CBL.Lhx.LLTO + swg.trans + reFace + bal.absorb"] (L0: Fizikai SoT)
```

1. **Epistemological States (Megértési Állapotok):**
   - `scaffold` (didaktikai mankó, intuitív, de elméletileg pontatlan, pl. kezdő CBL).
   - `precise` (kinetikailag pontosított, pl. középhaladó CBL).
   - `deconstructed` (atomi elemeire bontott, fizikai valóság, pl. haladó CBL).

2. **A "CBL" (Cross Body Lead) Didaktikai Evolúciója:**
   - **Kezdő Szint (`scaffold`):** "Első alapeseti benyomás" $\rightarrow$ A férfi egy mozdulattal elvezeti a nőt, a nő pedig átmegy a túloldalra.
   - **Haladó Szint (`deconstructed`):** A CBL valójában nem egyetlen figura, hanem egy zenei ütem (projektidő: 4 ütés) alatt lefutó **kinetikai döntéssorozat (kinematic chain)**:
     - *Path opening:* A férfi kilép a bal oldali vonalról, szabaddá téve a Follower futási pályáját (`pathOpen`).
     - *Active following:* A nő aktív vezetés nélkül, önmagából adódóan alaplépést táncol ezen a pályán.
     - *COD Deactivation:* A nő elülső `cod` (change of direction) végpontján a férfi vezetéssel plusz gyorsítást/impulzust ad, hatástalanítva a nő természetes visszafordulási szándékát.
     - *Transition:* A nő átsétál/átfut (`wlk`/`run`) a túloldalra.
     - *Resynchronization & reFace:* A zenei ütem végén a nő reszinkronizálja magát és visszafordul a partnere felé (`reFace`).
     - *Momentum Absorption:* A nő a testében felgyűlt kinetikus energiát kiegyensúlyozásokkal (`bal.absorb`) felemészti, beállva a következő alaplépés nyugalmi állapotába.

### B. A "Fogalom Utó-Tisztázó" (Refinement Engine) működése
A rendszer támogatja, hogy a tanár a diákok profilján vagy a tantervben fokozatosan "leleplezze" a fogalmak mögötti fizikai valóságot:
1. **Diák Skill-Matrix Frissítés:** Amikor a diák kezdő szinten elsajátítja a CBL-t, a rendszer rögzíti az `L3: CBL [mastery_level = 2]` állapotot.
2. **Szemantikai Feloldás (Unveiling):** Haladóbb kurzusokon a rendszer felajánlja a tanárnak, hogy bontsa fel a diák CBL tudását az atomi elemekre:
   - *"A tanuló csoport elérte a Haladó szintet. Szeretné a 'CBL' didaktikai fogalmat feloldani a hozzá tartozó 'reFace', 'swg.trans' és 'bal.absorb' atomi készségekre a profiljukon?"*
3. **Didaktikai Öröklődés:** Az adatbázisban a dekonstruált szilánkok tartalmazzák a szülő `scaffold` kapcsolatát, így a diák elméleti tudása mindig visszavezethető a legalapvetőbb mozdulatoktól a komplex kulturális figuranevekig.

### C. Jelző-Transzponálás és Szemantikai Lehorgonyzás (Modifier Transposition & Semantic Anchoring)
Különösen komplex didaktikai probléma, ha a tanár egy elméletileg még le nem bontott, pontatlan scaffold fogalmat (pl. `CBL`) ruház fel egyedi stílusbeli vagy fizikai jelzővel (pl. *"a férfi a zenei kérdésben [1-4 ütem] lépés helyett csak kicsúsztatja a szabad lábát"*).

Hogy ez a jelző a későbbi pontosítás (dekonstrukció) után ne vesszen el, hanem automatikusan átkerüljön a pontosított fogalmi tér megfelelő helyére, a rendszer a **Szemantikai Lehorgonyzás (Semantic Anchoring)** technológiáját használja.

#### 1. A Működés Alapelve (Metadata Intersection Matching)
Minden összetett mozdulat-jelző (modifier) rendelkezik saját **szemantikai cél-metaadatokkal (Target DNA)**, amelyek leírják, hogy a mozdulat melyik dimenziót érinti:
- **Szerep (Role):** Leader (férfi) / Follower (nő).
- **Időzítés (Timing):** Beats 1-4 (zenei kérdés) / Beats 5-8 (zenei válasz).
- **Testrész (Body Part):** Lower body (lábfej, súlypont) / Upper body (karok, kapcsolódás).

Amikor a tanár a pontatlan `CBL` fogalomhoz hozzákapcsol egy ilyen stílus-jelzőt (pl. `flare` / `reachOut`), a háttérben futó **Refinement Engine**:
1. Beolvassa a jelző metaadatait: `flare` $\rightarrow$ `{ role: 'Leader', timing: '1-4', body_part: 'lower_body' }`.
2. Megvizsgálja a `CBL` dekonstruált kinetikai láncolatát (sub-components).
3. Kiszámítja a jelző metaadatainak és a CBL alkatrészeinek **metszetét (Semantic Intersection)**.
4. Megtalálja az egyetlen tökéletesen illeszkedő komponenst: **`CBL.pathOpen`** (amely szintén `{ role: 'Leader', timing: '1-4', body_part: 'lower_body' }` tulajdonságú).
5. **Automatikusan lehorgonyozza (áttranszponálja)** a jelzőt a pontosított alkatrészre: `CBL.pathOpen[modified_by: flare]`.

#### 2. Hogyan oldja fel ezt a UAA 2.0 Szótár-Asszisztens?
Amikor a tanár beírja a felületre a hanyag/pontatlan megfogalmazást: *"cbl, de a férfi az 1-4-re kicsúsztatja a lábát"*:
1. A rendszer felismeri a `CBL` szülőfogalmat és a megadott kinetikai változtatást.
2. A fenti lehorgonyzási logika alapján azonosítja, hogy ez a `CBL.pathOpen` fázist érinti.
3. Az **UAA 2.0 algoritmus** segítségével automatikusan felajánlja a legördülő menüben a két lehetséges szabványos és tömör elnevezést a general-specific skálán:
   - **`maFlareCbl`** (Man's Flare Cross Body Lead): ha a mozdulat stílusjegyként a díszítésre (flare) fókuszál.
   - **`maRchOutCbl`** (Man's Reach Out One Step Cross Body Lead): ha a mozdulat funkcionálisan a lépés messzire nyújtására (reach out) fókuszál.
4. A tanár egyetlen kattintással jóváhagyja az elnevezést, és a rendszer a háttérben az új kifejezést **szülő-gyermek kapcsolatban** rögzíti a `CBL`-lel, megőrizve a pontos belső dekonstruált képletét is.

### D. Rendhagyó Lexikális Idiómák (Irregular Idioms & Lexical Overrides)
Minden természetes nyelv tartalmaz rendhagyó szerkezeteket és kivételeket. Ennek oka a nyelvhasználat ökonómiája (Zipf törvénye): a leggyakrabban használt kifejezések kiejtése és írása a történelem során rendkívül lerövidül és torzul a kényelem kedvéért.

A DANCE leíró nyelvben is előfordulnak olyan **hagyatéki kódok**, amelyek nem követik az UAA 2.0 logikus levezetési szabályait, de a kényelem és a megszokás miatt megkerülhetetlenek:
- *Példa:* face = **`fac`** (logikus rövidítés) és back = **`bck`** (logikus rövidítés, testrész).
- *A rendhagyó kifejezés:* Az egymás mögött, azonos irányba néző táncosok pozíciója (a shadow pozíció általános formája) a **"face-to-back"**.
  - A logikus kód a szabályok szerint: `fac2bck` lenne.
  - A kényelmi, hagyatéki kód viszont: **`f2b`**.

Hogy ezek a rendhagyó esetek ne "rúgják szét" a rendszerszintű konzisztenciát, a Dependency DNA-t és az automatizált refaktorizációs pipeline-t, az alábbi **Rendhagyó Idióma (Irregular Idiom)** protokollt vezetjük be:

#### 1. A Rendhagyó Leképezés (Irregular Mapping)
A szótárban az `f2b` kódot különleges, **`is_irregular = true`** jelzővel látjuk el. Ez a jelző azt jelenti, hogy a kód karaktersora manuális felülírás (override), és nem a standard UAA 2.0 generálja.

Az adatbázisban a **Szemantikai DNA** (szülő-gyermek viszony) azonban **ugyanolyan szigorúan és hiánytalanul megmarad**, mint a szabályos szavaknál:
```
[Irregular Term: f2b] (id: UUID_f2b, is_irregular: true)
       │
       ├───> [Index 1]: UUID_face (face / fac)
       ├───> [Index 2]: UUID_to (to / 2)
       └───> [Index 3]: UUID_back (back / bck)
```
Az `f2b` a háttérben nem vak szöveg, hanem a `face` + `to` + `back` szemantikai atomok közvetlen, UUID-alapú láncolata.

#### 2. Hogyan kezeli a rendhagyó szavakat a Refaktorizációs Pipeline?
Amikor végrehajtunk egy rendszerszintű cserét az alapelemeken (pl. a `back` kódját `bck`-ról átírjuk `bka`-ra, hogy a `bck` felszabaduljon a `beckon` számára):
1. **Biztonsági pajzs (Irregular Shield):** A refaktorizációs compiler észleli, hogy az `f2b` rekord `is_irregular = true` állapotú.
2. **Karakter-megőrzés:** A rendszer **NEM** írja át automatikusan az `f2b` karaktersorát `f2bka` formára, mert tudja, hogy ez egy szándékos, kényelmi idióma.
3. **Szemantikai Konzisztenccia:** Az óravázlatok AST fává alakításakor a háttérben az `f2b` továbbra is a megváltozott `back` (UUID_back, immár `bka`) atomra fog mutatni.
4. **HITL Figyelmeztetés (Warning):** A migráció végén a linter csupán egy jelentést küld a tanárnak:
   - *"A 'back' atom kódja megváltozott (`bck` ──> `bka`). Az 'f2b' rendhagyó idióma továbbra is a helyes atomra mutat a háttérben, de a felszíni karaktereit változatlanul hagytam a kényelem érdekében. Szeretné felülvizsgálni a helyesírást?"*

---

## 12. "Fuzzy-to-Precise" Videó Ingestion és Fokozatos Megismerési Pipeline (Incremental Concept Discovery Workflow)
Amikor a tánctanár egy új videót elemez (pl. egy újonnan megjelent fesztivál összefoglalót vagy egy Instagram-kihívást), gyakran találkozik olyan **elsőre felfoghatatlan, komplex kinetikai elemekkel**, amelyeket az adott pillanatban képtelen a precíz L0/L1 fizikai szintaxissal leírni. 
Ha ilyenkor rákényszerítenénk a precíz leírást, az megbénítaná a kutatói kreativitást (analysis paralysis).

A rendszer ezt az **"Inkrementális Megismerési Pipeline" (Incremental Discovery Pipeline)** segítségével kezeli, amely lehetővé teszi a homályos (fuzzy) ötletek és videók gyors rögzítését, majd azok fokozatos, lépésről lépésre történő letisztázását.

```
┌────────────────────────────────────────────────────────┐
│ 1. Akvizíció: Videó feltöltés + Időbélyegzés (01:12)   │
└───────────────────────────┬────────────────────────────┘
                            │
                            ▼
┌────────────────────────────────────────────────────────┐
│ 2. Sandbox Draft: Szabad szavas leírás / Voice Memo    │
│    "furcsa csavaró lábcsúsztatás, mintha süllyedne"    │
└───────────────────────────┬────────────────────────────┘
                            │
                            ▼
┌────────────────────────────────────────────────────────┐
│ 3. Ideiglenes Címkézés (Didactic Scaffold):            │
│    `tmp.backScrew` (Használható óravázlatban)          │
└───────────────────────────┬────────────────────────────┘
                            │
                            ▼
┌────────────────────────────────────────────────────────┐
│ 4. AI-Asszisztált Dekonstrukció (Gemini 2.5 Pose API) │
│    "3. ütésre jobb sarok pivot + csípő-dőlés balra"    │
└───────────────────────────┬────────────────────────────┘
                            │
                            ▼
┌────────────────────────────────────────────────────────┐
│ 5. Hivatalos Integráció (Lexicalization & Lineage):    │
│    `pvtScrew` / `sldScrew`                             │
└────────────────────────────────────────────────────────┘
```

### A. A Workflow Lépései (Step-by-Step)

#### 1. Videó Rögzítés és Időbélyegzés (Video Segmentation)
*   **Felület:** A kutatószoftver beépített videólejátszójában a tanár elindítja a videót.
*   **Akció:** Amikor meglátja az érdekes mozdulatot, egyetlen billentyűleütéssel (pl. `Space` vagy egy nagy piros "Core Mark" gombbal) kijelöli az időintervallumot (pl. `01:12.4` - `01:15.8`).
*   **Rendszer-integráció:** A háttérben létrejön egy `video_segments` rekord, amely szorosan kapcsolódik a videóhoz, de még nincs hozzárendelve véglegesített fogalomhoz.

#### 2. Sandbox Draft & Szabad Szavas Körülírás (The Fuzzy Inbox)
*   **Akció:** A rendszer felugró ablakában a tanár nem kódol, hanem kiönti a gondolatait. Írhat szabad szöveggel, vagy rögzíthet egy gyors **Voice Memo-t** (hangjegyzetet), amit a rendszer automatikusan szöveggé alakít (Whisper / Google Cloud Speech-to-Text).
*   *Példa leírás:* *"Olyan, mintha a férfi hátracsúsztatná a lábát egy csavaró mozdulattal, miközben süllyed a súlypontja, és közben a nő elhalad előtte. Olyan, mint egy csavar."*
*   **Adatbázis-állapot:** Létrejön egy új fogalom rekord a `terms` táblában:
    ```json
    {
      "id": "UUID_tmp_backScrew",
      "primary_abbreviation": "tmp.backScrew",
      "name_hu": "Hátsó csavaros csúsztatás (Ideiglenes)",
      "epistemological_state": "fuzzy_draft",
      "description_hu": "Olyan, mintha a férfi hátracsúsztatná a lábát egy csavaró mozdulattal, miközben süllyed a súlypontja...",
      "type": "sandbox_draft"
    }
    ```

#### 3. Ideiglenes Címkézés és Didaktikai Használat (The Scaffold Placeholder)
*   **Akció:** Ahhoz, hogy az új ötletet azonnal lehessen használni az esti óravázlatban, a rendszer felajánl egy ideiglenes, jól megjegyezhető kódot (pl. `tmp.backScrew` vagy `ch.twistSlide`).
*   **Óravázlat integráció:** A tanár beírja az esti óravázlatába: `CBL.pathOpen & tmp.backScrew`.
*   A rendszer engedi ezt a leírást, nem dob linter hibát, mert a `tmp.backScrew` létezik a `terms` táblában mint `sandbox_draft`. A tanulók tudásmátrixában is megjelenik mint "megismert kihívás", de még nincs dekonstruálva.

#### 4. AI-Asszisztált Dekonstrukciós Tükör (AI Pose Mirror)
*   A Google Cloud Run háttérfolyamata kivágja az érintett 3.4 másodperces videószegmenst, és kulcsképkockákat (keyframes) generál.
*   **Az AI Szerepe (Gemini 2.5 Pro Multimodal):** A tanár rákattint az **"Analyze with AI"** gombra. A Gemini megkapja a videószegmenst és a tanár szabad szavas megfogalmazását.
*   Az AI nem helyettesíti a humán kontrollt, hanem **tükröt tart (Cognitive Mirror)**:
    - *"Elemeztem a videót a megadott időbélyegnél. Azt látom, hogy a férfi a jobb lábán végez egy 180 fokos sarkon forgást (Pivot), miközben a bal lába nyújtva csúszik hátra (Slide) és a súlyvonala 15 cm-t süllyed. Ez megfelel a szabad szavas 'csavaró csúsztatás' leírásodnak."*
    - *"Javasolt atomi összetevők a szótárból:* `pvt.R.180` (jobb láb pivot), `sld.L.back` (bal láb hátra csúsztatás), `lvl.down` (súlyvonal süllyesztés).*
    - *Didaktikai hasonlat javaslat:* 'Csavarmenet' (Screw thread) - a láb úgy fúródik a földbe, mint egy csavar."*

#### 5. Fokozatos Megemésztés, Kipróbálás és Hivatalos Integráció (HITL Formalization)
*   **Humán-in-the-Loop jóváhagyás:** A tanár az esti órán leteszteli a mozdulatot. Tapasztalatokat gyűjt: *"A diákoknak valóban a 'csavarmenet' hasonlat segített megérteni a súlypontsüllyesztést."*
*   A tanár visszatér a **Research Dashboard-ra**, és megnyitja a `tmp.backScrew` szócikket formalizálásra:
    1.  **Státusz frissítése:** `fuzzy_draft` ──> `precise` vagy `deconstructed`.
    2.  **Hivatalos kód generálása:** Az UAA 2.0 felajánlja a hivatalos elnevezést a general-specific skálán (pl. `pvtScrew` vagy `sldScrew`).
    3.  **Szemantikai DNA rögzítése:** A `term_components` táblába bekerülnek a pontosított fizikai UUID-k (`pvt.R.180`, `sld.L.back`, `lvl.down`).
    4.  **Didaktikai Metaadatok mentése:** Hozzáadódik a "Csavarmenet" story, a videós deeplink és a tanítási tapasztalat.
    5.  **Automata Óravázlat Frissítés (Refactoring Pipeline):** A rendszer átvizsgálja az óravázlatokat, és a korábbi ideiglenes `tmp.backScrew` kódokat automatikusan frissíti a végleges `pvtScrew` kódra.

---

## 13. Vizuális Annotáció és "Ideális Kinetika" Korrekciós Rendszer (Visual Annotation, Overlay Drawing & Kinematic Correction)
A versenyzői szintű és a bonyolult solo videók elemzése során gyakran előfordul, hogy a táncos mozgása nem 100%-ig tiszta, vagy a kivitelezés nem éri el a kívánt elméleti ideált. Hogy a rendszer ne csak passzív leíró eszköz legyen, hanem aktív **fejlesztési, oktatási és visszajelzési (feedback) platform**, bevezetjük a **Vizuális Annotációs és Rétegzési Rendszert (Visual Overlay & Kinetic Markup Engine)**.

Ez a modul lehetővé teszi, hogy a tanár közvetlenül a videó képkockáira rajzoljon (pályagörbéket, szövegeket, testrész-kapcsolatokat), vizuálisan ábrázolva a *"mi történik"* (actual) és a *"hogyan kellene történnie"* (ideal) közötti különbséget.

```
┌────────────────────────────────────────────────────────┐
│  [Videó Lejátszó] (Next.js HTML5 Canvas Overlay)       │
│                                                        │
│   ( ◯ ) <-drawn: lvl.upCu (Ideális Súlypont Ív)        │
│    /|\                                                 │
│   / | \  <-drawn: Kinematic Pair Link (Ronde & Leg)    │
│    / \                                                 │
│   /   \  <-actual pose (MediaPipe / ViTPose Skeleton)  │
└────────────────────────────────────────────────────────┘
```

### A. Technológiai és Adatarchitektúra (Annotation Data Model)
A rajzolt elemek nem statikusan "ráégnek" a videó fájlra, hanem **dinamikus, időbélyegzett vektor-rétegekként (time-stamped vector layers)** kerülnek mentésre. Ez biztosítja, hogy a rajzok szerkeszthetők, kikapcsolhatók és a leíró nyelv kódjaival összekapcsolhatók legyenek.

#### 1. Az Annotációs Adatbázis Séma (`video_annotations` & `annotation_shapes`)
```sql
CREATE TABLE video_annotations (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    video_segment_id UUID REFERENCES video_segments(id) ON DELETE CASCADE,
    created_by UUID REFERENCES users(id),
    title VARCHAR(255), -- Pl. "Súlypont emelés javítás"
    description TEXT,   -- "A ronde indításakor a medence essen be, hanem upCu ívet írjon le."
    created_at TIMESTAMPTZ DEFAULT NOW()
);

CREATE TABLE annotation_shapes (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    annotation_id UUID REFERENCES video_annotations(id) ON DELETE CASCADE,
    frame_number INT,        -- Vagy pontos milliszekundum időbélyeg
    shape_type VARCHAR(50),  -- 'line_trajectory', 'arrow', 'text_bubble', 'skeleton_link', 'angle_marker'
    vector_data JSONB NOT NULL, -- Koordináták: { points: [{x: 120, y: 340}, ...], color: '#FF0000', stroke_width: 4 }
    associated_term_id UUID REFERENCES terms(id) ON DELETE SET NULL -- Pl. UUID_lvl_upCu (lvl.upCu)
);
```

### B. Didaktikai és Edzői Felhasználási Esetek (Core Use Cases)

#### 1. Kinetikai Finomítás és Ötletelés (Biomechanical Enrichment)
*   **Példa:** A tanár lát egy oldallépést a videón. Úgy érzi, a táncos túl laposan lép.
*   **A szoftveres akció:** Kiválasztja a `Line/Curve Tool`-t, és berajzol egy felfelé ívelő parabolát (Upper Curve - `upCu`) a táncos medencéjének magasságában.
*   **Szemantikai Csatolás:** A berajzolt ívhez hozzárendeli a `lvl.upCu` (súlyvonal felső ívű mozgása) és az `rcp.limb` (végtag-végtag közötti mozgáspár / kinetic pairing a ronde-dal) kódot a szótárból.
*   **Az eredmény:** A szoftver nemcsak a rajzot tárolja, de a videószegmenshez most már hivatalosan kapcsolódik az **"így kellene kivitelezni"** elméleti modell kódja is.

#### 2. Segédtanárok és Iskolák Betanítása (Choreography Mapping & Standardization)
*   Ha a fő tanár (fejlesztő) készít egy rendkívül komplex, versenyzői szintű koreográfiát:
    1.  A zenei ütemeket és a dekonstruált kódokat pontosan összehangolja a videó idősávjával.
    2.  A videóra rárajzolja a kritikus erővonalakat, a feszítések irányait, és a karok ideális térbeli íveit.
    3.  A segédtanárok a **"Standardized Teaching View"** segítségével pontosan látják, hogy mit és miért kell megkövetelni a diákoktól. Nincs többé *"ki hogyan emlékszik a mozdulatra"* bizonytalanság: a vizuális réteg és a kódos leírás együttesen adja ki az abszolút igazságot.

#### 3. HITL Versenyzői Hibajavítás és Táv-Coaching (Asynchronous Feedback & Correction)
*   **A munkafolyamat:**
    1.  A versenyző feltölti az edzés-videóját a saját profiljáról a rendszerbe, és visszajelzést kér (pl. *"Kérem a bachata solo 2. részének javítását"*).
    2.  A tanár megnyitja a videót a **Coaching Canvas** felületen.
    3.  A tanár a videót kockáról kockára pörgetve (scrubbing) közvetlenül a képernyőre firkál:
        *   Piros színnel berajzolja a versenyző rossz, beeső térdtengelyét.
        *   Zöld nyíllal berajzolja a kívánt, kifelé mutató ideális térdirányt.
        *   Ráilleszt egy szövegbuborékot a megfelelő zenei ütemre: *"Itt süllyessz és feszítsd a bokát!"*
    4.  A rendszer elmenti a visszajelzést. A versenyző a saját felületén megnyitja a videót, és a lejátszás során az adott másodpercekben a képernyőn dinamikusan megjelennek a tanár színes "firkái" és javító kommentjei.

#### 4. Pose-Estimation vs. Drawn Ideal (Automata Biomechanikai Kontraszt)
*   A legmagasabb szintű technológiai integráció során a rendszer a MediaPipe vagy ViTPose segítségével **automatikusan kirajzolja a táncos csontvázát (actual pose skeleton)** kék színnel.
*   A tanár zöld színnel berajzolja az **ideális csontvázat (ideal skeleton)** vagy a kívánt végpont-pályagörbéket.
*   A rendszer kiszámítja a két görbe közötti eltérést (Delta), és számszerűsíthető biomechanikai visszajelzést ad: *"A ronde fázisban a bal lábnyújtás 12 fokkal elmarad az elméleti ideáltól, a medence süllyedése pedig 8 cm-rel mélyebb a kelleténél."*

---

## 14. Kreatív Koreográfia-Tervező és 3D Show Készítő Rendszer (Creative Choreography Planner, Timeline Mashup & 3D Avatar Engine)
Ez a modul a rendszer **koronája**. A megszerzett elméleti tudást (szótár, atomi kódok, dekonstruált szilánkok), a meglévő videóarchívumot és a vizuális annotációt egyetlen, **zene-központú kreatív alkotói munkaállomássá (Choreographer workstation)** gyúrja össze.

A cél egy olyan digitális környezet, amely támogatja a koreográfus "álom-munkafolyamatát": az inspirációtól (zenevágás, szabad ötletelés, referenciák gyűjtése) a pontos zenei-kinetikai térképen át (timeline, zenei hangsúlyok, többtáncos formációk) a 3D-s szimulációig.

```
┌────────────────────────────────────────────────────────┐
│ [AUDIO TIMELINE] BPM: 120 (Salsa on1)                   │
│ Beat: [ 1 . 2 . 3 . 4 ] & [ 5 . 6 . 7 . 8 ]            │
├────────────────────────────────────────────────────────┤
│ [VIDEO TRACK]   [ Video_A.mp4 (01:12) ] -> [ Video_B ] │ (Referencia videók mashupja)
├────────────────────────────────────────────────────────┤
│ [NOTATION TRACK] `cbl.pathOpen`       -> `pvtScrew`    │ (Mozaikszavas kinetikai kódok)
├────────────────────────────────────────────────────────┤
│ [FORMATION GRID]  (A)  (B)  ───>  (A)──(B)  (Térform.) │ (2D/3D táncos pozíciók)
└────────────────────────────────────────────────────────┘
```

### A. A Tervező Munkaállomás Adatmodellje (Choreography Data Model)
A zenei idősávra fűzött események kezelésére egy rendkívül rugalmas, többrétegű és skálázható adatmodellt vezetünk be:

```sql
-- 1. A Koreográfia / Show Projekt
CREATE TABLE choreography_projects (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    tenant_id UUID REFERENCES tenants(id) ON DELETE CASCADE,
    title VARCHAR(255) NOT NULL,
    theme TEXT,                     -- Téma / Karakterleírás / Dramaturgia
    script_storyboard TEXT,         -- Forgatókönyv / Szöveges storyboard
    audio_file_url TEXT,            -- Megvágott zenei sáv URL-je
    bpm INT,                        -- Projekt alap BPM-je
    competition_category VARCHAR(100), -- Versenykategória (Pl. "Salsa Solo Elite")
    time_limit_seconds INT,         -- Kűridő limit másodpercben
    created_at TIMESTAMPTZ DEFAULT NOW(),
    updated_at TIMESTAMPTZ DEFAULT NOW()
);

-- 2. A Projektben résztvevő táncosok/szerepek listája
CREATE TABLE project_dancers (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    project_id UUID REFERENCES choreography_projects(id) ON DELETE CASCADE,
    label VARCHAR(50) NOT NULL,    -- Pl. "Leader 1", "Follower 1", "Group_A_1"
    assigned_user_id UUID REFERENCES users(id) ON DELETE SET NULL,
    color_code VARCHAR(7)           -- Színkód a vizuális ábrázoláshoz (Pl. "#FF5733")
);

-- 3. Az Idővonal Csópontjai (Timeline Beats)
CREATE TABLE timeline_nodes (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    project_id UUID REFERENCES choreography_projects(id) ON DELETE CASCADE,
    measure_number INT NOT NULL,    -- Hányadik 8-as ütem (bar/measure)
    beat_number INT NOT NULL,       -- Hányadik ütés (1-8 közötti érték)
    sub_beat FLOAT DEFAULT 0.0,     -- Törtütések (Pl. 0.5 a "és" / "&" ütésekhez)
    absolute_time_ms INT,          -- Abszolút időbélyeg a zenében (milliszekundumban)
    musical_accent VARCHAR(100),    -- Zenei hangsúly / "Hit" leírása (Pl. "Trombita accent", "Dombó")
    storyboard_note TEXT,           -- Forgatókönyvi / Drámai megjegyzés ezen az ütésen
    created_at TIMESTAMPTZ DEFAULT NOW()
);

-- 4. Kinetikai és Vizuális Réteg (A táncosok egyéni mozdulatai az adott ütemen)
CREATE TABLE timeline_dancer_actions (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    timeline_node_id UUID REFERENCES timeline_nodes(id) ON DELETE CASCADE,
    project_dancer_id UUID REFERENCES project_dancers(id) ON DELETE CASCADE,
    term_id UUID REFERENCES terms(id) ON DELETE RESTRICT, -- A kódolt mozdulat (Pl. `pvtScrew`)
    video_reference_segment_id UUID REFERENCES video_segments(id) ON DELETE SET NULL, -- A forrás-videó kivágott darabkája
    custom_instruction TEXT,       -- Egyedi instrukció a táncosnak
    PRIMARY KEY (timeline_node_id, project_dancer_id)
);

-- 5. Térformációs Réteg (A táncosok 2D koordinátái a színpadi téren az adott ütemen)
CREATE TABLE choreography_formations (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    timeline_node_id UUID REFERENCES timeline_nodes(id) ON DELETE CASCADE,
    project_dancer_id UUID REFERENCES project_dancers(id) ON DELETE CASCADE,
    position_x FLOAT NOT NULL,     -- Relatív X pozíció a színpadon (0.0 - 100.0)
    position_y FLOAT NOT NULL,     -- Relatív Y pozíció a színpadon (0.0 - 100.0)
    orientation_angle INT DEFAULT 0, -- Nézési irány fokban (0-360, ahol 0 a nézőtér)
    PRIMARY KEY (timeline_node_id, project_dancer_id)
);
```

---

### B. A "Dream" Alkotói Munkafolyamat (Choreographer's Workflow)

#### 1. Fázis: Zenei Projekt Alapítás és Keretek (Preparation)
*   **Akció:** A koreográfus létrehozza a projektet (Pl. *"Salsa Duo Show 2026"*). Feltölti a megvágott hangsávot.
*   **Szabály-ellenőrzés:** Megadja a versenykategóriát (Pl. *"Max 2 perc 15 mp"* és *"Megengedett stílusok: Salsa, Rumba, Afro"*). A rendszer a háttérben folyamatosan ellenőrzi a hosszt és a felhasznált kódok kategóriáit (UAA 2.0 szűrőkkel), és figyelmeztet, ha nem megengedett mozdulatot vagy túl hosszú zenét használunk.

#### 2. Fázis: Videó Mashup és Idővonali Beillesztés (Visual Splicing)
*   **Szeletelés (Slicing):** A rendszer automatikusan felosztja a hangsávot a BPM alapján 8-as ütemekre (measures) és ütésekre (beats). A tanár vizuálisan látja az ütemrácsot.
*   **Mashup:** A tanár végigpörgeti a stúdió saját videótárát (vagy a "Fuzzy-to-Precise" pipeline-on át rögzített új referenciákat).
*   **Drag & Drop:** A legszebb referenciákat egyszerűen **ráhúzza** az idővonal kiválasztott ütemeire (Pl. *"A 12. ütem 1-4. ütésére tegyük be a 'Viral Rumba Challenge' 01:14-es vállhullám szilánkját"*). A rendszer automatikusan rögzíti a `video_reference_segment_id`-t.

#### 3. Fázis: Kinetikai és Forgatókönyv Térkép (Semantic Overlay)
*   A koreográfus elhelyezi a "zenei csomópontokat" (Hits / Accents). Ráírja a hangsúlyok nevét (Pl. *"Trombita leütés"*, *"Harang ütés"*).
*   A legfontosabb hangsúlyokra azonnal ráilleszti a legkifejezőbb kódolt mozdulatokat (Pl. `lvl.upCu` vagy `cod.rebd`).
*   A köztes időszakokra vagy szabad szöveges jegyzetet ír, vagy rávonzza a standard átvezető alaplépéseket, fokozatosan feltöltve a teljes idősávot.

#### 4. Fázis: Többtáncos Formációk Tervezése (Spatial Formations)
*   A felületen megjelenik egy 2D-s rács (a színpad felülnézete).
*   A koreográfus az idővonal egy adott ütemén (Pl. a 16. ütem 5. ütésén) az egérrel elhelyezi a táncosok ikonjait (Leader 1, Follower 1). Berajzolja az elmozdulásuk irányát és a nézési szögüket.
*   A rendszer az ütemek lejátszása során **interpolálja a pozíciókat**, így a tanár folyamatos animációként látja, hogyan fognak a táncosok áthelyezkedni a színpadon.

---

### C. 3D Szimulációs és Avatár Motor (3D Avatar Simulation Engine)
Ez a funkció teszi a rendszert valódi, jövőbe mutató **kutatóállomássá**. Az **AST (Abstract Syntax Tree) Parser** és a **Szemantikai DNA** lehetővé teszi, hogy a kódolt mozdulatokat ne csak szövegként értelmezzük, hanem vizuális mozgássá alakítsuk.

```
[Notation Code: lvl.upCu & pvtScrew] ──> [AST Parser] ──> [Kinematic bone rotation data] ──> [Three.js 3D Avatar Rendering]
```

#### 1. Hogyan fordítódik a kód 3D mozgássá? (Code-to-Motion compiler)
1.  **Szemantikai DNA felbontása:** A rendszer beolvassa az idővonalra írt kódokat (Pl. `pvtScrew`). A `term_components` táblából lekéri az atomi L0-s fizikai összetevőket: `pvt.R.180` (jobb láb pivot) + `sld.L.back` (bal láb csúszás hátra) + `lvl.down` (súlypont süllyesztés).
2.  **Anatómiai leképezés (Joint / Bone Mapping):** A rendszer minden atomi kódhoz hozzárendel egy parametrizált 3D csontváz-rotációs és elmozdulási mátrixot:
    *   `lvl.down:15` $\rightarrow$ A csípőízület (root joint) Y-tengelyű elmozdulása lefelé: `-15cm`.
    *   `pvt.R.180` $\rightarrow$ A jobb boka és talp ízület Y-tengelyű rotációja: `+180°`.
    *   `sld.L.back` $\rightarrow$ A bal csípő- és térdízület nyújtása, a bal boka elmozdulása hátrafelé a padlósíkon (Z-tengely).
3.  **Időbeli szinkronizáció (Time Warping):** A rendszer leolvassa a kód zenei ütemét (Pl. 1-4 ütés). A 3D-s animáció sebességét automatikusan a projekt BPM-jéhez igazítja, elosztva a rotációkat a megadott ütések időtartama alatt.

#### 2. Vizuális 3D Megjelenítés (Three.js / WebGL Web-Viewer)
*   A szoftver tartalmaz egy könnyű, böngészőben futó **3D WebGL szimulációs panelt**.
*   A koreográfus a lejátszás gombra kattintva látja, ahogy a virtuális humanoid avatár(ok) elvégzik az idővonalra írt mozgássorozatot a megadott térformációknak megfelelően.
*   **AI Mozgás-Generáció (AI Motion Synthesis):** Ha olyan összetett vagy ideiglenes kód szerepel (`sandbox_draft`), aminek nincs még precíz L0-s 3D-s leképezése, a háttérben futó **Motion Synthesis AI** (pl. egy finomhangolt Motion Diffusion Model) a szabad szavas leírás alapján legenerálja az avatár ideiglenes 3D mozgásfájlját (glTF/FBX formátumban), amit a tanár kézzel tovább finomíthat.

#### 3. Miért forradalmi ez az oktatásban és a betanításban?
*   **A "Koreográfia Digitális Ikerje" (Digital Twin):** A show teljes mértékben létezik a felhőben 3D szimulációként, kódolt szintaxisként és referencia-videókként.
*   **Azonnali megértés:** Ha a segédtanár nem biztos egy mozdulatban, nemcsak a kódot és a videó-mashupot látja, hanem a 3D panelt elforgatva, tetszőleges kameraállásból, lassítva nézheti meg a virtuális avatár "ideális" kinetikai kivitelezését, ízületi szögeit és súlypont-ívelését.
*   **Személyre szabott virtuális edzőtárs:** A versenyző otthon, a 3D-s avatárral együtt táncolva gyakorolhatja a show-t, akár VR-szemüvegen (WebXR) keresztül is, mintha az edzője folyamatosan mellette állna és mutatná az "ideális" mozgást.

---

## 15. A Tánctudományi Ökoszisztéma és a Jövőbeli Szolgáltatások (The Grand Vision, SaaS Ecosystem & Future Horizons)
A DANCE projekt végső és maximálisan elérhető víziója nem csupán egy zárt stúdiószoftver, hanem egy globálisan összekötő, oktatás-központú **Tánctudományi és Kreatív Alkotói Ökoszisztéma (Choreographic SaaS & Knowledge Platform)**. 

Ez a jövőkép a meglévő elméleti és technológiai alapokat (AST parser, Szemantikai DNA, 3D szimulátor, videó annotációs engine) kiterjeszti a globális piac, az aszinkron kollaboráció, az okos hardverek és a szerzői jogvédelem irányába.

```
                  ┌──────────────────────────────────────────────┐
                  │      DANCE GLOBÁLIS TUDÁSBÁZIS & SAAS        │
                  └──────────────────────┬───────────────────────┘
                                         │
       ┌─────────────────────────────────┼─────────────────────────────────┐
       ▼                                 ▼                                 ▼
┌──────────────┐                  ┌──────────────┐                  ┌──────────────┐
│  Git-for-    │                  │  Didaktikai  │                  │  Kinetikai   │
│  Dance       │                  │  Intelligens │                  │  Szerzői Jog │
│  Collab      │                  │  Gap-Engine  │                  │  IP Ledger   │
└──────────────┘                  └──────────────┘                  └──────────────┘
```

### A. Jövőbeli Szuper-Funkciók (Horizon 2-3 Core Features)

#### 1. "Git-for-Dance" — Elágazó és Egyesíthető Táncváltozatok (Choreographic Version Control)
Csakúgy, mint ahogy a szoftverfejlesztők a Git-et használják a forráskód kezelésére, a koreográfusok és oktatók is képesek lesznek verziókezelni a táncmozdulatokat és show-műsorokat:
*   **Elágaztatás (Branching):** Egy magyarországi iskola létrehozza a saját `bachata-fundamental` ágát. Egy spanyolországi iskola ezt leklónozza (Fork/Branch), és hozzáadja a saját egyedi stíluselemeit (pl. `sensual-fusion-v2`).
*   **Kód-Összehasonlítás (Choreographic Diff):** Az AST parser segítségével a rendszer vizuálisan és szintaktikailag ki tudja mutatni két koreográfia-ág közötti különbséget (pl. *"A spanyol változat a 12. ütemnél a standard lépés helyett `pvtScrew` rotációt és csípősüllyesztést használ"*).
*   **Egyesítés (Pull Requests):** A nemzetközi tanári közösség javaslatokat küldhet egymásnak a mozdulatok finomítására. Ha a főkoreográfus jóváhagyja (Merge), az új verzió beépül a hivatalos tantervbe.

#### 2. Didaktikai Intelligencia és Automata Hiányelemző (Predictive Gap Engine)
A szoftver nemcsak rögzíti, de elemzi is a tanulók fejlődési dinamikáját a skill mátrix és a tanterv között:
*   **Akadály-Detektálás:** A rendszer észleli, ha egy csoport 60%-a hetek óta elakad egy bonyolult mozdulatnál (pl. `maRchOutCbl`).
*   **Ok-Okozati Elemzés:** Az AST szülő-gyermek láncolatán (Szemantikai DNA) keresztül kiszámítja az elakadás okát: *"A tanulók azért buknak el a `maRchOutCbl` formációnál, mert 70%-uk még nem sajátította el stabilan a `sld.L.back` (bal láb hátra csúsztatása) atomi komponenst."*
*   **Automata Kurrikulum-Ajánló:** A rendszer automatikusan felajánl egy 3 alkalmas átvezető/előkészítő gyakorlatsort (didaktikai scaffolding), amellyel a tanár gyorsan és célzottan pótolhatja a hiányzó fizikai alapokat.

#### 3. Kinetikai Szerzői Jogvédő és Tánclicenc Jegyzék (Choreographic IP & Registry Ledger)
A művészvilág egyik legnagyobb problémája az egyedi mozdulatsorok és koreográfiák engedély nélküli másolása és a forrás megjelölésének elmaradása.
*   **Choreographic Fingerprint (Szemantikai Hashing):** Minden egyedi koreográfia és karakteres mozdulat lefordítható egy egyedi, titkosított AST hash-re (kinetikai ujjlenyomat).
*   **IP Registry:** A koreográfusok levédethetik show-műsorukat a rendszer globális jegyzékében.
*   **Automatizált Licenckezelés:** Ha egy másik iskola a rendszeren belül beépíti a levédett show-t a saját tantervébe vagy fellépési anyagába, a rendszer automatikusan számlázza a licencdíjakat, és elosztja a jogdíjakat a készítők között (SaaS-szintű mikrofizetésekkel).

#### 4. Okos Hardver & Wearable Szenzor Integráció (Real-Time Haptic Biofeedback)
A 3D-s szimuláció kiterjesztése a fizikai világba okoseszközök és viselhető szenzorok segítségével:
*   **Foot-Pressure & IMU Tracking:** Okos talpbetétek, okosórák vagy mozgásérzékelő pántok integrációja.
*   **Valós idejű összehasonlítás:** A tanuló tánc közben hordja az eszközöket. A szoftver összeveti a beérkező szenzoradatokat (gyorsulás, talpnyomás, szögelfordulás) az AST-ben tárolt "Ideális Kinetika" elméleti modelljével.
*   **Haptikus Visszajelzés:** Ha a tanuló súlypont-süllyesztése vagy az irányváltás időzítése eltér az elméleti ideáltól, az okosóra vagy a talpbetét enyhe rezgéssel (haptikus jelzéssel) azonnal figyelmezteti a hiba pillanatában, drasztikusan felgyorsítva az izommemória kialakulását.

---

### B. Globális Szolgáltatási Modellek (Business & Service Ecosystem)

#### 1. Choreography Asset Store — A Táncosok Eszköztára
Hasonlóan a játékfejlesztésben ismert Unity Asset Store-hoz, a rendszer egy globális piacteret (Marketplace) biztosít:
*   Az elit, világhírű koreográfusok feltölthetik a **"Kinetikailag Minősített" (Kinetically Certified)** show-anyagaikat, komplett csomagban:
    - A megvágott, engedélyezett zenét és a hozzá tartozó BPM rácsot.
    - A teljes, dekonstruált kinetikai kódsort (AST szintaxist).
    - A komplett 3D-s humanoid avatár mozgásfájlokat.
    - A vizuális canvas overlay rajzokat és a hozzájuk csatolt didaktikai sztorikat, metaforákat.
*   Bármely helyi stúdió/tanár megvásárolhatja ezeket a tananyagcsomagokat a SaaS rendszerben, és egyetlen kattintással beemelheti a saját oktatási portfóliójába, így biztosítva, hogy a világ legújabb trendjeit azonnal, a legmagasabb szakmai pontossággal taníthassa a saját diákjainak.

#### 2. Spatial Computing & Gamified Dance Academy (WebXR VR/AR)
A kiterjesztett valóság (Apple Vision Pro, Meta Quest) bevonása az otthoni tanulásba:
*   **Virtual Dance Mirror:** A tanuló a VR/AR szemüvegben látja saját magát (a szobája kameráján keresztül) és közvetlenül maga mellett az **ideális mozgást végző 3D avatárt**.
*   **Gamified Combos:** Pontokat kap a helyes ritmusért, a megfelelő ízületi szögekért és a sikeresen összekötött bonyolult kód-sorozatokért. Az elért pontokkal szinteket léphet a globális ranglistán.

#### 3. Globális Tánckutatási és Standardizációs Intézet (The Dance Standard Alliance)
A DANCE rendszer tudásbázisa idővel a világ legnagyobb, legpontosabb és legárnyaltabb táncleíró adatbázisává nőheti ki magát:
*   A rendszer adatai és tudományos mélységű dekonstrukciói alapul szolgálhatnak egy nemzetközi standard felállításához, amely végre tudományos, rendszerszintű és egységes nyelvet biztosít a táncművészetnek, hasonlóan a zenében a kottához vagy a programozásban a programozási nyelvekhez.













