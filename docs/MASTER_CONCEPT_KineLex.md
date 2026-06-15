# MASTER_CONCEPT (Dance Systems Architecture)

Version: 4.4.0 (DANA v1.34.0 platform-elv adoptáció: P52–P62, D095 + ontológia mint tudásgráf re-base jegyzet)
Date: 2026-06-16

> **DANA Platform-elv Adoptáció (v1.34.0):** A KineLex adoptálja a konszolidációs elveket. Kiemelten: **P56/D095** (tenant-izoláció `organization_id` + RLS; a fogalomtár megosztása buborék-scope-pal, láthatóság ⟂ funkció), **P58** (a kánon ↔ tanári dialektus a scoped-overlay kaszkád referencia-esete, a P49 mintán), **P57** (az AAA rövidítési/ütközésfeloldó szabályok a közös Rules-as-Data policy-mintát követik, de **külön domain** a pénzügyi routingtól), **P62** (traceability). 
> **Optimalizációs és Skálázási Paradigmaváltás (P63-P70):** **P65 (Graph-Native Knowledge):** a KineLex ontológia természeténél fogva **gráf** (L0–L4 rétegek, általános–speciális tengely, fogalom-evolúció, dialektus-kompatibilitás) → cél a **property graph / SKOS-jellegű** tárolás (pl. Neo4j, Apache AGE) a relációs táblák helyett, **stabil, verziózott `term_id`-kkal** (ezt fogyasztja a MeCat D084 szerint). **P69 (Compiler-as-a-Service):** Az AST parsing és a szemantikai refaktorizáció egy izolált aszinkron pipeline-ba kerül (Git-for-Dance), nem valós idejű DB tranzakcióként fut. Aktiválás a megfelelő Phase-ben (lásd `BACKLOG_PROMPT_CACHE` PR-014).

Ez a dokumentum a "DANCE" projekt szoftverarchitektúrájának, fogalomterének és formális leíró nyelvének egyetlen igazságforrása (Source of Truth - SoT). A rendszer fő célja, hogy egy rendkívül tömör, szótárral visszakövethető (dictionary-backed), de gépileg és emberileg egyaránt könnyen írható és olvasható hibrid kódrendszert biztosítson a solo és social páros táncok (különösen salsa, mambo vagy "on2" salsa, bachata, chachacha, jazztánc, contemporary) leírására és ezáltal oktatási rendszerbe foglalására.

## 1. Rendszer-architektúra és Filozófia

### Vizuális és UI Követelmények (UI & Visual Guidelines)

**Egyértelmű betűtípusok:** A rendszer minden felületén (frontend), ahol a formális nyelv kódjai és az acronymok megjelennek, **kötelező** olyan "Arial egyszerűségű", letisztult, de szigorúan "disambiguation-friendly" (nem redundáns) betűtípust használni (pl. egyértelmű monospace vagy modern sans-serif), ahol a kis "l" betű, a nagy "I" betű és az "1" szám vizuálisan élesen elkülönül egymástól (pl. ligatúrák és serif hiánya). Ennek célja a kódok olvasásakor fellépő vizuális ütközések teljes kiküszöbölése.

### A Rendszer Felhasználási Gradiense: Pedagógiai Pólus és Tudományos Kutatói Pólus (Capability-Commitment-Authority Gradient)

A rendszer **nem egyetlen, homogén szoftver**, és nem is két, élesen szétválasztott külön termék, hanem egy **képesség-vállalás-jogosultság alapú gradiens** (Capability-Commitment-Authority Gradient) mentén szerveződik. A funkciókat **nem éles határvonal, hanem fokozatos átmenet** választja el aszerint, hogy a felhasználó milyen mélységig **képes** és **hajlandó** a fogalmak atomizálásával, dekonstrukciójával és tudományos pontosításával foglalkozni, bár az egyes tudományos kutatói funkciókhoz és szintekhez való hozzáférést egy központi adminisztrációtól igényelni kell:

- **Pedagógiai Pólus (Teaching Pole):** A "mezei" tánctanár napi munkáját szolgálja: tananyag fejlesztés és tisztítás, óratervezés, csoport- és tanítványkövetés, tudásszint-térképezés, részvétel- és tanítási mélység-alapú haladásmérés. A tanár itt a fogalmakat **fekete dobozként** (scaffold / egyezményes figuranév / saját, kitalált figuranév szinten) használja, és **nem köteles atomizálni** vagy a központi fogalomtárba regisztrálni. A teljes pedagógiai értéket úgy is megkapja, ha sosem lép a kutatói pólus felé.
- **Tudományos Kutatói Pólus (Research Pole):** A kutatói funkciók (tanárfüggetlen központi fogalomtár és rövidítésrendszer fejlesztése, atomi dekonstrukció, ontológia-kezelés, kombinatorikus variációs motor, vizuális annotáció, 3D szimuláció, IP-jegyzék) a tudásbázis mélyítését és a leíró nyelv fejlesztését szolgálják. Ez **opcionális, többletvállalás**, amely a pedagógiai adatokra épül.

A két pólus **ideális esetben összeér**: a pedagógiai követés adatai (mit, hányszor, milyen részletességgel tanítottak, milyen gyakorisággal gyakoroltak) táplálják a kutatást, a kutatás atomizált fogalmai pedig egyre finomabb pedagógiai követést és **absztraktabb, helyzetérzékenyebb, improvizatívabb tanulói tudást** tesznek lehetővé. A gradiens lényege, hogy minden tanár ott állhat meg, ahol a kompetenciája és a vállalása engedi — a rendszer minden szinten teljes értékű.

Ennek a gradiensnek és a hozzá tartozó jogosultsági rétegeknek a részletes leírását lásd a **16. fejezetben**, a pedagógiai modult a **17. fejezetben**, a tudás természetének (koreográfia vs. improvizáció) kezelését a **18. fejezetben**, az atomizáció-vezérelt improvizációs absztrakciót pedig a **19. fejezetben**.

---

## 2. Absztrakciós Szintek a Mozgásban (Movement Abstraction Layers)

A mozgásokat és a tudásanyagot öt egymásra épülő, egyre komplexebb absztrakciós szinten modellezzük a fizikai alapoktól a színpadi koreográfiákig:

**L0: Térbeli és Anatómiai Primitívek (Statika és Keretek)** Ez a legalapvetőbb szint, a "vászon", amelyen a tánc történik. Itt még nincs időbeli változás, csak állapotok és fizikai keretek.

- **Fizikai és környezeti összetevők (Physical and Environmental Components):** Tér, test, gravitáció, súly, alátámasztás, parkett sík, színpad vagy táncterem kitüntetett közönség/néző iránya, a színpad abszolút pontjai és irányai (stage center, stage back, stage front, stage left, stage right, diagonals). 
- **Testrészek (Body Parts):** A mozgásra képes izolált egységek (fej, vállöv, gerinctengely, csípő, végtagok). Testkontúrok (Body Lines / Imaginary Body Parts): a képzetes testrészek (pl. oldalvonal, laterál-mediális vonal) és a testhelyzetek burkoló görbéi (Silhouettes).
- **Táncirányok (Dancers Directions):** A test szemben iránya (origo direction), a test merőleges síkjai (sagittal plane, horizontal plane, lateral plane), a nézet vagy közönségirány (view), a kommunikáció iránya (communication direction), összetartozó táncosok pl. táncpárok (couples) egymáshoz viszonyított helyzete, amely: a táncpartnerek tánctartás vektorainak tengelye (dancing position axis), szögbeli nyitottsága (face-to-face / promenade position), a táncpartnerek távolsága (near / far). Orientáció (Orientation): a parkettsíkra vetített szöge (azymuth / horizontal angle) és az abból adódó relatív parkettirányok és relatív térirányok (forward, backward, diagonal, side, up and down).
- **Alátámasztás (Support):** Testsúly-megtartási pontok (több pontos / harmadik pont pl. kéztámasz, kétpontos terpesz, egypontos) és testhelyzetek (állás, guggolás, ülés, fekvés).

**L1: Atomi Műveletek és Kinetika (Dinamika és Változás)** Ezen a szinten jelenik meg az idő (`>` mint `from -> to` és `<` mint origin operátorokkal), azaz az L0-s pozíciók megváltoztatása. Ide tartoznak a legkisebb, tovább már nem bontható mozdulatok.

- **Pozíciók (Positions):** Állás, ülés, térdeplés, fekvés, keresztezett lábak, gimnasztikai pozíciók (pl. kézállás, gyertya állás), jóga ászanák (pl. padmászana, lefelé néző kutya póz).
- **Elemi Testmozdulatok és Szabadságfokok (Elemental Body Actions & Degrees of Freedom):** Ide tartoznak az olyan alapvető kinetikus akciók, mint a homorítás, domborítás, kontrakció, nyújtás, hajlítás, eltolás (shift), emelés (elevation / lift), hajítás (throw), ütés/csapás (hit / bump), rotation, tilt, nod. Kombinált testmozdulatok (head, shoulder girdle, hip): 8, C rolls, twists, corridor circles / paddle, waves, barrell roll / orbit or tangential path.
- **Mozdulat Útvonalak és Stílusok (Motion Paths / Motion Path Styles or Via elements):** Kör, egyenes, visszapattanó, emitter/collector irányok. Style: térbeli elmozdulás mikéntje (egylendületű/Direct, tört, hurkos/Looped, firkálós/Scribble).
- **Kiegyenlítő Mozdulatpárok (Compensatory / Counter-Balancing Gestures):** A csípő, a törzs és a végtagok olyan összehangolt mozdulatpárjai, amelyek a test alsó vagy felső központjában "kiegyensúlyozott csendet" (referenciapont-stabilitást) hoznak létre. Például a csípő teljes kitolása egy lábon állásig, miközben a másik kar oldalsó megnyújtása komplementer egyensúlyt ad, megőrizve a fej mozdulatlanságát. (Ide tartozik az azonos oldali lábkör/rond és a komplementer karkör együttes alkalmazása a törzs stabilitásáért).
- **Lépést Előkészítő Műveletek (Limb Actions):** Súlytalan lábáthelyezések (on-floor foot transfers: slide or glide, off-floor foot transfers: tap, kick, rond, flag), terpeszváltások (swaps). Súlyvonal- és Lábkinetika (lépés): Horizontális súlyvonal mozgatás (center of gravity progression), súlyvonal irányváltás (center of gravity change of direction), Vertikális súlyvonal mozgatás (center of gravity elevation / sink, godown) / ugrások (jumps, leaps), lépés, súlyláb mikro tengelyfordulat (amit a `*` operátor, pl. `*lFoot` jelöl).
- **Tánc alapmozdulatok (Legacy):** A hajlítás (plier), nyújtás (étendre), emelkedés (relever), siklás/csúszás (glisser), ugrás (sauter), kivetés/megiramodás (élancer), forgás (tourner).

**L2: Ciklikus Műveletsorok és Szóló Alapok (Struktúra és Ritmus)** Az L1-es atomok zenei struktúrára (periódusokra) fűzött, felismerhető mintázatai. A zenei idő kitöltésének strukturált módjai.

- **Zenei Ciklusok:** A zenei ütemezés és a táncot meghatározó "zenei kérdés" és "zenei válasz" részütemek ciklikussága.
- **Ciklikus Lépéssorok (Cyclic Step Patterns):**
  - **Alaplépések (Basic Steps):** Vonalon elvégzett "rezgés" (lengés) természetű, önmagába ciklikusan (periódus idő) visszatérő terpeszváltás-sorok (például túlgördülő/Over-rolling és irányváltó/Change of Direction fázisokból felépülő sorozat).
  - **Nevesített Szóló Karakter Mintázatok (Solo Patterns):** Táncos alap összetevőkkel változtatott "karakterizált" lépéssorozatok és forgások: crossover basic, slide basic, double front crosses, box, diamond, mambo jazz, stb.
  - **Ritmus Díszítések (Rhythm Adornments):** Lépést helyettesítő / kiegészítő műveletek: magasságváltás (leveling), láb talaj érintés (tap), rúgás (lick), taps (handclaps), kinyúlás (reach out), kimutatás (point out), vállrázás (shimmy).

**L3: Páros Alapfigurák (Interakció)** A tánc kiterjesztése a partnerek közötti fizikai és vizuális kommunikációra.

- **Páros Ciklikusság Alapfeladatai:** A vezető és követő szerep alapszabályai (vezető általi a követő alaplépésének végpont átírása, követő oldali semleges és szembeforduló állapot rekonstruálása a periódusidő végéig).
- **Vezetés és Követés Mechanikája (Lead and Follow Mechanics):** Erőhatások és holtjátékok, kerettartás, vizuális és fizikai kapcsolódási pontok és azok helyzete a vezetőhöz és követőhöz képest (tolt/húzott/tangenciális ív), prediktív (positive backlash) pattintások.
- **Alapvető Páros Építőelemek és Figurák (Core Couple Elements & Figures):** Egyszerű, vezetés-követéses rendszerű alapelemek és helycserék (például Cross Body Lead, keresztvezetések, alap páros forgások és kifordulások/Turnouts).

**L4: Szekvenciák és Koreográfiák (Láncolás)**

A legmagasabb absztrakciós szint, amely a korábbi elemek hosszabb, tudatos kompozíciója.

- **Páros és Szóló Sorok (Sequences):** Több L2/L3-as elem összefűzött, de még improvizatív keretek között alkalmazható láncolata.
- **Koreográfiák (Choreographies):** Időben kötött, specifikus zeneműre készített, rögzített összetett figurák és vizuális koncepciók (show elemek).

---

## 3. Adaptív Rövidítési Algoritmus és Szótár Verziók (AAA & Vocabulary Versions)

A leíró nyelv kódjainak tömörsége és írási sebessége érdekében a rendszer a **Rövidítési Skála-Spektrumra (ASS - lásd 4. fejezet)** támaszkodva egy dinamikus, használat-alapú és verziózható szótárkezelő algoritmust (AAA) üzemeltet.

### A. Szuper Rövidítés (Super AAA) - Az Elit Szint (ASS L1 & L2)

A Szuper Rövidítés a rendszer "Hírességek Csarnoka", ahová a leggyakrabban használt bázisfogalmak kerülnek, elfoglalva a **Rövidítési Skála 1 és 2 karakteres (L1, L2)** privilégizált kódjait. Ez egy háromfázisú, folyamatosan fejlődő életciklus:

1. **Fázis: Az Induló "v1.0 Csomag" Bootstrappingje (Initial Package Bootstrapping):**
  - A rendszer elemzi a meglévő tananyagot (syllabus).
  - A tudományos pólus (T4) jóváhagyásával létrejön egy zárt induló csomag, amelyben a leggyakoribb szavak fixen szuper-rövid (1-2 karakteres) L1/L2 kódokat kapnak.
2. **Fázis: Használat-Alapú Statisztika és Dinamikus Monitorozás:**
  - Aktív használat közben a rendszer méri a fogalmak használati gyakoriságát (Frequency Tracking).
3. **Fázis: Új Szótárverziók Bevezetése és Automatikus Migráció:**
  - Ha egy fogalmat fel kell minősíteni szuper-rövid L1/L2 szintre, vagy visszafokozni, a rendszer új szótárverzió bevezetését javasolja (Lektorálási folyamat).
  - Jóváhagyás után a **Visszamenőleges Frissítés (Retroactive Migration Pipeline)** (lásd 11.D fejezet) visszamenőleg átírja a régi óravázlatok kódjait, garantálva a történelmi konzisztenciát.

**Függetlenség az Absztrakciós Szintektől (Independence from Didactic Layers):**
Fontos tisztázni, hogy a Super AAA "elit" klubjába való bekerülés **kizárólag a statisztikai gyakoriságtól (Frequency)** függ, és teljesen független attól, hogy a fogalom anatómiai primitív (L0) vagy egy komplex páros figura (L3). A Rövidítési Skála (ASS L1-L4) és a Didaktikai Absztrakciós Szintek (L0-L4) két egymásra merőleges, független tengelyt alkotnak a rendszerben. Bármilyen didaktikai szintű fogalom kaphat szuper-rövidítést, ha a használati gyakorisága azt megköveteli.

### B. Standard Rövidítési Elvek (Standard AAA Rules) - A Keltető (ASS L3-L6)

Minden olyan fogalom, amely nem része a Super AAA elit (1-2 karakteres) készletének, a **Rövidítési Skála 3-6 karakteres (L3-L6)** tartományában, a "Keltetőben" kap helyet az alábbi automatizált elvek alapján:

1. **CPC-3 (Consonant Pattern Coding - 3 betűs - L3):**
  A szavakból kivonjuk a magánhangzókat, és az első három mássalhangzót tartjuk meg. (Pl. `glide` -> `gld`, `mambo` -> `mmb`).
2. **Kimondhatósági Kivétel (Pronounceability Exception - L3/L4):**
  Ha a CPC-3 kód nehezen kimondható, a hagyomány és érthetőség kedvéért szótag-alapú csonkolást használunk. (Pl. `caminando` -> `cami`, `cucaracha` -> `cuca`).
3. **Több Szavas Fogalmak Rövidítése (Multi-Word Concepts - Acronyms):**
  Ha egy bázisfogalom eleve több szóból áll (pl. `Stop and Go`, `Cross Body Lead`), nem a CPC-3 logikát, hanem a kezdőbetűs **Mozaikszó (Acronym)** logikát alkalmazzuk L3 (vagy L2) szinten.
  - *L_max (Full Word):* `stopAndGo`
  - *L3 (Standard):* `sag` (A szavak kezdőbetűiből: Stop And Go)
  - *L2 (Super):* `sg` (A kötőszók elhagyásával: Stop Go)

### C. A Két Elv Harmonikus Együttműködése (The Adaptive Synergy)

A Super AAA és a Standard AAA egyazon adaptív nyelvi életciklus szorosan együttműködő két fázisa:
1. **Kétirányú Ozmózis (Bi-directional Osmosis):** A Standard AAA (Keltető) biztonságos és determinisztikus. Ha itt egy kód (pl. `cuca`) használati statisztikája átlép egy küszöböt, felminősítést nyer a Super AAA (Hírességek Csarnoka) szintre (pl. `cc`). Ha egy trend lecseng, egy Super kód visszaminősülhet Standard szintre.
2. **Zökkenőmentes Identitás-Megőrzés (Seamless Identity Preservation):** Ha egy fogalom szintet lép (pl. L3-ról L2-re), a rövidítése megváltozik, de a `term_id` UUID-ja állandó marad. A Verziózott Szótár (13.D) révén a történelmi kódok sosem törnek el.

---

## 4. Rövidítési Skála-Spektrum (ASS) és Ütközésfeloldás

A 3. fejezet algoritmusait a **Rövidítési Skála-Spektrum (ASS - Abbreviation Scale Spectrum)** vezérli. A pontok felesleges burjánzásának elkerülésére és a kódok hosszának minimalizálására minden fogalom több tömörítési szinttel rendelkezhet.

### A. Minimalizált Tömörítési Szintek (Spectrum Layers)

Mivel a tánc tele van komplex és hasonló fogalmakkal, az L1-L3 tartomány nem nyújt elegendő teret a tömeges ütközések elkerülésére. Ezért a skálát a 4, 5 és 6 karakteres állapotokra is kiterjesztjük, hogy az emberi agy számára minden ponton azonnal felismerhető (intuitív) maradjon a kód.

1. **Level 1 (Core - 1 karakter / Super AAA):** A leggyakoribb báziselem kód (pl. `pendulum` -> `**p**`, `external` -> `**e**`).
2. **Level 2 (Strictly Minimal 2-char / Super AAA):** Kétkarakteres kód. (Pl. `pendulum` -> `**pd**`, `external` -> `**ex**`).
3. **Level 3 (Standard CPC-3 / Standard AAA):** Háromkarakteres kód. (Pl. `pendulum` -> `**pen**`, `external` -> `**ext**`).
4. **Level 4 (Standard AAA):** Négykarakteres kód. (Pl. `pendulum` -> `**pend**`, `external` -> `**extl**`).
5. **Level 5 (Standard AAA):** Ötkarakteres kód. (Pl. `pendulum` -> `**pendu**`, `external` -> `**extnl**`).
6. **Level 6 (Standard AAA):** Hatkarakteres kód. (Pl. `pendulum` -> `**pendlm**`, `external` -> `**extrnl**`).
7. **L_max (Full Word):** A fogalom teljes angol kiírása (pl. `pendulum`, `external`).

### B. Intuitivitást Elősegítő Harmonizáló Elv (Intuitive Cognitive Completion Rule)

A 3, 4, 5 és 6 karakteres rövidítések generálásakor a puszta magánhangzó-kivonás vagy szótagolás már nem mindig elég hatékony. Az emberi agy "első ránézésre kiegészíti" élményének (cognitive completion) garantálásához a két módszert **váltogatva, egy harmonizáló algoritmus** szerint alkalmazzuk:

- **A Szabály Hierarchiája 3+ karakteres szavaknál:**
  1. A rövidítésnek mindig az **eredeti szótag-határokat (Syllable Boundaries)** vagy a **kulcs-mássalhangzókat (Key Consonants)** kell megcéloznia.
  2. Ha a csonkolt szótag-vég magánhangzóra esne, és ez rontaná a szó vizuális sűrűségét, a következő kulcs-mássalhangzót húzzuk be.
  3. A szóvégi szuffixumokat (`-tion`, `-nal`, `-lum`) drasztikusan (mássalhangzókra) sűrítjük a hosszabb kódoknál.
- **Példa 1: `pendulum`**
  - L1: `p` (Kezdőbetű)
  - L2: `pd` (Első és harmadik mássalhangzó)
  - L3: `pen` (Első teljes szótag)
  - L4: `pend` (Első szótag + következő mássalhangzó - *kognitívan a legerősebb*)
  - L5: `pendu` (Két szótag)
  - L6: `pendlm` (Két szótag + az utolsó szótag mássalhangzós sűrítése)
- **Példa 2: `external`**
  - L1: `e`
  - L2: `ex` (Első vizuálisan erős tag)
  - L3: `ext` (Szótag + következő mássalhangzó)
  - L4: `extl` (L3 + a szóvégi záró mássalhangzó)
  - L5: `extnl` (L3 + utolsó szótag sűrítve)
  - L6: `extrnl` (Kivonásos CPC logikához közeledve)

Ez az elv biztosítja, hogy a skála bármely értékénél a kód kimondható vagy vizuálisan azonnal feloldható (decode-olható) legyen a tanár agyában.

### C. Kiosztási és Értelmezési Szabályok (Allocation & Interpretability Rules)

Az L1 és L2 szuperrövidítések odaítélése a következő elvek alapján történik:

1. **Gyakorisági vs. Atomizációs Prioritás (Frequency vs Atomicity Priority):** A szintek kiosztása a statisztikai gyakoriság és az atomizáltság egyensúlyán múlik. 
  - *Sarokkő esetek:* A `basic` (összetett) és a `back` (atomi irány) is a `b` betűre pályázik. Mivel a `basic` szót gyakrabban használjuk, az kapja az L1-es `**b**` kódot, míg a `back` hátrébb tolódik L2-re (`**ba**`).
2. **Szófaj/tulajdonság elkülönítése (Type disambiguation):** Hogy elkerüljük az irány és a testrész keveredését, a `back` (hátra irány) lesz az L2-es `**ba**`, a `back` (hát testrész) pedig az L3-as `**bck**`.
3. **Értelmezhetőségi Szóösszevonási Szabály (Interpretability Merge Rule):** Ha egy összetétel **tiltó vagy ellentétes értelmet kifejező előtagot** (pl. `in-`, `un-`, `non-`) tartalmaz, az kivételt képez a szuper-rövidítés alól. Nem vonható össze a bázisszóval.
  - *Példa:* `dir` = direct, `inDir` = indirect. Hiába lenne logikus az `ind` rövidítés az `inDir`-ből, elveszítenénk az `in` előtag jelentését. A logikai érthetőség felülírja a tömörítést.

### D. Dinamikus Ütközésfeloldó Protokoll (Compression Shift Rule)

Amikor CamelCase formátumban összetett szavakat képzünk (pl. *Left Arm*), a motor megpróbálja az L1 + L1 formát alkalmazni. Ha ütközést észlel egy bázisfogalommal, bekapcsol a **Tömörítési Skála Elmozdulás (Compression Shift Rule)** és az alábbi hierarchia szerint lép tovább:

1. **Domain-Specifikus Szó Prioritása (Domain-Specific Noun Priority):** A szigorú, univerzális minősítők (`l`, `r`) a legstabilabb alapkövek. Ütközés esetén **először mindig a domain-specifikus főnevet léptetjük fel** az L2 szintre, és a minősítőt hagyjuk L1-en.
  - *Kísérlet #1 (L1 + L1):* `l` + `a` $\rightarrow$ `la` -> **ÜTKÖZIK** a `la` (lady) bázisszóval. (Elvetve)
  - *Kísérlet #2 (L2-Modifier + L1-Noun):* `le` + `A` $\rightarrow$ `leA` -> Fókusz a minősítőn, főnév felismerhetetlen. (Elvetve)
  - *Kísérlet #3 (L1-Modifier + L2-Noun):* `l` + `Ar` $\rightarrow$ `**lAr**` -> **NINCS ÜTKÖZÉS!** A `l` egyértelmű, az `Ar` jól olvasható. (Elfogadva)
2. **Rokonértelmű Kifejezések Felajánlása (Synonym Suggestion Strategy):** Ha az elmozdulás sem oldja meg az alaki ütközést (vagy a kód már túlzottan felduzzadna), a rendszer **aktívan felajánlja egy rokonértelmű (szinonima) kifejezés használatát**, amelynek rövidített formája hatástalanítja az ütközést.
  - *Példa:* Ha egy új kifejezés lerövidítve `rot` lenne, de ez már foglalt a *rotation* által, a rendszer javasolhatja a *spin* (`spn`) vagy a *turn* (`tu`) szavak használatát a szótárban. Ezzel a nyelvtan organikusan fejlődik a leginkább ütközésmentes fogalmak felé.

Ez garantálja, hogy a kódok vizuálisan egyértelműek és jól verbalizálhatóak maradjanak.

---

## 5. Hibrid CamelCase és Pont (Dot) Szeparációs Nyelvtan

*(Megjegyzés: Az ebben a fejezetben és a dokumentum további részeiben szereplő kódok és rövidítések kizárólag feltételezett, nem előre eldöntött, illusztratív példák!)*

A kódok láncolatának minimalizálása érdekében egy **hibrid rendszert** alkalmazunk: ahol a CamelCase egyértelmű és kompakt, ott azt használjuk; ahol viszont kétértelműség vagy strukturális tagolás szükséges, ott kötelezően bevezetjük a pont (`.`) vagy space ( ``) karakterek használatát.

### A. Mikor használunk CamelCase-t? (Takarékos láncolás)

A CamelCase-t használjuk a bázisszavak és standard jelzőik közvetlen összekapcsolására, az alábbi szigorú **Vizuális Egyértelműségi (Anti-Crowding)** szabályok betartásával:

1. **Tiszta Kisbetűs Összeolvadás (Lexical Merge):** Ha az összetett kód legfeljebb 4 karakterből áll, egyáltalán **nem használunk nagybetűt**. Csupa kisbetűvel írjuk, mert ilyen rövidségnél az emberi szem egyben is képes dekódolni.
  - *Példa:* `lt` (left turn), `lft` (left foot), `lto` (left turn open).
2. **Anti-Kapszula Szabály (No Consecutive Capitals):** A rendszerben **szigorúan tilos a csupa nagybetűből álló szótömbök (pl. LTO, LT, RT)** használata. Ezek vizuálisan agresszívek, a kis `l` és nagy `I` miatt könnyen félreolvashatók, és nem tagolják a szót. Két nagybetű sosem állhat közvetlenül egymás mellett!
3. **Főnévi Bázis (Base Noun) Kiemelése:** Kizárólag az összetétel **főnévi magja** (az akció vagy testrész) kaphat nagybetűt a szóhatáron. Az L1-es minősítők (l, r, f, b) mindig kisbetűsek maradnak.
  - *Példa:* A "Left Turn" esetében a `Turn` a főnév. Mivel az `lT` sértené az Anti-Kapszula szabályt (ha más is jönne utána), a Turn-t L2-re toljuk: `lTu` vagy `lTurn`.
  - *Példa:* "Left Foot" (l + ft) -> `lFt` (Az F nagy, a t kicsi, a szabály teljesül).
  - *Példa:* "Left Turn Open" -> Tilos az `LTO` és az `lTO`. Helyes megoldások: `lto` (mert 3 karakteres), vagy hosszabb tagolásnál `lTuOp`.
  - *Példa:* `bwdCami` (Backward Caminando - nem kell pont, mert a `C` nagybetű jelzi a határt, és kisbetű követi).

### B. Mikor kötelező a Pont (`.`) használata? (Szülő-Gyermek Viszony és Specifikáció)

A pont szeparátor használata szigorúan **csak strukturális (objektum-tulajdonság, hierarchikus) tagolásra** használható, szavak lineáris elválasztására nem!

1. **Szülő-Gyermek Viszony (Parent-Child) / Hierarchikus és anatómiai névterek (Anatomical Namespaces):**
  - *Szabály:* A pontot az objektumok egymásba ágyazottságának kifejezésére használjuk.
  - *Példa:* A gerinc végpontjainak és eltolásainak leírásakor: `**occi.shift.lat`** (occiput lateral shift) vagy `**body.arm.left`**. Ez logikailag pontos és kódolható.
2. **Állapotváltozások és Specifikációk (Properties & Types):**
  - *Példa:* Forgások és kivezetéseik specifikálásakor (ahol a kivezetés a forgás egy altípusa/tulajdonsága): `**lTu.tuO`** (Left Turn with Turnout) vagy `**rTu.tuB`** (Right Turn with Turnback). Itt sem használunk csupa nagybetűt (nem LLT.TO).

*(Fontos változás: A pontot TÖBBÉ NEM használjuk lineáris szóösszetételek vizuális elválasztására vagy rövidítési ütközések megelőzésére (pl. tilos az `l.a` a left arm-ra). Erre a CamelCase-en belüli Tömörítési Skála Elmozdulás (Compression Shift) szolgál, lásd a 4. fejezetet!)*

### C. Egyéb elválasztó operátorok

- `-` (Dash spaces): Időbeli szekvencia (egymás utániság), pl. `b1 - CBL - LLT`.
- `&` vagy `+`: Egyidejű (szimultán) akciók, pl. `M:cbl & F:spin` vagy `lh.b & rh.b`.
- `:` (Colon): Szerepek vagy bázis-időzítések megjelölésére, pl. `1:step` (az első ütésre történő lépés).

---

## 6. Ritmikai Szintaktika és Idővonal (Megtartandó Hagyaték & Hibrid Modell)

A 8-ütéses (8/4) ritmusú rendszerek zenei szintaktikáját szigorúan megőrizzük, de az egydimenziós (lineáris) szöveg és a többdimenziós (párhuzamos) tánc közötti feszültség feloldására egy **Hibrid Idővonal-Modellt** alkalmazunk.

*(Megjegyzés: A Span és Track operátorok jelenleg kidolgozás/megvitatás alatt állnak, a végleges szintaktika változhat!)*

### A. Pontszerű Zenei Markerek (Hit)

- **Ritmikai képlet:** `**12 + 34 & 56 + 78`**
- **Időzítési marker (`X:`):** A számérték és a kettőspont jelzi a konkrét zenei ütésre történő mozgásokat.
  - `**1:`** = Első ütésre történő mozgás (pl. `1:bwdCami` - lépés hátra az 1-re).
  - `**5:`** = Ötödik ütésre történő mozgás.
  - `**5&6:`** = Siettetett, sűrített ütemek (hastening).

### B. Időtartam Operátor (Span) - Egyszerű párhuzamosság (TERVEZET)

Amikor egy mozdulat (pl. egy karmozdulat) átível több lépésen, a kötőjel (`-`) használható az időtartam kifejezésére, amelyet a `+` vagy `&` kapcsol a lépésekhez.

- *Példa:* `1-4:upCu & 1:l 2:r 3:l` (Az 1-4. ütés alatt folyamatos upper curve, miközben 1, 2, 3-ra lépünk).

### C. Relatív Horgonyzás (Structural Anchoring) - Pedagógiai szint

Nem kötelező számokat használni. A kitartott mozdulat ráköthető egy ismert zenei/lépés blokkra, mint tulajdonság.

- *Példa:* `b1(upCu)` vagy `upCu.b1` -> "Csináld a Basic első felét, és közben csinálj egy upCu-t."

### D. Sáv-alapú Bontás (Track/Voice Separation) - Kutatói szint (TERVEZET)

Komplex izolációknál (ahol a testrészek független ritmikát táncolnak) a kódot névterekre (sávokra) bontjuk, amit a UI egy kottaszerű Timeline nézetben vizualizál:

- *Szöveges kód:* `Legs[1:l 2:r 3:l] & Arms[1-4:upCu]`

---

## 7. Vektor és Dinamika Operátorok (Adverbs of Movement)

A mozgás irányának, eredetének és mikro-dinamikájának leírására a rendszer egyedi szimbólumokat (operátorokat) használ. Ezek célja a kód tömörítése és a hosszú angol határozószók (from, to, around, spin) vizuális kiváltása. A UI felületeken ezeket gyorsgombokról (Kód-paletta) is el lehet érni.

### A. Irány és Vektor Operátorok (Térbeli mozgás)

- `>` **Cél / Irány (To / Towards / Transition into):** A mozdulat vagy átmenet célpontja. Valahová tart, valamivé válik.
  - *Példa:* `2h > hlock` (Kétkezes fogásból átmenet hand-lock pozícióba).
- `<` **Eredet / Honnan (From / Originating from):** A mozdulat vagy impulzus kiindulópontja.
  - *Példa:* `<chest` (A mellkasból induló mozdulat/impulzus).

### B. Dinamika és Kinetika Operátorok

- `*` **Forgás / Pördülés tengelye (Rotation / Spin axis rajta álló lábon):** A súlyláb talajon való elfordulását vagy egy extra mikrotengelyt jelöl.
  - *Példa:* `*lFoot` (Tengelyfordulat a bal lábon).

---

## 8. Általános - Speciális Tengely és Induktív Fogalom-Felfedezés (General-Specific Axis & Inductive Discovery)

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

### D. Diff-alapú Megfogalmazás (Diff-Based Formulation) - Helyettesítés

A rendszer támogatja a már létező, komplex fogalmakból kiinduló **eltérések (diff)** megfogalmazását. Ez lehetővé teszi, hogy a tanár egy hosszú, jól ismert szekvenciát használjon alapként, és csak azt az egy elemet definiálja, amelyben az ő variációja eltér a sztenderdtől.

Ezzel a módszerrel az új variáció közvetlen **szülő-gyermek (parent-child)** kapcsolatba kerül az eredeti figurával a rendszerben.

*Szintaktikai Javaslat (Csere operátor):* `AlapFogalom[eredeti_komponens -> új_komponens]`

1. **Példa (Salsa On1 Basic Step módosítása):**
  - Alapfogalom: `salsaOn1Basic` (amely magában foglalja a "collector" karmozdulatot a zenei kérdés alatt).
  - Új variáció: A "collector" kar helyett "colCod" (collector change of direction).
  - Diff kód: `**salsaOn1Basic[collector -> colCod]`** (vagy rövidítve: `b1[col -> colCod]`).
2. **Értelmezés:** A compiler ezt úgy olvassa, hogy "Vedd a `salsaOn1Basic` teljes fájlfáját, keresd meg benne a `collector` komponenst, és cseréld le a `colCod` komponensre". Ez minimalizálja a gépelést és azonnal érthető, miközben strukturális pontosságot ad.

A kutatás-fejlesztés felgyorsítására a rendszer tartalmaz egy logikai generátort, amely az alábbi dimenziók mentén állít elő új, még nem létező variációkat (kísérleti feladatokat):

- **Variációs Dimenziók:**
  1. *Kapcsolódás (Hold):* `2h`, `1h`, `rhx`, `lhx`, `noHand`
  2. *Pálya (Path/Trajectory):* `colUp`, `colDw`, `straight`, `loop`, `wave`, `zigzag`
  3. *Dinamika (Dynamics/Technique):* `rebd` (rebound), `sld` (slide), `gld` (glide), `cod` (change of direction)
  4. *Kivezetés (Ending):* `TO` (turnout), `TB` (turnback), `End` (alt end)
- **Kísérleti Generáció:** A CVE egy bázis kifejezéshez (pl. `Cruz`) kiszámítja az összes logikailag lehetséges, de az adatbázisban még **NEM** szereplő variációt.
- **Status: Experimental:** Ezeket a generált kódokat a rendszer `status = 'experimental'` címkével menti el, és a tanárok felületén a **"KutatóLab" / "Research Lab"** panelen jeleníti meg, mint kikísérletezendő, új didaktikai kihívásokat.

---

## 9. Core Inputok és Dekonstrukciós Életciklus (Core Inputs & Deconstruction)

A tánctanárok gyakran külső, karakteres mintákat (pl. Instagram videók, fesztivál workshopok anyagai) használnak a tananyag fejlesztéséhez. Ezeket a rendszer nem szimpla variációként, hanem **"Core Inputként" (Challenge Sentences)** kezeli, amelyek célja a stúdió saját "szókincsének" (vocabulary) bővítése és későbbi dekonstrukciója.

### A. A Core Input Életciklus (Core Input Lifecycle)

Minden új, külső forrásból származó komplex szekvencia az alábbi fázisokon megy keresztül:

1. **Raw Input (Akvizíció / Beviteli fázis):**
  - A tanár rögzíti a külső forrást (videó link, időzítés, stílus, eredeti előadó) és felviszi a komplex, egybefüggő leírást.
  - *Státusz:* `raw_input`
2. **Deconstructed (Dekonstrukció / Felbontás):**
  - A tanár a szerkesztőfelületen darabokra, atomi mozdulatokra (L0/L1 szilánkokra) bontja a Core Inputot.
  - *Státusz:* `deconstructed`
3. **Generalizing (Generalizáció / Integráció):**
  - A dekonstruált szilánkokat önállóan, más kontextusban, más alapfigurákhoz csatolva is tanítják a kurzusokon (pl. az eredeti core mintamondatból kiemelt egyedi vállhullám-irányváltást ráépítik egy standard `CBL`-re).
  - *Státusz:* `generalizing`
4. **Fully Integrated (Beépült tudás):**
  - A szilánkok önálló szócikként beépülnek a standard szótárba. A tanulók tudásmátrixában már nem a teljes core mondat, hanem a különálló elemek szerepelnek mint elsajátított improvizációs készségek.
  - *Státusz:* `fully_integrated`

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

## 10. Didaktikai Hasonlatok, Történetek és Ciklikus Lengés-Dinamika (Didactics & Cyclical Swing Dynamics)

A tánctanítás hatékonyságát a száraz kódok mellett a mögöttük rejlő történetek, vizuális hasonlatok és mély fizikai igazságok adják. A rendszer ezeket elsőrendű entitásként kezeli, összekötve a leíró nyelv szintaxisával.

### A. Didaktikai Hasonlatok és Történetek (Metaphors & Stories)

Minden fogalomhoz (`terms`) és mozgássorhoz hozzárendelhető egy vagy több didaktikai hasonlat (metaphor), sztori (story) és magyarázó kép, amelyek segítik a megértést:

- *Példa (Ciklikusság és az Autó-motor hasonlat):*
  - **Koncepció:** A Salsa és a Bachata ciklikus táncok. Rendelkeznek önmagukba visszatérő alaplépéssel (self-returning basic step).
  - **Hasonlat:** Az alaplépés olyan, mintha egy autó motorját indítanánk be, ahol a Follower (nő) az autó, a Leader (férfi) pedig a vezető. A motor beindul, de az autó egy helyben jár (ciklikus üzemmód), amíg el nem kezdjük irányítani.

### B. Inga- és Lengőmozgások Dinamikája (Pendulum Swing Dynamics - `swg.dir`)

A Salsa és Bachata alaplépései **inga- vagy lengőmozgások** (swinging/pendulum-type movements). Az inga egy adott zenei ütemre/lépésre vált irányt.
A leíró nyelvben ezt a `**swg.dir`** (swinging direction) tulajdonsággal és a `**chg`** (change) időzítési markerrel fejezzük ki:

1. **Salsa On1:** Az első lépésre/ütemre vált irányt az inga:
  - `**swg.dir.chg:1`** (Swing direction changes on beat 1).
2. **Salsa On2:** A második lépésre/ütemre vált irányt az inga:
  - `**swg.dir.chg:2`** (Swing direction changes on beat 2).
3. **Bachata:** A harmadik lépésre/ütemre vált irányt az inga:
  - `**swg.dir.chg:3`** (Swing direction changes on beat 3).

### C. Dinamikus Lengés-Átmenetek (Swing Transitions - `swgDir2` / `swg.trans`)

A felhasználó által felvetett `swgDir2` problémája arra az átmenetre utal, amikor az éppen táncolt alaplépés lengésiránya (és annak fázisa) átvált a következő lépés vagy figura lengésirányára.
Ezt a rendszerszinten `**swg.trans`** (swing transition) metaadattal modellezzük, amely leírja:

1. Az aktuális alaplépés lengési állapotát (Current Swing Phase).
2. A soron következő mozdulat elvárt induló lengési állapotát (Target Swing Phase).
3. A kettő közötti kinetikus fázisillesztést (Phase Alignment / Transition Mode), biztosítva a folyamatos, zökkenőmentes és természetes mozgásáramlást (flow).

---

## 11. Fogalom-Evolúció: Kombinált- és Alapértelmezett Szavak (Concept Evolution & Defaults)

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
  - *Szintaxis:* `**2hxR`** (A nagy `R` jelzi a domináns, felül lévő kezet. Ergonomikus, mert a nagybetű azonnal felhívja magára a figyelmet a madártávlati olvasáskor).
2. **2hxL (Left over Right):** Kétkezes keresztfogás, ahol a **bal kéz van felül**.
  - *Szintaxis:* `**2hxL`** (A nagy `L` jelzi a felül lévő kezet).
3. **Miért hibás a csupa kisbetűs forma (`2hxl` / `2hxr`)?**
  - A kis `l` betű könnyen összetéveszthető az `1`-es számmal (pl. `2hx1`), ami ritmikai vagy darabszámbeli zavart okozhat a kódértelmezőben és az emberi szemnek is. A nagy `L` és `R` használata ezt a vizuális ütközést 100%-ban kiküszöböli.

### C. Alapértelmezett Esetek Hanyag/Rövidített Evolúciója (Defaulting & Contextual Shorthands)

A kutatás kezdetén a tanárok mindent a legmagasabb részletességgel (max detail zoom) írnak le. Később a leggyakoribb variációk "alapesetté" válnak, és a nyelvben ráragad a "sima" jelző, elhagyva a felesleges specifikációkat.

1. **A Hanyag Evolúció (Shorthand Evolution) működése:**
  - *Kezdeti fázis (Full Specification):* `rhxUpLaCar` (Right hand cross, upper lady's caress).
  - *Evolúciós fázis (Shorthand):* Ha nincs más caress megadva, az `rhxCar` automatikusan a leggyakoribb `rhxUpLaCar`-t jelenti.
2. **Szoftveres támogatás (Context-Aware Defaults):**
  - A szótárban minden fogalomhoz hozzárendelhető egy `**is_default_representant`** (alapeset reprezentáns) logikai mező.
  - Ha a tanár az óravázlatban a rövidebb `rhxCar` kódot használja, a parser a háttérben automatikusan feloldja azt a teljes `rhxUpLaCar` jelentésre, de a képernyőn meghagyja a tanár által preferált rövid, kényelmes formát.
  - Ha később egy új, alternatív caress jelenik meg (pl. `rhxLoLaCar` - lower caress), azt kötelezően ki kell írni, de a régi alapeset továbbra is megmaradhat a hanyag `rhxCar` alakban.

### D. Komponens-Öröklődés és Szemantikus Refaktorizációs Pipeline (Dependency DNA & Refactoring)

A legnagyobb kockázat a lexikalizált szavaknál (pl. `rhx`), hogy ha a háttérben megváltoztatunk egy alapelemet (pl. a `h` = hand helyett bevezetjük a `ha` kódot, hogy a `h` felszabaduljon a `hip` részére), a sima szöveges keresés és csere (`Search & Replace`) működésképtelen vagy veszélyes lesz, mert nem látja a kapcsolatot az `rhx` és a `h` között.

Ennek feloldására egy szigorú **szemantikus függőségi gráfot** és egy **refaktorizációs fordítómotort (Compiler)** alkalmazunk:

1. **A Szemantikai "DNA" (Összetételi Képlet) tárolása:**
  Minden olyan kifejezés, amely más alapelemekből áll össze (legyen az CamelCase vagy már teljesen lexikalizált, egybeírt `rhx`), az adatbázisban **soha nem veszíti el a szülő-gyermek kapcsolatát**. 
   A szótárban minden összetett kifejezés mögött kötelezően tároljuk annak **strukturális képletét (DNA Recipe)** egy relációs táblán keresztül (`term_components`):
   Az `rhx` nem egy statikus karakterlánc, hanem a háttérben az `**r`**, `**h`** és `**x**` fizikai fogalmak UUID-fókusza.
2. **Absztrakt Szintaxisfa (AST) Alapú Értelmezés:**
  A leíró nyelven írt óravázlatokat a rendszer nem sima szövegként kezeli, hanem egy **AST Parser** segítségével lefordítja egy objektum-orientált fává, ahol minden egyes elem és karakter a mögöttes adatbázis-beli UUID-jára mutat.
3. **Az Automatikus Refaktorizációs Pipeline (The Migrator):**
  259|   Amikor a fejlesztő vagy a tanár megváltoztat egy alapelemet (pl. `h` $\rightarrow$ `ha` szabály):
   260|   - **Step 1 (Impact Analysis):** A rendszer a `term_components` tábla segítségével azonnal kilistázza az összes érintett összetett és lexikalizált szót (pl. kimutatja, hogy a `h` módosítása érinti a `rhx`, `lhx`, `2hx` kifejezéseket).
   261|   - **Step 2 (Auto-Regeneration):** A refaktorizációs motor a képletek alapján automatikusan újragenerálja a lexikalizált szavak új karaktersorozatát: `r` + `ha` + `x` $\rightarrow$ `**rhax`**.
   262|   - **Step 3 (Semantic Migration):** A rendszer végigfut az összes adatbázis-rekordon (óravázlatok, videó-időbélyegek leírásai). Minden szöveget lefordít AST fává, elvégzi az UUID-alapú token-cseréket (pl. `rhx` helyett `rhax`), majd visszamenti a frissített szöveget.
   263|   - **Step 4 (Safety Validation):** A linter ellenőrzi az új nevek biztonságát (lásd 11.E szabály-hierarchiát az ütközések és stílusváltások kezelésére).

Ez a háttér-architektúra garantálja a **100%-os skálázhatóságot és biztonságot**: a nyelvtan és a kódok szabadon és hanyagul fejlődhetnek a felszínen, miközben a motor mélyén az adatintegritás és a visszakövethetőség matematikai precizitással megmarad.

### E. Komponens-Módosulási Konfliktusok és Szabály-Hierarchia (Conflict Resolution & Rules Priority)

Ha egy alapelem módosítása (pl. `h` $\rightarrow$ `ha`) megváltoztatja egy lexikalizált vagy összetett szó hosszát, ez közvetlenül ütközhet a tömörségi, helyesírási és szeparációs szabályokkal. Ennek feloldására egy szigorú, prioritás-alapú döntési hierarchiát és egy "Human-in-the-loop" (HITL) kapuőrzési folyamatot alkalmazunk.

#### 1. Rendszerszintű Szabály-Hierarchia (Rules Priority)

Amikor a refaktorizációs compiler újragenerál egy kódot, az alábbi prioritási sorrend szerint dönti el a végső helyesírási és összevonási állapotot (fentről lefelé haladva):


| Prioritás | Szabály Neve                                | Leírás / Megkötés                                                                                                                                                                                                | Konfliktus Kezelés                                                                                                                                                                                                                     |
| --------- | ------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **1.**    | **Ütközésmentesség (No Collision)**         | Az új kód semmilyen körülmények között nem ütközhet éles bázisszóval.                                                                                                                                            | **Átléphetetlen korlát.** Ütközés esetén azonnali HITL leállítás történik.                                                                                                                                                             |
| **2.**    | **Szemantikai DNA konzisztencia**           | A kódnak felépíthetőnek kell lennie a szülő UUID-k sorrendjéből.                                                                                                                                                 | Ha a képlet sérül, a kód érvénytelen.                                                                                                                                                                                                  |
| **3.**    | **Vizuális Érthetőség (Lexical Integrity)** | A kód hosszától függően a compiler újraértékeli a szeparációt (pl. ha a `h` $\rightarrow$ `ha` miatt `rhx`-ből `rhax` lesz, ez még egybefüggően olvasható, de ha `lhdshldx` lenne belőle, az már olvashatatlan). | **Szeparációs Állapotgép (Transition):** - Ha a kód $\le$ 4 karakter: csupa kisbetűs összeolvadás engedélyezett (pl. `rhax`). - Ha a kód > 4 karakter: Kötelező visszalépés CamelCase-re (pl. `rHaX`) vagy Dot Case-re (pl. `r.ha.x`). |
| **4.**    | **Pronounceability (Kimondhatóság)**        | A kódnak verbalizálhatónak kell lennie (ASS Level 2 elv alapján).                                                                                                                                                | Ha nehezen kiejthető (pl. `rhtx`), a linter figyelmeztetést (Warning) küld a tanárnak jóváhagyásra.                                                                                                                                    |
| **5.**    | **Minimális Hossz (ASS Max Compression)**   | Törekedni kell a lehető legrövidebb reprezentációra.                                                                                                                                                             | Alárendelt az 1-4. pontoknak.                                                                                                                                                                                                          |


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

- **A kezdeti koncepció (Beginner Level):** A zenei kérdésre (1-4. ütem) a táncos az alap 3 lépés helyett 5 lépést tesz. Elnevezzük `**5s`**-nek (5 steps), ami kisebb, sűrűbb lépéseket és gyorsabb érzetet ad.
- **A mélyebb didaktikai igazság (Advanced Level):** Magasabb szinten a táncos takarékoskodik a súlyvonal-váltásokkal (flow optimalizáció). Nem tesz 5 teljes lépést, hanem 5 zenei történést / hangsúlyt táncol le (pl. testizoláció, vállpattintás, tap súlyáthelyezés nélkül).
- **A szemantikai elcsúszás (Semantic Drift):** Kiderül, hogy az általános, felsőbb kategória a `**5a`** (5 accents - 5 hangsúly). Az `**5s`** (5 steps) valójában a `**5a`** egy specifikus, kezdő szintű megvalósulása (ahol minden hangsúlyhoz teljes lépés/súlyáthelyezés társul).
- **A logikai reláció:** `5s` is-a `5a` (where `weight_transfer = true`).

#### 2. Az Ontológiai Elcsúszás Kezelésének Folyamata (OntoShift Pipeline)

Amikor egy korábbi bázisfogalomról kiderül, hogy az egy új, absztraktabb fogalom gyermeke (hyponymja), a rendszer az alábbiak szerint hajtja végre a struktúra átrendezését:

1. **A Kapcsolat Átírása (Taxonomical Re-mapping):**
  - Nem töröljük az `5s` kódot, mert az a kezdő órák videóiban és leírásaiban fizikailag pontos (ott tényleg léptek).
  - Az adatbázisban az `5s` rekord `parent_term_id` mezőjét beállítjuk az újonnan létrehozott `**5a`** UUID-jára.
  - Az `5s` tulajdonságaként rögzítjük a specifikus megszorítást: `{ weight_transfer: true }`.
2. **AST-Alapú Szemantikai Fordítás (Semantic Expansion):**
  - A parser felkészül arra, hogy a jövőben az `5s` kódot a háttérben `**5a.step`** formában is képes legyen értelmezni, biztosítva a szigorú logikai ekvivalenciát.
3. **Irányított Curriculum Migráció (Guided Migration Toolkit):**
  - A rendszer átvizsgálja az összes történelmi óravázlatot, ahol a régi `5s` szerepelt.
  - Nem hajt végre vak szöveges cserét, hanem egy **HITL (Human-in-the-Loop) Refaktorizációs Panel** elé tárja a döntést a tanárnak:

> 🔄 **Ontológiai Refaktorizációs Asszisztens:**
>
> *"Észleltük, hogy az `**5s`** (5 steps) fogalmat átminősítette az `**5a`** (5 accents) gyermekévé.*
> *Az adatbázisban 42 történelmi óravázlat használja az `5s` kódot. Hogyan szeretné ezeket frissíteni?*
>
> 1. **Megtartás (Keep Specific):** *"Hagyja változatlanul `5s`-ként az érintett órákon, mert a didaktikai szint alapján (Salsa Cat 01) ott valóban fizikai lépéseket végeztek a tanulók."*
> 2. **Általánosítás (Generalize to `5a`):** *"Cserélje le `5a`-ra az órákon, mert a kontextus alapján a zenei hangsúlyok elhelyezése volt a cél, és a lépés másodlagos."*
> 3. **Dekonstruált Csere (Deconstruct to `5a.step`):** *"Cserélje le az explicit `5a.step` Dot Case formára a maximális elméleti precizitás érdekében."*
>
> *[Gomb: Végrehajtás kiválasztott órákon]*

---

## 12. Szemantikai Ontológia, Didaktikai Approximáció és Fokozatos Pontosítás (Didactic Scaffolding & Epistemological Refinement)

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
4. Megtalálja az egyetlen tökéletesen illeszkedő komponenst: `**CBL.pathOpen`** (amely szintén `{ role: 'Leader', timing: '1-4', body_part: 'lower_body' }` tulajdonságú).
5. **Automatikusan lehorgonyozza (áttranszponálja)** a jelzőt a pontosított alkatrészre: `CBL.pathOpen[modified_by: flare]`.

#### 2. Hogyan oldja fel ezt a UAA 2.0 Szótár-Asszisztens?

Amikor a tanár beírja a felületre a hanyag/pontatlan megfogalmazást: *"cbl, de a férfi az 1-4-re kicsúsztatja a lábát"*:

1. A rendszer felismeri a `CBL` szülőfogalmat és a megadott kinetikai változtatást.
2. A fenti lehorgonyzási logika alapján azonosítja, hogy ez a `CBL.pathOpen` fázist érinti.
3. Az **UAA 2.0 algoritmus** segítségével automatikusan felajánlja a legördülő menüben a két lehetséges szabványos és tömör elnevezést a general-specific skálán:
  - `**maFlareCbl`** (Man's Flare Cross Body Lead): ha a mozdulat stílusjegyként a díszítésre (flare) fókuszál.
  - `**maRchOutCbl`** (Man's Reach Out One Step Cross Body Lead): ha a mozdulat funkcionálisan a lépés messzire nyújtására (reach out) fókuszál.
4. A tanár egyetlen kattintással jóváhagyja az elnevezést, és a rendszer a háttérben az új kifejezést **szülő-gyermek kapcsolatban** rögzíti a `CBL`-lel, megőrizve a pontos belső dekonstruált képletét is.

### D. Rendhagyó Lexikális Idiómák (Irregular Idioms & Lexical Overrides)

Minden természetes nyelv tartalmaz rendhagyó szerkezeteket és kivételeket. Ennek oka a nyelvhasználat ökonómiája (Zipf törvénye): a leggyakrabban használt kifejezések kiejtése és írása a történelem során rendkívül lerövidül és torzul a kényelem kedvéért.

A DANCE leíró nyelvben is előfordulnak olyan **hagyatéki kódok**, amelyek nem követik az UAA 2.0 logikus levezetési szabályait, de a kényelem és a megszokás miatt megkerülhetetlenek:

- *Példa:* face = `**fac`** (logikus rövidítés) és back = `**bck`** (logikus rövidítés, testrész).
- *A rendhagyó kifejezés:* Az egymás mögött, azonos irányba néző táncosok pozíciója (a shadow pozíció általános formája) a **"face-to-back"**.
  - A logikus kód a szabályok szerint: `fac2bck` lenne.
  - A kényelmi, hagyatéki kód viszont: `**f2b`**.

Hogy ezek a rendhagyó esetek ne "rúgják szét" a rendszerszintű konzisztenciát, a Dependency DNA-t és az automatizált refaktorizációs pipeline-t, az alábbi **Rendhagyó Idióma (Irregular Idiom)** protokollt vezetjük be:

#### 1. A Rendhagyó Leképezés (Irregular Mapping)

A szótárban az `f2b` kódot különleges, `**is_irregular = true`** jelzővel látjuk el. Ez a jelző azt jelenti, hogy a kód karaktersora manuális felülírás (override), és nem a standard UAA 2.0 generálja.

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

## 13. A "Fuzzy-to-Precise" Életút és a Dinamikus Névfeloldás (Fuzzy-to-Precise & Dynamic Resolution)

A rendszer kialakításakor három párhuzamos célt valósítunk meg a tánctanárok napi munkafolyamatainak (workflow) támogatására, különösen az új mozdulatok rögzítése és a különböző iskolák eltérő terminológiájának kezelése terén:

1. **Teljes megfogalmazási és pontatlansági szabadság** a tánctanároknak a jegyzeteléskor (hirtelen ötletek rögzítése).
2. **Fokozatos, rendszer által támogatott tisztázás** és atomokra bontás (AI és UI asszisztencia az idő múlásával).
3. **Dinamikus névváltozatok és hivatkozhatóság:** Az atomizált (és ezáltal a hétköznapokban túl bonyolulttá váló) objektumok tetszőleges részletességi szinten történő elnevezése és hivatkozása (a full-detailtől az egyezményes neveken át a teljesen egyedi, tanári azonosítókig).

### A. 1. Cél: Teljes Szabadság a Jegyzetelésben (The Fuzzy Inbox)

Amikor a tánctanárnak hirtelen eszébe jut egy ötlet, vagy lát egy videót, gyakran *"hirtelen leírja, de még azt sem tudja, hány ütemből áll"*. Ha a rendszer azonnal megkövetelné a precíz UAA 2.0 kódolást, az megbénítaná a kreativitást.

- **A Szabad Szavas Objektum (Fuzzy Draft):** A tanár bármit beírhat (pl. *"furcsa csavaró lábcsúsztatás, mintha süllyedne"*), vagy rögzíthet egy hangjegyzetet, esetleg csak egy videó-időbélyeget.
- **Rendszer-reakció:** A rendszer ezt nem dobja vissza hibaként. Létrehoz egy `fuzzy_draft` státuszú objektumot a szótárban, amely kap egy ideiglenes azonosítót (pl. `tmp.furcsaCsuszas`).
- **Azonnali Használat:** A tanár ezt az ideiglenes azonosítót azonnal használhatja az esti óravázlatában (pl. `CBL & tmp.furcsaCsuszas`), anélkül, hogy a mögöttes fizika tisztázva lenne.

### B. 2. Cél: Fokozatos Tisztázás és Atomizáció (The Refinement Engine)

Idővel a rendszer proaktívan segít a tanárnak a korábbi "homályos" objektumainak tisztázásában és atomokra bontásában.

- **AI-Asszisztált Dekonstrukciós Tükör:** Amikor a tanárnak van ideje (pl. a "Research Dashboard" felületen), a rendszer (Gemini 2.5 Pro Multimodal segítségével, ha van videó) kielemzi a szabad szavas leírást, és javaslatot tesz az atomi összetevőkre.
  - *"A 'furcsa csavaró lábcsúsztatás' valószínűleg a következő atomokból áll: `pvt.R.180` + `sld.L.back` + `lvl.down`."*
- **Szemantikai DNA Rögzítése:** A tanár jóváhagyja vagy módosítja a javaslatot. Ekkor a `fuzzy_draft` átalakul `deconstructed` (atomizált) állapottá. A rendszer a háttérben véglegesen rögzíti az objektum pontos fizikai képletét (Szemantikai DNA).
- **Visszamenőleges Tudás-Szétosztás (Retroactive Skill Distribution):** A dekonstrukció pillanatában a rendszer **automatikusan jóváírja** az újonnan felfedezett atomi komponenseket minden olyan diák tudástérképén, aki a homályos szülő-fogalmat korábban tanulta — így a tudás nem ragad a "halott aliasban". A teljes pipeline-t lásd a **18.F fejezetben**.

### C. 3. Cél: Dinamikus Névváltozatok és Részletességi Skála (Dynamic Aliasing & Resolution)

Amikor egy mozdulatot atomokra bontunk, a fizikai képlete (pl. `pvt.R.180 + sld.L.back + lvl.down`) a mindennapi óravázlatokban olvashatatlanná és kezelhetetlenné válik. A többféle iskola ráadásul másképp nevezi ugyanazt a rokonértelmű kifejezést. 
Ennek feloldására a rendszer egy **Dinamikus Névfeloldó (Dynamic Resolution)** réteget alkalmaz. 

Ugyanaz a mögöttes Szemantikai DNA (UUID-láncolat) egyszerre **több hivatkozható névváltozattal (Alias)** rendelkezhet a részletességi skálán, és ezek mindegyike érvényes hivatkozás:

1. **Level 0 (Full-Detail / Atomi szint):** A nyers fizikai képlet (`pvt.R.180 + sld.L.back + lvl.down`). Ritkán írjuk le kézzel, de a gép ezen a szinten értelmez mindent.
2. **Level 1 (Köztes Csoportfogalmak / Intermediate):** Részlegesen összevont kódok, amelyek még tartalmaznak atomi részleteket (pl. `pvtScrew + lvl.down`).
3. **Level 2 (Egyezményes Figuranevek / Conventional):** A nemzetközi vagy iskolai standard rövidítés (pl. `sldScrew`).
4. **Level 3 (Önkényes Tanári Név / Custom Alias):** A tanár saját, teljesen egyedi elnevezése (pl. `LevKedvencHelycseréi001` vagy `PetiFuraForgasa`).

### D. Fogalomtár Evolúció és Verziózott Nézetek (Dictionary Evolution & Versioned Views)

A rendszer fejlődése és a tanárok kollektív tapasztalata során a fogalmak jelentése, a hierarchia vagy akár a rövidítések elvei folyamatosan finomodnak (evolúció). Hogy egy régi óravázlat vagy koreográfia-kód ne "törjön el" egy utólagos változtatás miatt, a rendszer bevezeti a **Verziózott Szótár (Versioned Dictionary)** koncepcióját.

- **Új Verziók (SemVer):** Minden alkalommal, amikor egy fogalomkör strukturálisan megváltozik, vagy egy rövidítési szabály (pl. a CamelCase logika) módosul, a szótár egy új verziószámot kap a háttérben (pl. v2.8.0 $\rightarrow$ v3.0.0). A régi mozdulatok és kódok metaadatai befagynak a saját verziójukban.
- **Választható Nézetek (Versioned Views):** A tanárok a felületen egy "View" legördülő menüben választhatnak, hogy egy 2024-es koreográfiát az **"Eredeti (Legacy) Nézetben"** akarnak-e olvasni (ahol minden az akkori, régi kódokkal jelenik meg), vagy a **"Legújabb (Latest) Nézetben"**, ahol a fordítómotor (compiler) a régi UUID-kat dinamikusan a legújabb, legfrissebb rövidítési szabályok (pl. Anti-Kapszula CamelCase) szerint rendereli ki a képernyőre.

**Hogyan kezeli ezt a rendszer technológiailag?**

- **Alias Rendszer:** Az adatbázisban a tanár bármilyen egyedi nevet (aliast) hozzárendelhet egy atomizált objektumhoz.
- **Szabad Hivatkozhatóság:** A tanár az óravázlatba beírhatja, hogy `LevKedvencHelycseréi001`. A parser (AST) felismeri az aliast, és a háttérben azonnal feloldja a teljes atomi képletre. A tanárnak sosem kell a bonyolult kódot gépelnie.
- **Nézet-Váltás (Detail Slider):** A UI felületen a tanár egy csúszkával állíthatja, hogy az óravázlatot milyen részletességgel akarja látni. Ugyanaz a sor megjelenhet úgy, mint `LevKedvencHelycseréi001`, de ha a csúszkát elhúzza "Atomi" irányba, a szöveg szétnyílik a `pvt.R.180 + sld.L.back + lvl.down` képletre, vagy bármilyen köztes állapotra.
- **Iskolák Közötti Fordító (Rosetta Stone):** Mivel minden egyedi név (Alias) ugyanarra az atomi DNA-ra mutat, ha egy másik iskola tanára olvassa az óravázlatot, a rendszer automatikusan az ő iskolájában használt egyezményes névre (vagy az atomi képletre) tudja fordítani a `LevKedvencHelycseréi001` kifejezést. Ez tökéletesen megoldja a különböző iskolák eltérő terminológiájából fakadó káoszt.

### E. Lokális és Központi Fogalomtár (Local vs. Central Dictionary Governance)

A rendszer architektúrája szigorúan elválasztja a tanárok saját, egyedi fogalom-objektumait a központi, közös fogalomtárba (Central Dictionary) bekerülő "certified" (hitelesített) fogalmaktól. Ez biztosítja a szabadság és a rendszerszintű tisztaság egyensúlyát.

- **Lokális Fogalomtár (Local Scope):** A tanár bármilyen új, egyedi fogalmat vagy aliast bevezethet az óravázlataiban (pl. `PetiFuraForgasa`). Ezek az objektumok azonnal használhatók, de kezdetben kizárólag az ő saját rendszerén (Local Scope) belül élnek és hivatkozhatók. Nem szemetelik tele a globális névteret.
- **Commit a Központi Fogalomtárba (Propose to Central):** Ha a tanár úgy érzi, hogy az általa létrehozott vagy atomizált fogalom univerzálisan hasznos (pl. egy új alaplépés, vagy egy nagyon gyakori anatómiai variáció), beküldheti (commit-olhatja) azt egy rendszerszintű felülvizsgálatra (Review).

**A Központi Minősítő Rendszer (Review & Rating System):**
A beküldött új fogalmakat vagy a meglévő rekordok szerkesztési igényeit a kutatói szintű felhasználók (T4) vagy a rendszer adminisztrátorai minősítik. A beküldött (proposed) elemek a következő állapotokat vehetik fel:

1. **Elfogadott (Accepted):** A fogalom (és annak UAA kódja) tökéletesen illeszkedik a rendszer logikájába, ütközésmentes, és bekerül a "Certified" központi szótárba. Innentől minden felhasználó számára elérhető az auto-complete-ben.
2. **Visszautasított (Rejected):** A javaslat logikailag hibás, redundáns (már létezik más néven), vagy sérti a szeparációs és kódolási szabályokat. *(A fogalom a tanár lokális terében továbbra is megmarad, de globálisan nem jelenik meg).*
3. **Módosítással elfogadott (Accepted with modifications):** Az ötlet jó és szükséges, de a javasolt rövidítés vagy hierarchia ütközik a szabályrendszerrel (pl. életbe lép a Tömörítési Skála Elmozdulás). A központ módosítja a beküldött adatokat (pl. `leA` helyett `lAr`), de a tanár az eredeti elnevezését megtarthatja saját lokális Aliasként.
4. **Kipróbálási célból elfogadott (Accepted for trial / Experimental):** A fogalom új, határeset, vagy egy kialakulóban lévő táncstílus sajátja, aminek a rendszerszintű létjogosultsága még kérdéses. Bekerül a központi térbe, de kap egy "Experimental" taget. Később a statisztikai használat (Frequency) dönti el a végleges sorsát és rövidítési szintjét.

---

## 14. Vizuális Annotáció és "Ideális Kinetika" Korrekciós Rendszer (Visual Annotation, Overlay Drawing & Kinematic Correction)

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

- **Példa:** A tanár lát egy oldallépést a videón. Úgy érzi, a táncos túl laposan lép.
- **A szoftveres akció:** Kiválasztja a `Line/Curve Tool`-t, és berajzol egy felfelé ívelő parabolát (Upper Curve - `upCu`) a táncos medencéjének magasságában.
- **Szemantikai Csatolás:** A berajzolt ívhez hozzárendeli a `lvl.upCu` (súlyvonal felső ívű mozgása) és az `rcp.limb` (végtag-végtag közötti mozgáspár / kinetic pairing a ronde-dal) kódot a szótárból.
- **Az eredmény:** A szoftver nemcsak a rajzot tárolja, de a videószegmenshez most már hivatalosan kapcsolódik az **"így kellene kivitelezni"** elméleti modell kódja is.

#### 2. Segédtanárok és Iskolák Betanítása (Choreography Mapping & Standardization)

- Ha a fő tanár (fejlesztő) készít egy rendkívül komplex, versenyzői szintű koreográfiát:
  1. A zenei ütemeket és a dekonstruált kódokat pontosan összehangolja a videó idősávjával.
  2. A videóra rárajzolja a kritikus erővonalakat, a feszítések irányait, és a karok ideális térbeli íveit.
  3. A segédtanárok a **"Standardized Teaching View"** segítségével pontosan látják, hogy mit és miért kell megkövetelni a diákoktól. Nincs többé *"ki hogyan emlékszik a mozdulatra"* bizonytalanság: a vizuális réteg és a kódos leírás együttesen adja ki az abszolút igazságot.

#### 3. HITL Versenyzői Hibajavítás és Táv-Coaching (Asynchronous Feedback & Correction)

- **A munkafolyamat:**
  1. A versenyző feltölti az edzés-videóját a saját profiljáról a rendszerbe, és visszajelzést kér (pl. *"Kérem a bachata solo 2. részének javítását"*).
  2. A tanár megnyitja a videót a **Coaching Canvas** felületen.
  3. A tanár a videót kockáról kockára pörgetve (scrubbing) közvetlenül a képernyőre firkál:
    - Piros színnel berajzolja a versenyző rossz, beeső térdtengelyét.
    - Zöld nyíllal berajzolja a kívánt, kifelé mutató ideális térdirányt.
    - Ráilleszt egy szövegbuborékot a megfelelő zenei ütemre: *"Itt süllyessz és feszítsd a bokát!"*
  4. A rendszer elmenti a visszajelzést. A versenyző a saját felületén megnyitja a videót, és a lejátszás során az adott másodpercekben a képernyőn dinamikusan megjelennek a tanár színes "firkái" és javító kommentjei.

#### 4. Pose-Estimation vs. Drawn Ideal (Automata Biomechanikai Kontraszt)

- A legmagasabb szintű technológiai integráció során a rendszer a MediaPipe vagy ViTPose segítségével **automatikusan kirajzolja a táncos csontvázát (actual pose skeleton)** kék színnel.
- A tanár zöld színnel berajzolja az **ideális csontvázat (ideal skeleton)** vagy a kívánt végpont-pályagörbéket.
- A rendszer kiszámítja a két görbe közötti eltérést (Delta), és számszerűsíthető biomechanikai visszajelzést ad: *"A ronde fázisban a bal lábnyújtás 12 fokkal elmarad az elméleti ideáltól, a medence süllyedése pedig 8 cm-rel mélyebb a kelleténél."*

---

## 15. Kreatív Koreográfia-Tervező és 3D Show Készítő Rendszer (Creative Choreography Planner, Timeline Mashup & 3D Avatar Engine)

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

- **Akció:** A koreográfus létrehozza a projektet (Pl. *"Salsa Duo Show 2026"*). Feltölti a megvágott hangsávot.
- **Szabály-ellenőrzés:** Megadja a versenykategóriát (Pl. *"Max 2 perc 15 mp"* és *"Megengedett stílusok: Salsa, Rumba, Afro"*). A rendszer a háttérben folyamatosan ellenőrzi a hosszt és a felhasznált kódok kategóriáit (UAA 2.0 szűrőkkel), és figyelmeztet, ha nem megengedett mozdulatot vagy túl hosszú zenét használunk.

#### 2. Fázis: Videó Mashup és Idővonali Beillesztés (Visual Splicing)

- **Szeletelés (Slicing):** A rendszer automatikusan felosztja a hangsávot a BPM alapján 8-as ütemekre (measures) és ütésekre (beats). A tanár vizuálisan látja az ütemrácsot.
- **Mashup:** A tanár végigpörgeti a stúdió saját videótárát (vagy a "Fuzzy-to-Precise" pipeline-on át rögzített új referenciákat).
- **Drag & Drop:** A legszebb referenciákat egyszerűen **ráhúzza** az idővonal kiválasztott ütemeire (Pl. *"A 12. ütem 1-4. ütésére tegyük be a 'Viral Rumba Challenge' 01:14-es vállhullám szilánkját"*). A rendszer automatikusan rögzíti a `video_reference_segment_id`-t.

#### 3. Fázis: Kinetikai és Forgatókönyv Térkép (Semantic Overlay)

- A koreográfus elhelyezi a "zenei csomópontokat" (Hits / Accents). Ráírja a hangsúlyok nevét (Pl. *"Trombita leütés"*, *"Harang ütés"*).
- A legfontosabb hangsúlyokra azonnal ráilleszti a legkifejezőbb kódolt mozdulatokat (Pl. `lvl.upCu` vagy `cod.rebd`).
- A köztes időszakokra vagy szabad szöveges jegyzetet ír, vagy rávonzza a standard átvezető alaplépéseket, fokozatosan feltöltve a teljes idősávot.

#### 4. Fázis: Többtáncos Formációk Tervezése (Spatial Formations)

- A felületen megjelenik egy 2D-s rács (a színpad felülnézete).
- A koreográfus az idővonal egy adott ütemén (Pl. a 16. ütem 5. ütésén) az egérrel elhelyezi a táncosok ikonjait (Leader 1, Follower 1). Berajzolja az elmozdulásuk irányát és a nézési szögüket.
- A rendszer az ütemek lejátszása során **interpolálja a pozíciókat**, így a tanár folyamatos animációként látja, hogyan fognak a táncosok áthelyezkedni a színpadon.

---

### C. 3D Szimulációs és Avatár Motor (3D Avatar Simulation Engine)

Ez a funkció teszi a rendszert valódi, jövőbe mutató **kutatóállomássá**. Az **AST (Abstract Syntax Tree) Parser** és a **Szemantikai DNA** lehetővé teszi, hogy a kódolt mozdulatokat ne csak szövegként értelmezzük, hanem vizuális mozgássá alakítsuk.

```
[Notation Code: lvl.upCu & pvtScrew] ──> [AST Parser] ──> [Kinematic bone rotation data] ──> [Three.js 3D Avatar Rendering]
```

#### 1. Hogyan fordítódik a kód 3D mozgássá? (Code-to-Motion compiler)

1. **Szemantikai DNA felbontása:** A rendszer beolvassa az idővonalra írt kódokat (Pl. `pvtScrew`). A `term_components` táblából lekéri az atomi L0-s fizikai összetevőket: `pvt.R.180` (jobb láb pivot) + `sld.L.back` (bal láb csúszás hátra) + `lvl.down` (súlypont süllyesztés).
2. **Anatómiai leképezés (Joint / Bone Mapping):** A rendszer minden atomi kódhoz hozzárendel egy parametrizált 3D csontváz-rotációs és elmozdulási mátrixot:
  - `lvl.down:15` $\rightarrow$ A csípőízület (root joint) Y-tengelyű elmozdulása lefelé: `-15cm`.
  - `pvt.R.180` $\rightarrow$ A jobb boka és talp ízület Y-tengelyű rotációja: `+180°`.
  - `sld.L.back` $\rightarrow$ A bal csípő- és térdízület nyújtása, a bal boka elmozdulása hátrafelé a padlósíkon (Z-tengely).
3. **Időbeli szinkronizáció (Time Warping):** A rendszer leolvassa a kód zenei ütemét (Pl. 1-4 ütés). A 3D-s animáció sebességét automatikusan a projekt BPM-jéhez igazítja, elosztva a rotációkat a megadott ütések időtartama alatt.

#### 2. Vizuális 3D Megjelenítés (Three.js / WebGL Web-Viewer)

- A szoftver tartalmaz egy könnyű, böngészőben futó **3D WebGL szimulációs panelt**.
- A koreográfus a lejátszás gombra kattintva látja, ahogy a virtuális humanoid avatár(ok) elvégzik az idővonalra írt mozgássorozatot a megadott térformációknak megfelelően.
- **AI Mozgás-Generáció (AI Motion Synthesis):** Ha olyan összetett vagy ideiglenes kód szerepel (`sandbox_draft`), aminek nincs még precíz L0-s 3D-s leképezése, a háttérben futó **Motion Synthesis AI** (pl. egy finomhangolt Motion Diffusion Model) a szabad szavas leírás alapján legenerálja az avatár ideiglenes 3D mozgásfájlját (glTF/FBX formátumban), amit a tanár kézzel tovább finomíthat.

#### 3. Miért forradalmi ez az oktatásban és a betanításban?

- **A "Koreográfia Digitális Ikerje" (Digital Twin):** A show teljes mértékben létezik a felhőben 3D szimulációként, kódolt szintaxisként és referencia-videókként.
- **Azonnali megértés:** Ha a segédtanár nem biztos egy mozdulatban, nemcsak a kódot és a videó-mashupot látja, hanem a 3D panelt elforgatva, tetszőleges kameraállásból, lassítva nézheti meg a virtuális avatár "ideális" kinetikai kivitelezését, ízületi szögeit és súlypont-ívelését.
- **Személyre szabott virtuális edzőtárs:** A versenyző otthon, a 3D-s avatárral együtt táncolva gyakorolhatja a show-t, akár VR-szemüvegen (WebXR) keresztül is, mintha az edzője folyamatosan mellette állna és mutatná az "ideális" mozgást.

---

## 16. A Tánctudományi Ökoszisztéma és a Jövőbeli Szolgáltatások (The Grand Vision, SaaS Ecosystem & Future Horizons)

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

- **Elágaztatás (Branching):** Egy magyarországi iskola létrehozza a saját `bachata-fundamental` ágát. Egy spanyolországi iskola ezt leklónozza (Fork/Branch), és hozzáadja a saját egyedi stíluselemeit (pl. `sensual-fusion-v2`).
- **Kód-Összehasonlítás (Choreographic Diff):** Az AST parser segítségével a rendszer vizuálisan és szintaktikailag ki tudja mutatni két koreográfia-ág közötti különbséget (pl. *"A spanyol változat a 12. ütemnél a standard lépés helyett `pvtScrew` rotációt és csípősüllyesztést használ"*).
- **Egyesítés (Pull Requests):** A nemzetközi tanári közösség javaslatokat küldhet egymásnak a mozdulatok finomítására. Ha a főkoreográfus jóváhagyja (Merge), az új verzió beépül a hivatalos tantervbe.

#### 2. Didaktikai Intelligencia és Automata Hiányelemző (Predictive Gap Engine)

A szoftver nemcsak rögzíti, de elemzi is a tanulók fejlődési dinamikáját a skill mátrix és a tanterv között:

- **Akadály-Detektálás:** A rendszer észleli, ha egy csoport 60%-a hetek óta elakad egy bonyolult mozdulatnál (pl. `maRchOutCbl`).
- **Ok-Okozati Elemzés:** Az AST szülő-gyermek láncolatán (Szemantikai DNA) keresztül kiszámítja az elakadás okát: *"A tanulók azért buknak el a `maRchOutCbl` formációnál, mert 70%-uk még nem sajátította el stabilan a `sld.L.back` (bal láb hátra csúsztatása) atomi komponenst."*
- **Automata Kurrikulum-Ajánló:** A rendszer automatikusan felajánl egy 3 alkalmas átvezető/előkészítő gyakorlatsort (didaktikai scaffolding), amellyel a tanár gyorsan és célzottan pótolhatja a hiányzó fizikai alapokat.

#### 3. Kinetikai Szerzői Jogvédő és Tánclicenc Jegyzék (Choreographic IP & Registry Ledger)

A művészvilág egyik legnagyobb problémája az egyedi mozdulatsorok és koreográfiák engedély nélküli másolása és a forrás megjelölésének elmaradása.

- **Choreographic Fingerprint (Szemantikai Hashing):** Minden egyedi koreográfia és karakteres mozdulat lefordítható egy egyedi, titkosított AST hash-re (kinetikai ujjlenyomat).
- **IP Registry:** A koreográfusok levédethetik show-műsorukat a rendszer globális jegyzékében.
- **Automatizált Licenckezelés:** Ha egy másik iskola a rendszeren belül beépíti a levédett show-t a saját tantervébe vagy fellépési anyagába, a rendszer automatikusan számlázza a licencdíjakat, és elosztja a jogdíjakat a készítők között (SaaS-szintű mikrofizetésekkel).

#### 4. Okos Hardver & Wearable Szenzor Integráció (Real-Time Haptic Biofeedback)

A 3D-s szimuláció kiterjesztése a fizikai világba okoseszközök és viselhető szenzorok segítségével:

- **Foot-Pressure & IMU Tracking:** Okos talpbetétek, okosórák vagy mozgásérzékelő pántok integrációja.
- **Valós idejű összehasonlítás:** A tanuló tánc közben hordja az eszközöket. A szoftver összeveti a beérkező szenzoradatokat (gyorsulás, talpnyomás, szögelfordulás) az AST-ben tárolt "Ideális Kinetika" elméleti modelljével.
- **Haptikus Visszajelzés:** Ha a tanuló súlypont-süllyesztése vagy az irányváltás időzítése eltér az elméleti ideáltól, az okosóra vagy a talpbetét enyhe rezgéssel (haptikus jelzéssel) azonnal figyelmezteti a hiba pillanatában, drasztikusan felgyorsítva az izommemória kialakulását.

---

### B. Globális Szolgáltatási Modellek (Business & Service Ecosystem)

#### 1. Choreography Asset Store — A Táncosok Eszköztára

Hasonlóan a játékfejlesztésben ismert Unity Asset Store-hoz, a rendszer egy globális piacteret (Marketplace) biztosít:

- Az elit, világhírű koreográfusok feltölthetik a **"Kinetikailag Minősített" (Kinetically Certified)** show-anyagaikat, komplett csomagban:
  - A megvágott, engedélyezett zenét és a hozzá tartozó BPM rácsot.
  - A teljes, dekonstruált kinetikai kódsort (AST szintaxist).
  - A komplett 3D-s humanoid avatár mozgásfájlokat.
  - A vizuális canvas overlay rajzokat és a hozzájuk csatolt didaktikai sztorikat, metaforákat.
- Bármely helyi stúdió/tanár megvásárolhatja ezeket a tananyagcsomagokat a SaaS rendszerben, és egyetlen kattintással beemelheti a saját oktatási portfóliójába, így biztosítva, hogy a világ legújabb trendjeit azonnal, a legmagasabb szakmai pontossággal taníthassa a saját diákjainak.

#### 2. Spatial Computing & Gamified Dance Academy (WebXR VR/AR)

A kiterjesztett valóság (Apple Vision Pro, Meta Quest) bevonása az otthoni tanulásba:

- **Virtual Dance Mirror:** A tanuló a VR/AR szemüvegben látja saját magát (a szobája kameráján keresztül) és közvetlenül maga mellett az **ideális mozgást végző 3D avatárt**.
- **Gamified Combos:** Pontokat kap a helyes ritmusért, a megfelelő ízületi szögekért és a sikeresen összekötött bonyolult kód-sorozatokért. Az elért pontokkal szinteket léphet a globális ranglistán.

#### 3. Globális Tánckutatási és Standardizációs Intézet (The Dance Standard Alliance)

A DANCE rendszer tudásbázisa idővel a világ legnagyobb, legpontosabb és legárnyaltabb táncleíró adatbázisává nőheti ki magát:

- A rendszer adatai és tudományos mélységű dekonstrukciói alapul szolgálhatnak egy nemzetközi standard felállításához, amely végre tudományos, rendszerszintű és egységes nyelvet biztosít a táncművészetnek, hasonlóan a zenében a kottához vagy a programozásban a programozási nyelvekhez.

---

## 17. Felhasználási Gradiens és Jogosultsági Rétegek (Capability Gradient & Permission Layers)

A rendszer **nem egyetlen monolitikus szoftver**, és nem is két élesen szétvágott külön termék. A funkciókat egy **képesség- és vállalás-alapú gradiens** (Capability & Commitment Gradient) szervezi, amelynek két végpontja a **Pedagógiai Pólus** és a **Tudományos Kutatói Pólus**. A felhasználók nem "dobozokba" kerülnek, hanem egy folytonos skálán helyezkednek el aszerint, hogy mennyire mélyen kívánnak a fogalmak atomizálásával foglalkozni.

### A. A Gradiens Vizualizációja

```
PEDAGÓGIAI PÓLUS  ◄─────────────────── GRADIENS ───────────────────►  KUTATÓI PÓLUS
(fekete-doboz használat)                                          (atomi dekonstrukció)

[Vendég] ─ [Tanuló] ─ [Segédtanár] ─ [Tánctanár] ─ [Fejlesztő Tanár] ─ [Kutató/Koreográfus]
   │           │            │              │                │                    │
 részvétel  saját       óravezetés,    óratervezés,     fogalmak          atomizáció,
 naplózás   skill-      jelenlét-      tudástérkép,     tisztázása,       ontológia,
            mátrix      rögzítés       csoportkövetés   fuzzy→precise     CVE, 3D, IP
```

- A skála **bal oldalán** a fogalmakat **fekete dobozként** (egyezményes figuranév / scaffold szinten) használják. Itt a teljes pedagógiai érték elérhető **anélkül**, hogy bárki egyetlen mozdulatot is atomizálna.
- A skála **jobb oldalán** a felhasználó belép a tudományos rétegbe: dekonstruál, ontológiát rendez, kombinatorikus variációkat generál, 3D-ben szimulál.
- A **fontos elv:** minden pozíció a skálán **önmagában teljes értékű**. A tanár ott állhat meg, ahol a kompetenciája és a vállalása engedi.

### B. Jogosultsági Rétegek (Permission Tiers)

A gradiens szoftveresen **kumulatív jogosultsági rétegekre** képződik le. Minden magasabb réteg tartalmazza az alatta lévők összes képességét.


| Réteg  | Szerep                                                | Fő Képességek                                                                                                                                                       | Pólus-helyzet                       |
| ------ | ----------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------- |
| **T0** | **Vendég / Tanuló (Guest / Student)**                 | Saját jelenléti napló és skill-mátrix megtekintése; a tanult anyag (egyezményes nevek szintjén) visszanézése; videós önértékelés.                                   | Pedagógiai (passzív)                |
| **T1** | **Segédtanár (Assistant)**                            | Jelenlét rögzítése; a fő tanár óratervének levezetése; a "Standardized Teaching View" használata; tanulói haladás bejelölése.                                       | Pedagógiai                          |
| **T2** | **Tánctanár (Teacher)**                               | Óratervezés; tanfolyam- és csoportkezelés; tudásszint-térképezés; a Fuzzy Inbox használata (szabad jegyzetelés); egyezményes és önkényes (alias) nevek hivatkozása. | Pedagógiai (aktív, gradiens közepe) |
| **T3** | **Fejlesztő Tanár (Developer-Teacher)**               | A saját fuzzy draftjainak atomizálása a Refinement Engine-nel; Semantic Anchoring; lokális szótárbővítés; Core Input dekonstrukció.                                 | Átmeneti zóna (pedagógia → kutatás) |
| **T4** | **Kutató / Koreográfus (Researcher / Choreographer)** | Globális szótár- és ontológia-kezelés (OntoShift); Kombinatorikus Variációs Motor (CVE); 3D szimuláció; vizuális annotáció; IP-jegyzék; Git-for-Dance merge-jogok.  | Kutatói                             |


### C. Adat-Áramlási Irány és a Pólusok Összeérése (The Convergence)

A két pólust **nem fal választja el, hanem adatáramlás köti össze** — kétirányú, egymást tápláló ciklusban:

1. **Lentről felfelé (Pedagógia → Kutatás):** A pedagógiai követés nyers adatai (mit, hányszor, milyen részletességgel tanítottak, hol akadtak el a csoportok) **táplálják a kutatást**. A gyakran ismételt fuzzy fogalmak az Induktív Fogalom-Felfedező motor (lásd 7.B) számára jelzik, hogy érdemes őket atomizálni és standardizálni.
2. **Fentről lefelé (Kutatás → Pedagógia):** Az atomizált, tudományosan pontosított fogalmak **egyre finomabb pedagógiai követést** tesznek lehetővé (lásd 17. és 20. fejezet). Egy atomizált fogalom mentén a tanuló tudása nemcsak "tudja/nem tudja" bontásban, hanem atomi készség-szinten is térképezhető, és ez nyitja meg az utat az **absztraktabb, helyzetérzékenyebb, improvizatívabb tudás** felé.

> **Tervezési alapelv:** A rendszer a háttérben mindig a kutatói pólus teljes mélységét tartja készenlétben (minden fogalom mögött ott a Szemantikai DNA potenciálja), de a felszínen csak annyit mutat, amennyit az adott réteg felhasználója kért. A pedagógus sosem botlik bele a tudományos komplexitásba, a kutató pedig sosem veszíti el a pedagógiai forrásadatokat.

---

## 18. Pedagógiai Modul: Óratervezés és Tudáskövetés (Pedagogy Module: Lesson Planning & Knowledge Tracking)

Ez a fejezet a **Pedagógiai Pólus** (T1-T2) napi munkáját szolgáló modult írja le. A modul **nem igényel atomizációt**: a "mezei" tánctanár az egyezményes figuranevek és a saját aliasai szintjén tervez és követ. Mégis, a háttérben gyűjtött adatok pontossága fokozatosan nő, ahogy a fogalmak (opcionálisan) atomizálódnak.

### A. A Tudásmérés Három Növekvő Bizonyossági Szintje (Three Confidence Tiers of Knowledge)

A rendszer **fokozatos bizonyosság-elven** méri a tudást: az olcsón, automatikusan beszerezhető (de gyenge) jeltől halad a drága, de erős jel felé. A tanárnak nem kötelező a magasabb szintekre lépnie.


| Szint  | Neve                                              | Adatforrás                                      | Erősség                | Jelentése                                                                                     |
| ------ | ------------------------------------------------- | ----------------------------------------------- | ---------------------- | --------------------------------------------------------------------------------------------- |
| **K1** | **Részvételi Bizonyíték (Exposure)**              | Beléptetőrendszer / jelenléti napló             | Gyenge (valószínűségi) | "A tanuló **jelen volt**, amikor ezt tanítottuk."                                             |
| **K2** | **Begyakorlottsági Kvantum (Repetition Quantum)** | Óravezetési naplózás (mit hányszor ismételtünk) | Közepes                | "A tanuló **ismételten találkozott** az elemmel (1. letanítás, 2. ismétlés, 3. ismétlés...)." |
| **K3** | **Minőségbiztosítás (Quality Assurance)**         | Tanári értékelés / videós ellenőrzés / szenzor  | Erős                   | "A tanuló **bizonyítottan, minőségi szinten tudja** az elemet."                               |


### B. K1 — Részvétel-alapú Tudástérkép (Exposure-Based Mapping)

A legolcsóbb és teljesen automatizálható jel. Az alapelv: **maga a részvétel ténye is tudást implikál.**

- **A logikai lánc:** Ha a beléptetőrendszer rögzíti, hogy egy vendég részt vett az **X. tanfolyam Y. óráján**, ahol **Z1** volt a *tervezett* tananyag és **Z2** az, amit *sikerült is leadni*, akkor a tanuló "tudásszintje" az adott területen (pl. `salsa.on2`) a leadott `Z2` anyaggal **valószínűségileg gazdagodik**.
- **Tervezett vs. Leadott (Planned vs. Delivered):** A rendszer szigorúan megkülönbözteti a tanár óratervét (`planned_curriculum`) attól, amit ténylegesen leadtak (`delivered_curriculum`). Az óra végén a tanár egyetlen mozdulattal jelzi, meddig jutott. Csak a `delivered` anyag számít exposure-jelnek.
- **Valószínűségi súlyozás:** A K1 jel sosem jelent "biztos tudást". A skill-mátrixban `exposure_count` és `confidence: low` címkével jelenik meg (pl. *"3 órán találkozott a `CBL`-lel — valószínűleg ismeri a belépő szintjét"*).

```sql
CREATE TABLE attendance_records (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    student_id UUID REFERENCES users(id),
    lesson_id UUID REFERENCES lessons(id),
    checked_in_at TIMESTAMPTZ,           -- Beléptetőrendszer időbélyege
    source VARCHAR(30) DEFAULT 'door'     -- 'door' | 'manual' | 'qr'
);

CREATE TABLE lessons (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    course_id UUID REFERENCES courses(id),
    sequence_number INT,                  -- Hányadik óra a tanfolyamon (Y)
    planned_curriculum JSONB,             -- Tervezett term_id-k (Z1)
    delivered_curriculum JSONB,           -- Ténylegesen leadott term_id-k (Z2)
    taught_at TIMESTAMPTZ
);
```

### C. K2 — Begyakorlottsági Kvantum (Repetition Quantum & Teaching Detail)

A tanítás közben a rendszer **kvantálja az egyes elemek (mozdulatfüzérek) előfordulásait és a tanítási részletességet.** Ez adja a K1-nél jóval pontosabb térképet.

1. **Előfordulás-kvantálás (Occurrence Quantization):** Minden alkalommal, amikor egy elem megjelenik az órán, a rendszer növeli a tanuló adott elemhez tartozó számlálóját, és **megjelöli a tanítási epizód típusát**:
  - `first_teaching` (első letanítás — lassú, részletes magyarázat),
  - `repetition_2`, `repetition_3`, ... (ismétlések),
  - `recall` (korábbi anyag felidézése),
  - `application` (más kontextusban, kombinációban való alkalmazás).
2. **Tanítási Részletesség (Teaching Detail / Zoom Level):** A rendszer rögzíti, hogy az elemet milyen mélységben tanították (pl. csak megmutatták / lebontották / technikailag finomították). Ez közvetlenül összekapcsolható a Részletességi Skálával (13.C) és a Megértési Állapotokkal (`scaffold` / `precise` / `deconstructed`).
3. **Szerep-tudatosság (Role Awareness — KÖTELEZŐ):** Minden tanítási epizódot **kötelezően a táncos szerepéhez kötünk** (`Leader` / `Follower` / `Solo`). Ugyanazt a `CBL`-t a Leader és a Follower **gyökeresen máshogy** tanulja (a vezetés és a követés nem ugyanaz a készség, lásd 19.B.3). A Switch táncosok (akik mindkét szerepet tanulják) szerepenként külön sort kapnak — a kettő sosem mosható egybe.

```sql
CREATE TABLE teaching_events (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    lesson_id UUID REFERENCES lessons(id),
    term_id UUID REFERENCES terms(id),
    role VARCHAR(10) NOT NULL,            -- KÖTELEZŐ: 'Leader' | 'Follower' | 'Solo'
    occurrence_type VARCHAR(20),          -- 'first_teaching' | 'repetition_2' | 'recall' | 'application'
    teaching_detail VARCHAR(20),          -- 'shown' | 'broken_down' | 'refined'
    musicality_context VARCHAR(50),       -- Melyik zenei frázishoz / hangsúlyhoz kötötték (lásd 19. fej.)
    created_at TIMESTAMPTZ DEFAULT NOW()
);
```

### D. K3 — Minőségbiztosítási Réteg (Quality Assurance Overlay)

A skill-mátrix legerősebb jele. Itt a tanár (T2) vagy szenzor (T4) **megerősíti**, hogy a részvétel és az ismétlések tényleg tudássá értek.

- **Tanári megerősítés:** A tanár a tanuló profilján egy elemet `mastery_level`-re emel (pl. `2` = stabil végrehajtás, `3` = improvizatívan alkalmazza). Ez felülírja a valószínűségi K1/K2 becslést.
- **Bizonyíték-alapú megerősítés:** Videós önértékelés, vizuális annotáció (13. fej.) vagy haptikus szenzor (15.A.4) adatai automatikusan emelhetik a `mastery_level`-t.

### E. A Skill-Mátrix Egyesített Adatmodellje (Unified Skill Matrix)

A három szint egyetlen, összevont nézetben jelenik meg a tanuló profilján. Minden bejegyzés **megőrzi a forrását és a bizonyosságát**, így a tanár átláthatja, mi puszta részvétel és mi bizonyított tudás.

A skill-mátrix kulcsa **összetett**: a tudást nem csak elemenként (`term_id`), hanem **szerepenként (`role`) is külön tartjuk nyilván**. Így a térkép pontosan kifejezi, hogy *"a diák a `CBL`-t Followerként K3 szinten tudja, de Leaderként még csak K1 szinten expozálódott neki"*. A Switch táncos ugyanahhoz az elemhez két külön sort kap.

```sql
CREATE TABLE student_skill_matrix (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    student_id UUID REFERENCES users(id),
    term_id UUID REFERENCES terms(id),
    role VARCHAR(10) NOT NULL,             -- KÖTELEZŐ: 'Leader' | 'Follower' | 'Solo'
    exposure_count INT DEFAULT 0,          -- K1: hány leadott órán fordult elő
    repetition_count INT DEFAULT 0,        -- K2: hány ismétlési kvantum
    deepest_teaching_detail VARCHAR(20),   -- K2: legmélyebb tanítási részletesség
    mastery_level INT DEFAULT 0,           -- K3: 0=nincs, 1=ismeri, 2=stabil, 3=improvizatív
    confidence VARCHAR(10),                -- 'low' (K1) | 'medium' (K2) | 'high' (K3)
    acquired_via VARCHAR(20) DEFAULT 'direct', -- 'direct' (közvetlen tanulás) | 'retroactive' (visszamenőleges jóváírás, lásd 18.F)
    source_term_id UUID REFERENCES terms(id), -- Ha 'retroactive': melyik dekonstruált szülő-fogalomból származik
    last_seen_at TIMESTAMPTZ,
    UNIQUE (student_id, term_id, role)
);
```

> **A pedagógiai modul ígérete:** A "mezei" tánctanár pusztán azzal, hogy ebben a rendszerben tervezi és vezeti az óráit (jelenlét + leadott anyag jelölése), **automatikusan, mellékhatásként megkapja** a csoportjai és tanítványai részvétel-alapú tudástérképét. Ha többet vállal (ismétlés-kvantálás, minőségi megerősítés), a térkép arányosan pontosabb lesz — de a belépő szintű érték már nulla extra adminisztrációval is működik.

### F. Visszamenőleges Tudás-Szétosztás (Retroactive Skill Distribution)

A pedagógiai modul (18.) és a Fuzzy-to-Precise életút (13.) találkozásánál kritikus láncszem rejlik. A 13. fejezet megengedi, hogy a tanár **homályos (`fuzzy_draft`) fogalmakat** hozzon létre (pl. `tmp.PetiFuraForgasa`), a 20. fejezet pedig rögzíti, hogy a diákok ezt megtanulták (K1/K2). De **mi történik, ha a tanár 3 hónappal később a Refinement Engine-nel atomizálja** ezt az elemet (`pvt.R` + `sld.L`)?

- **A probléma (a régi "halott alias"):** Naiv esetben a diák profilján egy értelmét vesztett `tmp.PetiFuraForgasa` alias maradna, és a rendszer **nem tudná**, hogy a diák valójában már gyakorolta a pivotot és a csúszást is. A tudás "csapdába esne" a homályos szülő-fogalomban.
- **A megoldás — Retroactive Skill Distribution:** Amikor egy tanár (T3+) dekonstruál egy fuzzy fogalmat, a rendszer a háttérben **automatikusan szétosztja az újonnan felfedezett atomi komponenseket** minden olyan diák `student_skill_matrix` táblájába, aki a szülő-fogalmat korábban tanulta.

**A folyamat (Distribution Pipeline):**

```
[T3 tanár: tmp.PetiFuraForgasa  ──Refinement Engine──►  pvt.R + sld.L]
                       │
                       ▼  (Trigger: epistemological_state -> 'deconstructed')
┌──────────────────────────────────────────────────────────────────┐
│ 1. Érintett diákok lekérése:                                       │
│    SELECT * FROM student_skill_matrix WHERE term_id = UUID_tmp...  │
├──────────────────────────────────────────────────────────────────┤
│ 2. Minden érintett (student, role) párra: a szülő K1/K2 jelét      │
│    örökítsd az új atomokra (pvt.R, sld.L), azonos role-lal.        │
│    acquired_via = 'retroactive', source_term_id = UUID_tmp...      │
├──────────────────────────────────────────────────────────────────┤
│ 3. A régi tmp.* sor megmarad (lineage), de a profilon az atomok    │
│    is megjelennek a háttérben jóváírt tudásként.                   │
└──────────────────────────────────────────────────────────────────┘
```

1. **Bizonyosság-örökítés (Confidence Inheritance):** A visszamenőleg jóváírt atomi készség **sosem kaphat magasabb bizonyossági szintet, mint a forrás**. Ha a diák a `tmp.PetiFuraForgasa`-t csak K1 (exposure) szinten ismerte, az `pvt.R` és `sld.L` is csak K1-et örököl — soha nem K3-at. A `mastery_level` (K3) megerősítés mindig **közvetlen, friss bizonyítékot** igényel, sosem öröklődik visszamenőleg.
2. **Szerep-megőrzés (Role Preservation):** A szétosztás **szerep-érzékeny**: ha a diák a fogalmat Followerként tanulta, az atomokat is Followerként írjuk jóvá (lásd 18.C role oszlop).
3. **Visszakövethetőség (Lineage):** A jóváírt sorok `acquired_via = 'retroactive'` és `source_term_id` mezővel jelölik, honnan származnak. Így a tanár átláthatja, mi közvetlen tanulás és mi visszamenőleges levezetés, és egy esetleges téves dekonstrukció visszavonható (a retroaktív sorok törölhetők a forrás alapján).
4. **Idempotencia:** Ha a diák az atomot időközben közvetlenül is megtanulta (`direct`, magasabb bizonyossággal), a retroaktív jóváírás **nem rontja le** azt — a rendszer a kettő közül a magasabb bizonyosságot tartja meg.

> **Összekapcsolás a 20. fejezettel:** A Retroactive Skill Distribution adja az atomizáció-vezérelt absztrakció (20.A) mérhető alapját: amint egy homályos fogalom atomizálódik, a diákok tudása is azonnal, visszamenőleg atomi szintűvé válik — feltárva, hogy egy-egy atomot (pl. a pivotot) több korábbi fogalomból is ismernek, ami megnyitja az átvihető, kombinálható, improvizatív tudás útját.

### G. Szakmai Utódlás és Roster Tananyagok (Succession Training & Roster Curriculums — D079, D080)

A DANA ökoszisztéma szakmai utódlási motorja (D079) és a belső munka-marketplace (D080) közvetlenül a KineLex pedagógiai moduljára és a közös formális nyelvre támaszkodik. Ezzel küszöböljük ki az old-school „kockás-papíros", ad-hoc kinevelést, amely a segédtanárok elvérzéséhez vezetett.

1. **Roster Tananyagok (Roster-Specific Curriculums):**
   - Minden senior tanár (mentor) a KineLex-ben definiálhat egy vagy több **Mentor-Tananyagot (`roster_curriculum`)**. Ez egy irányított gráf vagy strukturált óraterv-sorozat, amely tartalmazza a mentor egyedi módszertani elemeit (pl. Dóri egyedi bachata technikái, megjelölve a `tmp.*` vagy atomizált `pvt.*` kódokkal).
   - A roster-tanuló (tanítványsegéd / leendő tanár) ezen a dedikált tananyagon halad végig. A haladást a rendszer a `student_skill_matrix` (18.E) táblában követi.

2. **Szakmai Dialektus-Kompatibilitás (Dialect & Dictionary Matching — D080):**
   - A hálózaton belüli automatizált munkahely-párosítás (D080) alapfeltétele, hogy a jelentkező segédtanár **ugyanazt a módszertani nyelvet beszélje**, mint az órát kiíró senior.
   - A KineLex egy **„Dialektus-Metrikát" (`dialect_compatibility`)** számol a mentor és a jelölt között a közös fogalmak és azonos dekonstrukciós származási fák (Lineage Tree — 9.B) alapján.
   - Ha egy órához (pl. Dóri Bachata órája) helyettesítő vagy segédtanár szükséges, a Marketplace (D080) csak azokat ajánlja fel kiemelt prioritással, akiknek a `dialect_compatibility` mutatójuk meghaladja a beállított küszöböt (pl. 85%-os egyezés az órán használt fogalomtárral).

3. **Mastery-alapú Kinevelési Elszámolás (Succession Validation & Verification):**
   - A mentor-override (D016) kifizetésének és az Earned Equity (D061) tulajdonszerzésnek feltétele a kinevelés minőségének igazolása. Ez nem puszta adminisztráció, hanem a **K3 minőségbiztosítási szint (18.D)** elérése a roster-tananyagon.
   - A tanuló/segédtanár **mastery szintjeit** a mentor igazolja a KineLex felületén (pl. KineLex videó-annotáció és mozgás-korrekció segítségével, 14. fej.), vagy a rendszer automatikusan kalkulálja a diákok visszajelzéseiből (D069) és a leadott órák K2-es statisztikáiból.

```sql
CREATE TABLE mentor_roster_curriculums (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    mentor_id UUID REFERENCES users(id),
    name VARCHAR(100) NOT NULL,            -- pl. 'Dori Bachata Sensual Lady Styling Curriculum'
    description TEXT,
    target_role VARCHAR(10) NOT NULL,      -- 'Leader' | 'Follower' | 'Solo'
    created_at TIMESTAMPTZ DEFAULT NOW()
);

CREATE TABLE roster_curriculum_steps (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    curriculum_id UUID REFERENCES mentor_roster_curriculums(id),
    sequence_order INT NOT NULL,
    term_id UUID REFERENCES terms(id),      -- A KineLex-ben definiált fogalom/mozdulat
    required_mastery INT DEFAULT 2          -- Elvárt K3-as szint (pl. 2=stabil végrehajtás)
);

CREATE TABLE student_roster_progress (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    student_id UUID REFERENCES users(id),
    curriculum_id UUID REFERENCES mentor_roster_curriculums(id),
    completed_steps_count INT DEFAULT 0,
    started_at TIMESTAMPTZ DEFAULT NOW(),
    last_validated_at TIMESTAMPTZ,
    status VARCHAR(20) DEFAULT 'in_progress' -- 'in_progress' | 'completed' | 'suspended'
);
```

---

## 19. A Tudás Természete: Koreográfia kontra Improvizáció (The Nature of Knowledge: Choreography vs. Improvisation)

A tudásmérés (20. fejezet) legnagyobb kihívása, hogy a **"tud egy mozdulatsort" megfogalmazás önmagában megtévesztő**. Két, gyökeresen eltérő tudásfajta létezik, amelyeket a rendszernek külön kell kezelnie.

### A. A Shakespeare-szonett Probléma (The Memorized-Block Problem)

Egy komplex mozdulatsort meg lehet tanulni **zárt koreográfiaként** — ahogy egy angolul nem beszélő ember is bemagolhat egy Shakespeare-szonettet. Az ilyen tudás:

- **nem variálható szabadon:** a tanuló legfeljebb a blokkok sorrendjét cserélgeti, de az elemeket nem tudja önállóan kontextusba helyezni;
- **nem improvizatív:** kiszakítva a betanult láncból az egyes elemek "nem élnek";
- **kontextusvak:** nem reagál a partnerre, a térre vagy a zenei pillanatra.

Ezzel szemben az **improvizatív (generatív) tudás** olyan, mint egy nyelv valódi beszélése: a tanuló az elemeket szabad mondatokká fűzi, a pillanathoz igazítva.

> **Rendszerszintű következmény:** Ugyanaz a `term_id` szerepelhet a skill-mátrixban "betanult blokk része" és "szabadon alkalmazható atom" minőségben is. A rendszer ezt a `knowledge_modality` mezővel különbözteti meg, mert a kettő pedagógiailag **nem ekvivalens**.

```sql
ALTER TABLE student_skill_matrix
  ADD COLUMN knowledge_modality VARCHAR(20) DEFAULT 'memorized_block';
  -- 'memorized_block'  : zárt koreográfiaként tudja (Shakespeare-szonett)
  -- 'block_reorder'    : blokk-sorrendet cserélget
  -- 'free_application' : elemként, szabadon kombinálja (generatív)
  -- 'improvisational'  : kontextusra (zene/partner) reagálva alkalmazza
```

### B. A Négy Social Dance Kompetenciatengely (The Four Social-Dance Axes)

A social táncban (Salsa, Bachata) az érték **nem a betanult lánc**, hanem négy, egymástól független kompetenciatengely együttese. A rendszer ezt a négy dimenziót **külön-külön is méri** a skill-mátrixban:

1. **Elemkiválasztás — "Mit táncoljak?" (Element Selection):** A tanuló a pillanatban a legjobb elemet választja ki a repertoárjából.
2. **Időmenedzsment / Mondatalkotás (Time Management & Musicality):** A tanult mozdulatok **rá-morfolása a zenére** — a mozdulat zenei frázishoz, hangsúlyhoz illesztése (lásd részletesen a 20. fejezetet).
3. **Kommunikáció (Communication / Lead & Follow):** Párban a tökéletes vezetés vagy követés — a mozdulat partnerrel való valós idejű egyeztetése.
4. **Stílusválasztékosság (Style & Expression):** Díszítés, személyiség, gag-ek, a hangulat fokozása.

A kompetenciatengelyeket is **szerepenként** mérjük (`role`): a kommunikáció tengelye különösen szerep-függő (vezetni és követni külön készség), de a Switch táncosnál az elemkiválasztás és a stílus is eltérhet a két szerepben.

```sql
CREATE TABLE student_competency_axes (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    student_id UUID REFERENCES users(id),
    dance_domain VARCHAR(30),              -- pl. 'salsa.on2', 'bachata'
    role VARCHAR(10) NOT NULL,             -- KÖTELEZŐ: 'Leader' | 'Follower' | 'Solo'
    element_selection INT DEFAULT 0,       -- "mit táncoljak"
    musicality INT DEFAULT 0,              -- időmenedzsment / mondatalkotás
    communication INT DEFAULT 0,           -- vezetés / követés
    style_expression INT DEFAULT 0,        -- díszítés / személyiség
    updated_at TIMESTAMPTZ DEFAULT NOW(),
    UNIQUE (student_id, dance_domain, role)
);
```

### C. Az Előadóművészeti (Performance) Tengely

Az előadóművészeti / versenytáncban a koreográfus a fenti négy kontextustengelyt (elemkiválasztás–időmenedzsment–kommunikáció–stílus) **előre megoldja és rögzíti**. Ez **nem hátrány, hanem fókuszáló döntés**: pont ezért a táncos a felszabaduló kapacitását a **mély technikai, dinamikai és esztétikai kihívásokra** fordíthatja:

- maximális mozgásminőség (precíz ízületi szögek, súlypont-ívek, lásd 13. fej.),
- dinamika és energiamenedzsment,
- kiemelt esztétika és előadói jelenlét.

A rendszer ezt a tengelyt a **vizuális annotációs (13.)** és **3D korrekciós (15.A.4)** modulokkal támogatja, ahol a "Drawn Ideal vs. Actual Pose" kontraszt számszerűsíthető technikai visszajelzést ad.

### D. A Két Terület Összeérése — Az Ideális Táncos Profilja (The Convergence Target)

A pedagógiai pólus végső, **rendszerszinten a háttérben megcélzott** célja, hogy a két terület összeérjen. Az ideális táncos egyetlen mozdulatfolyamban egyesíti mind a hat dimenziót:

```
                   ┌─────────────────────────────────────────────┐
                   │           AZ IDEÁLIS TÁNCOS (Vízió)          │
                   └─────────────────────────────────────────────┘
  SOCIAL tengelyek (improvizáció)        │   PERFORMANCE tengelyek (mélység)
  ─────────────────────────────         │   ─────────────────────────────
  • azonnali elemkiválasztás (mit)       │   • maximális technikai komplexitás
  • zenére morfolás (mikor/hogyan)       │   • nagy dinamika
  • tökéletes vezetés/követés (kivel)     │   • kiemelt esztétika
  • stílus, díszítés, gag (hogyan érzem)  │   • előadói jelenlét
                   └──────────────► VILÁGBAJNOK SZINT ◄──────────┘
            (maximális komplexitás könnyedén, nagy dinamikával és esztétikával)
```

> **A "tiszteletben tartott határ" elve:** A rendszer **nem kényszerít** senkit a teljes konvergenciára. Egy hobbi social táncosnak a négy social tengely fejlesztése a cél; egy versenyzőnek a performance mélység. De a **háttérarchitektúra mindkét utat egyszerre méri és támogatja**, így ha egy tanuló (és tanára) elindul a teljesség felé, a rendszer végig vele tart — anélkül, hogy bárkire ráerőltetné a másik világ elvárásait.

---

## 20. Atomizáció-Vezérelt Improvizációs Absztrakció (Atomization-Driven Improvisational Abstraction)

Ez a fejezet köti össze a leíró nyelv atomizációs képességét (13. fej.) a tudás minőségi természetével (18. fej.). **Központi tézis:** ahogy a tanár megfogalmazása egyre atomizáltabbá válik, **a tanulók tudásszintje is egyre atomizáltabban válik megfogalmazhatóvá** — és pont ez nyitja meg az utat, hogy a tudásuk a betanult blokkból (Shakespeare-szonett) **absztraktabb, környezetre alkalmazkodó, improvizatív tudássá** alakuljon.

### A. A Megfogalmazás és a Tudás Atomizációjának Párhuzama (Parallel Atomization)

```
A TANÁR MEGFOGALMAZÁSA                       A TANULÓ TUDÁSÁNAK LEÍRÁSA
─────────────────────────                    ──────────────────────────
fuzzy_draft  ("furcsa csúsztatás")     ──►   "találkozott egy homályos elemmel" (K1)
egyezményes  ("sldScrew")              ──►   "tudja az sldScrew figurát"          (K2)
atomizált    ("pvt.R + sld.L + lvl")   ──►   "tudja a pivotot, a csúszást ÉS a    (K3, atomi)
                                              súlypontsüllyesztést — külön-külön is"
```

A párhuzam pedagógiai haszna: amíg egy fogalom csak egyezményes névként létezik, a tanuló tudása is csak "tudja/nem tudja" bontásban mérhető. Amint a fogalom **atomizálódik**, a tanuló tudása is **atomi készségekre bomlik** — és kiderül, hogy pl. a pivotot már három másik figurából is ismeri. Ez teszi a tudást **átvihetővé (transferable)** és kombinálhatóvá.

### B. A Didaktikai Absztrakció-Vezérlő Motor (Abstraction Pacing Engine)

A rendszer aktívan **segíti a tanárt** abban, hogy a betanult blokkot generatív tudássá oldja fel. A motor a 18.C ismétlés-kvantumokra épül, és **az ismétlésszám függvényében javasol absztrakciós lépéseket**:

1. **1-2. ismétlés (Stabilizáció):** A motor **nem avatkozik be**. A cél az elem változatlan formájú berögzítése (motoros memória). A `knowledge_modality` itt még `memorized_block`.
2. **Szétbontás és Összekeverés (Decompose & Remix):** A 2. változatlan ismétlés *után* a rendszer felajánlja, hogy a tanár **bontsa szét** az elemet, és **keverje össze a régi, korábban tanult elemekkel**. (Pl.: *"A csoport kétszer stabilan letáncolta a `sldScrew`-t. Javaslom, kösd most a már ismert `CBL`-hez és `rsp`-hez, hogy a tanulók a blokkból kiemelve, szabad átmenetként is gyakorolják."*) → `knowledge_modality` célállapot: `free_application`.
3. **Variáció-Összehasonlítás (Side-by-Side Variations):** A motor felajánlja **ugyanazon elem különféle variációinak egymás mellé tételét**, hogy a tanuló a különbségek tudatosításával ne egy merev formát, hanem egy **variációs teret** tanuljon meg (pl. `sldScrew` vs. `pvtScrew` vs. `flareScrew` közvetlen összehasonlítása).

```sql
CREATE TABLE abstraction_suggestions (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    group_id UUID REFERENCES groups(id),
    term_id UUID REFERENCES terms(id),
    trigger_repetition_count INT,          -- Hányadik ismétlés váltotta ki
    suggestion_type VARCHAR(30),           -- 'decompose_remix' | 'side_by_side' | 'musicality_deviation'
    suggested_partners JSONB,              -- Mely régi elemekkel keverjük / hasonlítsuk
    status VARCHAR(20) DEFAULT 'offered',  -- 'offered' | 'accepted' | 'dismissed'
    created_at TIMESTAMPTZ DEFAULT NOW()
);
```

### C. A Musicality mint Absztrakciós Dimenzió (Musicality as an Abstraction Axis)

A musicality (időmenedzsment, 19.B.2) ugyanúgy az atomizáció-vezérelt absztrakció dimenziója: a **"tipikusan előre sejthető" zenei frázisok** (pl. egy 8-as vége, egy break, egy trombita-hangsúly) **idejéhez kreatív mozdulat-eltéréseket rendel**.

- **Sejthető frázis → kreatív deviáció:** A rendszer ismeri a zenei struktúra előre jelezhető pontjait (a ritmikai szintaxisból, 5. fej.). Ezekhez **deviációs ajánlásokat** kapcsol: ahelyett, hogy a tanuló a frázis végén is a "default" alaplépést táncolná, a motor felkínál egy kontextushoz illő díszítést, hangsúlyt vagy irányváltást.
- **Kapcsolat a Kombinatorikus Variációs Motorral (7.C):** A CVE által generált variációk itt **zenei lehorgonyzást** kapnak — nem önmagukban, hanem egy konkrét, sejthető zenei pillanathoz rendelve válnak gyakorolható improvizációs feladattá.
- **Mérés:** Az így gyakorolt deviációk a `teaching_events.musicality_context` mezőn keresztül naplózódnak, és a `student_competency_axes.musicality` tengelyt emelik.

### D. Az Absztrakciós Életút a Skill-Mátrixban (The Abstraction Lifecycle)

A három modalitás-lépcső (18.A) így kapcsolódik össze egyetlen, mérhető fejlődési ívvé, amelyet a tanár az Abstraction Pacing Engine ajánlásaival vezérel:

```
[memorized_block] ──(2 stabil ismétlés)──► [block_reorder]
        │                                          │
        │                              (decompose & remix régi elemekkel)
        │                                          ▼
        └──────────────────────────────► [free_application]
                                                   │
                                  (side-by-side variációk + musicality deviáció)
                                                   ▼
                                          [improvisational]
                              (kontextusra — zene + partner — reagáló, generatív tudás)
```

> **A rendszer csendes nagy célja:** Minden egyes atomizációs lépés (akár a tanár fogalmainak tisztázása, akár a tanuló tudásának finomítása) egy aprócska lépés afelé, hogy a tanuló a betanult szonettből **valódi nyelvi beszélővé** váljon: aki a zenét hallva azonnal kiválasztja a megfelelő elemet, ráalakítja a zeneiségre, kommunikál a partnerével, és stílussal fűszerezi — azaz a 18.D ideális táncosává. A rendszer ezt **a háttérben, a határokat tiszteletben tartva, de következetesen célozza meg.**

---

