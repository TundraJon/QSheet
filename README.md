# QSheet

Quick Spreadsheet — a fast, lightweight, HTML-based spreadsheet app. Runs in any browser, independent of operating system.

Live at: https://tundrajon.github.io/QSheet/ (once GitHub Pages is enabled)

## Working rules

- **Never build or edit code without explicit instruction.** A bug report or feature request gets logged to `## Build Queue` below — with a precise description and, where possible, a root-cause diagnosis — and nothing else. Code only gets written once the user explicitly says to build it (e.g. "Go ahead and build," "Build them sequentially"). Reporting a bug is not, by itself, a request to fix it.
- **This applies to design instructions too, not just bug reports.** A message describing what something should look like or how it should behave is still just a design decision to log to `## Build Queue` — it is NOT build permission, even when it reads like an instruction. Only something to the effect of "build the build queue" / "go ahead and build" authorizes writing code. If there is even a 1% chance a message might not be a build instruction, ask before touching any code.

## Status

Project just started. Build documents go in `docs/`.

- [ ] Build documents added to `docs/`
- [ ] Initial code added
- [ ] GitHub Pages enabled

## Build Queue

Logged from the V2 → V3 gap review. Nothing here is approved to build yet. Full spec is in `docs/build-queue.md`; V2 is kept in `docs/reference/qsheet-v2.html` for reference only.

### A. Regressions (in V2, lost in V3)

1. CSV export/import and Clear Sheet missing (BQ-01.2).
2. Bottom status bar with Count/Sum/Average missing; `updateStats()` no longer writes anything (BQ-01.1).
3. `IF`, smart-IF guessing, and AutoSum removed.
4. Merge/Unmerge removed (BQ-04.2).
5. Official Table no longer styles headers or sorts; it only stores a rule (BQ-09.1/09.2).
6. "In Progress" amber conditional rule removed (BQ-05/06).
7. List-validation warning removed (BQ-09.3).
8. Formula Wizard live preview removed (BQ-07).

### B. Bugs in V3

9. Cell-reference math is broken: `=A1+B1` returns `#ERROR!` (reference-replacing regex requires spaces around the reference).
10. Freeze Panes: offsets assume 80px columns but no widths are set; sticky `<tr>` rows are unreliable (BQ-10.2).
11. Cell range selection is mouse-only (`onmousedown`/`onmouseover`); range select does not work by touch on Pixel 7. This blocks stats, Insights, and range formatting.
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
25. Sidebars (320px) take most of a phone screen; need a bottom-sheet or full-screen treatment.

### E. Recommended approach (pending approval)

26. Keep the V3 shell (ribbon tabs, multi-sheet bar, settings, cell dock, freeze modal) and rebuild on a proper formula engine and touch selection model, restoring section A items.

## Build Log (completed)

_Nothing built yet._
