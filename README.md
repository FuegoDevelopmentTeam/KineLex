# DANCE — SaaS & Curriculum Workstation
Empowering dance educators and choreographers through a structured physical notation language, Abstract Syntax Trees (AST), dynamic scaffolding, and 3D humanoid simulation.

---

## 🚀 Getting Started

### 1. Prerequisities
Make sure you have **Node.js** (v18+) and **PostgreSQL** installed and running on your local machine.

### 2. Install Dependencies
Run the package installer from the root directory to set up Next.js, React, Tailwind CSS, TipTap, and Three.js:
```bash
npm install
```

### 3. Database Setup (PostgreSQL)
1. Create a local PostgreSQL database named `dance_db`:
   ```sql
   CREATE DATABASE dance_db;
   ```
2. Initialize the tables, indexes, constraints, and Row-Level Security (RLS) rules by executing the schema script:
   ```bash
   psql -d dance_db -f db/schema.sql
   ```

### 4. Running the Legacy Ingestion Pipeline
To import the 621 elavult (legacy) abbreviations from `Abbreviations.json` into your local PostgreSQL tables, resolving categories, parent-child hierarchies, synonyms, and antonyms in a smart two-pass process:
```bash
npx ts-node db/migrations/migrate_legacy.ts
```

### 5. Running the Development Server
Launch the Next.js development server to start building:
```bash
npm run dev
```
Open [http://localhost:3000](http://localhost:3000) in your browser to inspect the **Interactive Curriculum Editor & Notation AST Parser** in real-time!

---

## 📁 Repository Structure
- `db/schema.sql`: Complete PostgreSQL production DDL database schema with index optimizations.
- `db/migrations/migrate_legacy.ts`: Fully automated hibrid ingestion pipeline to migrate legacy data with parent hierarchy and category mapping.
- `src/lib/db.ts`: Database connection wrapper with global PgPool optimizations for serverless environments.
- `src/lib/parser.ts`: The Abstract Syntax Tree (AST) dance notation parser, tokenizer, and dictionary linter with spell-checking and DDR expansions.
- `src/pages/index.tsx`: High-fidelity interactive UI console to test parsing, linting diagnostics, scaffolding, and pipeline details.
- `MASTER_CONCEPT.md`: The central "Source of Truth" detailing UAA rules, CamelCase/Dot Case syntax, and long-term features (Git-for-Dance, 3D simulation).
