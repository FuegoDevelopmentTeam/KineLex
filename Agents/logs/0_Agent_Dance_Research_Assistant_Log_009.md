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