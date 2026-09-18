# zxc-desing — Element recipes

Merged from real admin / analytics / chat UIs. While building: **restyle what exists;
skip what does not and say so**. When adding a similar surface, follow this table.

Class contract: `skin-*` (chrome / control hooks), `dash-*` (analytics), semantic
Tailwind (`bg-primary`…).

---

## A. Interaction baseline (every clickable)

| Item | Spec |
|------|------|
| Press | `active:scale-[0.97]` or `.pressable`; `100ms`; ease `var(--deck-ease)` |
| Focus | `:focus-visible` ring `var(--ring)` offset 2px |
| Card hover | **No** `translateY` |
| reduced-motion | Keep short fades; press → brightness |
| reduced-transparency | Glass → `var(--card)` |

---

## B. Primitives (shadcn / Radix–style)

Shared `components/ui/` (or equivalents). Hang skin classes; drop `bg-white` / `shadow-sm`.

### B1 Button

| Variant | Spec |
|---------|------|
| Primary / default | `.skin-btn-primary`: vertical gradient (white 38% mix → primary → black 8% mix) + `inset 0 1px 0` top highlight; **no** plain `bg-primary` |
| Secondary | `bg-muted` |
| Outline / Ghost | Light border or transparent + hover muted |
| Destructive | Danger only |
| CTA | Accent family; optional light top highlight |
| Sizes | `h-10` / `sm h-8` / `lg h-11`; `rounded-xl` |

Bare `<button class="dash-btn-primary">` on business pages uses the same primary recipe.

### B2 Input / Textarea / Select

- `.skin-input`: `background: var(--input-fill)`; `box-shadow: var(--input-inset)`; transparent border  
- Focus: primary border + `0 0 0 3px color-mix(primary 22%, transparent)`; inset may soften slightly  
- SelectTrigger speaks the **same language** as Input  
- Secrets / passwords: may mask value; same visual language as Input  

### B3 Label / Switch / Radio / Checkbox

- Label: `text-sm`, not overly bold  
- Switch: checked = `primary`; fully rounded track  
- RadioGroup: selected ring primary  
- Checkbox: native if no component; checked = primary; align in tables  

### B4 Card

- `.skin-card`: `background: var(--card-face)` + `border: var(--card-stroke)` + `shadow: var(--elev-1)`  
- Header/Footer may use `border-border/60` dividers  
- Dark: optional `inset 0 0 0 1px rgba(255,255,255,0.06)`  

### B5 Table

- Headers: `11px`, `font-normal`, `text-muted-foreground`; Chinese **no uppercase**; English secondary headers may use `uppercase` + `tracking-wider` ([i18n.md](i18n.md))  
- Cells: normal weight; numbers `tabular-nums`  
- Sticky thead: `bg-card` / card-face—not a solid gray bar  
- Row hover: very light muted; no heavy shadows  

### B6 Badge / Chip

| Meaning | Treatment |
|---------|-----------|
| Default | muted 12% fill + muted text |
| success / leading | accent tint |
| warning / queued | warning tint |
| info / in progress / lag (business) | primary tint (**not** destructive for “lag”) |
| destructive / failed | destructive tint |

`rounded-full`; `text-[11px]`. Analytics UIs may also use `.dash-chip-*`.

### B7 Progress / compare bars

- Track: `h-2` `bg-muted` `rounded-[4px]`  
- Fill: chart gradient `28% transparent → 72% → solid` (ban short 75%→solid)  
- Compare helpers: `.dash-compare-fill-lead|lag|flat` when present  

### B8 Dialog / AlertDialog

- Overlay: `.apple-overlay` (28% / 48%)  
- Content: solid card + `--elev-2` + card-stroke; **do not** glass the whole dialog  
- Radius `0.875rem`; enter/exit fade+zoom with `deck-ease`  

### B9 Sheet / Drawer

- Sheet: `.apple-sheet`, `blur-strong`, ~350ms open / 200ms close, `deck-ease`  
- Draggable drawers: interruptible; right drawers may show a handle  
- Mobile sidebar: Sheet hosts sidebar content  

### B10 Dropdown / Tooltip / Separator / Breadcrumb

- Dropdown: `border-[var(--card-stroke)]` + `--elev-2`; `deck-ease` ~200ms  
- Tooltip: popover fill, short delay  
- Breadcrumb: muted trail + current semibold foreground; optional `.skin-breadcrumb-*`  

### B11 Tabs

- In-page tabs: selected white / `bg-background` + light shadow; no full primary paint slab  
- Config-style top tab grids: same selected language, looser spacing  

### B12 Skeleton / Empty / Toast / Command

- Skeleton: muted pulse, radius matches controls  
- EmptyState: icon + title + one line + optional CTA; may sit in skin-card  
- Toast: follow theme; success/fail as tints—not giant solid bars  
- Command (cmdk): `.skin-cmd-*` or elev floating panel; input matches skin-input  

### B13 Sidebar primitives

See shell.md. Inner panel `bg-transparent`.

---

## C. Chrome extensions and nav

| Element | Spec |
|---------|------|
| Nav group label | `12px` semibold muted |
| Nav item | `h-9` `text-[13px]` `.skin-nav-item` |
| Brand block | `.skin-brand` translucent |
| Status footnote | Plain text (“system online”, etc.) |
| Theme toggle | Light elev fill; **no** liquid-glass inside sidebar |
| Dry-run / alert strip | Sticky top; warning tint fill |
| Section left rail | Pill / rail selected = primary tint |
| Full-bleed switch | Route-level `p-0` |

---

## D. Analytics surfaces

| Element | Class / spec |
|---------|--------------|
| Panel | `.dash-panel` = card-face + elev + padding |
| Inset block | `.dash-inset` |
| Toolbar | `.dash-toolbar` |
| Title / sub | `.dash-title` 17px / `.dash-sub` 13px muted |
| KPI | `.dash-metric` 28–32px tabular |
| Segmented control | `.dash-seg` / `.dash-seg-btn-on|off` (time windows **not** `bg-primary`) |
| Win/loss / compare | `.dash-compare-*` + delta colors |
| Heat cells | chart-1 opacity steps; no neon |
| Charts (ECharts/Recharts) | series `--chart-*`; axes/grid muted + dashed; no glow fills unless extremely soft |
| Timeline / insights | chips + dash-panel sections |

---

## E. Admin surfaces

| Element | Spec |
|---------|------|
| Config section header | display title + muted description |
| Config form card | Card + Label/Input/Switch/Select + “Test” Outline + “Save” Primary |
| Secret fields | Masked Input; test actions should not compete with the sole primary CTA |
| Sync corridor | Step/corridor viz: nodes = tint fill+stroke; links muted; current = primary; no neon scan lines |
| Exam / gate pipeline | Done = accent, current = primary, failed = destructive tint |
| Setup wizard | Progress + RadioGroup + step cards; primary only on Next/Finish |
| Audit lists | Filter chip row + Card/Table + outline pagination |
| Sync diff dialog | Dialog elev-2; add/remove/change as accent/destructive/warning tint text |
| AI explain panel | Secondary card; Markdown via `.markdown-body`; trigger secondary/outline |
| Dashboard metric cards | skin-card; near-threshold → warning text color, not a red card fill |
| Host/service status | status-dot + Badge; SSE updates must not flash the whole page |

---

## F. Chat surfaces

| Element | Spec |
|---------|------|
| User bubble | Right; `rounded-2xl bg-primary text-primary-foreground`; images OK |
| Assistant bubble | Left; `border` + `bg-card` / light card-face; rejected = muted |
| Multi-part assistant | Same style, vertical gap |
| Thinking block | Muted collapsible; optional mono small text |
| Continue / rule debug | Warning dashed border panel |
| Retrieval strip | Under assistant bubble as inset; scores `font-tech` tabular |
| Composer | Bottom card: textarea + attach + Send Primary; large radius; elev-1 |
| Suggestion pills | Muted fill; hover near primary tint |
| Session sidebar | Role Select, virtual user Dropdown, mode Switch; Separators |
| Empty session | Centered copy + suggestion pills |
| Loading row | Spinner + muted copy |

Ops “AI assistant” pages follow chat + Dialog rules (skill chips, approval Dialog, Markdown message).

---

## G. Knowledge base / profiles / personas

| Element | Spec |
|---------|------|
| KB list card | Card + enable Switch + muted metadata |
| KB engine panel | Cluster of form cards; engine Tabs/Radio |
| File index Progress | Standard progress gradient fill |
| File preview | FluidDrawer / Sheet `apple-sheet` |
| User profile card | Platform groups; expand textarea; ids in mono |
| Persona / character card | Textarea editor; import/export secondary |
| Distill jobs | Dialog + Progress |

---

## H. Auth / accounts / embedded diagrams

| Element | Spec |
|---------|------|
| Login | Full-screen `page-mesh`; centered card-face card; primary gradient CTA |
| Accounts table | Table + Dialog CRUD; multi-session Switch |
| Archify / diagram embed | Full-bleed iframe; outer chrome outline/ghost; do not force glass inside the iframe |
| Diagram notes | Side cards as skin-card |

---

## I. Class cheat sheet

```
skin-sidebar skin-toolbar skin-toolbar-shine skin-brand skin-nav-item
skin-card skin-input skin-btn skin-btn-primary
pressable liquid-glass apple-sheet apple-overlay page-mesh chrome-edge
dash-panel dash-inset dash-toolbar dash-title dash-sub dash-metric dash-num
dash-chip dash-chip-* dash-seg dash-seg-btn-* dash-compare-fill-* dash-delta-*
dash-btn-primary dash-btn-secondary
font-display font-tech markdown-body
```

---

## J. Target probe checklist (copy)

```
[ ] Layout / Sidebar / Topbar
[ ] Button Input Select Switch Card Table Badge
[ ] Dialog AlertDialog Sheet Drawer Dropdown
[ ] Tabs Breadcrumb Command Toast Skeleton Empty
[ ] dash-panel / charts / win-loss bars
[ ] Config tabs / Wizard / Sync / Pipeline / Audit
[ ] Chat bubbles / Composer / KB / Profiles
[ ] Login / Accounts / Archify
```
