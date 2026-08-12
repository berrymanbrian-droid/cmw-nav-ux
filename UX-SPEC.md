# CMW UX Specification — Implementation Guide

> Reference prototypes: [My Book](my-book.html) | [My Glazes](my-glazes.html) | [My Inspiration](my-inspiration.html) | [Research Test](research-test.html)
> Live previews: https://berrymanbrian-droid.github.io/cmw-nav-ux/

---

## 1. Navigation Overhaul

### Problem
The original site has 3–5 layers of horizontal navigation stacked (top nav → sub-nav → view switcher → shelf tabs → type toggle). This creates confusion about "where am I" and pushes actual content far down the page.

### Solution: Collapsible Left Sidebar

**Replace** the sub-nav bar and view switcher pill bar with a persistent left sidebar.

**Sidebar structure (same on all pages):**

```
NAVIGATE (section label)
├── My Book          → /my-book
├── My Glazes        → /my-book/collection
├── My Clay Bodies   → /clay-bodies
├── My Reports       → /my-book/reports
├── My Presentations → /decks
├── My Stull Map     → /my-book/map
├── My Inspiration   → /inspiration (new page)
│
CALCULATORS
├── Glaze Calculator    → /calculator
├── Clay Body Calculator → /clay-bodies/calculator
│
TOOLS
├── Import        → /import
├── Drop Box      → /glazes/drop-box
├── Workbooks     → /glazes/workbooks
```

**Behavior:**
- Sidebar width: `w-60` (15rem / 240px)
- Position: `sticky top-0 h-screen` — sticks to viewport top on scroll
- Collapsible: chevron button collapses to `width: 0` with 200ms ease transition
- Main content area flexes to fill available space when sidebar collapses
- Transition only on `width`, `padding`, `border-width` (NOT `transition-all` — that causes scroll jitter)
- Active page highlighted with `bg-brand-50 text-brand-700`
- All other items: `text-clay-600 hover:bg-neutral-50 hover:text-clay-900`
- Section labels: `text-[10px] font-bold uppercase tracking-widest text-clay-400`

**What to remove:**
- The horizontal sub-nav bar (My Book / Reports / My Glazes / My Bodies / Import / Drop Box / My Workbooks)
- The view switcher pill bar (Shelves / Collection / Map / Reports / The Commons / Class Commons)
- The "Score — work notebook →" and "My Presentations →" text link buttons

---

## 2. Page Header Pattern

### Before
Full paragraphs of explanatory text taking 4+ lines before content.

### After
Single-line header with inline badges and a dismissable guide panel:

```html
<header>
  <div class="flex items-center gap-3">
    <h1>My Book</h1>
    <!-- Privacy badge -->
    <span class="rounded-full border border-amber-200 bg-amber-50 px-2 py-0.5 text-[11px]">
      Public
    </span>
    <!-- Or for private pages: -->
    <span class="rounded-full border border-green-200 bg-green-50 px-2 py-0.5 text-[11px]">
      🔒 Private
    </span>
  </div>
</header>
```

**Page Guide component** (dismissable onboarding):
- Shows below the header by default
- Purple gradient border, book/beaker/heart icon on left
- 1–3 numbered steps explaining the page
- "Got it, don't show again" dismisses it
- Small "How to use this page" button appears when dismissed
- State persisted to localStorage

---

## 3. Filter System (Faceted Dropdowns)

### Replace inline filter chips with dropdown menus

**Filter bar structure:**
```
Filter: [Cone ▾] [Finish ▾] [Atmosphere ▾] [Folder ▾] [Clear all ✕]
```

**Each dropdown:**
- Button: `rounded-lg border border-silver-300 bg-white px-3 py-1.5 text-xs font-medium`
- Menu: absolute positioned, `w-44` to `w-56`, `rounded-lg border shadow-lg`
- Items: checkboxes with labels, multi-select
- Active state: purple count badge on the button
- "Clear all ✕" appears to the right of the last dropdown when any filter is active

**Folder/Labs special dropdowns:**
- Wider (`min-w-[10rem]`)
- Shows selected value in the button label (e.g., "Iron series" instead of "All folders")
- Highlights with `border-brand-600 bg-brand-50 text-brand-800` when active

**Available filter attributes:**
- Cone: 04, 06, 1, 4, 5, 6, 7, 8, 9, 10, 11, 12
- Finish: Gloss, Satin, Matte, Opalescent, Crystalline
- Atmosphere: Reduction, Oxidation, Wood, Soda
- Folder: user's project folders + "No folder"
- Shelf (My Glazes only): Mainstays, Notebook, Testing, Wishlist, Archive
- Labs (My Glazes only): Lab 1–5 names

---

## 4. View Toggle (Icon-based)

### Replace text-based List/Tiles/Chemistry toggle with icon buttons

```
[≡ list] [⊞ tiles] [⚗ chemistry]
```

- Container: `rounded-lg border border-silver-300 p-0.5`
- Active button: `bg-brand-600 text-white p-1.5`
- Inactive: `text-neutral-400 hover:text-brand-700 p-1.5`
- Icons: 16x16 SVG (list lines, grid squares, beaker/flask)

---

## 5. Glaze Card Design System

### Three distinct card types that should NOT be confused:

**A. Glaze recipe cards** (content):
- Left border colored by cone: green=cone 10, amber=cone 9, blue=cone 6 (`border-l-[3px]`)
- Warm gradient background: `bg-gradient-to-br from-white to-amber-50/30`
- Ingredient tags: `rounded bg-amber-50/60 border border-amber-100/80 px-1.5 py-0.5 text-[10px]`
- Type label at top: `text-[10px] font-bold uppercase tracking-widest text-clay-400` (GLAZE, CLAY BODY, SLIP)
- Shelf tag at top-right (see section 6)

**B. Tool/action cards** (utilities):
- Dashed border: `border-dashed border-silver-300`
- Neutral background: `bg-neutral-50`
- Icon-forward layout
- Compact, single row

**C. Promo/From CMW cards** (marketing):
- Dark inverted: `bg-clay-900`
- White headings, `text-clay-300` body, `text-brand-300` CTAs
- Hover: `hover:bg-clay-800 hover:shadow-lg`

---

## 6. Colored Shelf Tags

Every recipe card shows a colored tag indicating which shelf it belongs to:

| Shelf | Border | Background | Text | Dot |
|-------|--------|------------|------|-----|
| Mainstay | `border-green-300` | `bg-green-50` | `text-green-700` | `bg-green-500` |
| Notebook | `border-neutral-300` | `bg-neutral-50` | `text-neutral-600` | `bg-neutral-400` |
| Testing | `border-amber-300` | `bg-amber-50` | `text-amber-700` | `bg-amber-500` |
| Project | `border-purple-300` | `bg-purple-50` | `text-purple-700` | `bg-purple-500` |
| Wishlist | `border-blue-300` | `bg-blue-50` | `text-blue-700` | `bg-blue-500` |
| Archived | `border-slate-300` | `bg-slate-50` | `text-slate-500` | `bg-slate-400` |

**Tag HTML:**
```html
<span class="inline-flex items-center gap-1.5 rounded-full border [border-color] [bg-color] px-2.5 py-0.5 text-[11px] font-semibold [text-color]">
  <span class="h-2 w-2 rounded-full [dot-color]"></span>
  Mainstay
</span>
```

These tags should appear consistently anywhere a recipe is shown (My Book cards, My Glazes list, search results, calculator, etc.).

---

## 7. Pagination & Show Controls

**Results bar:**
```
[count] recipes in [shelf]  |  Show [12 ▾]  |  Sort by [Recently updated ▾]  |  [view toggle icons]
```

**Pagination (below grid):**
```
Showing 1–12 of 18     [Prev] [1] [2] [Next]
```

- Hide pagination when all items fit on one page
- Reset to page 1 on filter change, sort change, or shelf change
- "Show" options: 12, 24, 48, 96
- Sort options: Recently updated, Name (A–Z), Name (Z–A), Cone

---

## 8. Bulk Selection (My Glazes)

**Select all checkbox** above the grid:
```
☐ Select all 65 visible
```

**Bulk toolbar** (appears when 1+ items selected):
```
[65 selected] [Move to folder ▾] [Set subset...] [Set labs...] [Delete selected] ... [Clear selection]
```

- Toolbar: `rounded-lg border border-brand-200 bg-brand-50/50 px-4 py-2.5`
- "Set labs" button: amber styled (warning)
- "Delete selected" button: red styled (danger)
- "Clear selection" right-aligned

---

## 9. Dark Mode

**Trigger:** Toggle `class="dark"` on `<html>` element.

**Global overrides needed (add as `<style>` block):**
```css
.dark .text-clay-400, .dark .text-clay-500, .dark .text-clay-600,
.dark .text-clay-700, .dark .text-clay-800 { color: #e8e4e0 !important; }
.dark .text-clay-900 { color: #fff !important; }
.dark .bg-white { background-color: #332b26 !important; }
.dark .bg-neutral-50 { background-color: #1f1a17 !important; }
.dark .border-silver-200, .dark .border-silver-300,
.dark .border-neutral-200 { border-color: #4a4039 !important; }
.dark .bg-silver-100 { background-color: #4a4039 !important; }
```

**Page guide in dark mode:**
```css
.dark #page-guide { background: linear-gradient(to right, #2e1065, #332b26) !important; border-color: #6d28d9 !important; }
.dark #page-guide h2 { color: #fff !important; }
.dark #page-guide p { color: #d4cec8 !important; }
.dark #page-guide strong { color: #fff !important; }
```

**Sidebar in dark mode:**
- `bg-white dark:bg-clay-900`
- `border-neutral-200 dark:border-clay-800`
- Nav items: `dark:text-white dark:hover:bg-clay-800`

---

## 10. Inline Action Buttons

**Replace the bottom "Tools" section cards** on My Book with inline buttons in the search/sort row:

```
[Search___________] Sort by [Recently updated ▾]    [📷 Bulk Image Upload] [⬇ Import Recipes] [📅]
```

- Buttons: `rounded-lg border border-silver-300 bg-white px-3 py-1.5 text-xs font-medium`
- Icons: 14x14 SVG inline with text
- Push to right with `ml-auto` wrapper

---

## 11. My Inspiration Page (NEW)

A visual mood board for saving pottery/glaze inspiration images.

**Layout:** Standard grid (NOT masonry columns), left-to-right row flow.
```css
display: grid;
grid-template-columns: repeat(4, 1fr);
gap: 12px;
```

**Image cards:**
- `rounded-xl border border-silver-200 bg-white overflow-hidden`
- Image with hover scale effect (`group-hover:scale-105`)
- Below image: title, optional note (2-line clamp), colored hashtag tags

**Upload functionality:**
- "Upload Images" primary button opens modal
- Drag/drop zone + file picker
- Fields: Note (textarea), Tags (comma-separated input), Project folder (dropdown)
- Uses FileReader API to display uploaded images client-side immediately

**Detail modal (click any image):**
- Full-screen overlay, image left (large), info panel right (320px)
- Editable title, hashtag tags (add/remove), notes textarea, folder dropdown
- Tag input: `#` prefix built-in, Enter/comma to add, pills with × to remove
- Suggested tags as clickable chips below input
- Save & Close / Delete buttons at bottom

**Privacy:** "Private" green badge next to page title (only you can see inspiration).

---

## 12. Research Test Report Page

**Workflow bar** (in content area, not a site-level nav):
```
[Make: Lab | Calculator | Quantity] [Test: This Test | Design | Sweep] [Fire: Firing] [Compare: Compare]
```
- Each stage has progressively deeper purple tint
- Active item: `bg-brand-600 text-white`
- Container: `rounded-lg border border-silver-200 bg-white p-2`

**Report layout:**
- Header: Test ID + status badge + title + action buttons
- Metadata cards row: Cone, Tiles count, Start date, Last fired date
- Two-column: results/observations/recipe (left 2/3) + test info/project/related/actions (right 1/3)
- Fired results: tile photos in a grid with labels, "best result" highlighted with `ring-2 ring-brand-500`

---

## 13. Footer Styling

**From CMW promo cards** — dark/inverted to distinguish from content:
```html
<a class="block rounded-lg bg-clay-900 p-4 hover:bg-clay-800 hover:shadow-lg">
  <div class="text-[10px] uppercase text-brand-300">Join</div>
  <div class="text-sm font-semibold text-white">CMW membership</div>
  <p class="text-xs text-clay-300">...</p>
  <span class="text-xs font-semibold text-brand-300">See levels →</span>
</a>
```

---

## 14. Brand Colors (Tailwind Config)

The brand color should be **purple**, not blue:

```js
brand: {
  50: '#faf5ff', 100: '#f3e8ff', 200: '#e9d5ff',
  300: '#d8b4fe', 400: '#c084fc', 500: '#a855f7',
  600: '#7c3aed', 700: '#6d28d9', 800: '#5b21b6',
  900: '#4c1d95', 950: '#2e1065'
}
```

---

## 15. Continue Where You Left Off (My Glazes)

A "recently touched" section at the bottom of the page:

```
*Continue where you left off* — your latest tests & recipes

[Card] [Card] [Card] [Card] [Card]   ← 5 across on desktop
```

Each card: dot + type label (GLAZE / TEST · PRE-FIRE), name, date, action button (Start a test → / Batch & print →).

---

## Summary of Removed Elements

1. Sub-nav horizontal tab bar
2. View switcher pill bar (Shelves/Collection/Map/Reports/Commons/Class Commons)
3. "Score — work notebook →" and "My Presentations →" floating text links
4. Bottom "Tools" card section (replaced with inline buttons)
5. Emoji icons on shelf tabs (replaced with plain text)
6. Text-based List/Tiles/Chemistry labels (replaced with icon buttons)
7. Wordy "Timeline — your tests by date →" and "Projects — your lines of inquiry →" links (timeline became an icon button, projects became the Folder filter)
8. Multi-paragraph header descriptions (collapsed into dismissable guide)
