# mahongg-leaderboard
Mah Jongg Leaderboard for NoLibs Mah Jongg group

⚠️ One thing to know about localStorage on GitHub Pages
The data saves per browser, per device — so if one club member logs a game on their laptop, it won't automatically sync to another member's computer. Since all 12 of you share one leaderboard, I'd recommend designating one person as the "scorekeeper" who logs all results from a single device/browser. That way the data stays consistent.
If you ever want true multi-user sync across devices, that would require a small backend (like Firebase), which I can set up for you — but for a casual club, the single-device approach works great.

# 🀄 NLNA Mah Jongg Club Leaderboard

A live leaderboard for the Northern Liberties Neighbors Association Mah Jongg Club, hosted on GitHub Pages and powered by a Google Sheet. Club members can view the leaderboard from any device. The scorekeeper updates the Google Sheet after each session — no coding required.

---

## How It Works

- The website (`index.html`) is hosted for free on **GitHub Pages**
- It reads data from a **Google Sheet** (two tabs: `Players` and `Games`)
- When a member visits the page and clicks **↻ Refresh**, it pulls the latest data from the Sheet and recalculates all standings on the fly
- The scorekeeper (Ellie) is the only person who needs access to the Google Sheet

---

## Live Site

> `https://YOUR-USERNAME.github.io/mahjong-leaderboard`

Replace `YOUR-USERNAME` with your GitHub username.

---

## One-Time Setup

### 1. Google Sheet Structure

The Sheet needs **two tabs** with exact names and column headers as described below.

---

#### Tab: `Players`

Rename the default "Sheet1" tab to `Players`. Row 1 is the header row. Add one row per club member.

| id | name |
|----|------|
| 1 | Rachel |
| 2 | Louise |
| 3 | Jeff |
| 4 | Donna |
| 5 | Ellie |
| 6 | Monica |
| 7 | Sasha |
| 8 | Sung |
| 9 | Lystra |
| 10 | Jill |
| 11 | Player 1 |
| 12 | Player 2 |

- `id` — a unique number for each player. Never reuse or change a player's id.
- `name` — the display name shown on the leaderboard. You can edit this at any time.

---

#### Tab: `Games`

Add a second tab named `Games`. Row 1 is the header row. Add one row per game after each session.

| date | type | winner_id | points | player1_id | player2_id | player3_id | player4_id | notes |
|------|------|-----------|--------|------------|------------|------------|------------|-------|
| 2026-03-05 | win | 1 | 50 | 1 | 3 | 7 | 9 | 13579_1 |
| 2026-03-05 | wall | | | 2 | 4 | 6 | 10 | |

**Column reference:**

| Column | Required? | Description |
|--------|-----------|-------------|
| `date` | ✅ | Date of the session in `YYYY-MM-DD` format |
| `type` | ✅ | Either `win` or `wall` |
| `winner_id` | For wins only | The `id` of the winning player from the Players tab |
| `points` | For wins only | Total points for the hand (base value + any bonuses) |
| `player1_id` – `player4_id` | ✅ | The `id` numbers of all four players at the table. For wins, the winner must be one of these four. |
| `notes` | Optional | Winning hand identifier (see format below). Leave blank for wall games. |

**Notes / Winning Hand Format:**

Use the `CATEGORY_ROW` convention from the annual NMJL card:

```
13579_1    →  "2024 Hands" section, row 1
NEWS_5     →  "NEWS" section, row 5
CONSEC_3   →  "Consecutive Run" section, row 3
```

Leave `notes` blank for wall games.

---

### 2. Publish the Sheet to the Web

The website reads the Sheet via a public CSV export. The Sheet itself stays private — publishing to web only allows the site to *read* the data, not edit it.

1. In Google Sheets, go to **File → Share → Publish to web**
2. In the first dropdown, select **Players**. In the second, select **Comma-separated values (.csv)**. Click **Publish**, then **OK**.
3. Repeat for the **Games** tab.

You only need to do this once. After that, edits to the Sheet are picked up automatically whenever the page is refreshed.

---

### 3. Upload to GitHub and Enable GitHub Pages

1. Create a new **public** repository on GitHub (e.g., `mahjong-leaderboard`)
2. Upload `index.html` to the repository root — **it must be named `index.html`**
3. Go to the repository **Settings → Pages**
4. Under **Source**, select **Deploy from a branch**, choose `main`, folder `/ (root)`, and click **Save**
5. After about 60 seconds, your site will be live at `https://YOUR-USERNAME.github.io/mahjong-leaderboard`

---

## Updating Scores After a Session

1. Open the Google Sheet and go to the **Games** tab
2. Add one new row per game played, filling in all required columns
3. Google Sheets auto-saves
4. Notify club members to click **↻ Refresh** on the leaderboard page

> 💡 **Tip:** Keep a printed copy of the id-to-name list at sessions so you can quickly look up player id numbers when logging results.

---

## Updating Player Names

To rename a member (e.g., replace "Player 1" with a real name), edit the `name` column in the **Players** tab. The leaderboard will reflect the new name on the next refresh. Do **not** change the `id` number — this is how the app links players to their game history.

---

## Scoring Reference (NMJL Rules)

| Points | Description |
|--------|-------------|
| 25–35 | Standard / easier hands |
| 40–60 | Mid-difficulty hands |
| 65–75 | Difficult hands |
| 80–85 | Hardest / rarest hands |
| +10 | Self-picked bonus (won from the wall) |
| +20 | Jokerless hand bonus |
| +30 | Self-picked AND Jokerless |

Enter the **total** (base + bonus) in the `points` column.

---

## Troubleshooting

**The leaderboard shows "Could not load data from Google Sheets"**
- Make sure both `Players` and `Games` tabs exist with exact names (case-sensitive)
- Make sure the Sheet has been published to the web as CSV for each tab (see step 2 above)
- Check that the column headers in row 1 exactly match the names listed above

**A player's stats look wrong**
- Verify their `id` number is consistent across all rows in the Games tab
- Check that the `winner_id` matches one of the four `player_id` columns in that row

**The page isn't updating after I edit the Sheet**
- Click the **↻ Refresh** button on the leaderboard page (the page does not auto-update)
- Google Sheets can take up to a minute to reflect new data via the CSV export

---

## Club Info

**NLNA Community Center Mah Jongg Club**  
Meets the 1st and 3rd Wednesday of each month, 6:30–8:00pm ET  
700 N 3rd Street, Philadelphia PA 19123  
[nlna.org/community-center](https://www.nlna.org/community-center)

---

## RSVP / Signup Feature

### Overview

The **RSVP tab** on the leaderboard shows who has signed up for the next session. It reads from a Google Form linked to your Sheet. The **Session tab** in the Sheet lets you check off who actually attended, and pre-populates player columns in the Games tab so you only need to fill in the winner and points.

---

### Step 1 — Create the Google Form

1. Go to [forms.google.com](https://forms.google.com) and click **Blank form**
2. Title it something like **NLNA Mah Jongg — Session Signup**
3. Add a single question:
   - Question text: **Your name**
   - Question type: **Dropdown**
   - Add all 12 player names as options: Rachel, Louise, Jeff, Donna, Ellie, Monica, Sasha, Sung, Lystra, Jill, Player 1, Player 2
4. Click the **Send** button (top right) → copy the link — you'll need it in Step 3

---

### Step 2 — Link the Form to your Sheet

1. In the Form editor, click the **Responses** tab
2. Click the **green Sheets icon** ("Link to Sheets")
3. Choose **Select existing spreadsheet** and pick your existing leaderboard Sheet
4. Google will create a new tab called **"Form Responses 1"** in your Sheet
5. **Rename that tab** to exactly `Signups` (right-click the tab → Rename)
6. Publish the `Signups` tab to the web as CSV, same as you did for Players and Games (File → Share → Publish to web → select Signups → CSV → Publish)

---

### Step 3 — Add the Form URL to the leaderboard

1. Open `index.html` in a text editor
2. Find this line near the top of the `<script>` section:
   ```
   const FORM_URL = '';
   ```
3. Paste your Form link between the quotes:
   ```
   const FORM_URL = 'https://forms.gle/yourformlink';
   ```
4. Save and re-upload `index.html` to GitHub

---

### Step 4 — Create the Session tab (attendance checklist)

Add a new tab to your Sheet named `Session`. Set it up as follows:

**Columns A–C: Attendance checklist**

| player_name | player_id | attending |
|-------------|-----------|-----------|
| Rachel | 1 | FALSE |
| Louise | 2 | FALSE |
| Jeff | 3 | FALSE |
| Donna | 4 | FALSE |
| Ellie | 5 | FALSE |
| Monica | 6 | FALSE |
| Sasha | 7 | FALSE |
| Sung | 8 | FALSE |
| Lystra | 9 | FALSE |
| Jill | 10 | FALSE |
| Player 1 | 11 | FALSE |
| Player 2 | 12 | FALSE |

- Select cells C2:C13, then go to **Insert → Checkbox** — this turns the FALSE values into real checkboxes
- At the meeting, check each person who is present

**Columns E–M: Pre-populated game rows (Tables 1–3)**

In column E row 1, add headers matching the Games tab:
`date` | `type` | `winner_id` | `points` | `player1_id` | `player2_id` | `player3_id` | `player4_id` | `notes`

Then in E2, enter today's session date. In columns I–L (player1_id through player4_id), use formulas that pull the IDs of checked players and group them by table. Since you drag names into table groups manually, the simplest approach is:

- Use columns E–F alongside the attendance list as a scratch area to note which player IDs are at each table
- Then manually type the 4 player IDs into the game rows in the Games tab, using the Session tab as your reference

This avoids complex formula logic while still keeping everything in one place.

---

### Resetting Between Sessions

After each session:
1. In the `Signups` tab: select all response rows (not the header) and delete them, **or** go to the Form → Responses → delete all responses
2. In the `Session` tab: uncheck all checkboxes in column C (select C2:C13 → press Delete)

---

### How the RSVP Tab Works

- Shows the next session date and a count (e.g. "7 of 12 signed up")
- Lists each person who submitted the form, in order of submission
- Updates when a member clicks **↻ Refresh**
- The form link button lets members sign up directly from the leaderboard page

