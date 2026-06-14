# APP_STATE_KineLex.md — KineLex Active Project State

Version: 0.1.0  
Date: 2026-06-14  
Current Phase: **Phase 0: Bootstrap & Core Schema**  
Status: IN_PROGRESS (Not started yet)  

Ez a fájl a KineLex modul **aktív rendszer- és fejlesztési állapotát (Source of Truth)** rögzíti. Minden ide belépő Agentnek kötelező ezt elsőként beolvasnia, hogy lássa, hol tart a projekt, és mik a soron következő lépések.

---

## 🧭 1. Jelenlegi Fázis és Mérföldkövek (Phase & Milestones)

A KineLex jelenleg a **Phase 0** (legelső lépések) kapujában áll. Bár a táncelméleti koncepció és nyelvtan (v4.3.0) rendkívül részletes, a szoftveres implementáció a legelső lépéseknél tart.

```
[►] Phase 0: Bootstrap & Core Schema  <-- JELENLEG AKTÍV
[ ] Phase 1: Basic Operations & CRUD
[ ] Phase 2: Automated & IoT Integration
[ ] Phase 3: Financial & Revenue Engine
[ ] Phase 4: Succession & Marketplace (PR-001 Deferred)
```

---

## 🎯 2. Aktív Backlog (Soron következő feladatok - Next Up)

Mielőtt bármilyen komplex elméleti logikát (pl. ICCR, szótár-refaktorizáció, utódlási tananyag) kódolnánk, a szoftveres alapot kell lehelyezni.

### Task 0.1: Repozitórium Inicializálása & Könyvtárszerkezet
*   **Státusz:** `[PENDING]`
*   **Leírás:** Hozd létre a KineLex webalkalmazás alapvető Next.js / Supabase könyvtárszerkezetét.
*   **Kimenet:** `/app` mappa Next.js boilerplate-tel, `/supabase` mappa konfigurációval.

### Task 0.2: Adatbázis Inicializálása (Core Schema migrations)
*   **Státusz:** `[PENDING]`
*   **Leírás:** Hozd létre az alapvető Supabase PostgreSQL táblákat (pl. `terms` a szókincshez, `users` auth, `student_skill_matrix`).
*   **Kimenet:** Első Supabase migrációs fájlok (`0001_initial_schema.sql`).

### Task 0.3: Környezeti változók & Auth konfiguráció
*   **Státusz:** `[PENDING]`
*   **Leírás:** Állítsd be a Next.js és Supabase összeköttetést (.env.local), és konfiguráld az alapvető autentikációt.

---

## 🧊 3. Elnapolt / Jövőbeli Feladatok (Deferred Backlog)

A magasabb fázisok specifikációit és promptjait biztonságosan eltároltuk az alábbi helyen, hogy ne terheljük a jelenlegi kódoló Agentet:
*   `docs/BACKLOG_PROMPT_CACHE_KineLex.md` (Succession & Marketplace - PR-001)

---

## 📜 4. Fejlesztési Napló (Changelog)
*   **2026-06-14 (v0.1.0):** Első rendszerállapot leírás és fázis-kapu modell definiálása. (Szőnyi Levente / KineLex Domain Expert)
