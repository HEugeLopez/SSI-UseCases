# SSI Use-Case Matrix

An interactive, searchable matrix of **Self-Sovereign Identity (SSI)** use cases, mapped to industries with a concrete scenario and benefit for each. It's a single static HTML page that loads its data live from a [Supabase](https://supabase.com) table.

![Status](https://img.shields.io/badge/status-live-success) ![Stack](https://img.shields.io/badge/stack-HTML%20%2B%20Supabase-1f5fe4)

## What it does

- Renders a grouped matrix: each **use case** spans its related **industries**, each with a **specific scenario** and **benefit**.
- **Live search** across industry, scenario, and benefit, with match highlighting.
- **Filter chips** to focus on a single use case.
- Data is fetched at page load from Supabase — edit the database, refresh the page, done. No build step, no framework.

## Project structure

```
.
├── index.html              # The web page (fetches from Supabase)
├── db/
│   └── supabase_setup.sql  # Schema + seed data + read-only access policy
└── README.md
```

> The SQL file is **reference only** — it's the one-time setup that built the database. The live page does not depend on it.

## Setup

### 1. Database (Supabase)

1. Create a project at [supabase.com](https://supabase.com).
2. Open **SQL Editor → New query**, paste the contents of `db/supabase_setup.sql`, and run it. This creates the `ssi_use_cases` table, inserts the rows, and enables a read-only (`SELECT`) policy for anonymous access.
3. Verify: `select count(*) from ssi_use_cases;`

### 2. Connect the page

In **Project Settings → API**, copy your **Project URL** and **anon public** key. Open `index.html` and set them near the bottom of the script:

```js
const SUPABASE_URL  = "https://YOUR-PROJECT-REF.supabase.co";
const SUPABASE_ANON_KEY = "YOUR-PUBLIC-ANON-KEY";
```

### 3. Run it

Open `index.html` in a browser, or host it (see below).

## Editing the data

Don't touch the HTML. Go to **Table Editor → `ssi_use_cases`** in Supabase and add/edit/delete rows directly. The `sort_order` column controls display order and keeps each use case's industries grouped — give new rows a number that slots them where you want. Refresh the page to see changes.

## Deploying

It's a static file, so any static host works:

- **GitHub Pages** — push to a repo, then **Settings → Pages**, set source to `main`. Live at `https://USERNAME.github.io/REPO/`.
- **Netlify / Vercel** — connect the repo; auto-deploys on every push.

## A note on the API key

The **anon** key in `index.html` is safe to expose because Row Level Security is enabled and only grants read access — it cannot modify data. **Never** commit the Supabase **`service_role`** key (the secret one); it bypasses RLS entirely.

## License

MIT — do what you like.
