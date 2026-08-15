# Mindframe — Teaser Site

A static teaser/landing page for **Mindframe**, an AI-native social challenge RPG.
Styled to the game's own colorway (gold/amber, neon violet, teal on near-black) and
built from the game's actual art.

## Files

- `index.html` — the entire site: markup, styles, scripts. No build step, no framework.
- `assets/` — web-optimized art (logo, hero key art, character portraits).
- `SETUP-SUPABASE.md` — how to wire the Request Access form to a Supabase table.

## Running locally

Open `index.html` in any browser, or serve the folder with any static server.

## Email capture

The Request Access form posts to a Supabase table (`signups`) using the public anon
key + an insert-only row-level-security policy. Until `MINDFRAME_CONFIG` in
`index.html` is filled in, the form shows a "not live yet" notice instead of failing.
Full steps: [SETUP-SUPABASE.md](SETUP-SUPABASE.md).

## Content policy

This page intentionally contains **only** public-facing teaser material: the name,
tagline, premise, a handful of characters and art, the six class names, and the genre
line.

It deliberately excludes everything from the pitch deck that isn't for public eyes —
funding figures, budget, pricing and revenue model, investment terms, team details,
market/competitor data, build status and timeline, and the technical architecture
(including any internal tooling screenshots). Keep it that way when editing.
