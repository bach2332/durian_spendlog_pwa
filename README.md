# Sầu riêng 2027

Expense log for the durian orchard (2027 crop), based on [Spendlog](https://github.com/bach2332/spendlog_pwa) with durian icons, theme and categories. Browser keys use the prefix `dr_` and data is stored in `durian2027.json`, so it never touches Spendlog data on the same `github.io` domain.

A fast daily expense logger that runs entirely in the browser as an installable **PWA**. There is no build step and no backend: the app is a single `index.html`, and your data is stored in **your own GitHub Gist**.

- Amounts are in **Vietnamese đồng (VND)**, entered in thousands (type `35` → `35.000 ₫`).
- The app UI and the in-app User Guide are in **Vietnamese**.
- Hosted as a static site on **GitHub Pages**.

## Features

| Area | What it does |
|---|---|
| **Two orchards** | Every entry belongs to **Vườn lớn** (`g:"L"`, green), **Vườn nhỏ** (`g:"N"`, orange) or **Rẫy cà phê** (`g:"C"`, brown). History and Statistics share one selector **Tất cả / Lớn / Nhỏ / Cà phê** (default Tất cả). |
| **Quick entry** | Amount and category (dropdown, **required**, empty by default and after each save) on one row, then the orchard buttons (exactly one, **required**, no default, reset after each save), optional description below, one-tap date picker. Keyboard flow: Amount → Enter → category → Description → Enter saves. |
| **History** (History + Filter merged) | Day / Week / Month / Year or custom date range, search box, **⚙ Lọc thêm** (categories, amount range, show voided), total + count, **📋 Review** / copy for Excel, optional entry picking. Tap an entry to edit or delete. |
| **Statistics** | Day / Week / Month / Year views (default Month; Year shows monthly bars). Daily bar chart, per-category breakdown with percentages, category radar chart (one axis per current category with spending in the period; categories with no spending are listed below it), monthly budget progress. Tap a category to open History filtered by that category. |
| **CSV import** | Bulk-import entries from a CSV file with a simple format (see below). |
| **Monthly budget** | Set a limit; the progress bar turns amber at 80% and red at 100%. |
| **GitHub Gist storage** | Data syncs to a secret Gist; works across devices with the same token and Gist ID. |
| **Zalo notifications** | Optional, **experimental**: a Zalo bot message for every new, edited or deleted entry. |
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
Entry rows show the **category first** (colored dot), then the description (if any) and the creator / editor on a smaller second line. Tap an entry (in Recent or History) to open its details.
- **Only the description and the category can be edited.** Date and amount are read-only.
- To correct an amount or date, delete the entry and add it again.
- **Delete never removes data.** It adds a *void* entry with a negative amount and marks the original as voided (shown struck through). Totals cancel out and the full history is preserved.
- Delete confirmation is an in-app dialog (browser `confirm()` can be blocked in embedded viewers).

### Statistics
Switch between day, week and month. Tap any category row to open a popup listing the underlying entries (description, amount, date) and the category total for the selected period. Voided entries and their negative counterparts are included so the list always adds up to the total.

### History (merged History + Filter)
The tab bar has 4 tabs: **Nhập · Lịch sử · Thống kê · Cài đặt** (the separate Filter tab is gone; nothing was removed).
- **Period:** Ngày / Tuần / Tháng / Năm with ‹ › and *Về hôm nay*, or **Tùy chọn** (from–to dates; starts from the current period). Statistics has no custom range; switching to it while on Tùy chọn returns to Tháng.
- **Orchard selector** (shared with Statistics) and **🔍 search** (description or category) are always visible.
- **⚙ Lọc thêm** opens categories (multi-select), amount range (in thousands) and *show voided entries*; the button shows the number of active conditions (`⚙ Lọc ②`). When collapsed, active conditions appear as chips with ✕. *↺ Bỏ tất cả điều kiện* clears them.
- Voided entries and void records are hidden by default (the total is the same; they cancel out).
- Count and total of the current list; **📋 Review** opens the review table (date, orchard, category · description, amount, per-orchard subtotals, total) for exactly the listed entries, with **Copy (Excel)**. *Chọn khoản để Review* adds checkboxes to review only some entries.
- Tapping a category in Statistics opens History filtered by that category (same period and orchard).
- Filter state is kept while switching tabs and reset when the app is reopened.

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
- **new entry** (`🧾 Chi mới`, with the orchard line `🌳 Vườn lớn` / `🌱 Vườn nhỏ`: amount, description, category, date and the day's total),
- **edit** (`✏️ Sửa chi`, only when the description or category actually changed; old → new),
- **delete entry** (`🗑️ Xóa chi`: amount, date, description, category).

Nothing is sent when the Zalo fields are empty, for category / budget changes, or for CSV import.

**On/off switch vs. disconnect.** The **Gửi thông báo Zalo** switch at the top of the section pauses sending on this device while keeping the bot token and chat ID (`dr_zl_mute=1`); nothing is queued while paused and pending messages are dropped when pausing. Turning it back on resumes normal sending. **Gửi thử** still works while paused. **Xóa kết nối** (with confirmation) deletes the token and chat ID from this device. Applying a quick-setup config with Zalo turns the switch on.

**Messages are sent only after the change is on the Gist.** Saving queues the message on the device (`dr_zq`); after a sync that re-reads the Gist and finds the change (new entry id, edit with the same or newer `upd`, the void record of a delete), the queued messages are sent in order. If the sync fails (no network, GitHub error, expired token) the message waits; it is sent after the next successful sync, also after the app is reopened, with an extra line `⏳ Ghi lúc hh:mm dd/mm` when it is more than 2 minutes late. If Zalo itself is unreachable the message stays queued. If the browser storage is full, the add / edit / delete is rolled back with a warning and nothing is queued. Switching Gist or logging out clears the queue. Sync merges from other devices do not send messages (the device that made the change already did).

Setup (per the Zalo Bot Platform documentation):
1. In Zalo, open the official account **Zalo Bot Manager** and choose **Create bot**; the bot name must start with "Bot". Zalo sends you the **bot token**.
2. In Settings → *Thông báo Zalo*, paste the token and tap *mở getUpdates*. The page waits about 30 seconds: send the bot a message on Zalo meanwhile, then find `chat` → `id` in the result.
3. Paste it as **Chat ID**, tap **Lưu**, then **Gửi thử** (send test).

Technical notes and caveats:
- Every entry message has at most 4 lines: **orchard** (upper case) / **who · action** / **amount · category** / **date · description**:
  ```
  🌳 VƯỜN LỚN
  🧾 Chị Lan · Chi mới
  💸 8.500.000 ₫ · Mua phân
  📅 15/11 · Phân NPK 10 bao
  ```
  Edit: changed parts as `old → new` (`🌱 VƯỜN NHỎ  (chuyển từ 🌳 Vườn lớn)`, `💸 1.500.000 ₫ · Làm cỏ → Làm công`, `📅 14/11 (hôm qua) · old → new`). Delete: `🗑️ name · Xóa chi` with `💸 −amount`. The date has no year (unless another year) and adds `(hôm qua)` or the weekday when not today. A late message (queued) ends with ` · ⏳ ghi lúc hh:mm`. Test / quick setup: `✅ name · Kết nối Zalo OK` / `🧪 Gửi thử · dd/mm hh:mm` (or `⚡ Thiết lập nhanh`). The result is shown under the buttons (green = sent, orange = error with a hint: wrong bot token, or wrong chat ID / recipient has not messaged the bot) and as a toast.
- Calls go from the browser to `https://bot-api.zaloplatforms.com/bot<TOKEN>/sendMessage` as a form-encoded request with `mode: 'no-cors'`, so messages go out even if the API does not send CORS headers. The trade-off is that regular sends cannot read the response; only the test button tries to.
- Each notification is sent once, with no automatic retry (to avoid duplicates or late messages).
- The token and Chat ID are stored only in this browser (`localStorage`), per device.
- **Not verified against the real Zalo service.** Undocumented points: CORS behaviour, rate limits, time limits or prerequisites for a bot to message a user, and whether a bot can post to group chats. Treat the feature as experimental.

## Current user

- **Creator / last editor**: each entry shows its creator after the category (`· Lan`). An edit stores the editor in `eb` (with `upd`); the row adds `✏️` (`· Lan ✏️Tư` when someone else edited). The detail sheet shows `👤 Tạo: … · ✏️ Sửa cuối: … · dd/mm hh:mm`.
- **Người đang sử dụng**: a display name stored on the device (`dr_who`), shown in the header and saved as `by` on every new entry, delete record and CSV import row; it also appears in Zalo messages. Set it in **Cài đặt → 👤 Người dùng & vườn** or via the quick-setup text (`"n"`). Tapping the name in the header opens that section.

## Weather

A one-line strip under the title shows current weather (temperature, condition, today's rain chance, humidity); tap it to expand location, wind and a 3-day outlook (tap again to collapse, state remembered). Data from [Open-Meteo](https://open-meteo.com/) (free, no API key). Location is set in **Cài đặt → 👤 Người dùng & vườn** or via the quick-setup text (`"l"`). Until set, the strip says "Chưa thiết lập vị trí vườn" and tapping it opens that section.

Setting the orchard location (no typing coordinates):
- **🛰️ Vị trí hiện tại**: GPS of the phone (stand in the orchard). Warns if accuracy is worse than 200 m.
- Paste into the box (read automatically on paste, or tap **📍 Đọc vị trí**): a full Google Maps link (`!3d…!4d…` pin, `@lat,lon`, `?q=`/`query=`), plain coordinates (`11.94040, 108.45830`), or degrees-minutes-seconds (`11°56'25.4"N 108°27'29.9"E`). In Google Maps, long-press the orchard to drop a pin and copy its coordinates.
- Short links (`maps.app.goo.gl/…`) contain no coordinates and the browser cannot follow them (no CORS), so the app explains how to open the link and copy the full URL instead. If the shared text includes a place name, it tries that name first.
- Anything else is searched as a place name (OpenStreetMap Nominatim, Vietnam only).
- **Tên vườn** fills itself from the Google Maps place name or by reverse geocoding (Nominatim), and can be edited. Coordinates are rounded to 5 decimals (~1 m). Cached 30 minutes in `dr_wx`; expanding refreshes if stale.

## Quick setup (encrypted config text)

Settings → **Thiết lập nhanh** has two buttons and no text box on the page:

- **📥 Thiết lập nhanh** opens a dialog with a text box: paste the config text, tap **Tiếp**, enter the shared password, confirm the ☠️ override warning.
- **📋 Sao chép thiết lập hiện tại** asks for the shared password and copies this device's config (encrypted; GitHub, Zalo, user name, location) to the clipboard; if the browser blocks it, a read-only dialog shows the text with a copy button.

- The text is always encrypted: `DR27E1.` + base64url(`salt(16) | iv(12) | AES-256-GCM ciphertext+tag`), key = PBKDF2-SHA256(password, salt, 310,000 iterations). Plain text is rejected.
- Inside (JSON): `{"app":"dr27","t":token,"g":Gist ID,"zt":Zalo bot token,"zc":Zalo chat ID,"n":user name,"l":[place,lat,lon]}` — every key except `app` is optional. Categories and budget are **not** in the text: they live in the Gist and load automatically (`"c"`/`"b"` in older texts are ignored, so an old text can never overwrite the group's categories or budget).
- **One shared password** for the whole app. It is never stored in clear text: the code only holds its salted PBKDF2-SHA256 hash (`PW_HASH`) to check what is typed; nothing password-related is stored on the device. Opening Settings needs no password.
- Wrong password → "Sai mật khẩu"; truncated/corrupted text → asked to paste again.
- **Setup link:** `https://bach2332.github.io/durian_spendlog_pwa/#cfg=DR27E1.…` opens Settings with the quick-setup dialog pre-filled (read-only); the user taps **Tiếp**, enters the shared password and confirms the override warning as usual. The string sits after `#`, so it is never sent to the server; the app removes it from the address bar right away and keeps it only in `sessionStorage` until the dialog is answered (so an automatic reload after an app update does not lose it). Works whether the link opens a new tab or the app's existing tab (`hashchange`). Opened inside Zalo/Facebook's in-app browser, the app first explains that the setup would stay inside Zalo and suggests "Mở bằng trình duyệt". Note: on iPhone, a home-screen app has its own storage separate from Safari.
- If the config contains Zalo (bot token + chat ID), after applying it the app sends one message to that chat: `👤 <user name> / ✅ Zalo OK · ⚡ Thiết lập nhanh / 🕒 dd/mm hh:mm`; the result (sent / Zalo error) is added to the "Đã áp dụng cấu hình" toast.
- Override warning: after tapping Tiếp and entering the password, a ☠️ dialog warns that everything will be overwritten (no config details shown) and the "☠️ Ghi đè" button unlocks after a 3-second countdown.
- Secrets are never shown in full anywhere in the UI (`mask()`: first 2–4 and last 2 characters): the Settings fields (saved values appear only as a masked placeholder; leave a field empty to keep the saved value), and the Gist / getUpdates links (URLs are built only on click, not put in the page).

### Config builder (Settings → 🛠️ Tạo chuỗi cấu hình)

A separate tool for preparing config text and setup links for *other* devices; it never changes this device's config. Create-only by design: there is no decrypt function in this tool.

1. Type the shared password (checked against `PW_HASH`; not stored anywhere).
2. Fill in: user name, GitHub token, Gist ID, Zalo bot token + chat ID, orchard location (**🔎 Tìm** looks up coordinates; default is Đà Lạt, Lâm Đồng).
3. **🔒 Mã hóa** shows the encrypted string and the setup link (`<app URL>#cfg=…`) with copy buttons.

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

## Orchards (Vườn lớn / Vườn nhỏ / Rẫy cà phê)

- Data: each item gets `"g":"L"` (Vườn lớn), `"g":"N"` (Vườn nhỏ) or `"g":"C"` (Rẫy cà phê). The list is the `GD` constant (name, short name, Zalo emoji, color); every screen loops over it. Void records copy `g` from the original. Items without `g` (entered before this feature) count as **Chưa phân vườn**; open one and pick an orchard to fix it. Nothing else in the Gist file changes; the two orchards are fixed in the app (name, icon, color).
- Entry: two big buttons under amount/category; exactly one must be chosen (no default). The chosen one gets a solid colored background, white text and a ✓; the other fades. Saving without one is blocked ("Chọn vườn").
- Edit sheet: the orchard can be changed like the category (updates `upd` / `eb`; Zalo edit message shows `🏡 Vườn lớn → Vườn nhỏ`).
- Statistics with **Tất cả**: a "Theo vườn" card (amount and % per orchard, tap to switch), stacked bars (green = lớn, orange = nhỏ, grey = chưa phân vườn). With **Lớn** / **Nhỏ** every figure, chart and the radar use only that orchard.
- Review / copy (Excel): columns `Ngày, Vườn, Danh mục, Nội dung, Số tiền`, plus subtotals per orchard.
- CSV import: optional column `vuon` (`lon` / `nho` / `cafe`, also `L` / `N` / `C`, `Rẫy cà phê`); empty = Chưa phân vườn.
- Icons: in the app, three drawn SVG icons (`#gi-L` big durian tree with fruit, `#gi-N` young durian tree, `#gi-C` coffee bush with red cherries). In Zalo messages, standard emoji: 🌳 Vườn lớn, 🌱 Vườn nhỏ, ☕ Rẫy cà phê (first line of every entry message).

## Entry screen and the on-screen keyboard

- The date button is a calm white button with a calendar icon and the date in black (`Hôm nay / 15/11`); tap it to open the date picker. By default it is today; another day shows the weekday (`T2`…`CN`) with the date, and the border and icon turn orange.
- **No back-dated entries.** The date picker starts at today (`min`). A past date (picked anyway, e.g. on iOS which ignores `min`, or left over when the app stayed open past midnight; checked again on Lưu) opens a ⛔ dialog "Không nhập khoản của ngày đã qua". **Ghi vào hôm nay** sets the date to today and prefixes the note with `[Ngày dd/mm]` (the real date); **Hủy** just resets the date to today. This keeps new entries out of periods that may already be closed. Editing an entry cannot change its date; CSV import is not restricted.
- While typing in the entry card **and** the visible viewport shrinks (keyboard open, detected with `visualViewport`), the app adds `body.kb`: weather strip, bottom tab bar and the sync status text are hidden, the header shrinks, and the entry card is scrolled to the top, so amount, category, orchard, note, date and **Lưu** stay above the keyboard. Focus alone (keyboard not open) changes nothing. On touch devices the amount field is no longer auto-focused at startup.

## Sync log (ⓘ)

Each sync compares data before and after: entries received from other devices (**⬇ Nhận**, new / newer `upd` / new void records) and entries this device pushed (**⬆ Gửi**, local entries missing or older on the Gist when the write happened). Syncs with changes are kept in `dr_synclog`, **last 10**. The status line says `… · 3 khoản từ máy khác` and a green **ⓘ 3** appears when received entries are unseen; tapping it opens **Nhật ký đồng bộ**: newest sync first, each with time, `⬇ Nhận n từ máy khác` / `⬆ Gửi n lên` and the entries with **Mới / Sửa / Xóa** tags (tap one to open it). After viewing, the ⓘ turns grey and stays. The first full load on a new device is not logged. Switching Gist clears the log.

## Pull from Gist (overwrite this device)

**Cài đặt → 🔗 Kết nối GitHub → ⬇️ Tải lại từ server (ghi đè máy này)** (the UI says "server" for the Gist) first asks for the shared app password (checked against the stored hash), then downloads the current Gist file and **replaces** this device's data with it (no merge, nothing is pushed). Before confirming, a ☠️ dialog shows how many entries the Gist has and how many local changes are not on the Gist yet (they will be lost); the confirm button waits 3 s. After pulling: the unsynced flag and this device's pending Zalo messages are cleared, the sync log records `⬇️ Ghi đè từ server · N khoản`. The Gist and other devices are not changed, no Zalo message is sent, and the device config (token, Gist ID, name, location, Zalo) is kept.

## Hard delete by date range (Xóa hẳn dữ liệu theo ngày)

**Cài đặt → 🗑️ Xóa hẳn dữ liệu theo ngày** is locked; tapping **🔒 Nhập mật khẩu để mở** asks for the shared app password (checked against the stored PBKDF2 hash, never stored). After unlocking, pick **Từ ngày / Đến ngày**; a preview shows how many rows will go (entries still counted, earlier deleted/void rows, per-orchard count and total). **🗑️ Xóa vĩnh viễn** opens a ☠️ confirm (button waits 5 s), then every row whose date is in the range is removed outright: no negative "Xóa" row, no Zalo message, no undo. Closing the section or leaving the app locks it again.

How it stays deleted on every device: the delete is stored in the Gist as a small rule, `purges: [{id, from, to, at, by, n}]`. On every sync, all devices drop rows whose date is inside `[from, to]` and that were created before `at` (creation time comes from the row id). So a device that was offline, or an older app version that pushes the old rows back, gets cleaned again on the next sync of an updated device. Rows created after the delete (even with an old date) are kept. Void rows pointing to a purged entry are removed too, and pending Zalo messages for purged rows are dropped. The last 5 deletes are listed under the form.

## Income entries (Khoản thu)

**Hidden by default.** Tap the version line at the bottom of the page 5 times to reveal **Cài đặt → 🧪 Tính năng ẩn**, then switch on **Cho nhập khoản thu**. The switch is shared through the Gist (`feat: {inc: {on, t}}`, newest change wins), so turning it on once enables it on every device after sync; turning it off hides the Chi/Thu switch and the income category tab again (existing income entries stay and are still shown). "Ẩn mục này" hides the section again on this device while the feature is off.

The entry screen has a **− Chi | + Thu** switch (default Chi). In Thu mode the card turns green, the category list shows the **income categories** (default: Bán sầu riêng loại 1 / loại 2 / loại 3 / xô, Bán cà phê, Thu khác; edit them in Cài đặt → Danh mục → tab **+ Thu**), and the button becomes **+ Lưu khoản thu**. Category and orchard are still required.

Data: an income entry is a normal item with `"dir":"in"`; items without `dir` are expenses, so existing data needs no migration. Deleting an income entry writes a void row that also carries `dir:"in"`. Income categories are stored in `incomeCats` with their own add/remove log `icatLog` (merged the same way as `categories`/`catLog`). Budget, "Hôm nay đã chi", the daily bar chart and the radar count expenses only.

Where income shows: green `+amount` rows; History filter **Tất cả / + Thu / − Chi** and totals `+thu · −chi`; Statistics tiles **Tổng thu · Tổng chi · Lãi/Lỗ**, income/profit under each orchard, a "Theo danh mục thu" card; Review / Excel copy has a **Loại** column and Thu / Chi / Chênh lệch totals. Zalo: `💰 name · Thu mới`, `💵 +amount · category`; edits/deletes say `Sửa thu` / `Xóa thu`.

## Closing the books per orchard (Chốt sổ)

**Cài đặt → 🔒 Chốt sổ theo vườn**: pick the orchard (required) → **Từ ngày** is automatic (the day after that orchard's last closing, or the date of its first entry) → pick **Đến ngày** (≤ today) → the preview shows entry counts and expense/income per category with totals → **🔒 Chốt sổ** → ☠️ confirm (5 s). The app syncs first so the numbers are current. No password is required.

After closing, every entry of that orchard dated on or before `to` is **locked forever**: the edit sheet becomes read-only, delete is hidden, new entries/CSV rows for that orchard and date are refused, and "Xóa hẳn dữ liệu" skips them. History shows 🔒 on locked rows. There is no reopen.

Data: `periods: [{id, g, no, from, to, at, by, n, tot, cats:{category:[count, amount]}, nin, tin, icats}]` in the same Gist file (snapshot of the numbers at closing time). Enforcement on every sync (`lockApply`): for a locked entry the version as of the closing time is kept (edits made later, e.g. by an old app version, are dropped), and entries created after the closing time that fall into a closed period are removed. If two devices close overlapping periods at the same time, the earlier closing wins.

Reports: **Thống kê → 🔒 Chốt sổ** lists, per orchard (respecting the orchard filter), each closed period (range, entries, who closed it, expense, income) and the still-open part; totals for closed vs. not closed on top. Tap a period for the detail: ĐÃ CHỐT stamp, Thu/Chi/Lãi, expense and income per category, who/when, running total for the orchard, **Review / Copy Excel** and **Xem trong Lịch sử** (opens History with that orchard and date range).

Zalo (sent only after the period is confirmed on the Gist), detailed format:
```
🌳 VƯỜN LỚN
🔒 Lan · Chốt sổ đợt 2
📅 01/11 → 15/11/2026 · 23 khoản
━━━━━━━━━━━━
▫️ Mua phân · 6 khoản
      18.400.000 ₫
…
━━━━━━━━━━━━
💸 TỔNG CHI 31.200.000 ₫
(income block, 💵 TỔNG THU and 📊 Lãi/Lỗ when the period has income)
📊 Lũy kế vườn (2 đợt): chi 76.500.000 ₫
```

## Version guard (minV)

Each app build has `APPV` (now `20261124a`). When this build writes the Gist it sets `minV` to the highest version seen. A device whose `APPV` is lower than the Gist's `minV` does not write; it shows "App trên máy này là bản cũ — tắt hẳn app rồi mở lại để cập nhật". Builds older than 20261121c don't know this rule, but their edits to closed periods and purged rows are cleaned by the next sync of an up-to-date device.

## Test mode (Chế độ test)

**Cài đặt → 🧪 Chế độ test**, per device. Fill the test config (GitHub token, Gist ID — empty creates a new test Gist on first sync, Zalo Bot token + Chat ID), **Lưu cấu hình test**, then switch on **Bật chế độ test trên máy này** (confirm; the app reloads). The test Gist must differ from the real one. Saved values are shown masked only.

While on: the header turns orange-striped with a **🧪 TEST** badge (theme colour orange too); the header icon, browser-tab favicon and iPhone `apple-touch-icon` switch to `icon-test-192.png` / `apple-touch-icon-test.png` (orange stripes + TEST band). An already-installed home-screen icon cannot change at runtime (it is fixed when the app is added to the home screen), and every data action — add, edit, delete, closing, purge, pull, sync — uses only the test Gist; Zalo messages go to the test bot/chat and start with `🧪 [TEST]`. How: at startup the app maps the connection/data keys in localStorage (`dr_token, dr_gist, dr_cache, dr_dirty, dr_base, dr_zl_token, dr_zl_chat, dr_zl_mute, dr_zq, dr_synclog`) to `t_dr_…`, so the real config and real local data are never touched; name, location and weather are shared. Switching off reloads back to the real data. **Xóa cấu hình test** removes all `t_dr_…` keys on this device (the test Gist on GitHub stays).

### CSV import with income

The CSV may have a `thuchi` column (also `kieu`, `type`, `dir`): `thu` / `in` / `income` / `+` makes the row an income entry (`dir:"in"`), anything else or empty is an expense. Income categories are matched against / added to `incomeCats` ("Thu khác" when empty). The preview shows expense and income counts and totals separately. The template file now includes `vuon` and `thuchi` columns and one income row.

## Category management (Cài đặt → Danh mục)

Applies to both tabs (− Chi / + Thu). Each category shows how many entries currently use it, e.g. `Mua cơm (4)` (live entries of that type only; deleted/void rows are not counted).

- **Sửa** renames the category. All entries using it (including their void rows) are switched to the new name and get `upd`/`eb`, so the change syncs to every device. The new name must not already exist in either list.
- **Xóa** requires choosing a **replacement category** from the same list (the dialog will not close without one). All entries using the deleted category move to the replacement, then the category is removed.
- Entries inside a closed period (chốt sổ) are never changed: they keep the old name and the toast says how many were kept. No Zalo message is sent for category changes.

### Sync log performance

The ⓘ dialog opens immediately with a "Đang tải nhật ký…" placeholder and renders on the next frame. Each log event keeps at most 50 entries per direction (`in`/`out`) plus the real totals (`nIn`/`nOut`); the dialog shows 15 per direction with **Xem thêm** for the rest of the kept ones and "…và N khoản khác" beyond that. Old oversized logs are trimmed on load. After importing 5,000 rows (CPU throttled 4×) the dialog now opens in ~0.13 s instead of ~6.9 s, and the stored log shrank from ~230 KB to ~2 KB.

## Delete reason (Lý do xóa)

Deleting an entry (expense or income) now requires a reason: the confirm dialog has a text box and will not proceed with fewer than 3 characters. The reason is stored on the void row as `why`, shown in lists (`💬 reason`, on both the void row and the struck-through original), and added as the last line of the Zalo delete message: `💬 Lý do: …`.
