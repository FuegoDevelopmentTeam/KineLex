# MASTER_CONCEPT (Dance Systems Architecture)
Version: 2.1.0 (Hybrid Pronounceable System with ASS)
Date: 2026-06-04

## 1. Rendszer-architektúra és Filozófia
Ez a dokumentum a "DANCE" projekt szoftverarchitektúrájának, fogalomterének és formális leíró nyelvének egyetlen igazságforrása (Source of Truth - SoT).
A rendszer fő célja, hogy egy rendkívül tömör, szótárral visszakövethető (dictionary-backed), de gépileg és emberileg egyaránt könnyen írható és olvasható hibrid kódrendszert biztosítson a solo és social páros táncok (különösen Salsa, Bachata) leírására.

---

## 2. Didaktikai Szintek (Abstraction Layers)
A mozgásokat négy absztrakciós szinten modellezzük:
1. **L0: Atomi / Geometriai és Anatómiai szint (Atoms & Primitives):** A test végpontjai (`occi`, `scr`), eltolások, irányok és kéz-koordináták (`6h`, `12h`).
2. **L1: Core Lépések és Forgások (Core Patterns):** Alaplépések és bázis forgások (`CBL`, `LLT`, `MRT`, `Cami`, `Cuca`, `Tri`, `aida`).
3. **L1: Kinetikus Simított Átmenetek (Smooth Transients - SM00-SM21):** Test- és vállhullámok, fej- és csípőkörzések rögzített listája.
4. **L3: Páros Szekvenciák és Figurák (Choreographies):** Időben láncolt, összetett minták és koreográfiai szálak (pl. `CBL.Lhx.LLTO`).

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
A kódok láncolatának minimalizálása érdekében egy **hibrid rendszert** alkalmazunk: ahol a CamelCase egyértelmű és kompakt, ott azt használjuk; ahol viszont kétértelműség vagy strukturális tagolás szükséges, ott kötelezően bevezetjük a pont (`.`) vagy egyéb karakterek használatát.

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
