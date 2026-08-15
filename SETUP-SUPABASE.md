# Wiring the Request Access form to Supabase

The form on the site posts emails to a Supabase table. Until you fill in the two
config values, submissions show "Signups aren't live quite yet."

Setup is ~5 minutes, all in your browser:

## 1. Create the project

1. Go to [supabase.com](https://supabase.com) → sign in → **New project**.
2. Name it anything (e.g. `mindframe`), pick a region near your users, set a strong
   database password (you won't need it for this), and create.

## 2. Create the table + security policy

In the dashboard, open **SQL Editor**, paste this whole block, and click **Run**:

```sql
-- Signups table
create table public.signups (
  id         uuid primary key default gen_random_uuid(),
  email      text not null,
  source     text,
  created_at timestamptz not null default now()
);

-- One row per email (case-insensitive); duplicate submits return 409,
-- which the site turns into "you're already on the list".
create unique index signups_email_unique on public.signups (lower(email));

-- Lock the table down: anonymous visitors may INSERT, nothing else.
alter table public.signups enable row level security;

create policy "anon can insert signups"
  on public.signups for insert
  to anon
  with check (true);

-- No select/update/delete policies for anon = the public key cannot read
-- or change the list. Only you (dashboard / service role) can.
```

## 3. Copy the two values into the site

In the dashboard: **Project Settings → API**.

- **Project URL** → e.g. `https://abcdefgh.supabase.co`
- **anon public** API key → a long `eyJ...` string

Open `index.html`, find `MINDFRAME_CONFIG` near the bottom, and fill them in:

```js
const MINDFRAME_CONFIG = {
  SUPABASE_URL: "https://abcdefgh.supabase.co",
  SUPABASE_ANON_KEY: "eyJ..."
};
```

Commit and push. Done.

> **Is the anon key safe to publish?** Yes — that's what it's for, *provided* RLS is
> enabled with insert-only policies as above. Anyone can add an email to the list
> (that's the point of the form); nobody can read, change, or delete the list with
> that key. Never put the `service_role` key in the site.

## 4. Reading your signups

Dashboard → **Table Editor** → `signups`. Export CSV from there, or query:

```sql
select email, created_at from public.signups order by created_at desc;
```
