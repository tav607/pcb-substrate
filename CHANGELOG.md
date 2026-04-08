# Changelog

PCB / Substrate value chain visualization. Files: `index.html` (EN) and `index-zh.html` (ZH).

---

## 2026-04-08 — Value chain rebuild (optics-basket format)

Rewrote both `index.html` and `index-zh.html` from a "supply chain map" (small flow diagram + 2×2 panel grid with mkt cap / P/E tables) into a focused **linear value chain visualization** in the same visual style as `../optics-basket/optics-basket-viz.html`.

### Stages and companies

| # | Stage | Stage color | Cards | Row layout |
|---|---|---|---|---|
| 1 | ◆ Upstream Cu Foil | purple `#a855f7` | 4 | 2 + 2 |
| 2 | ● CCL · 覆铜板 | emerald `#10b981` | 4 | 2 + 2 |
| 3 | ■ IC Substrate · FC-BGA / BT | cyan `#00d4ff` | 5 | 3 + 2 |
| 4 | ▲ PCB · 主板 | amber `#f59e0b` | 2 | 1 row |

15 listed companies total:

| Stage | Companies (ticker) |
|---|---|
| Cu Foil | JX Advanced Metals (5016.T), Mitsui Kinzoku 三井金属 (5706.T), Furukawa Electric 古河电工 (5801.T), Lotte Energy Materials (020150.KS) |
| CCL | EMC 联茂 (2383.TW), 生益科技 Shengyi (600183.SS), TUC 台燿科技 (6274.TW), ITEQ 台光电 (6213.TW) |
| IC Substrate | Unimicron 欣兴 (3037.TW), Ibiden 揖斐电 (4062.T), Kinsus 景硕 (3189.TW), Samsung Electro-Mechanics SEMCO (009150.KS), Nan Ya PCB 南电 (8046.TW) |
| PCB | 胜宏科技 Shenghong (300476.SZ), WUS Printed Circuit 沪电股份 (002463.SZ) |

### Layout / format

- Reused the optics-basket value chain CSS verbatim: IBM Plex Mono + Inter fonts (Google Fonts), `bg-grid` + `bg-gradient` background, rounded `value-chain-container` card, `chain-stage` blocks with gradient icon squares and colored divider, `chain-company` cards with colored left border + hover translate effect.
- **Flow direction**: vertical (upstream → downstream stacked top to bottom), stages connected by SVG downward arrows (`.chain-arrow`). An earlier draft used 4 columns side-by-side; flipped to vertical because the horizontal layout did not visually convey upstream→downstream.
- **Stage interior**: companies are arranged horizontally inside each stage (`chain-companies` is `flex-direction: row` + `flex-wrap: wrap`), not stacked vertically. The stage header divider is a centered colored bar via `::after` pseudo-element (instead of full-width `border-bottom`) so it works with the wide horizontal-band layout.
- **Forced row counts** via modifier classes on `.chain-companies`:
  - `.cols-2` → `max-width: 760px` (caps to 2 cards/row, used by Cu Foil / CCL / PCB)
  - `.cols-3` → `max-width: 1140px` (caps to 3 cards/row, used by IC Substrate)
  - `flex-wrap` handles overflow → 4 cards become 2+2, 5 cards become 3+2.
- `.chain-company` width: `flex: 1 1 280px; min-width: 260px; max-width: 360px` so cards size themselves based on row count.

### Content rules

- **No tables, no financial data** — market cap / Forward P/E columns removed entirely. Page is a value chain map, not a screener.
- **No glass core substrate path** — earlier draft had a fork (organic vs glass), removed.
- Each card carries only: ticker (colored), company name, one-line role description.
- **Each description must stay scoped to its stage**. Reviewed all 15 cards; cleaned 2:
  - JX Advanced Metals: removed "溅射靶材" / "sputtering targets" and "半导体材料整合供应商" / "integrated semi materials supplier" — these belong upstream of the wafer fab, not in the PCB Cu foil stage.
  - SEMCO: trimmed from 3 hooks (position + NVIDIA + KRW 1T capex) to 2 hooks for parity with other Substrate cards.
- Other potentially-distracting business areas of these companies (Furukawa's electrical infra, Mitsui Kinzoku's zinc/lead/door-locks, Ibiden's ceramic DPF, Unimicron / Nan Ya PCB's regular PCB, Lotte's battery foil) are deliberately not mentioned.

### Player selection

Starting point was 8 user-specified names: kinsus, unimicron, ibiden, jx metal, mitsui kinzoku, 生益, 胜宏, EMC (2383).

Researched obvious major listed players missing per stage and added back 7 with user approval:

- **Cu Foil** added: Furukawa Electric, Lotte Energy Materials
- **CCL** added: TUC (Taiwan Union Tech), ITEQ
- **IC Substrate** added: SEMCO, Nan Ya PCB
- **PCB** added: WUS

**Flagged but not added** (need user reconsideration of "low-exposure conglomerate" exclusion which may be stale as of 2025-2026):
- Doosan Corp (000150.KS) — its Electro-Materials BG is the Korean CCL leader, reportedly a sole-source CCL supplier for an NVIDIA next-gen AI chip; tripling CCL capex in 2025.
- LG Innotek (011070.KS) — FC-BGA utilization 80%+, building "Dream Factory", USD 700M FC-BGA business target by 2030.

**Confirmed correctly absent**:
- Shinko Electric Industries (6967.T) — historically top-3 substrate; **delisted 2025-06-06** via JIC / DBJ / Mitsui take-private.

**Other previously-excluded names** (still excluded per user's "low-exposure conglomerate" rule): Nan Ya Plastics (1303.TW — distinct from Nan Ya PCB 8046.TW), China Jushi, Asahi Kasei, Panasonic, Ajinomoto, Corning, DNP.

### Files touched

- `index.html` — English version, full rewrite
- `index-zh.html` — Chinese version, full rewrite
- `supply-chain.html` — **left untouched** (untracked draft, older content with Kingboard Holdings still present which was removed from the live files in commit `3b0e3dd`). Likely safe to delete.

### Reference

Visual reference: `/home/paladinllq/projects/ai-semi-research/optics-basket/optics-basket-viz.html` (and `optics-basket-viz-en.html`). The PCB value chain copies its `value-chain-container`, `chain-stage`, `chain-company`, `chain-stage-header` / `-icon` / `-title` / `-subtitle`, and `chain-companies` CSS — only difference is no fork/merge connectors (linear instead) and `chain-companies` flips from column to row.
