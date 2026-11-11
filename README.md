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
| **Statistics** | Daily bar chart, per-category breakdown with percentages, category radar chart (one axis per current category with spending in the period; categories with no spending are listed below it), monthly budget progress. Tap a category to see the entries behind its total. |
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
2. **User ID (Git ID)**: your GitHub username. Optional; only used to build the "Open Gist on GitHub" link.
3. **Mật khẩu (GitHub Token)**: paste the token.
4. **Gist ID**: leave empty the first time and the app creates a new secret Gist. On another device, enter the existing Gist ID.
5. Tap **Kết nối & tải** (Connect & load). The status line at the top should read **Đã đồng bộ** (synced) or **Đã lưu hh:mm** (saved).

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

How syncing works:
- On start (and when the app returns to the foreground with nothing unsynced) the app loads the Gist; files above the API's inline size limit are fetched through their raw URL.
- Every change is applied locally first, cached in `localStorage`, then written to the Gist (the whole file is replaced). Failed saves are marked as pending and retried on the next save or app start.
- The device keeps a local cache, so the app opens and records entries without network access.
- Concurrent edits from two devices are **last write wins** (see Limitations).

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
- Calls go from the browser to `https://bot-api.zaloplatforms.com/bot<TOKEN>/sendMessage` as a form-encoded request with `mode: 'no-cors'`, so messages go out even if the API does not send CORS headers. The trade-off is that regular sends cannot read the response; only the test button tries to.
- Each notification is sent once, with no automatic retry (to avoid duplicates or late messages).
- The token and Chat ID are stored only in this browser (`localStorage`), per device.
- **Not verified against the real Zalo service.** Undocumented points: CORS behaviour, rate limits, time limits or prerequisites for a bot to message a user, and whether a bot can post to group chats. Treat the feature as experimental.

## Current user and Settings password

- **Người đang sử dụng**: a display name stored on the device (`dr_who`), shown in the header and saved as `by` on every new entry, delete record and CSV import row; it also appears in Zalo messages. It can come from the setup QR (`"n"` field).
- **Mật khẩu Cài đặt**: there is a built-in group default (only its PBKDF2 hash is in the code, `DEF_LOCK`); each device can change it, and "Quên mật khẩu?" returns to the default. Opening the Settings tab always asks for the password; it locks again when you leave the tab or the app goes to the background. Only a salted PBKDF2-SHA256 hash (310,000 iterations) is stored (`dr_lock`), never the password. Scanning a setup QR sets the QR password as this device's Settings password. "Quên mật khẩu?" wipes the device's config (token, Gist ID, Zalo, name, password; Gist data is untouched) so it can be set up again.
- This lock protects the screen, not the stored token: `localStorage` is plain text on the device.

## QR quick setup

Settings → **Thiết lập nhanh bằng QR**.

- **Hiện mã QR** (on a device that is already set up) asks for a password (min. 8 characters), encrypts the config (GitHub user, token, Gist ID and, if enabled, Zalo bot token and chat ID) and shows it as a QR code. **Lưu ảnh QR** saves it as a PNG.
- **Quét mã QR** (on a new device) opens the camera, or reads a QR image from the gallery, asks for the password, shows a summary, and on **Áp dụng** fills the settings and loads the data. A wrong password is rejected and can be retried.
- Encryption: AES-256-GCM with a key derived by PBKDF2-SHA256 (310,000 iterations, random 16-byte salt, random 12-byte IV), all via the browser's Web Crypto API (needs HTTPS). QR text: `DR27E1.` + base64url(`salt | iv | ciphertext+tag`). Plain JSON inside: `{"app":"dr27","u":"…","t":"…","g":"…","zt":"…","zc":"…"}`.
- The password is never stored or hard-coded (this repo is public). Share it separately from the QR image. Forgot it: make a new QR. If both QR and password leak: revoke the token on GitHub and make a new QR.
- Scanning uses `BarcodeDetector` when available, otherwise `lib/jsQR.min.js`; the QR is drawn with `lib/qrcode.min.js`. Both are bundled locally (MIT) and loaded only when needed.

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
- **Last write wins:** every save replaces the whole file, so simultaneous edits from two devices can overwrite each other.
- **Growth:** an entry is roughly 120 bytes of JSON; a single file beyond about 1 MB is no longer returned inline by the Gist API (the app falls back to the raw URL), and every save rewrites the full file. GitHub's authenticated API rate limit is 5,000 requests per hour.
- Whole-đồng integers only; amounts are entered in thousands (CSV import uses full đồng).
- Vietnamese-only UI. Verified in headless Chromium only, not on every real device or browser.

## Ideas under consideration (not implemented)

- **Audit trail** for multi-user use: append-only events recording who created, edited or voided each entry, with before/after values.
- **Moving off Gist** to a hosted database such as Supabase (Postgres, per-user login, row-level security, database-side audit triggers); Firebase and Cloudflare D1 were considered as alternatives.
- **Local-first sync queue** and per-month data files to keep saves fast as data grows.
