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
