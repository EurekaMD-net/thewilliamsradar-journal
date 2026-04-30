---
title: "Williams AO/AC Signal Methodology"
slug: "methodology"
description: "How the Williams Radar detects AO/AC buy signals. Full technical specification: formulas, signal classification, universe definition, and backtested results from 2001–2026 weekly data."
tags: ["methodology", "williams-ao", "williams-ac", "technical-analysis"]
draft: false
---

## The Williams AO/AC Indicator: Technical Specification

The Williams Radar uses two oscillators developed by Bill Williams, described in _Trading Chaos_ (1995) and _New Trading Dimensions_ (1998). This page documents the exact formulas, signal classification, scanning logic, and the backtested base rates we measured before going live.

---

### What Is the Awesome Oscillator (AO)?

The **Awesome Oscillator** measures the difference between short-term and long-term price momentum using the **midpoint** of each bar.

**Formula:**

```
Midpoint = (High + Low) / 2
AO = SMA(5, Midpoint) - SMA(34, Midpoint)
```

Where `SMA(n, x)` is the simple moving average of `x` over the last `n` periods.

**Interpretation:**

- **AO > 0**: short-term momentum stronger than long-term trend — bullish bias.
- **AO < 0**: short-term momentum weaker than long-term — bearish bias.
- **AO crossing zero from below**: potential momentum shift.

---

### What Is the Accelerator Oscillator (AC)?

The **Accelerator Oscillator** measures the rate of change of market driving force. It is derived from the AO.

**Formula:**

```
AC = AO - SMA(5, AO)
```

The AC **leads the AO** — it changes direction before the AO does.

**Interpretation:**

- **AC turning positive while AO rising**: acceleration phase — strongest entry zone.
- **AC turning negative while AO still positive**: deceleration — momentum stalling.
- **Both AO and AC positive and aligned**: full confirmation signal.

---

### Signal Classification

| Signal Type                | Conditions                                                                                                                 | Interpretation                              |
| -------------------------- | -------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------- |
| **S1 — Early Momentum**    | AO crosses above zero, AC turning positive (1–2 bars)                                                                      | Momentum accelerating — watchlist           |
| **S2 — Full Confirmation** | Two consecutive bars with AO > 0 and AC > 0 sustained                                                                      | High-confidence setup — entry consideration |
| **S2-Degraded**            | A ticker that completed the full S2 pattern in recent weeks but the configuration has since expired without follow-through | No active entry — context only              |
| **Pre-Radar**              | AO approaching zero from below with AC positive                                                                            | Pre-signal — monitoring only                |

Signal classification is **not** a buy/sell recommendation. See the [Disclaimer](/about).

---

### Scanning Universe

**255 US-listed equities** across **13 sector ETFs** covering the major segments of the US market: XLU, XLI, XLY, XLV, XLRE, XLK, XLF, XLC, XLB, IBB, XLP, XBI, XLE. The full ETF-to-sector mapping is on the [About](/about) page.

The scanner runs every **Friday after US market close** (18:00 Mexico City time). It fetches weekly OHLCV data from Alpha Vantage, calculates AO and AC per ticker, and classifies matching signals.

Data source: Alpha Vantage — adjusted weekly closes.

---

### Weekly Output

1. **CSV file** — signal table: Ticker, Sector, AO, AC, Signal type, Week label.
2. **Journal entry** — published at `thewilliamsradar.com/w{NN}-{YYYY}` with narrative analysis.
3. **Telegram notification** — delivered immediately after scan completes.

---

### Backtested Base Rates (2001–2026)

Before publishing the radar live, we ran the same AO/AC logic against 25 years of weekly data — 2001 through 2026 — to measure base rates with the same rules we use now. The numbers below are the results of those tests; they are why we publish, and they are what the live record will be measured against.

#### Sector ETF backtest

**338 signals** across 8 sector ETFs, 2001–2026 weekly data:

| Group     | ETFs Tested | Hit Rate (8W) | Avg Return (8W) | Max Drawdown |
| --------- | ----------- | ------------- | --------------- | ------------ |
| Defensive | XLU, XLP    | **70.0 %**    | +2.44 %         | −5.78 %      |
| Cyclical  | XLI, XLY    | 62.1 %        | +2.81 %         | −7.12 %      |
| Energy    | XLE         | 55.4 %        | +2.10 %         | −9.40 %      |

#### Individual ticker backtest

**3,774 S1 signals** across 79 individual tickers (XLU, XLP, XLE, XLI constituents), 2001–2026:

| Sector            | Tickers | Avg Hit Rate (8W) | Avg Return (8W) | Avg Max Drawdown |
| ----------------- | ------- | ----------------- | --------------- | ---------------- |
| XLU (Utilities)   | 20      | **72.7 %**        | +5.07 %         | −4.9 %           |
| XLP (Staples)     | 19      | 65.4 %            | +3.10 %         | −3.2 %           |
| XLI (Industrials) | 21      | 62.7 %            | +4.80 %         | −5.1 %           |
| XLE (Energy)      | 17      | 56.9 %            | +5.40 %         | −9.2 %           |

#### S2 confirmation tightens the cohort

**491 S2 signals** (−87 % vs S1's 3,774) across the same 79-ticker universe. S2 requires two consecutive confirmation bars rather than one, materially shrinking the count of fired entries and concentrating on the higher-quality configurations.

**Key insight from the backtest**: in individual tickers, AO confirmation lags S1 by an average of **10–18 weeks**. The patient investor has 2–4 months of observation before the AO confirms the move — patience is part of the system, not a flaw of it.

---

### Known Limitations of the Backtest

Two limitations we are explicit about:

1. **Survivorship bias** — the universe used for the historical backtest is the _current_ (2026) membership of XLU / XLI / XLP / XLE. Historical results from 2001–2026 therefore ignore every name that was delisted, went bankrupt, or dropped out of its sector ETF in that window. Future versions will reconstruct point-in-time membership.
2. **Lookahead bias (now fixed)** — the original backtests filled the trade at the close of the signal bar — the same bar whose AO/AC was used to decide the signal. The current engine fills at the next bar's open, which is what a live trader can actually do.

The live record published from W17 onward is run under the same engine that produced the corrected backtests above — no retrofit, no edits.

---

### What This Is Not

The Williams Radar does **not**:

- Predict price targets or stop levels.
- Account for position sizing or portfolio construction.
- Incorporate fundamental analysis, earnings, or macro factors.
- Constitute investment advice of any kind.

---

_For questions, reach out via [X/Twitter @fedemoctezuma](https://x.com/fedemoctezuma)._
