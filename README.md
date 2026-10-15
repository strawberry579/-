# Double 11 Quant Lab v5 — Codex-ready source

This repository contains the **original v5 app** split into editable source files. It is not a rewritten optimizer.

## Run locally

```bash
python3 -m http.server 8000
```

Open http://localhost:8000. Alternatively, open `index.html` directly. No npm dependencies.

## Files

- `index.html`: layout and controls
- `src/styles.css`: design
- `src/app.js`: state, discounts, coupon eligibility, search, stress tests, UI
- `tests/source.test.cjs`: basic structural checks (not correctness proof)
- `AGENTS.md`: instructions for Codex, known bugs, and acceptance criteria

## Source code map

- `initial()`: default products and voucher configuration
- `items()`: normalizes checked products and deposit fields
- `solveModel()`: coupon/partition search and packet allocation
- `packetDP()`: packet allocation subroutine inside `solveModel()`
- `display()`: estimated payment and breakdown
- `auditFixedPlan()`: fixed-plan stress scenarios

## IMPORTANT accounting assumption

**Product unit price is the TOTAL presale price INCLUDING deposit**. `depositFace` is the nominal deposit deducted from remaining balance; `depositPaid` is the amount actually paid. Total customer cost = paid deposit + discounted remaining balance. Do not add deposit on top of unit price.

## Caveats

- v5 can terminate search early; a candidate is not necessarily globally optimal.
- 16-red-packet exact allocation limit may affect two-order comparisons.
- Threshold evaluation and stacking are configurable hypotheses, not verified Taobao policies.
- Currency uses floating-point JS in the original; migrate to integer fen for production.
- Current automated tests only check source structure, not discount correctness.
