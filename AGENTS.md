# Codex implementation guide

Goal: refactor this working v5 baseline into a transparent, testable shopping optimizer without changing the user-visible accounting convention.

## Priorities

1. **Correctness before UI features**. Extract a pure `calculateOrder` function from DOM-coupled code. All money in integer cents (fen).
2. Ensure `price` INCLUDES prepaid deposit. `depositFace` is the amount credited against total; `depositPaid` is the real sunk payment. Validate `0 <= depositPaid <= depositFace <= price`. Do not double count deposits or deposit discounts.
3. Implement explicit eligibility and discount order: product/store discount; member discount; livestream discount; category vouchers; consumption vouchers; red packets. Order/eligibility MUST be configurable and labeled as an assumption, not an official platform claim.
4. Build a **real comparison** of exactly 1 order vs exactly 2 nonempty orders, with shared coupon inventory and red packets each used at most once. Show per-order items, voucher names, packet IDs, discount amounts, deposits and total cash paid.
5. Remove arbitrary 16-packet restriction where possible; use an exact dynamic programming or ILP approach for supported input sizes. If truncated, disclose bound and candidate status.
6. Add deterministic tests for: price 199 vs 226 replacement; ¥599 fashion threshold before/after a ¥20 upfront discount; ¥620 total with ¥50 face deposit and ¥30 paid; zero-deposit items; coupon mutual exclusion; 10 packet cap; 20 packets split across two orders; no duplicate coupon/packet use; 1 vs 2 orders.
7. Keep JSON import/export backward-compatible with key `d11quant-v2` and old data shapes. No hidden external requests.

## Definition of done

- `npm test` passes with meaningful arithmetic assertions.
- Running identical inputs gives identical results.
- An early-stopped search is never labeled proven global optimum.
- Fixed-plan stress test shows which exact coupons pass/fail per rule.
- UI lets user edit any product, price, category, nominal deposit, paid deposit, and red packet.
- README explains every calculation and limitation in Chinese.

Do not assume partial-refund coupon restoration is permitted; model only if explicitly user-verified.
