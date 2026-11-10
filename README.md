# Sầu riêng 2027 (Durian 2027 ledger)

A mobile-first PWA for tracking **expenses and revenue** of a durian orchard for the 2027 crop. It is a single `index.html` (vanilla JS, no build step) hosted on GitHub Pages, and your data is stored in **your own GitHub Gist**. The UI is in Vietnamese; amounts are in VND.

Derived from [Spendlog](https://github.com/bach2332/spendlog_pwa), with revenue tracking added.

## Features

- **Quick entry** with a **Chi (expense) / Thu (revenue)** switch. Amount is typed in thousands (`12500` → `12.500.000 ₫`).
- **Durian-specific categories** (editable in Settings)
  - Expense: Phân bón, Thuốc BVTV, Nhân công, Điện nước tưới, Xăng dầu & vận chuyển, Vật tư & dụng cụ, Xử lý ra hoa, Cải tạo đất, Chi phí khác.
  - Revenue groups: Sầu riêng loại 1, loại 2, loại 3 / xô, Thu khác.
- **Profit / loss** (Thu − Chi) shown for today, the selected day/week/month, and the whole 2027 crop (from 01/01/2027).
- **History** by day, week, month with per-day net totals. Tap an entry to edit its note and category or delete it.
- **Statistics**: revenue vs expense bars per day, expense by category, revenue by group; tap a row to see the entries behind it.
- **Filter & Review**: filter by period, type (revenue/expense), category, keyword and amount; review table with profit/loss; copy as tab-separated text for Excel.
- **Monthly expense budget** with progress bar.
- **CSV import** and offline support.
- Delete never removes data: it appends a negative *void* entry. Only note and category are editable.

## Setup

1. **Create a GitHub repo** and upload all files in this folder to its root. Enable **Settings → Pages → Deploy from a branch → `main` / root**.
2. Open `https://<user>.github.io/<repo>/`.
3. Create a token with only the **`gist`** scope: <https://github.com/settings/tokens/new?scopes=gist&description=sau-rieng-2027>.
4. In the app: **Cài đặt → Kết nối GitHub**, paste User ID and token, leave Gist ID empty the first time, tap **Kết nối & tải**. On other devices, enter the same token and the existing Gist ID.
5. Install to the home screen (Android Chrome: ⋮ → Install app; iPhone Safari: Share → Add to Home Screen).

The in-app **Hướng dẫn sử dụng** popup (Cài đặt) has the same steps in Vietnamese.

## CSV import

Header `date,type,note,category,amount` (UTF-8; `,` `;` or tab):

```csv
date,type,note,category,amount
2027-03-01,chi,Phân NPK 20 bao,Phân bón,"12.500.000"
01/03/2027,thu,Bán lô 1 cho thương lái,Sầu riêng loại 1,85000000
```

- `type`: `chi` (default if empty) or `thu`. `date`: `YYYY-MM-DD` or `DD/MM/YYYY`. `amount`: positive integer in **full đồng**.
- Unknown categories/groups are created automatically. Rows identical to existing entries are skipped by default.

## Data

Stored as `durian2027.json` in a secret Gist:

```json
{ "categories": ["Phân bón"], "incomeCats": ["Sầu riêng loại 1"], "budget": 0,
  "items": [ { "id": "…", "date": "2027-03-01", "note": "Phân NPK", "category": "Phân bón", "amount": 12500000 },
             { "id": "…", "date": "2027-03-05", "note": "Bán lô 1", "category": "Sầu riêng loại 1", "amount": 85000000, "dir": "in" } ] }
```

Revenue entries carry `"dir": "in"`; void entries carry `"type": "void"`, `"ref"` and the same `dir` as the original.

Browser keys use the prefix `dr_`, so this app does not clash with Spendlog on the same `github.io` domain.

## Updating

- Change the footer `v20261111` in `index.html` per release.
- Bump `VERSION` in `sw.js` only when `sw.js`, `manifest.webmanifest` or icons change.

## Security and limits

- The token lives in the browser's `localStorage`; use a `gist`-only token and never commit it. A secret Gist is unlisted, not encrypted.
- Last write wins if two devices save at the same time.
- Not tracked yet: weight (kg), price per kg, per-tree/plot costs, who entered each record. Ideas for a later version.
