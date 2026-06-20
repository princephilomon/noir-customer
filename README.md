# Noir — Customer Ordering Site

This is the guest-facing menu and ordering page, opened when someone scans the QR code at their table.

## Files
- `index.html` — the full ordering experience (menu, cart, place order)
- `config.js` — your Supabase credentials go here

## Setup

### 1. Connect to Supabase
This site needs to talk to the **same Supabase project** as your staff dashboard (separate repo). If you haven't set that up yet, see the staff repo's README first — it includes the SQL to create the shared `orders` table.

Once you have your Supabase project:
1. Go to **Project Settings → API**
2. Copy your **Project URL** and **anon public** key
3. Open `config.js` and paste them in:

```js
const SUPABASE_URL = "https://yourproject.supabase.co";
const SUPABASE_ANON_KEY = "your-anon-key-here";
```

### 2. Test locally
```bash
python3 -m http.server 8000
```
Open: `http://localhost:8000/?table=12`

Place a test order — it won't show anywhere visually here, but it's written to the database. Check it shows up on your staff dashboard's Kitchen screen.

### 3. Deploy
Deploy this repo to Vercel, Netlify, or any static host. Once live, each table's QR code should point to:

```
https://your-deployed-url.com/?table=N
```

Swap `N` for each table's actual number. Generate one QR code per table.

## How it works
- The `table` URL parameter pre-fills the table number field, but the guest can edit it
- Guests browse the menu, build a cart (a side drawer styled like an order ticket), enter their name, and place the order
- On submit, the order is inserted into the shared `orders` table in Supabase with status `placed`
- From there, kitchen and floor staff (on the separate staff dashboard) pick it up in real time

## Notes
- If `config.js` is left with placeholder values, the site still works for browsing/demo purposes but orders won't actually sync anywhere (check browser console for a warning).
- No build step — plain HTML/CSS/JS, deployable as-is.
