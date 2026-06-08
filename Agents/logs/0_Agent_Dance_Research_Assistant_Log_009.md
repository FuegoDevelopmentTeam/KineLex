# Dance Research Assistant Log - 009
Dátum: 2026-06-07
Kezdeti állapot: A MASTER_CONCEPT.md frissítése a Hibrid Idővonal-Modellel (Span operátor, párhuzamosság) és a Diff-alapú helyettesítő megfogalmazással (komplex fogalmak mutációja).

## 1. Teljes Beszélgetéstörténet (AI_ready)
- **Kutatás fókusza:** Struktúrált, redundancia-mentes, didaktikailag és nyelvileg konzisztens táncleíró nyelv és tudásbázis felépítése (Pedagógiai + Kutatói).
- **Fázisok és elért mérföldkövek (v2.8.0-ig):**
  1. **Nyelvi Mag (UAA & Hibrid Szintaxis):** L0-L3 absztrakciós szintek, CPC-3 modell. Pont (.) operátor kizárólag szülő-gyermek hierarchiára. CamelCase egybeírás az ütközésekig.
  2. **Ütközésfeloldás (Compression Shift & ASS):** L1-L4 Rövidítési Skála-Spektrum. Ütközésnél a domain-specifikus főnév ugrik L2-re (pl. `lAr` = left arm). A rövidítések kiosztása a **Statisztikai Gyakoriság és Atomizáltság egyensúlyán** alapszik (`b` = basic, `ba` = back irány, `bck` = back testrész). UI szinten szigorú "disambiguation-friendly" betűtípus (l, I, 1 miatt).
  3. **Idővonal (Timeline & Hibrid Modell - ÚJ):** Lineáris szöveg és többdimenziós mozgás feloldása. 
     - Pontszerű esemény (Hit): `1:l`
     - Időtartam (Span): `1-4:upCu`
     - Sávos bontás (Track/Voice): `Legs[...] & Arms[...]`
     - Relatív Horgonyzás: `b1(upCu)`
  4. **Ontológia & Származtatás:** A `>` időbeli átmenet; a tényleges származtatás adatbázis (parent-child) szintű. 
     - **Diff-alapú Megfogalmazás (ÚJ):** Létező komplex fogalom helyettesítő variálása: `AlapFogalom[eredeti -> új]` (pl. `b1[col -> colCod]`). Ezzel szülő-gyermek kapcsolat jön létre.
  5. **Fuzzy-to-Precise & Scaffolding:** Hanyag leírások finomítása (Fuzzy Inbox -> Refinement Engine -> Dynamic Aliasing). Tanárok egyedi névhasználata (Rosetta Stone). Semantic anchoring (metaadat-metszet alapján horgonyzás).
  6. **Felhasználási Gradiens & Pedagógiai Modul:** T0-T4 jogosultságok. Pedagógiai fekete-doboz használat vs. Kutatói atomizáció. Tudásszintek: K1 (Részvétel), K2 (Begyakorlottság), K3 (Minőség) - minden mérés szerep-specifikus (Leader/Follower).
  7. **Tudás Természete & Visszamenőleges Jóváírás:** Koreográfia vs Improvizáció (Shakespeare-szonett analógia). Atomizáció-vezérelt absztrakciós ajánló. Egy fuzzy fogalom felbontásakor (Retroactive Skill Distribution) a rendszer visszamenőleg jóváírja az atomi elemeket a diákok profilján.
  8. **Ökoszisztéma:** Vizuális Annotáció (time-stamped vektorok videókon), 3D Code-to-Motion avatar, Git-for-Dance, Predictive Gap Engine.

---

## 2. Beszélgetés Archívum
### Beszélgetés #11 (2026-06-07)
- **User kérése:**
  - Javaslat a lineáris szöveg és a párhuzamos idősíkok (időtartam-események) kezelésére. Ha sorosan írjuk (`1: ... 2: ...`), az 1-4-ig tartó karmozdulat szétesik vagy redundáns lesz. Hogyan reprezentálható a párhuzamosság (zenével analógiában)?
  - A javaslat beépítése, de jelezve, hogy még nincs teljesen eldöntve.
  - Új "diff" megfogalmazási mód bevezetése: Ha van egy már rögzített, egyezményes figuránk (pl. `salsaOn1Basic`), de az adott variáció csak egyetlen dologban tér el (pl. `collector` helyett `colCod`), akkor lehessen csak ezt az eltérést megfogalmazni. Ezzel az új variáció közvetlen szülő-gyermeki kapcsolatba kerülne a bázissal.
- **Tervezés és Megoldások:**
  - A `MASTER_CONCEPT.md` 5. fejezetébe (Ritmikai Szintaktika) bekerült a **Hibrid Idővonal-Modell** koncepciója. Bemutat 4 szintet: Pontszerű (Hit) -> `1:l`, Időtartam (Span) -> `1-4:upCu`, Relatív Horgonyzás -> `b1(upCu)`, Sáv-alapú bontás (Track/Voice) -> `Legs[...] & Arms[...]`. Jelölve lett, hogy ez "TERVEZET", tehát még kidolgozás alatt áll.
  - A 7. fejezetbe bekerült a **Diff-alapú Megfogalmazás (Helyettesítés)**. Szintaktikai javaslat: `AlapFogalom[eredeti_komponens -> új_komponens]`. Példaként felhozva a `salsaOn1Basic[collector -> colCod]`. Ez pontos, tömör, minimalizálja a gépelést és automatikus `parent_term_id` kapcsolatot hoz létre a háttérben.
  - A MASTER_CONCEPT.md verziószáma **v2.8.0**-ra frissült.

### Beszélgetés #12 (2026-06-08)
- **User kérése:**
  - Kinetikai operátorok (határozószók) bevezetése a mozgás leírására, vizuálisan intuitív, billentyűzetről könnyen elérhető karakterekkel (from, to, spin, stb.).
  - Az "L1: Atomi Műveletek és Kinetika" didaktikai szint (2. fejezet) kategorizálásának pontosítása. A "Szabadságfokok" kiegészítése elemi testmozdulatokkal (homorítás, domborítás, csavarás, stb.). A "Végtag Műveletek" átnevezése/kiegészítése egy új kategóriával: "Kiegyenlítő Mozdulatpárok" (olyan komplementer mozdulatok, amelyek a test alsó/felső központjában kiegyensúlyozott csendet, referenciapont-stabilitást hoznak létre).
- **Tervezés és Megoldások:**
  - Létrehoztam a 6. fejezetet: **Vektor és Dinamika Operátorok (Adverbs of Movement)**. Bevezettük az irány és vektor operátorokat (`>` cél/átmenet, `<` eredet/honnan), valamint a dinamika operátort (`*` tengelyfordulat a súlylábon). Ezzel kiválthatók a hosszú angol prepozíciók.
  - A 2. fejezet L1-es szintjét újraírtam az új kategorizálás alapján:
    - **Elemi Testmozdulatok és Szabadságfokok:** ízületi rotációk, plusz alapvető kinetikus akciók (homorítás, domborítás, bump, stb.).
    - **Kiegyenlítő Mozdulatpárok (Compensatory / Counter-Balancing Gestures):** Új kategória a referenciapont mozdulatlanságát garantáló, testrészek közötti egyensúlyt teremtő mozdulatpárokra (pl. csípő kitolása vs. ellentétes kar nyújtása, azonos oldali lábkör és karkör).
  - A MASTER_CONCEPT.md verziószáma **v2.9.0**-ra frissült, a módosításokat commitoltam és pusholtam.

### Beszélgetés #13 (2026-06-08)
- **User kérése:**
  - Rendteremtés a CamelCase használatában. A kódolvasás során a csupa nagybetűs halmozódások (pl. LTO, LT) vizuálisan zavaróak. Csak a szükséges fogalmi határnál legyen nagybetű, és ne legyen nagybetű-torlódás. (Például: lFt, lTo).
  - Szótár evolúció és verziókövetés: Amikor a fogalomkörök vagy elnevezések szabályai megváltoznak, a rendszer kapjon új verziószámot, és a UI-on lehessen régebbi verzióhoz tartozó nézetet (View) is választani a múltbeli óravázlatok sértetlen megjelenítése érdekében.
- **Tervezés és Megoldások:**
  - A 4. fejezetet átírtam a **Anti-Kapszula (No Consecutive Capitals)** és a **Főnévi Bázis (Base Noun) Kiemelése** szabályokkal. A csupa nagybetűs kódok (pl. LTO, MRT) hivatalosan is érvénytelenítve lettek. A mozgás főnévi magja (pl. Turn, Foot) kapja a nagybetűt, míg a minősítők (l, r) kisbetűsek maradnak. Ha egy kód 4 karakternél rövidebb, tisztán kisbetűvel is összeolvadhat (pl. lto, lft). Ha tagolni kell, a Tömörítési Skála alapján legalább L2 szintre toljuk az egyik elemet (pl. lTuOp vagy lFt).
  - A 12. fejezetet kiegészítettem a **D. Fogalomtár Evolúció és Verziózott Nézetek (Dictionary Evolution & Versioned Views)** szekcióval. Ez bevezeti a háttérben történő SemVer (Semantic Versioning) verziózást, és a UI-on választható "Legacy" vagy "Latest" nézeteket, így biztosítva a visszafelé kompatibilitást és a folyamatos fejlődést.
  - A MASTER_CONCEPT.md verziószáma **v3.0.0**-ra ugrott a szótári architektúra és a szintaxis jelentős rendszerszintű változása miatt. A módosításokat commitoltam és pusholtam.

### Beszélgetés #14 (2026-06-08)
- **User kérése:**
  - Értelmezhetőségi Szóösszevonási Szabály bevezetése: Ha egy összetétel egyik tagja tiltó vagy ellentétes értelmet fejez ki (pl. `in-`, mint az "indirekt" esetében), akkor az ne vonódjon össze a másik szóval (pl. `inDir` helyett `ind`). Ez azért fontos, mert bár logikus lenne az `ind` tömörítés, az `in` előtétszó jelentésmódosító/ellentétes funkciója elveszne.
  - Az ilyen jelentésmódosító jelzőket külön listában kell nyilvántartani, és a tömörítési algoritmus ne legyen rájuk hatással.
- **Tervezés és Megoldások:**
  - A `MASTER_CONCEPT.md` 7. fejezetébe (ASS - Rövidítési Skála-Spektrum) bekerült az **Értelmezhetőségi Szóösszevonási Szabály (Interpretability Merge Rule)**. E szerint a tiltó vagy ellentétes értelmű előtagok (pl. `in-`, `un-`, `non-`) kivételt képeznek a maximális tömörítés alól.
  - A példa (direct -> dir, indirect -> inDir, nem pedig ind) be lett építve a logikai megértés támogatására. A jelentésmódosítók ezáltal levédve maradnak a szuper-rövidítőtől.
  - A MASTER_CONCEPT.md verziószáma **v3.1.0**-ra frissült, a módosításokat commitoltam és pusholtam.

### Beszélgetés #15 (2026-06-08)
- **User kérése:**
  - Központi tudásbázis kezelése: Szét kell választani a tanárok saját, egyedi fogalom-objektumait a központi, közös fogalomtárba kerülő "certified" fogalmaktól.
  - A tanárok egyedi fogalmai kezdetben csak a saját rendszerükben (lokálisan) legyenek elérhetők, de lehessen őket "commit-olni" egy központi review-ra.
  - A központi fogalomtárban legyen egy minősítő rendszer: "elfogadott", "visszautasított", "módosítással elfogadott" és "kipróbálási célból elfogadott" állapotokkal.
- **Tervezés és Megoldások:**
  - A 12. fejezetet kiegészítettem az **E. Lokális és Központi Fogalomtár (Local vs. Central Dictionary Governance)** szekcióval.
  - Bevezettem a "Local Scope" és "Propose to Central" koncepcióját: a tanár lokálisan szemetelés nélkül használhatja a kódjait (pl. `PetiFuraForgasa`), majd beküldheti azokat a közösbe.
  - Dokumentáltam a 4 állapotú Központi Minősítő Rendszert (Review & Rating System): 1. Elfogadott (átment a szűrőn, auto-complete-be kerül), 2. Visszautasított (lokális marad, globálisan elvetve), 3. Módosítással elfogadott (pl. a központ érvényesíti az Anti-Kapszula vagy Tömörítési skála elvet, de a lokális alias megmarad), 4. Kipróbálási célból elfogadott (Experimental tag, a statisztikai gyakoriság dönt róla később).
  - A MASTER_CONCEPT.md verziószámát **v3.2.0**-ra frissítettem, a módosításokat commitoltam és pusholtam.

### Beszélgetés #16 (2026-06-08)
- **User kérése:**
  - A MASTER_CONCEPT.md 2. fejezetének (Absztrakciós Szintek) jelentős átírása és kibővítése. Új elnevezések és csoportosítások bevezetése (pl. Fizikai és környezeti összetevők, Testkontúrok, Táncirányok orientációja L0-ban; Pozíciók bevezetése L1-ben, L2 Ciklikus Lépéssorok bontása, Tánc alapmozdulatok hagyatékának rögzítése, stb.).
  - A dokumentum egészében az átírás harmonizációja és a verziószám frissítése.
- **Tervezés és Megoldások:**
  - Kicseréltem a 2. fejezet korábbi szövegét a megadott új, részletes szerkezetre.
  - Szinkronizáltam az operátorokra való korábbi hivatkozásokat (`>`, `<`, `*lFoot`), és ügyeltem arra, hogy a struktúra továbbra is kompatibilis maradjon a Fuzzy-to-Precise (12. fej) és a Pedagógiai Modul (17. fej) logikájával. A kibontott csoportok (mint Pozíciók, Testkontúrok) jobban megalapozzák a videós annotációk és a 3D avatár (14. fej) fejlesztését.
  - A MASTER_CONCEPT.md verziószámát **v3.3.0**-ra emeltem (Movement Abstraction Layers Refinement), a módosításokat commitoltam és pusholtam.

### Beszélgetés #17 (2026-06-08)
- **User kérése:**
  - A 3. fejezet korábbi átírása alapján harmonizálni kellett a Super AAA (Szuper Rövidítés) és a Standard AAA elvek esetlegesen közös és eltérő adaptív jellegét. Hogyan tud ez a két elv a legharmonikusabban együttműködni?
- **Tervezés és Megoldások:**
  - Létrehoztam a 3. fejezeten belül a **C. A Két Elv Harmonikus Együttműködése (The Adaptive Synergy)** szekciót.
  - Tisztáztam a szerepeket: a Standard AAA a determinisztikus, automatikus "Keltető" (Incubator), ahol minden fogalom elkezdi az életét. A Super AAA pedig a kurátori "Hírességek Csarnoka" (Hall of Fame), ahová a statisztikai használat révén kerülhetnek fel a legfontosabb fogalmak.
  - A dinamika alapja a "Kétirányú Ozmózis" (Felminősítés és Lefokozás), amely biztosítja a szűk (1-2 karakteres) elit névtér folyamatos tisztaságát és aktualitását.
  - Rögzítettem a "Zökkenőmentes Identitás-Megőrzés" szabályát: a mozdulat UUID-ja sosem változik. A 12.D Verziózott Szótár nézeteinek (Versioned Views) köszönhetően a tanár bármelyik régi kódot begépelheti, a UI automatikusan a legfrissebb érvényes verzióra fordítja azt kód-törés nélkül.
  - A MASTER_CONCEPT.md verziószámát **v3.4.0**-ra emeltem, a módosításokat commitoltam és pusholtam.

### Beszélgetés #18 (2026-06-08)
- **User kérése:**
  - A 7. fejezetet (Rövidítési Skála-Spektrum / ASS) helyezzük át a 3. fejezet (Rövidítési Algoritmusok) mögé 4. fejezetként, mivel szorosan oda tartozik.
  - Számozzuk át az összes alatta lévő fejezetet.
  - Harmonizáljuk a tartalmat, és írjuk át az egész rövidítési elv- és szabályrendszert úgy, hogy az ASS konkrét hierarchiája és integrációja a rendszerben tökéletesen logikus legyen.
- **Tervezés és Megoldások:**
  - A korábbi 7. fejezetet áthelyeztem a **4. Rövidítési Skála-Spektrum (ASS) és Ütközésfeloldás** névre. Az alatta lévő összes fejezet sorszámozását (5-től 20-ig) frissítettem, beleértve egy korábbi duplikált 8-as fejezetszám kijavítását is, valamint az összes belső kereszthivatkozást (pl. "lásd 13.D fejezet") is szinkronizáltam az új sorszámokkal.
  - A 3. és 4. fejezet tartalmát mélyen harmonizáltam:
    - Kifejtettem, hogy a **Super AAA (Elit szint)** az **ASS L1 és L2** rövidítési szintekre épül.
    - Kifejtettem, hogy a **Standard AAA (Keltető)** az **ASS L3 és L4** rövidítési szinteket fedi le.
    - Az "Ütközésfeloldás" (Compression Shift) és az "Értelmezhetőségi Szóösszevonási Szabály" logikusan az ASS spektrumába lett beágyazva a 4. fejezetbe.
  - Ezzel a rövidítési logikák egyetlen, hatalmas és összefüggő elméleti blokká váltak a dokumentum elején.
  - A MASTER_CONCEPT.md verziószámát **v4.0.0**-ra emeltem az átfogó szerkezeti refaktorálás miatt, a módosításokat commitoltam és pusholtam.

### Beszélgetés #19 (2026-06-08)
- **User kérése:**
  - A Super AAA "Hírességek Csarnokába" való bekerülés logikájának megkérdőjelezése: Miért volt a Super AAA a didaktikai szintekhez (L0-L4) és a C0-C4 kategóriákhoz kötve? A Super AAA tagsága (rövidség) a gyakoriságtól függ, nem pedig az atomizációs szinttől (L0-L4). A C0-C4 besorolás létjogosultsága megkérdőjelezhető.
- **Tervezés és Megoldások:**
  - Teljesen egyetértettem a felhasználó érvelésével: A didaktikai szint (L0-L4) a mozdulat szemantikai komplexitását jelenti, míg az ASS skála (L1-L4) a kód szintaktikai hosszát. A kettő független, ortogonális tengely.
  - A 3. fejezetből **teljesen eltávolítottam a "Szuper Rövidítések Kategóriái (C0-C4)" szekciót**, mivel ez mesterségesen és feleslegesen kötötte össze a jelentéstartalmat a rövidítési szinttel.
  - Helyette beszúrtam a **"Függetlenség az Absztrakciós Szintektől (Independence from Didactic Layers)"** alapelvet. Bármilyen szintű (L0-L4) fogalom megkaphatja a szuper-rövidítést (ASS L1-L2), ha a statisztikai használata ezt indokolja.
  - A MASTER_CONCEPT.md verziószámát **v4.1.0**-ra emeltem (Decoupling Super AAA from Didactic Layers), a módosításokat commitoltam és pusholtam.

### Beszélgetés #20 (2026-06-08)
- **User kérése:**
  - Több szóból álló bázisfogalmak (pl. *Stop and Go*, *Cross Body Lead*) rövidítésének tisztázása: beemelni, hogy több szóból álló fogalmaknál az L3 szint ne CPC-3 (mássalhangzó csonkolás) legyen, hanem Acronym (kezdőbetűs mozaikszó).
  - Az ASS skála kiterjesztése L4-től L6-ig (ahol az L_max a teljes szó), mert az L1-L3 skála kevés a komplex tánc fogalmak alaki ütközéseinek elkerülésére.
  - Az "Intuitivitást elősegítő harmonizáló elv" bevezetése a 4, 5, 6 karakteres rövidítéseknél: A szótag-alapú és mássalhangzós (CPC) csonkolás váltogatása (pl. pendulum: p, pd, pen, pend, pendu, pendlm), hogy "az agy azonnal kimondja".
  - Szinonima javaslása (Rokonértelmű kifejezések felajánlása) ütközés esetén.
- **Tervezés és Megoldások:**
  - A 3.B fejezetben lefektettem a **Több Szavas Fogalmak (Multi-Word Concepts - Acronyms)** szabályt, ahol L3 a kezdőbetűkből (pl. `sag`), az L2 a kötőszók elhagyásából áll (pl. `sg`).
  - A 4.A fejezetben az ASS skálát **L1-L6 és L_max** (Full Word) tartományra bővítettem ki.
  - A 4.B fejezetben kidolgoztam az **Intuitivitást Elősegítő Harmonizáló Elv (Intuitive Cognitive Completion Rule)** szabályát 8+ karakteres szavakra. A rövidítések a szótaghatárokra és a kulcs-mássalhangzókra épülnek, sűrítve a szóvégeket (`-tion`, `-lum`). A `pendulum` és `external` szavak példáján bemutatva a kognitív L1-L6 feloldást.
  - A 4.D fejezetbe (Ütközésfeloldás) bekerült a **Rokonértelmű Kifejezések Felajánlása (Synonym Suggestion Strategy)**: ha az elmozdulás sem menti meg az alaki ütközést (pl. `rot` már foglalt), a rendszer szinonimát (pl. `spin` -> `spn`) ajánl fel.
  - A MASTER_CONCEPT.md verziószámát **v4.2.0**-ra emeltem, a módosításokat commitoltam és pusholtam.

### Beszélgetés #21 (2026-06-08)
- **User kérése:**
  - Az "Intuitivitást elősegítő harmonizáló elv" (ICCR) kiterjesztése: ne csak a 8+ karakteres szavakra vonatkozzon, hanem már a 3+ karakteres szavakra is érvényes legyen, hiszen a rövidebb szavak is igényelhetik a szótag/mássalhangzó alapú kognitív feloldást a 3-6 karakteres tartományban.
- **Tervezés és Megoldások:**
  - A 4.B fejezetben az ICCR szabály hatályát "8+ karakteres szavaknál"-ról **"3+ karakteres szavaknál"**-ra módosítottam.
  - Szöveges pontosítás: A 4, 5, 6 karakteres rövidítések mellet a "3, 4, 5 és 6 karakteres" megfogalmazást alkalmaztam, illetve finomítottam, hogy "már nem mindig elég hatékony" a puszta kivonás.
  - A MASTER_CONCEPT.md verziószámát **v4.2.1**-re emeltem, a módosításokat commitoltam és pusholtam.