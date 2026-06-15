# [CROSS-MODULE DELEGATION] MeCat → KineLex

**Feladó:** DANA Master Concept Builder / MeCat Domain Expert  
**Dátum:** 2026-06-15  
**Státusz:** OPEN — válasz / szerződés-tervezet szükséges a KineLex Tech Lead-től (`1_Agent_Development_Architect`)  
**DANA hivatkozások:** D084 (Megosztott Ontológia), D088 (óraterv→playlist), D093 (CUE-adatbázis), P45 (Data Contracts)

---

## Kontextus

A MeCat a DANA **Média & Tartalom Sík** kanonikus modulja. A zenei tag-taxonómia tánc-fogalmai **nem duplikálhatók** — a KineLex fogalomtár az egyetlen forrás (`term_id`). A MeCat a `docs/MeCat_Music_Tags.md` hierarchikus tag-rendszerét ehhez köti.

---

## Kért deliverable-ek (prioritás szerint)

### 1. Verziózott Ontológia-Export API (P45) — **P0 / Phase 3 előtt kötelező**
*   **Cél:** A MeCat `media_tags.term_id` mezője hivatkozhasson kanonikus KineLex fogalmakra.
*   **Export tartalom (minimum):**
    *   Tánc/zene **műfaj** (`genre`), **alműfaj** (`subgenre`), **stílus/időzítés** (`dance_style_timing`: on1/on2…), **regionális eredet** (`origin`).
    *   **Pedagógiai usage** fogalmak, amelyek KineLex-ben már léteznek (pl. footwork, partnerwork, shine).
    *   **Musicality/CUE** kapcsolódó fogalmak (lásd §3).
*   **Szerződés:** verziózott JSON/GraphQL snapshot + `ontology_version` string; breaking change esetén semver + migration note.
*   **MeCat oldali fogyasztás:** tag-gelő motor csak exportált `term_id`-t írhat `genre`/`subgenre`/`usage` kulcsokhoz.

### 2. MAP / Musicality fogalom-harmonizáció — **P1**
*   A MeCat **MAP** (nyolcas térkép) és **szerkezet-szegmens** jelölései (intro/verse/bridge/break…) igazodjanak a KineLex **ritmikai szintaktikához** (Hit, Span, Structural Anchoring — lásd `MASTER_CONCEPT_KineLex.md` §6).
*   **Kimenet:** mapping tábla: MeCat `structure_segment.type` ↔ KineLex idővonal-marker (pl. `i`, `v`, `ref`, `B`, `E`, `S@`).
*   **Cél:** az óraterv (D026) zenei sávja és a mozdulat-idővonal ugyanazon „nyelven" beszéljen.

### 3. CUE-pont ↔ Mozdulat-fogalom kötés (D093) — **P1 / Phase 5**
*   A MeCat **CUE-adatbázis** egyes CUE-pontjai hivatkozhatnak KineLex **mozdulat- vagy musicality-fogalmakra** (`term_id`), pl. „break@bar33 → shine prompt", „bridge soft → close-embrace transition".
*   **Kért API:** CUE olvasás/írás során opcionális `linked_term_ids[]` mező validálása a KineLex ontológia ellen.
*   **Visszacsatolás MeCat→KineLex:** aggregált statisztika — mely zenei CUE-k párosulnak leggyakrabban mely mozdulatokkal (Golden Data a pedagógiai ontológiához).

### 4. Óraterv zenei slot-séma (D088) — **P0 / Phase 5**
*   A KineLex óraterv entitásban definiálandó **zenei slot** mezők, amelyeket a MeCat illesztőmotor olvas:
    *   `lesson_segment_id`, `pedagogical_goal` (term_id), `target_energy_arc`, `required_tags[]`, `forbidden_tags[]`, `target_bpm_range`, `duration_hint_sec`.
*   **Esemény (P44):** `LessonPlanPublished` / `LessonPlanUpdated` — MeCat feliratkozik és generál/ajánl playlistet.

---

## Elfogadási kritériumok

1. A MeCat Phase 3 taggelő motor **nem hoz létre** saját tánc-műfaj neveket — csak KineLex `term_id`-t.
2. Az ontológia-export **visszafelé kompatibilis** minor verzióknál.
3. A CUE↔fogalom kötés opcionális, de ha megadva, invalid `term_id` elutasítandó.

---

## Kapcsolattartó oldal (MeCat)

*   Spec: `../MeCat/docs/MASTER_CONCEPT_MeCat.md` §5, §6 (CUE), §10  
*   Tag-taxonomia: `../MeCat/docs/MeCat_Music_Tags.md`  
*   MeCat Tech Lead: `1_Agent_Development_Architect` (MeCat)
