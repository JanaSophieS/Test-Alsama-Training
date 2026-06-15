# Alsama Live Poll

A real-time polling page built with plain HTML/JS and Supabase.

---

## 1. Create the Supabase table

In your Supabase project, open the **SQL Editor** and run:

```sql
-- Create table
create table votes (
  id           uuid primary key default gen_random_uuid(),
  option_index integer not null check (option_index between 0 and 3),
  created_at   timestamptz not null default now()
);

-- Enable Row Level Security
alter table votes enable row level security;

-- Allow anyone to insert a vote
create policy "anon insert"
  on votes for insert
  to anon
  with check (true);

-- Allow anyone to read votes
create policy "anon select"
  on votes for select
  to anon
  using (true);
```

Then enable **Realtime** on the `votes` table:
- Go to **Database → Replication**
- Under "Source", toggle `votes` to **ON**

---

## 2. Add your Supabase credentials to index.html

Open `index.html` and find these two lines near the bottom:

```js
const SUPABASE_URL  = 'YOUR_SUPABASE_URL';   // TODO: replace with your value
const SUPABASE_ANON = 'YOUR_SUPABASE_ANON_KEY'; // TODO: replace with your value
```

Replace the placeholder strings with your values from:  
**Supabase → Project Settings → API**

- `SUPABASE_URL` → "Project URL"  
- `SUPABASE_ANON` → "Project API keys → anon / public"

---

## 3. Deploy via GitHub + Vercel

1. Push this repo to GitHub (or it may already be there).
2. Go to [vercel.com](https://vercel.com) → **Add New Project** → import your GitHub repo.
3. Leave all build settings as defaults (Vercel will detect a static site).
4. Click **Deploy**.

The `vercel.json` included here routes all requests to `index.html`, which is correct for a single-page app.

Every `git push` to the main branch will trigger a new Vercel deployment automatically.
