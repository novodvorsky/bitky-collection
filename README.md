# Bitky v dejinách sveta: Collection Tracker

A phone-first tracker for the Kremnica Mint (Mincovňa Kremnica) silver coin series *Bitky v dejinách sveta* (Battles in World History).

- **Series I**: 15 coins, 2021–2026 (Thermopylae → Stalingrad)
- **Series II**: started 2026 (Marathon, Gaugamela, more as the mint announces them)

Every coin is a Niue 2 Dollars piece in Ag 999/1000, 31.1 g (1 oz), 40 mm, patinated.

## Use it

Open the GitHub Pages link on your phone. To install it like an app:

- **iPhone (Safari):** Share → Add to Home Screen
- **Android (Chrome):** ⋮ menu → Add to Home screen / Install app

Tap the circle next to a coin to mark it as owned. Tap a coin to see its photos and details.
The **Collection** tab backs up or restores your checkmarks and has **Export PDF**, which asks which one-page list you want:

- **Missing Coins**: what you still need, with empty boxes to tick by hand (a shopping list for fairs and dealers)
- **My Collection**: the coins you own
- **Full Checklist**: every coin, ticked or empty

Coins not released yet are marked *(upcoming)*. The app remembers your last choice and lists it first.

## Sync between phones

Checkmarks save on the phone first, then to the private repo `novodvorsky/bitky-data` (file `collection.json`).
If both phones change something, the newest change for each coin wins.

One-time setup:

1. On GitHub, open **Settings → Developer settings → Personal access tokens → Fine-grained tokens → Generate new token**
   (direct link: https://github.com/settings/personal-access-tokens/new).
2. Name it `Bitky phones` and pick the longest expiration offered.
3. **Repository access:** *Only select repositories* → `bitky-data`.
4. **Permissions → Repository permissions → Contents:** *Read and write*.
5. Generate the token and copy it.
6. In the app, open **Collection → Sync**, paste the token, and tap **Connect This Phone**. Do this on each phone.

The token can only read and write `bitky-data`. It is stored only on your phones. You can revoke it on GitHub at any time.
When it expires, the app shows "Token rejected": make a new token and connect again.

**Back Up Data** / **Restore from Backup** still work as a manual safety copy.

## Data

Coin details come from the Mincovňa Kremnica product pages and its 2021–2026 emission plans (checked November 2026).
Photos load directly from mint.sk and remain © Mincovňa Kremnica.

It's a single static `index.html` with no build step. See [PROJECT.md](PROJECT.md) for the full documentation.
