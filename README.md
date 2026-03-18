# 🌙 Eid Wishes Wall + Salami Collector

<div align="center">

![Eid Mubarak](https://img.shields.io/badge/Eid-Mubarak%20%F0%9F%8C%99-gold?style=for-the-badge&labelColor=0d0d30&color=f0c040)
![Status](https://img.shields.io/badge/Status-Live-brightgreen?style=for-the-badge&labelColor=0d0d30)

**A real-time public Eid celebration platform — community wishes wall with a digital Salami leaderboard.**

[🌐 Live Demo] ( eid-wishes-wall-salami-collector.vercel.app ) 

</div>

---

## ✨ Features

- 🏮 **Public Wishes Wall** — real-time lantern cards with 6 color themes
- 💰 **Salami Collection** — bKash / Nagad with tap-to-copy numbers
- 🏆 **Live Leaderboard** — Gold / Silver / Bronze tier system
- ✨ **Animations** — starfield canvas, crescent moon, floating Eid particles
- 🕌 **Mosque silhouette** footer SVG
- 📱 **Fully responsive** — mobile first design

---

## 🛠️ Tech Stack

| Layer | Technology | Purpose |
|-------|-----------|---------|
| **Frontend** | HTML5, CSS3, Vanilla JS | UI, animations, form logic |
| **Database** | Supabase (PostgreSQL) | Data storage + real-time |
| **Security** | Supabase RLS + anon key | No login required, server enforces rules |
| **Real-time** | Supabase WebSocket channels | Live updates without page refresh |
| **Fonts** | Google Fonts (Amiri, Tajawal) | Arabic + Bangla compatible |
| **Deployment** | Vercel | Zero-config static hosting |

---

## 🗄️ Database Schema

```sql
-- Wishes table
CREATE TABLE public.wishes (
  id         UUID         DEFAULT gen_random_uuid() PRIMARY KEY,
  name       VARCHAR(50)  NOT NULL CHECK (char_length(trim(name)) >= 2),
  message    VARCHAR(280) NOT NULL CHECK (char_length(trim(message)) >= 5),
  theme      VARCHAR(20)  DEFAULT 'gold' NOT NULL,
  created_at TIMESTAMPTZ  DEFAULT now() NOT NULL
);

-- Salamis table
CREATE TABLE public.salamis (
  id         UUID         DEFAULT gen_random_uuid() PRIMARY KEY,
  name       VARCHAR(50)  NOT NULL CHECK (char_length(trim(name)) >= 2),
  amount     INTEGER      NOT NULL CHECK (amount >= 10 AND amount <= 50000),
  method     VARCHAR(20)  DEFAULT 'bKash' NOT NULL,
  created_at TIMESTAMPTZ  DEFAULT now() NOT NULL
);
```

Both tables have **Row Level Security (RLS)** — public read + insert only. No update or delete allowed from the browser.

---

## 🚀 Getting Started

### Prerequisites
- [Supabase](https://supabase.com) account (free tier)
- Any code editor (IntelliJ, VS Code, etc.)
- [Vercel](https://vercel.com) or any other account for deployment

### Setup

**1. Clone the repository**
```bash
git clone https://github.com/YOUR_USERNAME/eid-wishes-wall.git
cd eid-wishes-wall
```

**2. Set up Supabase**
- Create a new project at [supabase.com](https://supabase.com)
- Go to **SQL Editor** → paste and run `supabase-schema.sql`
- Go to **Database → Publications** → run:
```sql
ALTER PUBLICATION supabase_realtime ADD TABLE public.wishes;
ALTER PUBLICATION supabase_realtime ADD TABLE public.salamis;
```
- Go to **Project Settings → API** → copy your **Project URL** and **anon/public key**

**3. Configure keys**

Open `index.html` and update the config block at the bottom of the file:
```javascript
const SUPABASE_URL  = 'https://YOUR_PROJECT_ID.supabase.co';
const SUPABASE_ANON = 'YOUR_ANON_KEY';
const BKASH_NUMBER  = 'YOUR_BKASH_NUMBER';
const NAGAD_NUMBER  = 'YOUR_NAGAD_NUMBER';
```

**4. Run locally**

Open `index.html` via your IDE's built-in server.
In IntelliJ: right-click `index.html` → **Open In → Browser**
URL should show `localhost:XXXX` — not `file://`

---

## 📦 Deployment

```bash
# Push to GitHub
git add .
git commit -m "🌙 Initial deploy — Eid Wishes Wall"
git push origin main
```

Then go to [vercel.com](https://vercel.com) or any other site → **New Project** → import your GitHub repo → **Deploy**. Done in 30 seconds. ✅

---

## 📁 Project Structure

```
eid-wishes-wall/
│
├── index.html              ← Complete single-file app (HTML + JS)
│   ├── HTML markup         — Page structure & all UI components
│   ├── Supabase config     — ⚠️ Replace with your real keys
│   ├── Star animations     — Canvas-based starfield
│   ├── Wish logic          — Submit, load, real-time subscription
│   └── Salami logic        — Leaderboard, confirm form, tier system
│
├── style.css               ← All styles & animations
│   ├── CSS variables        — Color theme tokens
│   ├── Lantern card themes — 6 color variants
│   ├── Leaderboard styles  — Rank badges & rows
│   └── Responsive rules    — Mobile-first breakpoints
│
├── supabase-schema.sql     ← Run once in Supabase SQL Editor
├── .gitignore
└── README.md
```

---

## 🔄 How Real-time Works

```
User submits wish
      ↓
Supabase INSERT → PostgreSQL
      ↓
PostgreSQL triggers replication event
      ↓
Supabase broadcasts via WebSocket
      ↓
All connected browsers receive event instantly
      ↓
New lantern card appears — no page refresh needed ✨
```

---

## 🎨 Wish Card Themes

| Theme | Accent Color | Icon |
|-------|-------------|------|
| Gold | `#f0c040` | 🌙 |
| Emerald | `#2ecc71` | 🕌 |
| Purple | `#9b59b6` | ✨ |
| Crimson | `#e74c3c` | 🌸 |
| Teal | `#1abc9c` | ⭐ |
| Rose | `#e91e8c` | 🌺 |

---

## 🏆 Salami Tier System

| Tier | Minimum | Badge |
|------|---------|-------|
| 🥇 Gold | ৳ 500+ | Gold border |
| 🥈 Silver | ৳ 200+ | Silver border |
| 🥉 Bronze | ৳ 50+ | Bronze border |

> Salami amounts are self-reported (honor system). Real transfers happen directly via bKash/Nagad to the host's number — the site only tracks and displays them publicly.

---

## 🔒 Security

- **Row Level Security** on all tables — no unauthorized writes
- **Rate limiting** via `localStorage` — 1 wish/5 min, 1 salami/10 min per browser
- **Input validation** on both client and database level
- **HTML escaping** on all user content — XSS protected
- **Anon key only** — service role key never sent to browser

---

## 🗺️ Roadmap

### V1 ✅ (Current)
- [x] Real-time wishes wall
- [x] bKash / Nagad salami collection
- [x] Live leaderboard with tier system
- [x] Confetti on submission
- [x] Rate limiting + spam protection

### V2 (Planned)
- [ ] Admin dashboard to remove spam
- [ ] Like / react button on wish cards
- [ ] Share wish as downloadable image
- [ ] Pagination / infinite scroll
- [ ] bKash Merchant API for verified payments
- [ ] Bangla / Arabic / English language toggle

---

## 👨‍💻 Author

**Rafiul Islam**

🎓 IoT & Robotics Engineering · University of Frontier Technology Bangladesh (UFTB)

📧 rafuulislam2004@gmail.com

🌐 [Portfolio](https://portfolio-website-rafiul.vercel.app) · 💼 [GitHub](https://github.com/rafiul254)

---

— free to use, modify, and distribute.

---

<div align="center">

Made with ❤️ for Eid · from Chuadanga, Bangladesh

**عيد مبارك · Eid Mubarak · ঈদ মোবারক 🌙**

</div>
