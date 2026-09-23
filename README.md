# CT Web Solutions — Rust prediction bot

Demo-only prediction-market research for 15-minute crypto contracts. The managed execution path uses durable order/fill accounting, side-specific confidence, conservative fair values, exposure limits, and fresh-quote checks. This repository is the Rust bot behind the results below; the separately published Python research beta is a different project.

## Recorded demo results

Snapshot: September 22, 2026, 10:36 p.m. CDT (September 23, 03:36 UTC).

| Sample | Wins / losses | Win rate | Net after recorded fees |
|---|---:|---:|---:|
| All 184 automated settlements | 149 / 35 | 81.0% | +$66.51 |
| Revised strategy, 92 settlements | 82 / 10 | 89.1% | +$43.71 |
| Latest 30 settlements | 26 / 4 | 86.7% | +$14.49 |

These are **demo exchange results**, not live-money returns. They do not demonstrate future profitability. Read [RESULTS.md](RESULTS.md) for exact amounts, cohort definitions, limitations, and downloadable settlement data.

## Run locally

Requires Rust/Cargo and your own Kalshi demo credentials. Run from the repository root. No credentials are distributed.

1. Create a local `.env` with `KALSHI_MODE=demo`, `KALSHI_DATA_MODE=demo`, `KALSHI_PAPER=0`, `KALSHI_API_KEY=your_demo_key_id`, and `KALSHI_PRIVATE_KEY_PATH=/absolute/path/to/your/demo-private-key.pem`.
2. Run `cargo build --release`.
3. Run `DASHBOARD_PORT=5150 cargo run --release -- --config config.yaml`.

Starting this command enables the configured bounded **demo** strategy; it can place demo exchange orders. Do not run multiple copies against the same account. Production mode is rejected by this deployment. The older simulation/strategy modules remain in the tree but are not the source of the reported demo results.

The local status endpoint is `http://127.0.0.1:5150/api/execution`. Runtime accounting persists under `logs/`; keep that directory across restarts. Never publish your `.env`, PEM key, or private runtime ledger.

## Relevant code

- `src/execution/runtime.rs`: strategy loop, audit records, quote-age checks.
- `src/signals/fair_value.rs`: probabilities, side confidence, conservative pricing.
- `src/execution/managed.rs`: order lifecycle, reconciliation, risk gates.
- `src/execution/ledger.rs`: durable fills, positions, settlement accounting.
- `tests/managed_execution.rs`: execution and accounting regression tests.

Validation: `cargo test --lib --test managed_execution --test test_signals`.

Independent educational project by CT Web Solutions; not affiliated with Kalshi. Demo liquidity, fills, and market behavior may differ from production. The separate perpetuals pilot is not included in this source release or in prediction performance.
