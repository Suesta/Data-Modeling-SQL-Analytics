# PostgreSQL Data Modeling & SQL Analytics

End-to-end mini-project that goes from **ER modeling → logical schema → physical SQL** in PostgreSQL. It demonstrates clean schema design, **PK/AK/FK integrity**, basic **transactions** (INSERT/UPDATE/DELETE + ROLLBACK) and **10 analytic queries** with clear results and explanations.

---

## Why this repo may interest you

* **Relational modeling** with traceable evolution (conceptual → logical).
* **Production-style DDL**: explicit PKs, FKs, Alternate Keys, domains.
* **Readable SQL**: consistent aliases, JOIN … ON, safe filtering, HAVING.
* **Evidence-driven**: each step is backed by screenshots / counts.
* **Compact & reproducible**: a single schema (`dbo`) with 3 core tables + 2 reference catalogs.

---

## Repository structure

```
.
├── README.md
├── LICENSE
├── BDA_PR2_Suesta_Arribas_Victor.pdf          # Full report (ER, logical schema, queries, evidence)
└── M2.888_PR2_Enunciado.pdf                    # Original assignment brief (context)
```

> Tip: The **“Appendix”** section inside the PDF contains the full-size screenshots for each query (Q1–Q10).

---

## Data model (summary)

* **dbo.dgt_type**: DGT catalog (vehicle **type/subtype**).
  `PK(cod_subtype_dgt)`; attributes: `cod_type_dgt, des_type_dgt, des_subtype_dgt`.

* **dbo.municipalities**: Spanish municipalities.
  `PK(cod_geo)`; attributes: `name_muni, province, population_muni, altitude, …`.

* **dbo.vehicles**: Registered vehicles.
  `PK(id)`; `FK(municipality) → municipalities(cod_geo)`,
  `FK(subtype) → dgt_type(cod_subtype_dgt)`; domain `new_used ∈ {'N','U'}`.

> The conceptual model also proposes **OWNERSHIP**, **DRIVER** and **FINE**. For the physical part of this assignment the loaded tables are the three above plus the DGT catalog.

---

## What’s implemented

### 1) Transactions (controlled changes on the catalog)

* **Q4**: `INSERT` a synthetic subtype `Z9` + verify.
* **Q5**: `UPDATE` its description + verify.
* **Q6**: `DELETE` `Z9` + verify + **ROLLBACK** to leave the DB unchanged.

### 2) Analytics (Q1–Q3, Q7–Q10)

* **Q1**: DGT subtypes (ordered).
* **Q2**: Number of municipalities in **Sevilla**.
* **Q3**: Municipalities of Sevilla starting with **A** or **U** (case-insensitive).
* **Q7**: **Tourism** vehicles in municipalities with **altitude > 950 m** (brand/model + join).
* **Q8**: Vehicles **per municipality** (COUNT + GROUP BY).
* **Q9**: Municipalities with **< 15,000 vehicles** (GROUP BY + HAVING).
* **Q10**: Vehicles by **DGT type × new/used** (two-dimensional aggregation).

The SQL for each query is shown in the report (Sec. 3.2) together with the result preview and a short explanation.

---

## How to run locally

1. **Create schema**

   ```sql
   CREATE SCHEMA IF NOT EXISTS dbo;
   ```

2. **Load tables** (use your client of choice: `psql`, pgAdmin, DBeaver).
   Create and populate:

   * `dbo.dgt_type`
   * `dbo.municipalities`
   * `dbo.vehicles`

3. **Run the queries** Q1–Q10 from the report.
   They assume the table/column names above and standard PostgreSQL.

> The project was executed on PostgreSQL via pgAdmin; any recent version (≥13) should work.

---

## Good practices highlighted

* **JOIN semantics**: `JOIN … ON` for relationships; filters in `WHERE`; join-affecting predicates kept in `ON`.
* **Logical precedence**: explicit parentheses for `AND`/`OR`.
* **Keys & referential integrity**: PKs, FKs and **alternate keys** (civil IDs).
* **Domains**: enumerations such as `new_used ∈ {'N','U'}`.

---

## Skills & tooling

`PostgreSQL · SQL (DDL/DML) · Data Modeling (ER → logical) · Transactions · Aggregations · Joins · HAVING · Documentation`

---

## License

This repository is released under the **MIT License** (see `LICENSE`).

---

## Contact

**Víctor Suesta Arribas** — MSc in Data Science
If you’re interested in the modeling approach or want to discuss SQL design decisions, feel free to reach out via GitHub.
