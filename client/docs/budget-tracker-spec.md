# Budget Tracker — UI Design Spec

Reference document for front-end build. Pair this with `budget-tracker-mockups.html` for visual context.

## Project context

- **Stack:** MERN (MongoDB, Express, React, Node.js)
- **Form factor:** Progressive web app, desktop-first, responsive
- **Scope (this phase):** Planning only — no actual expense tracking or variance reporting. Backend will be built after the front-end.

## Color palette

| Role        | Hex       | Usage                                                        |
|-------------|-----------|--------------------------------------------------------------|
| Primary     | `#B5A167` | Accent gold — active nav, CTAs, icons, focus borders, highlights |
| Primary     | `#FFFFFF` | Surfaces, cards, table backgrounds                           |
| Secondary   | `#003366` | Navy — top nav, headings, primary text, dark stripe rows     |

Supporting neutrals used in mockups:
- `#F5F3EC` — soft cream (section backgrounds, sidebars, callouts)
- `#FAF7EE` — lighter cream (selected/highlighted states, total columns)
- `#EFEDE5` — page background (outside the app shell)
- `#e5e5e5` / `#eee` / `#d8d8d8` — borders, dividers
- `#333` / `#666` / `#888` — body text tiers

## Typography

- Font stack: `-apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif`
- Headings: 500 weight, navy `#003366`
  - H1 (page title): 22–24px
  - H2 (section): 18–20px
- Body: 13–14px, weight 400
- Table cells: 11–12px
- Labels / micro: 11px uppercase, letter-spaced 0.5px

## Global components

### Top navigation (persistent)
- Background `#003366`, height ~52px
- Left: wallet icon (`ti ti-wallet`, gold) + "Budget Tracker" wordmark
- Center: 5 tabs — Setup, Income, Expenses, Categories, Overview
- Active tab: gold text `#B5A167` with 2px gold underline
- Inactive tabs: white at 50% opacity
- Right: bell icon + circular avatar (gold bg, navy initials)

### Buttons
- **Primary:** gold `#B5A167` bg, navy `#003366` text, no border, 6px radius
- **Secondary:** white bg, navy text, 1px navy border, 6px radius
- **Tertiary/dashed:** transparent bg, gold dashed border, gold text

### Icon library
Tabler Icons (`@tabler/icons-webfont`). Common icons used:
`ti-wallet`, `ti-bell`, `ti-calendar`, `ti-plus`, `ti-copy`, `ti-edit`, `ti-trash`, `ti-check`, `ti-x`, `ti-grip-vertical`, `ti-download`, `ti-printer`, `ti-home`, `ti-home-2`, `ti-car`, `ti-shopping-cart`, `ti-heartbeat`, `ti-device-tv`, `ti-pig-money`

---

## Screen 1 — Setup

**Purpose:** First-run / new-budget flow. Capture budget name and starting month.

**Layout:**
- Top nav (Setup tab active)
- Step indicator strip (cream bg): 4 steps — Setup ▸ Income ▸ Expenses ▸ Review. Active step has gold circle with navy number; future steps have outlined gray circle.
- Form area (white, padded 40px/60px), max-width ~520px:
  - Budget name input
  - Starting month picker — large card with gold border showing selected month, plus a calendar-icon button to change it
  - Info callout (cream bg, gold left border): "Your budget will run: [start] through [start+11]"
  - Primary CTA: "Continue to Income →"
  - Secondary CTA: "Cancel"

---

## Screen 2 — Income planning

**Purpose:** Spreadsheet entry of income sources × 12 months.

**Layout:**
- Top nav (Income active)
- Header strip: title "Income sources", subtitle "Plan income for [range] · Click any cell to edit"
- Actions (right side): "Fill across" (secondary) and "Add source" (primary)
- Spreadsheet table:
  - 14 columns: Source name | Jan–Dec | Total
  - Header row: cream bg, navy text, gold bottom border
  - Body rows: alternating subtle stripe
  - Total column: cream bg `#FAF7EE`, navy bold
  - Active/editing cell: 2px gold border on white
  - Footer row "Total income": navy bg, white numbers, gold label, gold annual total

---

## Screen 3 — Expense planning by category

**Purpose:** Same spreadsheet pattern as income, with categories as grouping mechanism.

**Layout:**
- Top nav (Expenses active)
- Two-column split:
  - **Left sidebar (220px, cream bg):** Category filter list with icon + name + item count badge. Active filter has white bg with gold left border. "Manage categories" dashed button at bottom (links to Screen 4).
  - **Main area:** header with title/subtitle and action buttons, then a grouped spreadsheet
- **Grouped spreadsheet:**
  - Each category is a section: navy header row spanning all columns with "▾ CATEGORY NAME" (expanded) or "▸ CATEGORY NAME — collapsed · N items · $total" (collapsed)
  - Items indented inside expanded groups
  - Subtotal row per category: cream bg, italic label, navy bold totals, gold bottom border
  - Same total-column treatment and editing-cell treatment as Screen 2

---

## Screen 4 — Category management

**Purpose:** CRUD for expense categories. Drag to reorder.

**Layout:**
- Top nav (Categories active)
- Header: "Manage categories" + "New category" primary button
- 2-column grid of category cards:
  - Each card: drag handle (left) + icon tile (navy bg, gold icon) + name + meta ("N expenses · $X/yr") + edit/trash actions (right)
  - Normal state: 1px gray border, white bg
  - Editing state: 2px gold border, cream bg, "EDITING" badge next to name, check/x actions instead of edit/trash
- Empty-state CTA at bottom: dashed gold border card "+ Add a new category"

---

## Screen 5 — 12-month overview (dashboard)

**Purpose:** Read-only summary of the plan.

**Layout:**
- Top nav (Overview active)
- Header: "[Year] budget overview" + date range subtitle, with Export and Print buttons
- **4 metric cards (grid):**
  1. Planned Income — cream bg
  2. Planned Expenses — cream bg
  3. Net Projected — *navy bg, white number, gold label* (featured)
  4. Avg Monthly — cream bg
- **Bar chart:** Monthly income vs expenses
  - Paired bars per month (navy = income, gold = expenses)
  - Gridlines + y-axis labels ($0 / $5k / $10k / $15k)
  - Legend top-right
- **Two-column footer:**
  - **Left:** "Expenses by category" — horizontal bar list, alternating navy/gold fills, with $ amount and % on the right
  - **Right:** "Monthly cash flow" — table (Month / Income / Expenses / Net) with abbreviated middle rows and a navy total row with gold label and gold net number

---

## Interaction states to implement

- Cell focus/edit: 2px gold border on white background
- Active nav tab: gold underline + gold text
- Hover on table rows: subtle background (TBD, suggest `#FAFAFA`)
- Drag handle hover: cursor `grab`
- Category card editing mode: 2px gold border + cream bg + state-change icons

## Suggested component hierarchy

```
<AppShell>
  <TopNav tabs="setup|income|expenses|categories|overview" />
  <main>
    <SetupScreen />          // wizard with StepIndicator + form
    <IncomeScreen />         // BudgetGrid (flat rows)
    <ExpenseScreen />        // CategorySidebar + BudgetGrid (grouped rows)
    <CategoriesScreen />     // CategoryCard grid (sortable)
    <OverviewScreen />       // MetricCards + BarChart + CategoryBreakdown + CashFlowTable
  </main>
</AppShell>
```

Shared components likely worth extracting: `BudgetGrid`, `EditableCell`, `MetricCard`, `IconTile`, `StepIndicator`, `TopNav`.

## Data model hints (for backend later)

```
Budget {
  id, name, startMonth (YYYY-MM), userId, createdAt
}
Category {
  id, budgetId, name, iconName, sortOrder, type: "expense"
}
LineItem {
  id, budgetId, categoryId (null for income), name, type: "income"|"expense",
  monthlyAmounts: [12 numbers]  // or normalized table of {month, amount}
}
```

## Out of scope this phase

- Actual expense tracking
- Variance vs plan
- Multi-currency
- Sharing / multi-user
- Recurring transaction logic beyond planned monthly amounts
