# GPA Ledger

A single-page, no-backend GPA and CGPA calculator. Add terms, add subjects with credits and grades, and see your term GPA and cumulative CGPA update live. The grade-to-points scale is fully editable, every subject can be left as a placeholder until a grade comes in, and your data can be exported to a formula-driven Excel workbook or backed up as JSON.

**Live:** https://tanaybhadula.github.io/CGPA-Calculator/

No installation, no server, no account. Open `index.html` in a browser and it works.

## Features

- **Term-wise GPA + cumulative CGPA**, both calculated live as you type
- **Editable grade scale** — change what each letter is worth (defaults to A+=4, A=3.5, B+=3, B=2.5, C+=2, C=1.5, D=1), or add/remove grades entirely
- **Add/edit/remove** terms and subjects freely
- **"Not counted (pending)"** grade option — add a subject as a placeholder before you have a grade for it; it's excluded from both GPA and CGPA until you set a real grade
- **Auto-save** to the browser, plus an explicit **Save** button with a "last saved" timestamp
- **Export to Excel** (`.xlsx`) — one sheet per term, a Grade Scale sheet, a Cumulative sheet, and a Read Me sheet, all wired together with real Excel formulas (not just pasted numbers)
- **Export / Import data (JSON)** — move your data between browsers, devices, or a fresh copy of this file

## How to use it

1. Open the [live site](https://tanaybhadula.github.io/CGPA-Calculator/), or open `index.html` in any modern browser (double-click it, or host it anywhere)
2. Click **+ Add term**, then **+ Add subject** inside it
3. Type the subject name, set its credits, and pick a grade from the dropdown (or leave it on "Not counted (pending)" until you have the grade)
4. Term GPA and the overall Cumulative GPA update immediately
5. Open the **Grade scale** panel to edit what each letter grade is worth, if your program's scale differs from the default

### Saving your data

The page saves automatically to the browser's `localStorage` on every edit — no action needed, and it survives closing the tab or restarting your computer. The **Save** button is there for visible confirmation (shows "Saved just now ✓" and a last-saved time), not because auto-save is off.

Important: `localStorage` is tied to where the page is being served from (its *origin*). That means:
- The same `index.html` opened from two different folders, servers, or URLs gets **separate** storage — they won't share data
- A copy hosted on GitHub Pages and a copy opened as a local file are two different origins
- Clearing your browser's site data for that origin, or opening in a private/incognito window, will not have your saved data

To move data between any of these (or just back it up), use **Export data (JSON)** / **Import data (JSON)** in the toolbar.

### Exporting to Excel

The **Export to Excel** button generates a `.xlsx` workbook with:

| Sheet | Contents |
|---|---|
| Read Me | Plain-language explanation of the formulas used |
| Grade Scale | Your current letter → points mapping |
| One sheet per term | Subject, Credits, Grade, plus formula columns for Grade Points (`VLOOKUP` into Grade Scale), Counted in GPA, Weighted Points, and Counted Credits, ending in a Term GPA row |
| Cumulative | Each term's credits/GPA (pulled by formula), rolling up into an overall Cumulative GPA |

Everything downstream is a live formula — editing a grade or credit value inside the spreadsheet recalculates GPA and CGPA on its own, and editing the Grade Scale sheet updates every term sheet that references it.

**Calculation formula** (same in the app and the spreadsheet):

```
Term GPA       = Σ(credits × grade points) / Σ(credits)   — for subjects in that term
Cumulative GPA = Σ(credits × grade points) / Σ(credits)   — across every subject in every term
```

A subject only counts if it has a positive credit value and a grade that matches a letter currently in the grade scale. "Not counted (pending)" (or any unmatched grade) is skipped entirely, not treated as zero.

## File structure

```
.
├── index.html    the entire app — markup, styles, and logic in one file
└── README.md     this file
```

## Tech

Vanilla HTML/CSS/JavaScript — no build step, no framework, no dependencies to install. The Excel export uses [SheetJS](https://sheetjs.com/) (`xlsx.full.min.js`), loaded from a CDN at runtime, purely to generate the `.xlsx` file in-browser.

Fonts: [Lora](https://fonts.google.com/specimen/Lora) and [Nunito Sans](https://fonts.google.com/specimen/Nunito+Sans), loaded from Google Fonts. Palette: deep violet (`#531f75`) and orange (`#f58220`).

## Known limits

- Data lives in the browser only — there's no server, sync, or account system. Use Export/Import (JSON) to move or back up data.
- Opening `index.html` as a local file needs an internet connection the first time, to load the Google Fonts and the Excel-export library from their CDNs; the page itself still works offline, just without the export feature or the custom fonts.
