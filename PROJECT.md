# Bitky v dejinách sveta: Project Documentation

Everything needed to understand, maintain, or rebuild this app without the original local folder.

- **Live app:** https://novodvorsky.github.io/bitky-collection/
- **Code (public):** https://github.com/novodvorsky/bitky-collection
- **Collection data (private):** https://github.com/novodvorsky/bitky-data, file `collection.json`
- **Built:** November 2026

---

## 1. What it is

A phone-first web app for tracking the Kremnica Mint (Mincovňa Kremnica) silver coin series
*Bitky v dejinách sveta* (Battles in World History). It's used mostly on an iPhone 16 and a OnePlus 8,
installed to the home screen.

Features:

- **Series I / Series II / Collection** bottom tab bar
- **Coin tray:** a grid of numbered slots that fill with a silver coin as coins are collected (the signature element)
- Compact coin list: battle-side thumbnail, name, years, and a circle to tick "owned"
- Search (accent-insensitive) and All / Collected / Missing filter
- Detail sheet per coin: swipeable photos, full-screen viewer, specs, link to mint.sk
- **Export PDF:** a single A4 page listing owned coins, no photos (jsPDF, loaded on demand)
- **Sync:** checkmarks saved to the private `bitky-data` repo, merged across phones
- **Backup / Restore:** JSON file as a manual safety copy
- Light/dark mode, Dynamic Type, reduced motion and transparency, Android Back button closes sheets

## 2. Files

| File | Purpose |
| --- | --- |
| `index.html` | The whole app: HTML, CSS, coin data, and JavaScript. No build step. |
| `manifest.webmanifest` | Home-screen install (name, icons, colors) |
| `icons/` | `apple-touch-icon.png` (180), `icon-192.png`, `icon-512.png`: silver coin with crossed swords |
| `README.md` | Short user guide and token setup |
| `PROJECT.md` | This document |

External dependencies: jsPDF 2.5.1 from cdnjs (only when exporting). Coin photos are hotlinked from mint.sk.

## 3. Coin data

### Series I (complete, 15 coins, 2021–2026)

All coins: Niue 2 Dollars · Ag 999/1000 · 31.1 g (1 oz) · 40 mm · patinated.

| No. | Battle | Year | Issued | Mintage | Designer |
| --- | --- | --- | --- | --- | --- |
| 1 | Thermopylae (Bitka pri Termopylách) | 480 BC | 2021 | 200 | Mgr. art. Martin Sabol |
| 2 | Salamis (Bitka pri Salamíne) | 480 BC | 2021 | 200 | Branislav Ronai |
| 3 | Zama (Bitka pri Zame) | 202 BC | 2021 | 200 | Mgr. art. Roman Lugár |
| 4 | Teutoburg Forest | AD 9 | 2022 | 250 | Mgr. art. Martin Sabol |
| 5 | Hastings | 1066 | 2022 | 250 | Branislav Ronai |
| 6 | First Crusade | 1096–1099 | 2022 | 250 | Mgr. art. Martin Sabol |
| 7 | Uji | 1180 | 2023 | 250 | Mgr. art. Roman Lugár |
| 8 | Orléans | 1428–1429 | 2023 | 250 | Mgr. art. Roman Lugár |
| 9 | Mohács | 1526 | 2023 | 250 | Mgr. art. Martin Sabol |
| 10 | Vienna | 1683 | 2024 | 250 | Mgr. art. Roman Lugár |
| 11 | Waterloo | 1815 | 2024 | 250 | Branislav Ronai |
| 12 | Little Bighorn | 1876 | 2024 | 250 | Mgr. art. Martin Sabol |
| 13 | Somme | 1916 | 2025 | 250 | Mgr. art. Martin Sabol |
| 14 | Pearl Harbor | 1941 | 2025 | 250 | not published |
| 15 | Stalingrad | 1942–1943 | 2026 | 250 | not published |

The mint's series page says 200 for Uji, but its product page and the 2023 emission plan say 250, so 250 is used.

### Series II (started 2026)

| No. | Battle | Year | Status (Nov 2026) |
| --- | --- | --- | --- |
| 1 | Marathon (Bitka pri Maratóne) | 490 BC | Announced, release moved to November 2026 |
| 2 | Gaugamela (Bitka pri Gaugamele) | 331 BC | Announced in the 2026 emission plan |

No photos or designers had been published yet, so the app draws a silver placeholder coin.

### Where the data came from

- Series page: https://www.mint.sk/vyrabame/bitky-v-dejinach-sveta (outdated; lists only 10)
- Emission plans: `https://www.mint.sk/emisny-plan-mincovne-YYYY` for 2021–2026. These were the most complete source.
- Product pages: found in `https://www.mint.sk/sitemap.xml` (search for `bitky-v-dejinach`). Each page has
  `Autor` (designer), `Náklad` (mintage), and photos in `data-src="/sites/default/files/styles/product_full/public/product_images/…"`.
- Photo files: `https://www.mint.sk/sites/default/files/product_images/<file>.jpg` (full size works without a token),
  or `/sites/default/files/files/<file>.jpg` for emission-plan photos.
- **Naming:** in the mint's files, **`averz` is the battle side** and **`reverz` is the monarch side** (Elizabeth II until 2022, Charles III from 2023).
  Two-coin photos always have the battle side on the left.

### Adding a new coin (e.g. Series II no. 3)

1. Check the latest emission plan and `sitemap.xml` for its product page.
2. In `index.html`, find `const COINS = [` and add an entry:
   ```js
   { id: 'II-03', series: 'II', no: 3, name: 'Battle of …', sk: 'Bitka pri …', era: '…', year: '…', issue: 2027,
     mintage: 250, designer: '…', images: [P+'…-averz-web.jpg', P+'…-reverz-web.jpg'], url: SHOP+'…' },
   ```
   - Put the **battle-side** photo first. Add `pair: true` if the first photo shows both sides (the thumbnail then crops to the left coin).
   - `images: []` shows the placeholder coin.
   - Remove the `status` field once a coin is released.
3. For Series II, lower `futureSlots` in `SERIES` as real coins are added (it draws dotted "coming" slots).
4. Commit and push. GitHub Pages updates in about a minute.

Coin IDs (`I-01` … `II-02`) are the sync keys: **never rename an ID**, or that coin's checkmark is lost.

## 4. Storage and sync

### On the phone (localStorage)

| Key | Content |
| --- | --- |
| `bitky-v-dejinach-sveta:v2` | `{ coinId: { o: 1 or 0, t: timeOfChangeMs } }`, the collection |
| `bitky-v-dejinach-sveta:v1` | Old format `{ coinId: 'YYYY-MM-DD' }`, read once and migrated |
| `bitky-v-dejinach-sveta:ui2` | Last tab and filter |
| `bitky-v-dejinach-sveta:sync` | `{ token, last }`, the GitHub token, on this phone only |

Removals are kept as `o: 0` with a time, so a removal on one phone wins over an older "owned" on the other.

### In GitHub (`novodvorsky/bitky-data/collection.json`)

```json
{ "app": "bitky-v-dejinach-sveta", "updated": "…", "coins": { "I-01": { "o": 1, "t": 1794159465022 } } }
```

### Sync algorithm (`syncNow` in `index.html`)

1. `GET /repos/novodvorsky/bitky-data/contents/collection.json`, reading the content and `sha`.
2. Merge: for every coin, the entry with the larger `t` wins.
3. If local had anything newer, `PUT` the merged file with the `sha`.
4. A 409/422 response means the other phone saved first: start again (up to 2 retries).
5. On no connection, show "Offline · will sync later" and retry every 30 s, on `online`, and on app open.

Sync runs on app open, when the app comes back to the foreground, and 1.2 s after any change.

### Token

A fine-grained personal access token. **Repository access:** only `bitky-data`. **Permissions:** Contents read and write.
Create it at https://github.com/settings/personal-access-tokens/new. "This token can't read and write bitky-data"
means the token's repository access or Contents permission is wrong; editing the token keeps the same string.
When it expires, make a new one and paste it in Collection → Sync on each phone.

## 5. Design decisions (Apple HIG review, November 2026)

- **Bottom tab bar** for the three top-level sections. Tabs navigate and never act.
- **Detail sheet** with grabber, close button, swipe-down to dismiss, and Back-button support via `history.pushState`.
- **Contrast tokens** (measured): label-2 `#6c6c70` 5.3:1 on white; accent `#0066d6` 5.6:1; owned green `#248a3d`
  with white glyph; dark mode `#409cff`, `#30d158`, `#98989f`. The first version had white on `#34c759` (2.2:1) and failed.
- **Text:** `font: -apple-system-body` on iOS so text follows Dynamic Type. Sizes are in rem. Inputs are at least 16 px,
  because iOS zooms the page when an input's text is smaller.
- **Touch targets** at least 44 px; safe-area insets for the notch and home bar.
- **Blur** only on floating bars (tab bar, sticky search); turned off under reduced transparency.
- **Signature:** the coin tray. Everything else stays quiet, system-like.

## 6. Testing checklist

- Phone widths 375 and 393 px, light and dark: no horizontal scroll, tray of 5×3, list readable
- Tick and untick, then reload: state persists
- Search "mohac" finds Mohács; Collected and Missing filters show empty-state text
- Open a coin, swipe photos, open full screen, Android Back closes it
- Export PDF: one page with all 17 coins ticked
- Sync: wrong token shows "GitHub rejected that token", a change appears in `bitky-data` within seconds,
  the other phone picks it up on reopen

## 7. Run it locally

```bash
git clone https://github.com/novodvorsky/bitky-collection.git
```

```bash
python -m http.server 8765 --directory bitky-collection
```

Then open http://localhost:8765. Use a local server rather than opening the file directly, so storage works.
