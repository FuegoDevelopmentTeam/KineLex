# Dance Research Assistant Log - 002
Dátum: 2026-06-03
Kezdeti állapot: Folytatódó kutatás a hibrid és kiejtés-barát architektúra irányába, kiindulási dokumentumok dekonstrukciójával, beleértve a legújabb Course 2025.2. Semester.md-t.

## 1. Teljes Beszélgetéstörténet (AI_ready)
- **Kutatás fókusza:** Hagyatéki óravázlatok, jógapóz-alapú testkondicionálás, nyújtások, és Salsa On2 + Afrokubai Rumba-infúziós kódok (16 db PDF, DOCX, XLSX és MD fájl) beolvasása, elemzése, és az ott használt terminológia integrálása egy tisztított hibrid rendszerbe.
- **Megállapított elvek (MASTER_CONCEPT v2.0.0-ba rögzítve):**
  - **UAA 2.0 (Unified Abbreviation Architecture):** Kiterjesztett 1-2 betűs jelzők készlete (`el`, `dp`, `sgl`, `trpl`, `M`, `F`, `lg`, `ft`, `sh`, `hp`, `k`, `hd`, `to`).
  - **Kimondhatósági Kivétel (Pronouncability Exception):** A kimondhatóság és verbalizáció javítása érdekében a kiejtést követő rövidítéseket alkalmazzuk (`long` -> `lon`, `short` -> `sho`, `caminando` -> `cami`, `cucaracha` -> `cuca`).
  - **Hibrid CamelCase & Dot Case:** CamelCase-t használunk a takarékos láncolásra (`elRT`, `bwdCami`), ha az egyértelmű. Pontot (`.`) kizárólag ütközéselkerülésre (`l.a` - left arm vs `la` - lady), hierarchikus névterekre (`occi.shift.lat`), állapot-specifikációra (`LLT.TO`), és két CPC-3 kód határára (`gld.sld`) teszünk.
  - **Ritmus Hagyaték:** A `12 + 34 & 56 + 78` ritmikai szintaktika és az `1:` típusú időzítési markerek támogatása.
- **Tudásintegrációs Dokumentumok:**
  - `old_course_individual_terms.md`: Egyedi legacy kifejezések glosszáriuma (pl. `CBL`, `LLT`, `RSP`, `CBB`, `dCRT`, `Hlock`, `Cami`, `Cuca`, `symC`, `symA`, clock coordinates `6h`/`12h`), kiegészítve a 2025.2 félév Floorwork és Jógapóz kódjaival (`alfs` - All Fours, `crb` - Crab, `prn` - Prone, `zSit`, `liz` - Lizard, `pig` - Pigeon, `cml` - Camel, `chld` - Child's pose, `cob` - Cobra, `Bfl` - Butterfly, `mats` - Matsyendrasana, `LyAtt` - Lying Attitude), valamint Salsa On2 / Rumba-kódjaival (`rsh`/`Rsh` - Rumba Shines/Right Shoulder, `on2RshB`, `baRshCuca`, `rshBaWlk`, `touchNflagB`, `on1cubBr`).
  - `old_course_essence.md`: Redundancia-mentes kurzustematikák (Salsa Cat 01-03 bázisok, Bachata kezdő/közép óravázlatok, 22 darab `SM00` - `SM21` Smooths tiszta definíciója), kiegészítve a 2025-ös Tuesday 18h Jazz & Stretch testkondicionáló és nyújtó moduljaival, valamint a legújabb Salsa On2 + Rumba Shines fúziós szálakkal.

---

## 2. Beszélgetés Archívum
### Beszélgetés #2 (2026-06-03)
- **User kérése:** Integráljuk plusz követelményeket: rövidebb jelzők (1-2 karakteres elevated -> `el`), jobban kimondható formák (`long` -> `lon` az `lng` helyett), hibrid CamelCase és Dot rendszer kidolgozása a Dot takarékosabb használatára, a zenei ritmus szintaktika beépítése, valamint a 16 csatolt hagyatéki fájl feldolgozása, köztük a legfrissebb `Course 2025.2. Semester.md` integrálása.
- **Megvalósítás:**
  - Speciális Python ZIP/XML/TXT scriptekkel dekonstruáltuk az összes `.docx`, `.xlsx`, `.pdf` és `.md` állományt plain text-re, majd beolvastuk és elemeztük őket.
  - Kigyűjtöttük az összes ismeretlen kifejezést, különös tekintettel a legújabb talajmunka, jóga és stretching pózokra, valamint az afrokubai rumba-infúziós Salsa On2 kódokra.
  - Kidolgoztuk az **UAA 2.0** és a **Hibrid CamelCase & Dot Case** szabályrendszert.
  - Létrehoztuk és bőségesen kibővítettük az `old_course_essence.md` és `old_course_individual_terms.md` fájlokat, valamint frissítettük a `MASTER_CONCEPT.md` fájlt a 2.0.0-s verzióra.
  - Az ideiglenes segédfájlokat és scripteket letakarítottuk a munkatérből.
