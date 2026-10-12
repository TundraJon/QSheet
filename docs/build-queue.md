# QSheet Build Queue (BQ-01 to BQ-10)

CRITICAL RULES & ENVIRONMENT SPECIFICATIONS:
>  * Never build or generate code without explicit user instruction. We must always remain in a "wait-to-build" state until the user explicitly commands us to write or modify code.
>  * Target Device: The user is exclusively using a Google Pixel 7 mobile phone and does not have access to a desktop or laptop computer. Every UI element, touch target, button layout, modal, and workflow must be fully optimized, responsive, and frictionless on mobile screens.
> 
Stage 1: Core Engine & Data Grid (BQ-01)
 * BQ-01.1 Grid Layout & Navigation:
   * Variable rows and columns (A-Z, row numbers).
   * Smooth keyboard navigation (Enter, Tab, arrow keys).
   * Cell selection (single cell and rectangular ranges).
   * Live summary statistics display (Count, Sum, Average) for selected ranges.
 * BQ-01.2 File Management:
   * CSV export and import support.
   * Clear sheet functionality.
Stage 2: Comprehensive Formula Engine (BQ-02 & BQ-03)
 * BQ-02.1 Core Math & Aggregation: SUM, AVERAGE, COUNT, COUNTA, MIN, MAX, MEDIAN, PRODUCT, ABS, SUBTOTAL, rounding (ROUND, ROUNDUP, ROUNDDOWN)
 * BQ-02.2 Conditional Calculations: SUMIF, SUMIFS, COUNTIF, COUNTIFS, AVERAGEIF, AVERAGEIFS
 * BQ-02.3 Logic & Error Control: IF, IFS, AND, OR, NOT, IFERROR
 * BQ-02.4 Text & String Processing: CONCAT, TEXTJOIN, LEFT, RIGHT, MID, LEN, TRIM, CLEAN, UPPER, LOWER, PROPER, SUBSTITUTE
 * BQ-02.5 Date & Time Math: TODAY, NOW, DATE, DATEDIF, YEAR, MONTH, DAY, EOMONTH
 * BQ-02.6 Financial Core: PMT (loans/amortization), FV (compound growth/retirement), PV (discounted cash flows), NPV (capital budgeting), and IRR (internal rate of return on uneven cash flows).
 * BQ-02.7 Information & Inspection: ISBLANK, ISNUMBER, ISTEXT, ISERROR, ISERR, CELL
 * BQ-03 Advanced Lookups, References & Data Retrieval: XLOOKUP, VLOOKUP, HLOOKUP, INDEX, MATCH, OFFSET, INDIRECT, CHOOSE, FILTER, SORT, UNIQUE
Stage 3: Formatting & Structure (BQ-04)
 * BQ-04.1 Cell Styling: Font weights (bold, italic, underline), text alignment, text color, and background fill color.
 * BQ-04.2 Structure Controls: Cell merging (colSpan/rowSpan) and unmerging across selected ranges.
 * BQ-04.3 Number Formatting: General, Currency ($), Percentage (%), and Decimal (0.00).
Stage 4: Conditional Formatting & Smart Presets (BQ-05 & BQ-06)
 * BQ-05 Rule Engine Tiers:
   * BQ-05.1 Highlight Cells Rules (Basic comparisons: >, <, =, between, text contains).
   * BQ-05.2 Top/Bottom Rules (Rankings & averages: Top/Bottom 10/%, above/below average).
   * BQ-05.3 Color Scales (3-color heatmaps with smooth gradients).
   * BQ-05.4 Custom Formulas & Row Highlighting (Boolean formula triggers, e.g., =MOD(ROW(),2)=0 for zebra striping).
 * BQ-05.5 UX Workflow Controls:
   * Target range selector (e.g., A5:D26).
   * "Entire row" toggle checkbox to automatically extend formatting across entire rows.
 * BQ-06 Smart Presets & Previews:
   * One-click preset palettes (Soft Alert Red/Green, Warning Amber, Subtle Heatmaps).
   * Plain-English live rule summaries explaining what the rule does before application.
Stage 5: Interactive Formula Wizard (BQ-07)
 * BQ-07 Guided Function Builder Modal:
   * Category and keyword search for complex functions (XLOOKUP, SUMIFS, PMT, etc.).
   * Dynamically generated argument input fields with clear descriptions.
   * Live evaluated result preview and one-click formula insertion into active cells.
Stage 6: Innovative Mobile UX & Intelligence (BQ-08)
 * BQ-08.1 Natural Language Formula Shorthand (Regex-based Parser):
   * Offline plain-English translation for quick calculations (e.g., typing "average sales in b2 to b50" converts to =AVERAGE(B2:B50)).
 * BQ-08.2 Context-Aware Neighborhood & Function Guessing:
   * Aggregation Family (SUM, AVERAGE, MIN, MAX, COUNT): Intelligent neighborhood scanning (upward first, then left) to instantly capture contiguous numeric ranges.
   * Lookups (XLOOKUP, VLOOKUP): Auto-inspects the cell immediately to the left as the default lookup value.
   * Conditional Intelligence (IF): Multi-scenario smart pattern engine with 5+ contextual signatures that analyzes adjacent cells (e.g., detecting variance columns for over/under budget checks, status columns for completion logic, or threshold percentages).
   * Financial Models (PMT): Auto-maps nearby labeled headers ("Rate", "Term", "Amount") into argument slots.
 * BQ-08.3 One-Click "Instant Insights" Sidebar:
   * Passive panel that automatically detects highlighted numeric ranges and surfaces sparkline trends, highest/lowest callouts, and data distribution instantly.
 * BQ-08.4 Interactive "Time Travel" Undo History Visualizer:
   * Visual timeline drawer tracking recent edits and formula changes like a commit history, allowing users to scrub backward and forward safely.
Stage 7: Advanced Data Organization & Structure (BQ-09)
 * BQ-09.1 Official Tables (Ctrl + T Equivalent):
   * Semantic range wrapping with alternating row tints and header controls.
   * Structural referencing engine translating column header names into underlying coordinate ranges in formulas (e.g., =SUM(Sales[Amount])).
   * Dynamic range expansion that automatically carries down formulas and conditional formatting when new rows are added.
 * BQ-09.2 Interactive Sort & Filter:
   * Header dropdown action sheets for one-tap ascending/descending sorting (A-Z, Z-A).
   * Value-based checklist filtering with search bars to temporarily hide non-matching rows.
   * Non-destructive view masking integrated directly into aggregation calculations (SUM, AVERAGE, etc.).
 * BQ-09.3 Data Validation & Drop-Downs:
   * Rule builder for lists, whole numbers, and dates.
   * Mobile-optimized touch-friendly picker sheets for dropdown values to prevent mobile typing errors.
   * Integrity guard toast warnings for invalid manual inputs.
 * BQ-09.4 Flash Fill & Text to Columns:
   * Delimiter-based text splitting wizard (comma, space, hyphen, custom).
   * Pattern-matching heuristic engine for inline smart column filling.
Stage 8: Mobile UX Architecture & Ribbon Redesign (BQ-10)
 * BQ-10.1 Ribbon Tab Navigation (Top):
   * Organized top tabs (Home, Data, View / Insert) replacing cluttered mobile toolbars.
   * Home Tab: Font family selection (8 fonts including 3 handwriting styles: Caveat, Indie Flower, Shadows Into Light), text styling (bold, italic, underline, alignment), fill/text color pickers, and number formats.
   * Data Tab: Official Tables (Ctrl + T), Sort & Filter controls, Data Validation rules, and Text to Columns.
   * View / Insert Tab: Flexible Freeze Panes configuration, Conditional Formatting, and Formula Wizard (fx).
 * BQ-10.2 Flexible Freeze Panes Configuration:
   * Interactive modal selector allowing custom specification of rows and columns to freeze (beyond rigid Row 1 / Col A toggles).
 * BQ-10.3 Multi-Sheet Bottom Tab Bar:
   * Workbook tab bar at the bottom allowing seamless switching between multiple sheets (Sheet 1, Sheet 2, etc.) within the same file.
 * BQ-10.4 Expanded Grid Dimensions:
   * Increased grid scale to 200 rows and columns spanning from A through AZ (52 columns), fully optimized for Pixel 7 performance.
 * BQ-10.5 Settings & Cell Dock Preferences:
   * User-definable settings toggles for quick fx access and Data Validation picker chips on active cell selection.
