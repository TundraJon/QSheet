# QSheet

Quick Spreadsheet — a fast, lightweight, HTML-based spreadsheet app. Runs in any browser, independent of operating system.

Live at: https://tundrajon.github.io/QSheet/ (once GitHub Pages is enabled)

## Working rules

- **Never build or edit code without explicit instruction.** A bug report or feature request gets logged to `## Build Queue` below — with a precise description and, where possible, a root-cause diagnosis — and nothing else. Code only gets written once the user explicitly says to build it (e.g. "Go ahead and build," "Build them sequentially"). Reporting a bug is not, by itself, a request to fix it.
- **This applies to design instructions too, not just bug reports.** A message describing what something should look like or how it should behave is still just a design decision to log to `## Build Queue` — it is NOT build permission, even when it reads like an instruction. Only something to the effect of "build the build queue" / "go ahead and build" authorizes writing code. If there is even a 1% chance a message might not be a build instruction, ask before touching any code.

## Status

Project just started. Build documents go in `docs/`.

- [x] Build documents added to `docs/`
- [x] Initial code added (`index.html`, built from the V3 shell)
- [ ] GitHub Pages enabled

## Build Queue

Logged from the V2 → V3 gap review. Nothing here is approved to build yet. Full spec is in `docs/build-queue.md`; V2 is kept in `docs/reference/qsheet-v2.html` for reference only.

### A. Regressions (in V2, lost in V3)

1. ~~CSV export/import and Clear Sheet missing (BQ-01.2).~~ **Done — see Build Log 1.**
2. ~~Bottom status bar with Count/Sum/Average missing; `updateStats()` no longer writes anything (BQ-01.1).~~ **Done — see Build Log 1.**
3. ~~`IF`, smart-IF guessing, and AutoSum removed.~~ **Done — see Build Log 1.**
4. ~~Merge/Unmerge removed (BQ-04.2).~~ **Done — see Build Log 1.**
5. ~~Official Table no longer styles headers or sorts; it only stores a rule (BQ-09.1/09.2).~~ **Done — see Build Log 1.**
6. ~~"In Progress" amber conditional rule removed (BQ-05/06).~~ **Done — see Build Log 1.**
7. ~~List-validation warning removed (BQ-09.3).~~ **Done — see Build Log 1.**
8. ~~Formula Wizard live preview removed (BQ-07).~~ **Done — see Build Log 1.**

### B. Bugs in V3

9. ~~Cell-reference math is broken: `=A1+B1` returns `#ERROR!` (reference-replacing regex requires spaces around the reference).~~ **Done — see Build Log 1.**
10. ~~Freeze Panes: offsets assume 80px columns but no widths are set; sticky `<tr>` rows are unreliable (BQ-10.2).~~ **Done — see Build Log 3.**
11. ~~Cell range selection is mouse-only (`onmousedown`/`onmouseover`); range select does not work by touch on Pixel 7. This blocks stats, Insights, and range formatting.~~ **Done — see Build Log 1.**
12. Number format dropdown does nothing (also true in V2) (BQ-04.3).

### C. Never built in V2 or V3

13. ~~Formula functions (BQ-02/03): all except `SUM`, `AVERAGE`, `COUNT`, `MIN`, `MAX` (V2 also had `IF`). Includes SUMIF/SUMIFS, COUNTIF/COUNTIFS, AVERAGEIF/AVERAGEIFS, IFS/AND/OR/NOT/IFERROR, text, date, financial (PMT/FV/PV/NPV/IRR), info (ISBLANK etc., CELL), and lookups (XLOOKUP, VLOOKUP, HLOOKUP, INDEX, MATCH, OFFSET, INDIRECT, CHOOSE, FILTER, SORT, UNIQUE).~~ **Done — see Build Log 4.**
14. ~~Nested formulas / real formula parser (arguments are currently split on commas).~~ **Done — see Build Log 4.**
15. ~~Conditional formatting: Top/Bottom rules, above/below average, 3-color heatmap, custom formula rules, entire-row toggle, preset palettes, plain-English rule summaries (BQ-05/06).~~ **Done — see Build Log 7.**
16. ~~Formula Wizard: category/keyword search, per-function argument fields with descriptions (BQ-07).~~ **Done — see Build Log 5.**
17. Smart guessing for `IF` (5+ signatures), `XLOOKUP`/`VLOOKUP`, and `PMT` headers (BQ-08.2).
18. Time Travel: visual drawer, scrub forward/back, restore formatting/merges/rules (currently `alert()` restore of cell data only) (BQ-08.4).
19. Sort & Filter header dropdowns, value checklist filtering with search, view masking in aggregates (BQ-09.2).
20. Structured table references (`=SUM(Sales[Amount])`) and auto-expanding tables (BQ-09.1).
21. Data validation: date rule, touch picker sheets, integrity toasts (BQ-09.3).
22. Flash Fill and custom-delimiter Text to Columns (BQ-09.4).

### D. Pixel 7 / mobile fit (target device)

23. Mobile-optimized touch targets, layouts, and modals across all UI (critical rule).
24. Performance check for 200 rows x 52 cols (10,400 inputs; full-grid DOM queries on every selection and keystroke) (BQ-10.4).
25. ~~Sidebars (320px) take most of a phone screen; need a bottom-sheet or full-screen treatment.~~ **Done — see Build Log 2.**

### E. Recommended approach (pending approval)

26. ~~Keep the V3 shell (ribbon tabs, multi-sheet bar, settings, cell dock, freeze modal) and rebuild on a proper formula engine and touch selection model, restoring section A items.~~ **Done — see Build Log 1.**

### F. Reported after Build Log 2

27. ~~Freeze Panes: reported "on with no way to clear." Freeze defaults to 1 row + 1 column on every sheet, and the only control is the modal under View / Insert → Freeze Panes… (set both numbers to 0 to clear), which is easy to miss and always opens showing 1 / 1 instead of the current setting. Diagnosis only, not built: needs a visible Unfreeze/Clear control and the modal should show current values (related to item 10).~~ **Done — see Build Log 3.**
28. ~~Cell formatting (bold, italic, underline, font, text color, fill, alignment) was kept only on the screen, not in the cell data. It was lost on sheet switch, on Time Travel restore, and on rebuilds; separately, every edit to any cell wiped all fill colors (the conditional-formatting redraw blanked every cell's background). Also: Time Travel could crash when restoring a checkpoint taken before a sheet was added.~~ **Done — see Build Log 6.**
29. **My Lists — reusable named lists for data validation (decisions made, not approved to build; waits for item 30).** Requested: from the Settings menu the user can create / edit / delete common lists (example: `Str, Dex, Con, Int, Wis, Cha`) and use them for validation drop-downs and the cell-tap picker. Open questions: where lists are stored (they must survive closing the app — the app has no saving yet), whether a validation rule is *linked* to a list (edits flow through) or gets a *copy*, what happens when a list that is in use is deleted, how items are entered on a phone (one per row vs commas), starter lists, whether invalid entries warn or block, and whether to bundle with item 21 (picker sheet + date rule). **Decided:** build autosave (item 30) first; lists start empty (no starter lists); a value not in the list only warns (never blocks); include a "fill the selected cells with this list" button.
30. ~~**Autosave (DISCUSSION, not approved to build).** Proposed: the whole workbook (all sheets, values, formulas, formatting, rules, validations, tables, merges, freeze, settings) saves itself shortly after each change and when the app goes to the background, and reopens exactly as left; stored in the browser's IndexedDB (room for big sheets) with persistent storage requested; Time Travel history not saved; a New Workbook button (with confirm); Export / Import workbook file (.json) as a backup. **Decided:** ONE autosaved workbook (no named workbooks). **Save status line (requested):** a thin line at the very top of the app, above the header, showing the save state like Excel — Saved (with time) / Saving… / Unsaved changes / Save failed (tap to retry). **Settings option (requested):** an AutoSave on/off setting (default on). When off: changes are not saved until the user taps the status line (Save now), and the app warns before leaving with unsaved changes.~~ **Done — see Build Log 8.**
31. **Bigger sheets (DISCUSSION, not approved to build).** Question: the grid is fixed at 200 rows x 52 columns — what if more is needed? Findings: cell data is already stored sparsely (only used cells), so size does not affect saves beyond two numbers per sheet; the real cost is the screen (every cell is an on-screen input, 10,400 now). Two separate features: (a) append more rows / columns at the end (cheap, safe — needs per-sheet row/column counts, saved with the workbook, and a sensible cap), and (b) insert / delete rows or columns in the middle (large: it must rewrite formulas, merges, conditional-formatting ranges, validation ranges, tables, freeze panes and Time Travel). Whole-column rules would need an open-ended range (like A:A) to keep up with growth. Suggested order: autosave (30) first with a versioned save format, then (a), measure on the real phone, then (b).
32. **Save status line wording (requested, awaiting go-ahead to build).** Left side of the top status line reads `AutoSave ON` when on, and `AutoSave OFF - (💾 Save)` when off. `(💾 Save)` is only a visual label that shows where to tap — it is NOT a separate button. The whole strip is the single tap target, and tapping it ALWAYS saves, even when nothing has changed (rewrites the save, updates the time, shows a brief Saved message). Right side keeps the Saved / Saving… / Unsaved changes / Save failed state; the "— tap to save" tail on Unsaved changes would be shortened since the label now says it.

## Build Log (completed)

### Build Log 1 — Item 26: engine + touch foundation on the V3 shell

Built as `index.html` from the V3 shell (ribbon, multi-sheet bar, settings, cell dock, freeze modal all kept). Verified with an automated browser test run in a Pixel 7–sized emulated Chromium (70 checks passing); not yet tried on a physical phone.

- [x] **Formula engine** (Queue 9, 13-base, 14-base): real tokenizer + parser + evaluator. Handles cell refs (any case, `$A$1`), ranges, `+ - * / ^`, unary minus, comparisons, parentheses, nested calls, error values (`#DIV/0!`, `#NAME?`, `#VALUE!`, `#REF!`, `#CIRC!`, `#ERROR!`), circular-reference detection, and dependency-ordered recalculation (formulas can reference formulas below/after them). Functions are a registry (`FUNCTIONS`) ready for the Queue 13 functions. Currently registered: SUM, AVERAGE, COUNT, MIN, MAX, IF. AVERAGE now ignores blank/text cells.
- [x] **Touch selection** (Queue 11): tap selects, tap the selected cell again edits, long-press then drag selects a range (edge auto-scroll), plain drag scrolls. Cells no longer pop the keyboard on a tap. Header taps now actually select the column/row. Mouse: click, drag, double-click. Keyboard: arrows, Enter/F2, Tab, Esc, type-to-edit.
- [x] **Restored (Queue 1):** CSV export/import (real CSV quoting; round-trips formulas and zeros) and Clear Sheet, under the Data tab.
- [x] **Restored (Queue 2):** bottom status bar with live Count / Sum / Average.
- [x] **Restored (Queue 3):** IF (and the smart `=IF` guess) and AutoSum (Home tab).
- [x] **Restored (Queue 4):** Merge / Unmerge (Home tab); merges are stored per sheet and survive grid rebuilds.
- [x] **Restored (Queue 5):** Official Table header styling and sorting (tap a table column header). Sorting moves only the table's columns; formulas in sorted rows become plain values (a toast says so).
- [x] **Restored (Queue 6):** "In Progress" amber conditional rule, plus the Entire-row toggle (it existed in V2 and was lost in V3).
- [x] **Restored (Queue 7):** list-validation warning; validation now runs when a cell is committed instead of on every keystroke.
- [x] **Restored (Queue 8):** Formula Wizard live preview.
- Small shell tweaks needed for the above: page height uses `100dvh`; cell dock and toast moved up to clear the new status bar.

### Build Log 2 — Item 25: sidebars as bottom sheets + column width

- [x] **Column width:** every column is now a fixed 80px (it was ~230px, from the browser's default input width), so about 4–5 columns fit on a Pixel 7 instead of 1–2. Set in one place (`--col-width` in the CSS). 80px also matches the offsets the frozen-column code already assumed.
- [x] **Insights / Time Travel (BQ-08.3 / 08.4):** on phone-width screens they slide up as a bottom sheet (full width, max ~42% of screen height, sitting above the sheet tabs and status bar) instead of a 320px side panel that squeezed the grid. Only one sheet is open at a time, the grid can still be scrolled so the last rows clear the sheet, the cell dock hides while a sheet is open, the close button is a 40px touch target, Time Travel entries are taller, and Insights refreshes when opened. Wider screens keep the side-panel layout.

### Build Log 3 — Items 10 + 27: Freeze Panes fixed, Unfreeze button

- [x] **Unfreeze button** (View / Insert tab, next to Freeze Panes…): one tap clears all frozen rows and columns. It greys out when nothing is frozen and tracks each sheet separately.
- [x] **Freeze Panes… dialog** now opens showing the current rows / columns (it always showed 1 / 1 before) and states the limit for the screen. Requests larger than the limit are trimmed with a message so the grid can never be frozen into a state that can't scroll (about 2 columns / 14 rows on a Pixel 7).
- [x] **Freeze layout rebuilt** from the real measured cell positions instead of guessed sizes. Fixed: frozen row/column were see-through (scrolled text showed through), row numbers piled up at the top while scrolling, and a divider line now marks the freeze edge. Keyboard navigation no longer scrolls the active cell underneath the frozen area.
- [x] Changing the freeze no longer rebuilds the grid, so cell formatting (bold, colors, fonts) is no longer lost when you freeze or unfreeze. (Switching sheets / Time Travel still drops formatting — separate known issue.)
- Note: new sheets still start frozen at 1 row + 1 column (unchanged); say the word if the default should be "nothing frozen".

### Build Log 4 — Item 13: function library (+ no frozen panes by default)

- [x] **New sheets (and the first sheet) now start with nothing frozen.** Freeze Panes… and Unfreeze work as before.
- [x] **Math & aggregation:** SUM, AVERAGE, COUNT, COUNTA, MIN, MAX, MEDIAN, PRODUCT, ABS, SUBTOTAL (1–6, 9 and 101–106, 109), ROUND, ROUNDUP, ROUNDDOWN.
- [x] **Conditional:** SUMIF, SUMIFS, COUNTIF, COUNTIFS, AVERAGEIF, AVERAGEIFS. Criteria can be numbers, text, comparisons (`">25"`, `"<>apple"`) and wildcards (`"ap*"`, `"?????"`).
- [x] **Logic & errors:** IF, IFS, AND, OR, NOT, IFERROR.
- [x] **Text:** CONCAT, TEXTJOIN, LEFT, RIGHT, MID, LEN, TRIM, CLEAN, UPPER, LOWER, PROPER, SUBSTITUTE.
- [x] **Dates:** TODAY, NOW, DATE, DATEDIF (Y, M, D, MD, YM, YD), YEAR, MONTH, DAY, EOMONTH. Dates are stored as serial numbers; cells whose formula is a date function (DATE, TODAY, NOW, EOMONTH) display as `YYYY-MM-DD`.
- [x] **Financial:** PMT, FV, PV, NPV, IRR (money paid out is negative; checked against published spreadsheet reference values).
- [x] **Information:** ISBLANK, ISNUMBER, ISTEXT, ISERROR, ISERR, CELL (`address`, `row`, `col`, `contents`, `type`).
- [x] **Lookups & references:** VLOOKUP, HLOOKUP, XLOOKUP (exact, next smaller/larger, wildcard, search direction, if-not-found), INDEX, MATCH, OFFSET, INDIRECT, CHOOSE, FILTER, SORT, UNIQUE.
- [x] **Engine additions needed for the above:** blocks of cells (ranges) are now real 2-D values; operators work on whole ranges (`=A1:A10>5`, `=A1:A3*2`); results that are several cells (FILTER, SORT, UNIQUE, a bare `=A1:A5`, XLOOKUP returning a row) **spill** into the cells below / beside the formula, show `#SPILL!` if something is in the way, and update/shrink as the data changes. Formulas that read spilled cells work. Wrong argument counts return `#VALUE!`. New error codes: `#N/A`, `#SPILL!`, `#CALC!`.
- Known limits (not in the queue, say if you want any): the `&` text-join operator is not supported (use CONCAT — added in Build Log 5); SUBTOTAL does not skip filtered-out rows (filtering is queue item 19); dates before 1 Mar 1900 are off by one day; arithmetic on dates (`=TODAY()+7`) shows a plain number until number formats (item 12) exist; a spill that feeds another spilling formula more than two levels deep may need one extra edit to settle.

### Build Log 5 — Item 16: Formula Wizard + the `&` operator

- [x] **Wizard rebuilt (BQ-07)** around a catalog of all 67 functions (every function the engine has; a test fails if the two ever get out of step).
  - **Search + category:** type a word (`loan`, `lookup`, `sum if`, `retirement`, `join text`) and the list narrows; names that start with what you typed rank first. Category dropdown filters the list. Each row shows the name, category and a one-line plain-English description.
  - **Argument boxes** are generated for the chosen function: a syntax line (`PMT(rate, nper, pv, [fv], [type])`), then one box per argument with its name, a required / optional tag, a description, and an example as the placeholder.
  - **"+ Add" / "Remove last"** for functions that take any number of arguments or condition pairs (SUM, SUMIFS, COUNTIFS, IFS, CHOOSE, ...).
  - **Live Preview** shows the finished formula and its result as you type (errors are explained in words; FILTER / SORT / UNIQUE show the block they will produce). Formula + result stay pinned at the bottom of the dialog so they are visible while you scroll through the boxes. Missing required boxes are shown as `‹name›` and Insert stays disabled until they are filled.
  - **Typing help:** plain words are put in quotes for you (`apple` → `"apple"`, `>50` → `">50"` for conditions); cell references, numbers and tests such as `A2>100` are left alone. If cells are selected on the grid, the first range box is filled with that selection.
  - **Skipped optional boxes** get their default so later ones still line up (e.g. SORT with only sort_order filled becomes `=SORT(A1:A5,1,-1)`).
  - **One-tap Insert** writes the formula into the active cell (validated like any other entry) and adds it to Time Travel.
- [x] **`&` text-join operator:** `=A1&" "&B1`, `="Total: "&SUM(A1:A3)`, `=INDIRECT("A"&2)`. Binds looser than `+ -` and tighter than comparisons, works on whole ranges.
- [x] **Empty arguments** are now allowed, as in a spreadsheet: `=XLOOKUP(x,a,b,,-1)` skips if_not_found (keeps the `#N/A`).
- Not done (not requested): percent literals such as `5%` in formulas (type `0.05`).

### Build Log 6 — Item 28: formatting is no longer lost (sheet switch, Time Travel, edits)

- [x] **Formatting is now stored with each cell** (`data[cell].style`) instead of only on screen, so it travels with the sheet. Bold / italic / underline / font / text color / fill color / alignment now survive switching sheets, Time Travel restores (each checkpoint carries its own formatting), Clear Sheet (wipes it, as intended), grid rebuilds, freeze / unfreeze, merging, and CSV import (existing formatting on a cell is kept when the import fills it).
- [x] **Fill colors are no longer wiped by editing.** The conditional-formatting redraw used to blank every cell's background after every edit. It now only touches the cells a rule colored and puts each back to its own fill color when the rule stops applying; rule colors still show on top while they apply.
- [x] **Spilled cells keep their formatting** when a spill is rebuilt or shrinks.
- [x] **Time Travel no longer crashes** when you restore a checkpoint taken before a sheet was added (it lands on the last sheet that exists).
- Not changed: sorting a table still leaves formatting where it is (it does not move with the rows); number formats are still not built (item 12).

### Build Log 7 — Item 15: conditional formatting rules (BQ-05 / BQ-06)

- [x] **Rule builder** (View / Insert → Cond. Formatting) with every rule type:
  - **Highlight cells:** greater than, less than, equal to (numbers or text, any case), between (inclusive, either order), text contains.
  - **Top / bottom:** top N, bottom N, top N %, bottom N % (ties at the cut-off are included; a small % still colors at least one), above average, below average. Only numbers are ranked.
  - **3-color scale (heatmap):** smooth gradient from the lowest value through the median to the highest.
  - **Custom formula:** any true/false formula, written for the first cell of the range and adjusted for every other cell (`$` locks a column or row, e.g. `=$C2="Done"` colors by column C). `MOD(ROW(),2)=0` gives zebra stripes.
- [x] **Target range box** (e.g. `A5:D26`): pre-filled with your current selection, or the whole column of the active cell. Bad ranges are explained and block Add.
- [x] **Entire-row toggle** works with every rule type except the heatmap (whole rows of gradient make no sense, so the toggle hides).
- [x] **Ready-made looks (BQ-06):** Soft Red, Soft Green, Warning Amber for highlight rules and three subtle heatmaps (Green → Yellow → Red, Red → Yellow → Green, Blue → White → Red). Tapping one fills the color pickers, which you can still change. Rules color the fill and the text.
- [x] **Plain-English live summary** pinned at the bottom of the dialog, with a sample swatch and how many cells the rule colors right now (e.g. "In A1:A10, any cell that is between 30 and 60 gets the Soft Red look. Right now it colors 4 cells.").
- [x] **Rule list with Remove** on the same dialog (added so rules can be undone; later rules paint over earlier ones). Rules belong to their sheet and are part of Time Travel checkpoints. Rules made by older versions (preset names) still work.
- [x] **New functions MOD, ROW, COLUMN** (needed for the `MOD(ROW(),2)=0` example in the spec); they are in the Formula Wizard too, so it now lists 70 functions.
- [x] **Selection tint is see-through**, so fill colors and rule colors stay visible while cells are selected (before, the blue selection hid them).
- Notes: blank cells are never colored by a rule; greater/less than compare numbers only; rules are redrawn after every edit (a rule over all 10,400 cells redraws in well under half a second).

### Build Log 8 — Item 30: AutoSave

- [x] **The app now saves itself.** One workbook (all sheets, values, formulas, formatting, conditional-formatting rules, validations, tables, merges, freeze panes, which sheet you were on) is saved about a second after you stop changing things, and immediately when the app goes to the background. It reopens exactly as you left it, with a small "Restored your workbook" message.
- [x] **Save status line** across the very top (above the header), like Excel's: `AutoSave on` on the left and `● Saved 3:25 PM` / `● Saving…` / `● Unsaved changes — tap to save` / `● Save failed — tap to retry` on the right. Tapping it saves right now.
- [x] **AutoSave setting** (Settings, first row, on by default). When off, nothing is saved until you tap the status line, and the app warns before you leave with unsaved changes. It also warns while a save is failing. Switching it back on saves whatever was waiting. The other three settings are remembered between visits as well.
- [x] **Storage:** the browser's IndexedDB (persistent storage requested). If IndexedDB cannot be opened at all it falls back to localStorage. A failed write is shown in the status line and retried every few seconds. Each write replaces the previous save in one step, so it can't be left half-written.
- [x] **Compact, versioned save format:** only cells that hold something, each distinct format stored once, calculated values left out (recalculated on open). Five completely full sheets: about 1.3 MB (numbers), 2.1 MB (text), 2.5 MB (text with formatting on every cell) instead of 2.4 / 3.2 / 11.8 MB. Time Travel history is not saved (it starts fresh with "Opened workbook").
- [x] **Data tab → New Workbook / Export Workbook / Import Workbook.** New Workbook asks first and starts empty. Export writes `qsheet-workbook.json` (backup, or to move to another device). Import checks the file, asks before replacing, and refuses non-QSheet files and files from a newer version.
- [x] **Safety:** anything read from storage or an imported file is checked and cleaned (sheet names, rules, colors, option lists, table / merge / freeze numbers) and shown as plain text, so a file can't inject code. An unreadable or too-new saved copy is kept under a separate key instead of being overwritten; the app starts blank and says so.
- Not done: several named workbooks (decided: one for now); saving Time Travel history; cloud sync.

