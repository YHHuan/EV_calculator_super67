# 超六幸運七 EV Calculator
## Expected Value & Kelly Criterion Calculator for Super Six / Lucky Seven Baccarat

A Windows desktop application (Tkinter GUI) that computes **Expected Value (EV)** and **Kelly-criterion bet sizing** for every betting option in the Super Six / Lucky Seven baccarat variant, including all major side bets.  Card-by-card deck tracking keeps the EV estimates updated as the shoe depletes.

---

## Background — Super Six & Lucky Seven Baccarat

Standard baccarat is modified in some casinos with two structural rule changes and several side bets:

| Rule / Side bet | Description |
|-----------------|-------------|
| **Super Six (超六)** | Banker winning with a total of **6** pays only **0.5 : 1** on the standard Banker bet (instead of 0.95 : 1). The dedicated Super Six side bet compensates with a high payout. |
| **Lucky Seven (幸運七)** | A side bet that wins when Banker's winning hand totals exactly **7**. |
| **Super Lucky Seven (超級幸運七)** | Enhanced Lucky Seven variant with modified pay table. |
| **Big Tiger (大老虎)** | Banker wins with **6** using exactly three cards. |
| **Little Tiger (小老虎)** | Banker wins with **7** using exactly two cards (natural 7). |

These rule modifications shift the house edge significantly relative to standard baccarat — this calculator quantifies exactly how much.

---

## Features

### Core calculation
- **Full baccarat rules engine** — natural-hand detection, Player/Banker third-card draw logic, card normalization and validation
- **Deck composition tracking** — EV is recalculated each round based on cards already dealt; accuracy improves as the shoe empties
- **10 bet types evaluated simultaneously**:

| Bet | Chinese |
|-----|---------|
| Player | 閒 |
| Banker | 莊 |
| Tie | 和 |
| Player Pair | 閒對子 |
| Banker Pair | 莊對子 |
| Big Tiger | 大老虎 |
| Little Tiger | 小老虎 |
| Super Six | 超六 |
| Lucky Seven | 幸運七 |
| Super Lucky Seven | 超級幸運七 |

### Bet sizing
- **Kelly criterion** recommendation — fractional Kelly percentage is configurable so you can dial down risk
- Respects a configurable **bankroll cap** (禁用/啟用上限) to prevent over-betting

### Session tracking

| Metric | Description |
|--------|-------------|
| 本局盈虧 | Round P/L |
| 本局退水 | Round rebate earned |
| 累計下注 | Cumulative amount wagered |
| 累計輸贏 | Cumulative net P/L |
| 累計退水 | Cumulative rebate earned |
| 累計預期收益 | Cumulative expected return |

### Workflow
1. Input the dealt cards for the current round (minimum 4 cards required; 5–6 if a third card was drawn for either side)
2. App displays the latest EV for all 10 bet types and the Kelly-sized suggestion
3. Record which bet was placed and the outcome
4. Repeat for each round; deck state updates automatically
5. Hit **洗牌 (Shuffle)** at the start of a new shoe

### Configuration

| Setting | Description |
|---------|-------------|
| 本金 | Starting bankroll |
| 桌號 | Table number (for record keeping) |
| 籌碼 | Chip denomination |
| 退水‰ | Rebate rate (per mille) from the casino |
| 凱利% | Fraction of full Kelly to use (e.g., 25% half-Kelly) |
| 上限 | Enable / disable maximum bet cap |
| 鎖定 | Lock settings during play |

### Export
- **儲存出牌資料** — saves a round-by-round deal log to `出牌紀錄.xlsx`
- **儲存統計紀錄** — saves cumulative stats per bet option to `統計紀錄.xlsx`

---

## How to Run

This is a pre-compiled Windows executable — no Python installation required.

```
20250425 超六幸運七/
├── 20250425 超六幸運七.exe   ← double-click to launch
└── _internal/                ← required runtime files, do not move or delete
```

**Requirements:** Windows 10 / 11 (x64)

> Keep `_internal/` in the same directory as the `.exe`.  Moving the `.exe` alone will break it.

---

## Workflow Example

```
Round 1
  Input cards (Player–Banker–Player–Banker–[Player]–[Banker]):
    閒1: 5    莊1: K    閒2: 3    莊2: 7    閒3: —    莊3: —

  [最新建議] 輸入牌組: 5 K 3 7   勝出: 莊 (7)
  百家樂 Kelly 建議下注: $500

  ┌──────────────────────────────────────┐
  │ 閒   EV: -0.0132  建議: $0           │
  │ 莊   EV: +0.0054  建議: $500   ◄ ✓  │
  │ 幸運七 EV: +0.0821 建議: $120        │
  │ ...                                  │
  └──────────────────────────────────────┘

  [套用建議] 第 1 局  下注 $500  本局盈虧 +$475  本局退水 $5

  累計下注: $500   累計輸贏: +$475   累計退水: $5
```

---

## Technical Details

| Item | Detail |
|------|--------|
| Language | Python 3.13 |
| GUI framework | Tkinter |
| Key libraries | NumPy, Pandas (Excel export), itertools (combination enumeration) |
| Packaging | PyInstaller (one-directory mode) |
| Platform | Windows x64 |

### Architecture (single-file script)

```
BaccaratApp  (Tkinter main window)
├── validate_number / normalize_card / get_card_value   — input & card encoding
├── banker_should_draw                                   — baccarat draw rules
├── compute_metrics                                      — EV & probability engine
│     └── itertools.product over remaining deck combos
├── update_stats_text                                    — live cumulative display
├── on_enter                                             — process one round
├── on_shuffle                                           — reset deck state
├── toggle_lock / toggle_limits                          — UI controls
├── save_records                                         — export 出牌紀錄.xlsx
└── save_stats                                           — export 統計紀錄.xlsx
```

---

## Disclaimer

This software is for **statistical analysis and educational purposes only**.  EV calculations are theoretical estimates based on combinatorial enumeration of remaining deck states.  They do not guarantee any gambling outcome.  Please gamble responsibly and within the laws of your jurisdiction.
