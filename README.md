# Pokémon Deck Checklist Generator

A single-file web tool. Paste a deck list in standard export format and it builds a
printable checklist, plus exports for Notion. No server, no dependencies — just `index.html`.

## What it does

- Parses a deck list into **Pokémon / Trainer / Energy** sections (optional sub-groups too).
- Renders a clean, **printable** checklist with check boxes and calculated totals.
- **Download CSV for Notion** — headers match a Notion database for Merge-with-CSV.
- **Copy rows for Notion** — tab-separated rows you can paste into a Notion table.
- **Save deck as HTML** — exports the rendered sheet as its own standalone file.

## Deploy on GitHub Pages

1. Create a new repository (e.g. `pokemon-deck-checklist`).
2. Upload `index.html` and this `README.md` (drag-and-drop in the GitHub web UI is fine).
3. Go to **Settings → Pages**.
4. Under **Build and deployment → Source**, choose **Deploy from a branch**.
5. Select your default branch (`main`) and the `/ (root)` folder, then **Save**.
6. Wait ~1–2 minutes. Your URL appears at the top of the Pages settings:
   `https://<your-username>.github.io/pokemon-deck-checklist/`

That URL is the live tool.

## Use it on Discord

Discord can't run the tool inside a message — paste the Pages URL instead. Discord's
crawler reads the Open Graph tags in `index.html` and shows a preview card (title +
description). Clicking the link opens the tool in a browser.

To add a thumbnail to the card, host an image and add this to the `<head>` of `index.html`:

```html
<meta property="og:image" content="https://<your-username>.github.io/pokemon-deck-checklist/preview.png">
```

## Use it in Notion

The same URL embeds live in Notion: type `/embed`, paste the URL. The tool runs in the
iframe for viewing and printing. Note: file downloads are usually blocked inside an embed,
so grab the CSV by opening the URL in its own browser tab.

## Notion database columns

For **Merge with CSV** to map cleanly, the target Notion database needs these property
names exactly:

| Property | Type     | Notes                                   |
|----------|----------|-----------------------------------------|
| Card     | Title    | Card name                               |
| Qty      | Number   |                                         |
| Category | Select   | Pokémon / Trainer / Energy              |
| Group    | Text     | e.g. Greninja line, Supporters          |
| Set      | Select   | Set code (CRI, TWM, MEG…)               |
| Number   | Text     | Collector number                        |
| Deck     | Select   | Which deck the row belongs to           |

The first import via **Settings → Import → CSV** auto-creates these. After that, open the
database and use **••• → Merge with CSV** to append each new deck. Merging appends rows and
does not dedupe, so don't import the same deck twice.

## Deck list format

```
Pokémon: 11
Greninja line:
4 Froakie CRI 20
2 Frogadier CRI 21
Trainer: 13
Supporters:
2 Boss's Orders MEG 114
Energy: 3
7 Basic {W} Energy MEE 3
Total Cards: 60
```

- A line starting with **Pokémon**, **Trainer**, or **Energy** begins a section.
- Any other line ending in a colon (e.g. `Supporters:`) is a sub-group label.
- Card lines are `quantity name SET number`.
- `Total Cards:` lines are ignored; totals are recalculated from quantities.
