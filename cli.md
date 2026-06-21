# Miniso & Hamleys — Monitor Feed (CLI)

Monitor feed tasks sit and wait for a matching product to appear on the monitor feed. When something matches your keywords, the task grabs it and tries to checkout.

This is **not** the same as **Monitor Mode** (that one watches specific product links you pick yourself).

---

## Before you start

- Profiles set up in `profiles.csv`
- Proxies in `sites/miniso/proxies.txt` or `sites/hamleys/proxies.txt`
- Monitors running for Miniso / Hamleys so products actually hit the feed
- Task file in the right folder:
  - Miniso → `sites/miniso/tasks.csv`
  - Hamleys → `sites/hamleys/tasks.csv`

---

## How to turn on monitor feed

- Set `monitor_feed` to `true`

Leave `product_url` **empty** — the feed fills that in when a match comes through.

You **must** fill in `keywords`.

---

## Keywords

Comma-separated words matched against the product name from the feed.

| What you type | What it does |
|---|---|
| `+pokemon,+booster` | Product name must contain both words |
| `-damaged` | Skip anything with "damaged" in the name |

---

## Miniso task CSV

**File:** `sites/miniso/tasks.csv`

**Columns:**

`mode`, `monitor_feed`, `keywords`, `product_url`, `quantity`, `profile_name`, `payment_type`, `account`, `accountmode`, `preload`, `dummy`, `proxy_file`

**What to fill in for monitor feed:**

| Column | What to put |
|---|---|
| mode | `NORMAL` |
| monitor_feed | `true` |
| keywords | Your keyword filter (see above) |
| product_url | Leave blank |
| quantity | How many to buy (usually `1`) |
| profile_name | Name from `profiles.csv` |
| payment_type | `card` |
| preload | true/false |
| dummy | dmmpy pid if using preload mode |
| proxy_file | Leave blank for default proxies, or e.g. `resis` |

**Example row:**

```csv
mode,monitor_feed,keywords,product_url,quantity,profile_name,payment_type,account,accountmode,preload,dummy,proxy_file
NORMAL,true,pokemon,151,-damaged,,1,MyProfile,card,,false,,,
```

---

## Hamleys task CSV

**File:** `sites/hamleys/tasks.csv`

**Columns:**

`mode`, `monitor_feed`, `keywords`, `product_url`, `quantity`, `profile_name`, `payment_type`, `catchall`, `account`, `login`, `imap`, `proxy_file`

**What to fill in for monitor feed:**

| Column | What to put |
|---|---|
| mode | `FRONTEND`  |
| monitor_feed | `true` |
| keywords | Your keyword filter |
| product_url | Leave blank |
| quantity | Usually `1` |
| profile_name | Name from `profiles.csv` |
| payment_type | `card` |
| catchall | Only if `login=true` and you want a random email — e.g. `@yourdomain.com` |
| account | Email from `sites/hamleys/accounts.csv` (if logging in) |
| login | `true` to use a saved account, `false` for guest checkout | - they always turn login on for pokemon
| imap | Leave blank unless you know you need it - for catchall. |
| proxy_file | Leave blank for default proxies, or e.g. `resis` |

**Example — logged in:**

```csv
mode,monitor_feed,keywords,product_url,quantity,profile_name,payment_type,catchall,account,login,imap,proxy_file
FRONTEND,true,lego,harry potter,,1,MyProfile,card,,user@email.com,true,,
```

**Example — guest checkout:**

```csv
mode,monitor_feed,keywords,product_url,quantity,profile_name,payment_type,catchall,account,login,imap,proxy_file
FRONTEND,true,lego,harry potter,,1,MyProfile,card,,,false,,
```
