# ORE V0.2 Native Real 002 — First Red Result

**Case ID:** ORE-V0.2-NATIVE-REAL-002  
**Upstream:** OlaGreat/NovaSupport  
**Issue:** #1120 — Weekly digest "New supporters" count includes addresses whose only transaction that week failed  
**Frozen upstream/base:** `4d27890c18b6c961422a5676b366ae9b3240458d`  
**First-Red test commit:** `35200e425d3eebfd414727f948be1b27e9e79969`  
**Runner environment repair head:** `43fc5e145424f4a5339285c26cf8a13e211ec36d`

## Valid First Red

GitHub Actions run: **35932665395**  
Job: **107422596686**  
Workflow: **ORE Native Real 002 First Red**  
Conclusion: **FAILURE — VALID FIRST RED**

Environment prerequisites completed successfully:
- checkout: PASS
- Node setup: PASS
- dependency installation: PASS
- Prisma client generation: PASS

The target weekly-digest test file then executed.

Result:
- tests: 10
- pass: 9
- fail: 1
- failed test: `issue #1120: unique supporter query excludes failed transactions`

Observed assertion:
- expected `where.status`: `{ not: "failed" }`
- actual `where.status`: `undefined`
- error: `unique supporter query must exclude failed transactions`

This directly reproduces the reported defect at the query-contract boundary.

## Earlier environmental failure preserved separately

Run **35932115757** is NOT the issue First Red. It failed before test execution because Prisma required `DATABASE_URL`. That runner/environment failure remains historical evidence and is not reclassified as the product defect.

## Frozen repair boundary

The next repair, if authorized, is limited to the smallest source change needed so the `uniqueSupporters` query in `backend/src/services/weekly-digest.ts` excludes failed transactions consistently with the other weekly-digest queries.

Expected minimal change:
- add `status: { not: "failed" }` to the `where` clause of the `uniqueSupporters` query.

Forbidden during this repair:
- no weakening or deletion of the First-Red test;
- no unrelated production changes;
- no broad refactor;
- no alteration of expected truth after observing the failure;
- no upstream PR or merge;
- no claim beyond the tested scope.

## Current disposition

**FIRST RED PRESERVED. MINIMAL REPAIR NOT YET APPLIED.**

A later green regression would establish only the scope actually tested. It would not by itself establish full repository health, production readiness, upstream acceptance, or general ORQELON superiority.
