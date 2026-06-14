# 🏆 World Cup 2026 — Bracket Challenge

A web-based knockout bracket prediction game for the 2026 FIFA World Cup. Players predict who advances through every round of the knockout stage, from the Round of 32 all the way to the Champion.

Built as a single HTML file backed by a Supabase database. Supports 200–300 concurrent players.

---

## How It Works

### For Players
1. Visit the site URL and enter your **name** and a **password** (min 6 characters, letters + numbers)
2. Set a **password hint** on first registration to help you remember your password
3. Fill in the entire bracket — pick who advances from each match, round by round
4. Your picks cascade automatically: the team you pick in R32 becomes an option in R16, and so on
5. Hit **Save my picks** before the deadline
6. Once the tournament starts, return anytime to check your score on the **Leaderboard**

### For the Admin
- Log in via the **Admin password** field on the login page
- **Set up R32 teams** once the group stage draw is confirmed
- **Lock entries** before the first match kicks off
- **Enter match results** round by round as the tournament progresses — scores update automatically for all players
- **View participant brackets** by clicking any player row to expand their full pick breakdown

---

## Scoring System

| Round | Points per correct pick | Bonus | Max |
|---|---|---|---|
| Round of 32 | 1pt | +4pt if all 16 correct | 20pt |
| Round of 16 | 2pt | +4pt if all 8 correct | 20pt |
| Quarterfinals | 4pt | +4pt if all 4 correct | 20pt |
| Semifinals | 10pt | — | 20pt |
| Champion | 20pt | — | 20pt |
| **Total** | | | **100pt** |

### Tiebreaker
If two or more players finish with the same total points, the player who correctly predicted the **3rd place bronze medalist** ranks higher. The 3rd place pick carries no points — it is a tiebreaker only.

---

## Technical Stack

| Component | Technology |
|---|---|
| Frontend | Single HTML file (vanilla JS, no frameworks) |
| Database | [Supabase](https://supabase.com) (free tier) |
| Hosting | GitHub Pages (free) |

---

## Setup Instructions

### 1. Supabase Database

Create a free project at [supabase.com](https://supabase.com), then run the following in the **SQL Editor**:

```sql
create table participants (
  id bigint generated always as identity primary key,
  name text unique not null,
  pin text not null,
  hint text,
  picks jsonb default '{}',
  updated_at timestamptz default now()
);

create table settings (
  key text primary key,
  value text not null
);

insert into settings (key, value) values
  ('locked', 'false'),
  ('results', '{}');

alter table participants disable row level security;
alter table settings disable row level security;
```

### 2. Configure the HTML File

Open `index.html` in a text editor and update the following lines near the top:

```js
const SUPABASE_URL   = 'https://your-project.supabase.co';
const SUPABASE_KEY   = 'your-anon-public-key';
const ADMIN_PASSWORD = 'your-admin-password';
```

Your Supabase URL and anon key can be found in your Supabase project under **Settings → API Keys**.

### 3. Host on GitHub Pages

1. Create a new **public** GitHub repository
2. Upload `index.html` to the repository root
3. Go to **Settings → Pages** → set source to **Deploy from branch → main → / (root)**
4. Your site will be live at `https://yourusername.github.io/repository-name`

---

## Admin Guide

### Before the Tournament

1. **Update R32 teams** — once the FIFA group stage draw is confirmed, go to Admin panel → *Round of 32 — Team Setup* → Edit teams. Enter all 16 matchups in official bracket order. This automatically clears any existing player picks.
2. **Share the URL** with your players and set a submission deadline
3. **Lock entries** from the admin panel just before the first match kicks off

### During the Tournament

- Enter the winning team for each completed match in the Admin panel → *Enter match results*
- Scores and the leaderboard update instantly for all players
- The 3rd place match result can also be entered for tiebreaker resolution

### Password Reset

If a player forgets their password, they can use the **Forgot password?** link on the login page to retrieve their hint. If they've forgotten both, delete their entry from the admin panel so they can re-register.

---

## Tournament Timeline (2026)

| Milestone | Approximate Date |
|---|---|
| Group stage ends | ~27 June 2026 |
| R32 draw confirmed | ~28 June 2026 |
| Update teams in admin | As soon as draw is confirmed |
| Open entries to players | ~28–30 June 2026 |
| Deadline (lock entries) | Before first R32 match ~4 July 2026 |
| Knockout stage ends | ~3 August 2026 |

---

## Features

- ✅ Visual left-to-right bracket tree (R32 → R16 → QF → SF → Final → Champion)
- ✅ Cascading picks — teams advance round by round based on your selections
- ✅ Live leaderboard with round-by-round score breakdown
- ✅ Results bracket tab — compare actual results against your picks
- ✅ Score breakdown table with bonus point tracking
- ✅ 3rd place bronze medal tiebreaker pick
- ✅ Password hint system for forgotten passwords
- ✅ Admin panel to manage teams, results, entries and lock/unlock predictions
- ✅ Expandable participant view for admins to audit individual brackets
- ✅ Mobile-friendly responsive layout
- ✅ Supports 200–300 concurrent players

---

## License

Free to use for personal and group use. Built with ❤️ for football fans.
