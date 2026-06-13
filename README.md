# Monitor Feed + Hamleys Session Mode

Quick guide for Miniso and Hamleys tasks that wait on restocks instead of checking one product URL.

---

## What is Monitor Feed?

Normally you put a product URL in the task and the bot watches that item (or runs checkout straight away).

**Monitor Feed** is different. You give the task **keywords** instead of a URL. The task sits there waiting until our monitors pick something up that matches — then it runs checkout on that product.

You’ll see pings in the **Monitor Feed** window in the GUI when monitors fire. Your checkout tasks listen for those same pings.

**You still pick a checkout mode** (Normal, Frontend, Session, etc.). Monitor Feed only changes *where the product comes from* — the monitor ping instead of a URL in the task row.

---

## Keywords

Same idea as search keywords:

- `pokemon` — title must contain this
- `+pokemon,+booster` — must contain all of these
- `+pokemon,-plush` — must contain pokemon but not plush

Case doesn’t matter.

Leave **Product URL** empty when Monitor Feed is on.

---

## Miniso

Turn on **Monitor Feed** on the task.

Use **NORMAL** mode for the fastest path when a ping lands (the bot can use product id / SKU from the monitor and skip loading the product page).

What the task does:

1. Starts and waits on the feed
2. Monitor restocks something that matches your keywords
3. Task grabs that product and runs checkout

You don’t need a product URL in the CSV. Keywords are required.

---

## Hamleys

Turn on **Monitor Feed** on the task.

For restock speed, **FRONTEND** or **SESSION** mode works best. When the monitor sends a product id, the bot can add to cart without opening the product page first.

What the task does:

1. Waits on the feed (Session mode logs in first — see below)
2. Restock ping matches your keywords
3. Checkout runs on that product

Keywords required. No product URL in the task row.

---

## Hamleys Session Mode

Session mode is for people running **Monitor Feed + login** who don’t want to log in again on every ping.

**What you need on the task:**

- Mode: **SESSION**
- Monitor Feed: **on** (turns on automatically when you pick Session)
- Login: **on**
- Account: an email from your `accounts.csv` (saved account — not catchall signup)
- Keywords: your filter
- Profile + card as usual

**What happens:**

1. Task logs in once with your saved account
2. Sits on the monitor feed, keeping the session alive (checks every ~20 seconds — you won’t see spam in the logs unless something breaks)
3. When a restock matches, it uses that **same logged-in session** to checkout — no fresh login each time

If checkout fails because the item is OOS or the SKU got pulled, it clears that product and goes back to waiting for the next ping. Session stays up.

---

## Hamleys FRONTEND vs SESSION

| | FRONTEND + Monitor Feed | SESSION + Monitor Feed |
|---|---|---|
| Login each ping? | Usually yes | No — stays logged in |
| Best for | Occasional restocks | Sitting on feed for hours |
| Account | Saved or catchall (FRONTEND rules) | **Saved account only** |

Both can use product id from the monitor for quick add-to-cart.
