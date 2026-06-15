# BACKLOG_PROMPT_CACHE_KineLex.md — KineLex Local Prompt Cache

Version: 1.1.0 (Deferred Tech Tasks)  
Date: 2026-06-15  
Status: DEFERRED (PR-001 → Phase 4; PR-014 → Phase 4+ haladó)  

> **Protokoll SSoT:** `../DANA/docs/AGENT_PROTOCOL_STANDARD.md`

Ez a fájl a KineLex modul **helyi prompt váróterme (Prompt Cache)**. Ide kerülnek azok a magasabb szintű technikai specifikációk és fejlesztési promptok, amelyek a jelenlegi fázisban még nem végrehajthatóak, megakadályozva a kódoló Agentek túl korai implementációs kísérleteit.

---

## 📌 Aktív Fázis Ellenőrzés
*   **Jelenlegi projektfázis:** **Phase 0 (Bootstrap)** — Lásd: `docs/APP_STATE_KineLex.md`.
*   **Ez a backlog legkorábban itt aktiválható:** **Phase 4 (Succession & Marketplace)**.

*TILOS ezen feladatok kódolásába kezdeni, amíg az `docs/APP_STATE_KineLex.md` szerint a projekt el nem éri a Phase 4-et!*

---

## 📥 Eltárolt Fejlesztői Promptok (Cached Prompts)

### 🔴 PR-001 — KineLex Szakmai Utódlás és Roster Tananyagok (D079, D080)
```markdown
[TARGET_AGENT: KineLex 1_Agent_Development_Architect]
[DEPENDENCY: Phase 3 Completion in KineLex]
[SPEC_LINK: docs/MASTER_CONCEPT_KineLex.md#Section-18.G]

MŰSZAKI IMPLEMENTÁCIÓS PROMPT:
Kérlek, építsd be a szakmai utódlás pedagógiai hátterét és a dialektus-kompatibilitási motort a KineLex helyi Master Conceptjének 18.G fejezete alapján:

1. Adatbázis sémák (PostgreSQL):
   Hozd létre és futtasd le a migrációkat a `mentor_roster_curriculums`, `roster_curriculum_steps` és `student_roster_progress` táblákhoz a megadott SQL struktúrák szerint.
2. Roster-Tananyagok kezelése:
   - Építs UI felületet a seniorok számára, ahol egyedi mentor-tananyagokat (roster_curriculum) állíthatnak össze a KineLex fogalomtárból.
   - Valósítsd meg a student_roster_progress követését, amely a tanuló K3-as mastery szintjére (18.D) támaszkodik.
3. Dialektus-Metrika kiszámítása (Marketplace-integráció):
   - Fejlessz ki egy Supabase Edge Functiont vagy API végpontot, amely kiszámítja a dialect_compatibility (0-100%) mutatót a jelentkező és az óra fogalomtára között a KineLex Lineage Tree (9.B) származási vonalainak egyezése alapján.
```

### 🔵 PR-014 — Haladó: Pose-Estimation Auto Mozdulat-Primitív & Mozgás-Embedding — Phase 4+ (D084, D074, D026)
```markdown
[TARGET_AGENT: KineLex 1_Agent_Development_Architect]
[DEPENDENCY: Phase 4 Completion (fogalomtár + annotáció + pedagógia stabil)]
[SPEC_LINK: docs/MASTER_CONCEPT_KineLex.md 14. fej. (Vizuális Annotáció), 2. fej. (L0-L4); DANA D084, D074]

MŰSZAKI IMPLEMENTÁCIÓS PROMPT (haladó képesség — a "világ leghaladóbb elvei" kör):
1. Pose-estimation pipeline (MediaPipe/BlazePose vagy hasonló): videóból csontváz-keypontok kinyerése → automatikus mozdulat-primitív (L0-L1) jelölés-javaslat a video_annotations rétegbe.
2. Mozgás-embedding: a kinyert mozgásszekvenciák vektoros reprezentációja (pgvector) → "találj hasonló mozdulatot/figurát" keresés és az "ideális kinetika" eltérés-mérés (14. fej.) gépi támogatása.
3. Ontológia-kötés (D084): a felismert primitívek a kanonikus term_id-khoz illesztve, hogy a MeCat tagjei és a BeatPass skill-gráf (D074) ugyanazt a forrást használják.
FONTOS: csak javaslattevő réteg; a kanonikus fogalom-definíció továbbra is emberi (szakértői) jóváhagyással kerül a fogalomtárba.
```
