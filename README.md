# 🏠 House Schedule

A gamified family chore tracker for our household. It shows each day's chores, who they belong to, and lets the kids check things off in real time across all their devices. Streaks, badges, and a weekly head-to-head keep it fun.

**Live site:** https://skeptiksz.github.io/House-Schedule/

---

## What it does

- **Daily chore agenda** — tap any date to see that day's chores and who's assigned to each.
- **Two kids, fair rotation** — Isabella and Victoria swap rotating chores every other week. Shared chores ("Both") and parent chores never rotate.
- **Real-time sync** — check off a chore on one phone and it updates everywhere instantly.
- **Streaks & rivalry** — a head-to-head "Streak Showdown" with flames, a crown for the leader, and a weekly tug-of-war.
- **Trophy case** — 12 badges the girls can earn.
- **Rewards** — parent-defined perks unlocked by streaks and weekly wins.
- **Frozen history** — past days are locked and always show who *actually* did each chore, no matter how the schedule changes later.
- **PIN login** — each profile (Isabella, Victoria, Parents) is protected by a PIN.

---

## How it's built

The entire app is a **single `index.html` file** — all the HTML, styling, and logic live in that one file. There's no build step and no framework.

| Piece | What it does |
|-------|--------------|
| **`index.html`** | The whole app. Hosted on GitHub Pages. |
| **Firebase Realtime Database** | Stores completion records and hashed PINs so everything syncs across devices. |
| **GitHub Pages** | Hosts the file for free at the live URL above. |
| **`House_Chores_Master.xlsx`** | The master chore list. **Local editing tool only — never uploaded to the site.** |

The chore definitions themselves (names, descriptions, day assignments) live **inside `index.html`**. The database only ever stores two things: who checked off what, and the hashed PINs. It never knows the chore names.

---

## The two-file workflow

The app and the spreadsheet work together:

1. **`House_Chores_Master.xlsx`** is the source of truth for the chore list. Edit chores, assignments, and rotation here.
2. The spreadsheet is used to **regenerate `index.html`** whenever the chore list changes.
3. Only `index.html` gets uploaded to GitHub. The spreadsheet stays on your computer.

---

## Chore IDs

Every chore has a permanent ID like `KIT-010` or `PET-050`. These IDs are the database keys, so completion history stays attached to a chore even if you rename or move it.

**Prefixes**

| Prefix | Category |
|--------|----------|
| `PER` | Personal |
| `KIT` | Kitchen |
| `RST` | Restrooms |
| `UPS` | Upstairs |
| `LVG` | Living · Laundry · Dining |
| `LDY` | Laundry |
| `TRS` | Trash |
| `PET` | Pets |
| `OUT` | Outdoor |

**Rules**

- **Never change** an existing ID — even if you rename or rewrite the chore.
- **Never reuse** a deleted ID — retire it permanently.
- **New chores** get the next number in that category's sequence (IDs go in steps of 10, e.g. `KIT-050` after `KIT-040`, leaving room to insert between).

The same rules are printed at the bottom of the **Chores** tab in the spreadsheet.

---

## Rotation & frozen history

**Rotation** — Rotating chores (assigned `I` or `V` in the spreadsheet) swap between the girls every other week. The spreadsheet always describes **Week A**; the app automatically generates **Week B** by swapping. Shared (`B`) and parent (`P`) chores never swap.

**Frozen history** — The rotation is anchored to a fixed point in time, so any given calendar date's owners are permanent. Past days are **read-only** and always display who actually completed each chore, pulled from the stored record — so changing the schedule or editing the chore list never rewrites the past.

---

## Updating the app

When you have a new version of `index.html`:

1. Go to the repository on github.com.
2. Click **`index.html`** → the **pencil (Edit)** icon.
3. Use **Add file → Upload files** to upload the new version, replacing the existing one.
4. Add a short commit note (e.g. "Updated chores") and **Commit changes**.

GitHub Pages redeploys automatically within about a minute. Note that the URL is **case-sensitive**: `/House-Schedule/`.

---

## Editing the chore list

1. Open **`House_Chores_Master.xlsx`**.
2. Edit on the **Chores** tab:
   - **Chore ID** (column A) — follow the ID rules above for new chores.
   - **Category**, **Chore Name**, **Description**.
   - **Sun–Sat** columns — enter a code per day: `I` (Isabella), `V` (Victoria), `B` (Both), `P` (Parents), or blank (no one). Fill in **Week A only** — the app generates Week B automatically.
3. Save the spreadsheet and use it to regenerate `index.html`, then upload as above.

---

## Firebase

The app connects to a Firebase Realtime Database. The config (including the API key) is embedded in `index.html` — this is normal and safe for Firebase web apps, because the key only identifies the project and is locked down two ways:

- **API key restriction** — the key only works from `skeptiksz.github.io` (set in Google Cloud Console → APIs & Services → Credentials).
- **Database rules** — currently open read/write, which is fine for a private family app that only stores chore checkmarks.

**Forgot the Parents PIN?** Delete the `auth/parent` node in the Firebase console; the app will prompt to set a new one on next login.

---

## Resetting

A reset wipes all completion history (streaks and badges reset with it):

1. Log in as **Parents**.
2. Scroll to the bottom and tap **Reset all check-offs**, then confirm.

---

## Privacy

This is a private family tool. The only data stored is chore completion records (date, chore ID, which child, timestamp) and hashed PINs. No names, messages, or personal information beyond the kids' first names are kept in the database.

---

## Repository contents

```
House-Schedule/
├── index.html     # the entire app (this is what's hosted)
└── README.md      # this file
```

The master spreadsheet (`House_Chores_Master.xlsx`) is kept locally and is intentionally **not** in this repo.
