# SSC CGL Daily Quiz Web App 🎯

A professional, Testbook/SSC-style mock test website.
- Student page: `index.html` (name entry, 15-min timer, Save & Next, Mark for Review,
  Clear Response, question palette, result + solutions, Hindi/English Google Translate)
- Teacher dashboard: `admin.html` (passcode-protected live leaderboard with every student's score)
- Questions for Day 1 (Geography, 55 Qs) are in `questions.js`

Every day: paste the new questions into `questions.js`, push to GitHub → Vercel auto-deploys.

---

## 1️⃣ Setup Supabase (5 min, free)

1. Go to https://supabase.com → sign up free → **New Project** (choose any name/password).
2. Open **SQL Editor** and run this once:

```sql
create table quiz_results (
  id uuid primary key default gen_random_uuid(),
  name text not null,
  score numeric,
  total int,
  correct int,
  wrong int,
  skipped int,
  answers jsonb,
  quiz_title text,
  created_at timestamptz default now()
);

alter table quiz_results enable row level security;

create policy "public insert" on quiz_results
  for insert to anon with check (true);

create policy "public read" on quiz_results
  for select to anon using (true);
```

3. Go to **Project Settings → API** and copy:
   - **Project URL**  → into `supabase-config.js` as `SUPABASE_URL`
   - **anon public key** → as `SUPABASE_ANON_KEY`

## 2️⃣ Deploy on Vercel (3 min)

1. Create a GitHub account → **New repository** → name it `ssc-quiz` → upload all these files.
2. Go to https://vercel.com → sign up with GitHub → **Add New → Project** → import `ssc-quiz` → **Deploy**.
3. Done! Your site is live at `https://yourname.vercel.app`
   - Students take the test at the main link.
   - YOU view results at `https://yourname.vercel.app/admin.html` (passcode: `parmar123` — change it inside `admin.html`, line `const ADMIN_CODE=`).

## 3️⃣ Daily routine (per new PDF)

1. Open `questions.js` and replace the `QUESTIONS` array (keep the same format).
2. Change the quiz title inside `index.html` if you like.
3. Commit & push to GitHub → Vercel updates automatically in ~30 seconds. Share the link.

## Marking scheme (SSC CGL Tier-I style)
+2 correct, −0.25 wrong, 0 skipped, 15 minutes. Edit `MARKS_CORRECT` / `MARKS_WRONG` / `TOTAL_MINUTES` at the top of the script in `index.html` anytime.

## Notes
- The whole page (questions + options + solutions) can be toggled between English and Hindi via the built-in Google Translate buttons.
- Works fully offline for taking the test even if Supabase is not configured yet (scores just won't be stored).
