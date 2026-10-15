# Prodio — Your Personal Life OS

A luxury-minimal productivity operating system: habits, deep focus, tasks, goals, journal, notes, calendar, analytics, gamification, and an AI coach — wrapped in a calm, premium dashboard.

## Quick start

```bash
npm install
npm run dev
```

Open http://localhost:3000. **That's it** — Prodio runs fully local-first. All data is stored in your browser (localStorage) behind a data layer, seeded with realistic demo data so every screen is alive on first run.

## Features

| Area | What you get |
|---|---|
| **Dashboard** | Greeting + daily quote, progress ring, habit/task/focus/streak stats, mood · water · sleep · energy quick logs, weekly score chart, goal progress, journal prompt, mini calendar |
| **Habits** | Streaks, 30-day completion, weekly dots, monthly heatmap, counted habits (e.g. 8 glasses of water), reminders |
| **Tasks** | List / Kanban (drag & drop) / Calendar views, priorities, categories, subtasks, repeating tasks that respawn on completion |
| **Deep Focus** | Pomodoro + custom timer with animated ring, synthesized ambient sound (rain / forest / white noise — Web Audio, no files), session log, browser notifications |
| **Goals** | Milestones, progress steppers, deadlines, auto-complete with XP celebration |
| **Journal** | Markdown editor + preview, mood, gratitude chips, reflection, timeline, AI reflection |
| **Notes** | Markdown, folders, tags, search, pin, archive |
| **Calendar** | Month / week / agenda, recurring events, drag-to-reschedule tasks *and* events |
| **Analytics** | Weekly/monthly/quarterly productivity scores, focus & habit trends, mood × sleep × energy correlation, habit leaderboard |
| **Gamification** | XP, levels, 13 achievements with unlock toasts, daily streaks |
| **AI Coach** | Morning plan, evening review, weekly summary, task prioritization, habit suggestions, goal breakdown, free chat — grounded in your real data |

## Optional: enable the full AI coach

The coach works out of the box with a deterministic offline engine. To power it with Claude:

```bash
cp .env.example .env.local
# add: ANTHROPIC_API_KEY=sk-ant-...
```

Restart the dev server. The route ([src/app/api/ai/route.ts](src/app/api/ai/route.ts)) automatically switches from the stub to `claude-sonnet-5` (override with `PRODIO_AI_MODEL`). If the API errors, it degrades gracefully back to the stub.

## Optional: connect Supabase (~5 minutes)

Prodio ships Supabase-ready — schema, auth pages, and client helpers are already in place.

1. Create a project at [supabase.com](https://supabase.com)
2. In the SQL editor, run [supabase/schema.sql](supabase/schema.sql) (all tables + RLS policies + signup trigger)
3. Add to `.env.local`:
   ```
   NEXT_PUBLIC_SUPABASE_URL=https://xxxx.supabase.co
   NEXT_PUBLIC_SUPABASE_ANON_KEY=eyJ...
   ```
4. (Optional) Enable Google / Apple providers under **Authentication → Providers** in the Supabase dashboard

The `/login` and `/register` pages activate automatically when the env vars are present; without them they show a "local mode" notice. Syncing the stores to Supabase is the designed next step: each Zustand store maps 1:1 to a table in the schema.

## Architecture

```
src/
├── app/
│   ├── (app)/           # dashboard, habits, tasks, calendar, goals, focus,
│   │                    # journal, notes, analytics, coach, profile, settings
│   ├── (auth)/          # login, register (Supabase-gated)
│   └── api/ai/          # AI coach endpoint (Claude ↔ offline stub)
├── components/          # shell, shared design system, per-feature components
├── lib/                 # dates, stats, gamification rules, seeds, ambient audio,
│   ├── ai/              # prompts, context builder, stub coach
│   └── supabase/        # client helpers (inert without env vars)
├── stores/              # Zustand stores (persisted to localStorage)
└── types/               # shared types mirroring the DB schema
supabase/schema.sql      # full Postgres schema + RLS
```

**Design system:** white glassmorphism, 28px card radii, soft layered shadows, purple→indigo gradients, 8px grid, Lucide icons, full dark mode. Tokens live in [src/app/globals.css](src/app/globals.css).

## Useful things

- **Settings → Your data** — export everything as JSON, or reset to fresh seed data
- **Timer keeps running** across page navigation (module-level interval in the timer store)
- **Ambient sounds are synthesized** with the Web Audio API — zero audio assets

## Deploy

```bash
npm run build   # verify locally
```

Push to GitHub and import into [Vercel](https://vercel.com) — zero config needed. Add your env vars in the Vercel dashboard if using Supabase/Claude.
"# prodio" 
