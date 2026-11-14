# Sầu riêng 2027

Expense log for the durian orchard (2027 crop), based on [Spendlog](https://github.com/bach2332/spendlog_pwa) with durian icons, theme and categories. Browser keys use the prefix `dr_` and data is stored in `durian2027.json`, so it never touches Spendlog data on the same `github.io` domain.

A fast daily expense logger that runs entirely in the browser as an installable **PWA**. There is no build step and no backend: the app is a single `index.html`, and your data is stored in **your own GitHub Gist**.

- Amounts are in **Vietnamese đồng (VND)**, entered in thousands (type `35` → `35.000 ₫`).
- The app UI and the in-app User Guide are in **Vietnamese**.
- Hosted as a static site on **GitHub Pages**.

## Features

| Area | What it does |
|---|---|
| **Quick entry** | Amount and description on one row, category chips, one-tap date picker. Keyboard flow: Amount → Enter → Description → Enter saves. |
| **History** | Browse by day, week (Mon–Sun) or month, filter by category, tap an entry to edit or delete. |
| **Statistics** | Day / Week / Month / Year views (default Month; Year shows monthly bars). Daily bar chart, per-category breakdown with percentages, category radar chart (one axis per current category with spending in the period; categories with no spending are listed below it), monthly budget progress. Tap a category to see the entries behind its total. |
| **Filter & Review** | Flexible filter (time range, categories, keyword, amount range), pick entries, then open a review table with a grand total. Copy as tab-separated text for Excel or Google Sheets. |
| **CSV import** | Bulk-import entries from a CSV file with a simple format (see below). |
| **Monthly budget** | Set a limit; the progress bar turns amber at 80% and red at 100%. |
| **GitHub Gist storage** | Data syncs to a secret Gist; works across devices with the same token and Gist ID. |
| **Zalo notifications** | Optional, **experimental**: a Zalo bot message for each new or edited entry. |
| **Offline** | The app shell and the last synced data are cached on the device. |
| **User Guide** | Built-in popup with setup instructions (GitHub, Zalo). |

## Quick start

### 1. Deploy to GitHub Pages
1. Create a repository and upload every file from this project to the repository root (`index.html`, `sw.js`, `manifest.webmanifest`, four `.png` icons, `README.md`).
2. Go to **Settings → Pages → Build and deployment → Deploy from a branch**, choose `main` and `/ (root)`.
3. After a minute or two the app is live at `https://<your-username>.github.io/<repo-name>/`.

### 2. Create a GitHub token
1. Open <https://github.com/settings/tokens/new?scopes=gist&description=sau-rieng-2027>.
2. Keep only the **`gist`** scope, choose an expiration, and generate the token.
3. Copy it immediately (it starts with `ghp_` and is shown only once).

This is a **personal access token**, not your GitHub password. Never enter your real password in the app.

### 3. Connect the app
1. Open the app → **Cài đặt** (Settings) → **Kết nối GitHub (GitHub Connection)**.
2. **Mật khẩu (GitHub Token)**: paste the token.
3. **Gist ID**: leave empty the first time and the app creates a new secret Gist. On another device, enter the existing Gist ID.
4. Tap **Kết nối & tải** (Connect & load). The sync button at the top right should turn **green**.

### 4. Install on your phone
- **Android (Chrome):** menu ⋮ → *Install app* / *Add to Home screen*.
- **iPhone (Safari):** Share → *Add to Home Screen*.

## Using the app

### Add an expense
- The cursor starts in **Amount** with a numeric keyboard. Type the amount **in thousands of đồng**; the field shows a fixed `.000 ₫` suffix and a live preview such as `= 35.000 ₫`. Only positive whole numbers are accepted.
- Press Enter to jump to **Description**, press Enter again to save. Focus returns to Amount for the next entry.
- Choose a category with one tap. Typing a description you used before suggests it and auto-selects the category it had last time.
- The date chip defaults to **Hôm nay** (today); tap it to pick another date. Your category and date selection are kept after saving, which is handy for back-filling a day.

### Edit or delete
Tap an entry (in Recent, History or Filter) to open its details.
- **Only the description and the category can be edited.** Date and amount are read-only.
- To correct an amount or date, delete the entry and add it again.
- **Delete never removes data.** It adds a *void* entry with a negative amount and marks the original as voided (shown struck through). Totals cancel out and the full history is preserved.
- Delete confirmation is an in-app dialog (browser `confirm()` can be blocked in embedded viewers).

### Statistics
Switch between day, week and month. Tap any category row to open a popup listing the underlying entries (description, amount, date) and the category total for the selected period. Voided entries and their negative counterparts are included so the list always adds up to the total.

### Filter & Review
Pick a time range (presets: today, this week, this month, last month, this year, or custom dates), categories (none selected = all), a keyword, and an amount range (entered in thousands). Voided entries are hidden unless you tick *show voided*. Tick the entries you want and tap **Review** for a table (date, description, amount) with a total at the bottom. **Copy** produces tab-separated text.

### Settings
Each section is a collapsed row (icon, title, one-line status, ›) like the User Guide row; tap it to expand the details below, tap again to collapse. Status lines: GitHub connected + masked Gist, monthly budget, Zalo on/off, category count, etc. On a device with no GitHub token the app opens Settings with **Thiết lập nhanh** already expanded.
- **Thiết lập nhanh** (quick setup, see below).
- **GitHub connection** and **Log out** (clears the token and local cache from this device; the Gist is untouched).
- **Monthly budget** (in thousands, `0` disables it).
- **Categories** (default: Mua phân, Làm công, Làm cỏ, Phun thuốc, Thu hoạch, Mua cơm, Mua nước, Khác). Deleting a category does not change existing entries.
- **CSV import**.
- **Zalo notifications** (experimental).
- **User Guide** popup, also linked from each connection section.

## CSV import

Header row, UTF-8 (use "CSV UTF-8" when exporting from Excel):

```csv
date,note,category,amount
2026-11-01,Phân NPK 20 bao,Mua phân,"12.500.000"
01/11/2026,"Công làm cỏ, phun thuốc",Làm công,3000000
```

| Column | Required | Notes |
|---|---|---|
| `date` | yes | `YYYY-MM-DD` or `DD/MM/YYYY` |
| `note` | no | Description |
| `category` | no | Unknown categories are created automatically; empty becomes `Khác` |
| `amount` | yes | Positive integer in **full đồng** (not thousands). `35000`, `35.000`, `35,000` and a trailing `đ`/`₫`/`VND` are accepted |

- Delimiter can be `,`, `;` or tab. Vietnamese header aliases (`ngay`, `noi dung`, `danh muc`, `so tien`, …) are recognised.
- The preview shows how many rows will be imported, their total, new categories and any invalid rows (with line numbers, skipped).
- By default rows identical to existing entries (same date, description, category, amount) are skipped, so re-importing a file does not duplicate data.
- A sample file can be downloaded from the import card.
- Importing does not send Zalo notifications.

## Data storage and sync

Data lives in a single file, `durian2027.json`, inside a **secret Gist**:

```json
{
  "categories": ["Mua phân", "Làm công", "Khác"],
  "budget": 5000000,
  "items": [
    { "id": "…", "date": "2026-11-08", "note": "Phân NPK", "category": "Mua phân", "amount": 35000 },
    { "id": "…", "date": "2026-11-08", "note": "Xóa: Phân NPK", "category": "Mua phân", "amount": -35000,
      "type": "void", "ref": "<id of the voided entry>", "at": "2026-11-08T05:40:00.000Z" }
  ]
}
```
Voided originals carry `"voidedBy": "<id of the void entry>"`. Amounts are integer đồng.

Extra fields used for merging (all optional, older files without them still work): `"upd"` on an entry (ISO time of its last edit or void), `"catLog": {"<category>": {"on": true|false, "t": "<ISO time>"}}` (when each category was added or removed) and `"budgetT"` (when the budget was last set).

How syncing works (**pull → merge → push → verify**, the whole file is never blindly overwritten):
1. Every change is applied locally first and cached in `localStorage` (marked pending).
2. The app downloads the current Gist file (`cache: no-store`; files over the API's inline limit are fetched via their raw URL) and **merges** it with the local data:
   - entries: union by `id` (entries are never removed, a delete is an extra negative "void" entry, so nothing can be lost); if both sides changed the same entry, the newer `upd` wins and a `voidedBy` from either side is kept; if two devices voided the same entry, the extra void is turned into a 0 ₫ "Xóa trùng" record so the total stays right;
   - categories: per-category add/remove with timestamps (`catLog`), the newer action wins; a device that has never synced does not contribute its default categories;
   - budget: the newer `budgetT` wins.
3. The merged file is written back, then re-downloaded after ~1 s; if another device wrote in between and something of ours is missing, merge and write again (up to 3 tries). Anything still missing is restored the next time this device syncs, because its local copy keeps every entry.
4. Sync also runs on app start, when the app returns to the foreground, and every 60 s while it is open. These checks send `If-None-Match` with the last ETag, so an unchanged Gist answers `304` with no download and no write.
5. Sync button (top right) with the status text below it: the text shows the last result (green = synced, orange = error), the button syncs now. **Grey** = not connected (tap opens Settings), **spinning** = syncing, **green** = synced, **orange** = error or changes not sent yet (tap to retry). Tapping shows the result as a toast; the full message is also in the button tooltip.
- Connecting to a different Gist (or applying a config text with another Gist ID) clears the local copy first, so two ledgers are never mixed.

## Zalo notifications (experimental)

Optional. When both a **Bot token** and a **Chat ID** are saved on a device, the app sends one Zalo message per:
- **new entry** (amount, description, category, date and the day's total), and
- **edit** (only when the description or category actually changed; lists the old → new values).

Nothing is sent when the fields are empty, on delete, or on CSV import.

Setup (per the Zalo Bot Platform documentation):
1. In Zalo, open the official account **Zalo Bot Manager** and choose **Create bot**; the bot name must start with "Bot". Zalo sends you the **bot token**.
2. In Settings → *Thông báo Zalo*, paste the token and tap *mở getUpdates*. The page waits about 30 seconds: send the bot a message on Zalo meanwhile, then find `chat` → `id` in the result.
3. Paste it as **Chat ID**, tap **Lưu**, then **Gửi thử** (send test).

Technical notes and caveats:
- Messages use emoji labels to stay short:
  ```
  🌳 Chi mới
  💸 12.500.000 ₫ · Phân NPK 20 bao
  🏷️ Mua phân · 📅 12/11
  👤 Chị Lan
  📊 Hôm nay: 13.250.000 ₫
  ```
  Edits: `✏️ Sửa khoản chi` / `💸 amount · 📅 date` / `📝 old → new` / `🏷️ old → new` / `👤 user`. Test: `✅ Sầu riêng 2027 · Zalo OK` / `👤 user`.
- Calls go from the browser to `https://bot-api.zaloplatforms.com/bot<TOKEN>/sendMessage` as a form-encoded request with `mode: 'no-cors'`, so messages go out even if the API does not send CORS headers. The trade-off is that regular sends cannot read the response; only the test button tries to.
- Each notification is sent once, with no automatic retry (to avoid duplicates or late messages).
- The token and Chat ID are stored only in this browser (`localStorage`), per device.
- **Not verified against the real Zalo service.** Undocumented points: CORS behaviour, rate limits, time limits or prerequisites for a bot to message a user, and whether a bot can post to group chats. Treat the feature as experimental.

## Current user

- **Người đang sử dụng**: a display name stored on the device (`dr_who`), shown in the header and saved as `by` on every new entry, delete record and CSV import row; it also appears in Zalo messages. It is set only by the quick-setup text (`"n"`); there is no input for it. It is not shown in Settings; the header shows it ("Chưa thiết lập người dùng" until set).

## Weather

A one-line strip under the title shows current weather (temperature, condition, today's rain chance, humidity); tap it to expand location, wind and a 3-day outlook (tap again to collapse, state remembered). Data from [Open-Meteo](https://open-meteo.com/) (free, no API key). Location is set only by the quick-setup text (`"l"`); it is not shown in Settings. Until set, the strip says "Chưa thiết lập vị trí vườn" and tapping it opens Settings. Cached 30 minutes in `dr_wx`; expanding refreshes if stale.

## Quick setup (encrypted config text)

Settings → **Thiết lập nhanh** has two buttons and no text box on the page:

- **📥 Thiết lập nhanh** opens a dialog with a text box: paste the config text, tap **Tiếp**, enter the shared password, confirm the ☠️ override warning.
- **📋 Sao chép thiết lập hiện tại** asks for the shared password and copies this device's config (encrypted; GitHub, Zalo, user name, location) to the clipboard; if the browser blocks it, a read-only dialog shows the text with a copy button.

- The text is always encrypted: `DR27E1.` + base64url(`salt(16) | iv(12) | AES-256-GCM ciphertext+tag`), key = PBKDF2-SHA256(password, salt, 310,000 iterations). Plain text is rejected.
- Inside (JSON): `{"app":"dr27","t":token,"g":Gist ID,"zt":Zalo bot token,"zc":Zalo chat ID,"n":user name,"l":[place,lat,lon]}` — every key except `app` is optional. Categories and budget are **not** in the text: they live in the Gist and load automatically (`"c"`/`"b"` in older texts are ignored, so an old text can never overwrite the group's categories or budget).
- **One shared password** for the whole app. It is never stored in clear text: the code only holds its salted PBKDF2-SHA256 hash (`PW_HASH`) to check what is typed; nothing password-related is stored on the device. Opening Settings needs no password.
- Wrong password → "Sai mật khẩu"; truncated/corrupted text → asked to paste again.
- **Setup link:** `https://bach2332.github.io/durian_spendlog_pwa/#cfg=DR27E1.…` opens Settings with the quick-setup dialog pre-filled (read-only); the user taps **Tiếp**, enters the shared password and confirms the override warning as usual. The string sits after `#`, so it is never sent to the server; the app removes it from the address bar right away and keeps it only in `sessionStorage` until the dialog is answered (so an automatic reload after an app update does not lose it). Works whether the link opens a new tab or the app's existing tab (`hashchange`). Opened inside Zalo/Facebook's in-app browser, the app first explains that the setup would stay inside Zalo and suggests "Mở bằng trình duyệt". Note: on iPhone, a home-screen app has its own storage separate from Safari.
- Override warning: after tapping Tiếp and entering the password, a ☠️ dialog warns that everything will be overwritten (no config details shown) and the "☠️ Ghi đè" button unlocks after a 3-second countdown.
- Secrets are never shown in full anywhere in the UI (`mask()`: first 2–4 and last 2 characters): the Settings fields (saved values appear only as a masked placeholder; leave a field empty to keep the saved value), and the Gist / getUpdates links (URLs are built only on click, not put in the page).

### Config builder (Settings → 🛠️ Tạo chuỗi cấu hình)

A separate tool for preparing config text and setup links for *other* devices; it never changes this device's config.

1. Type the shared password (checked against `PW_HASH`; not stored anywhere).
2. Optional: paste an existing `DR27E1.…` string or a full setup link and tap **🔓 Giải mã** to fill the fields.
3. Fill or edit: user name, GitHub token, Gist ID, Zalo bot token + chat ID, orchard location (**🔎 Tìm** looks up coordinates; default is the Trùng Dương orchard, Pơng Drang, Đắk Lắk).
4. **🔒 Mã hóa** shows the encrypted string and the setup link (`<app URL>#cfg=…`) with copy buttons.

Token, Gist ID and Zalo fields are password-type inputs (tap 👁 to reveal). The password and every field are cleared when the section is closed, when **🧹 Xóa hết** is tapped, or when the app goes to the background.

## Updating and versioning

- The footer shows `🌳 Sầu riêng 2027 · vYYYYMMDD`. Change that date in `index.html` when you release so you can tell whether a device has the new build.
- `sw.js` has a `VERSION` constant. **Bump it only when `sw.js`, `manifest.webmanifest` or the icons change** (they are precached). Editing `index.html` alone does not require it.
- The service worker loads `index.html` from the network first (bypassing the HTTP cache) and falls back to the cached copy offline. When a new service worker takes over, the page reloads once automatically.
- Requests to other origins (GitHub API, Zalo) bypass the service worker.
- To see a new deploy: wait for the **Actions** run of "pages build and deployment" to finish (usually 1–2 minutes), fully close the app and reopen it online. If a device is stuck, hard-refresh or open the URL with `?v=2`. Avoid "clear site data": it also removes the saved token.

## Project structure

```
index.html              the whole app (HTML, CSS, vanilla JS)
sw.js                   service worker (offline shell, update behaviour)
manifest.webmanifest    PWA manifest
icon-192.png            PWA icon
icon-512.png            PWA icon (required by the service worker precache)
icon-maskable-512.png   maskable PWA icon
apple-touch-icon.png    iOS home-screen icon
README.md               this file
```
All files sit in one folder so the app works from any sub-path such as `/<repo-name>/`.

## Security and privacy

- The GitHub token and Gist ID are stored in the browser's `localStorage` on each device. Anyone with access to an unlocked device can use them. Use a token with **only the `gist` scope**, and revoke it at <https://github.com/settings/tokens> if a device is lost.
- Never commit a token to the repository. This repo is public; it contains only the app code, never your data.
- A **secret Gist is unlisted, not private**: anyone who has its URL can read it. It is not encrypted.
- All users of a shared token have the same full access, and the app cannot tell them apart.

## Known limitations

- **Single owner, no identity:** only the Gist owner's token can write, so multiple people must share one token. There is no record of who created or changed an entry.
- **Merge, not locking:** Gists cannot be locked, so two devices writing within the same second may briefly overwrite each other; the verify step and the next sync of the affected device restore the missing entries. Edit conflicts on the same entry use the device clocks (newer wins).
- **Growth:** an entry is roughly 120 bytes of JSON; a single file beyond about 1 MB is no longer returned inline by the Gist API (the app falls back to the raw URL), and every save downloads, merges and rewrites the full file (about 1.6 MB at 10,000 entries). GitHub's authenticated API rate limit is 5,000 requests per hour.
- Whole-đồng integers only; amounts are entered in thousands (CSV import uses full đồng).
- Vietnamese-only UI. Verified in headless Chromium only, not on every real device or browser.

## Ideas under consideration (not implemented)

- **Audit trail** for multi-user use: append-only events recording who created, edited or voided each entry, with before/after values.
- **Moving off Gist** to a hosted database such as Supabase (Postgres, per-user login, row-level security, database-side audit triggers); Firebase and Cloudflare D1 were considered as alternatives.
- **Local-first sync queue** and per-month data files to keep saves fast as data grows.
