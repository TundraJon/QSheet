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

13. Formula functions (BQ-02/03): all except `SUM`, `AVERAGE`, `COUNT`, `MIN`, `MAX` (V2 also had `IF`). Includes SUMIF/SUMIFS, COUNTIF/COUNTIFS, AVERAGEIF/AVERAGEIFS, IFS/AND/OR/NOT/IFERROR, text, date, financial (PMT/FV/PV/NPV/IRR), info (ISBLANK etc., CELL), and lookups (XLOOKUP, VLOOKUP, HLOOKUP, INDEX, MATCH, OFFSET, INDIRECT, CHOOSE, FILTER, SORT, UNIQUE).
14. Nested formulas / real formula parser (arguments are currently split on commas).
15. Conditional formatting: Top/Bottom rules, above/below average, 3-color heatmap, custom formula rules, entire-row toggle, preset palettes, plain-English rule summaries (BQ-05/06).
16. Formula Wizard: category/keyword search, per-function argument fields with descriptions (BQ-07).
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

