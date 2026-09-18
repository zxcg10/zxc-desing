# zxc-desing — Preflight (probe before editing)

Generic checklist for **any** existing sidebar/top-bar shell (shadcn Sidebar, custom
shell, Ant Layout, MUI Drawer, etc.). Goal: find what turns glass into a solid wall
**before** writing tokens. Do **not** turn this into a one-repo patch note.

Run once (`SRC` = frontend source root):

```bash
SRC="${SRC:-src}"

# Opaque ancestors (block backdrop sampling)
rg -n -- 'bg-sidebar|bg-background|bg-card|bg-white|background:\s*var\(--card\)' \
  "$SRC" --glob '*.{tsx,jsx,vue,svelte,css}' | head -60

# Sticky may sit outside the scroll container
rg -n -- 'sticky|overflow-y-auto|overflow-auto' \
  "$SRC" --glob '*{Layout,layout,Shell,shell,Sidebar,sidebar}*' | head -40

# Shine / skin classes globally killed
rg -n -- 'skin-toolbar-shine|display:\s*none' "$SRC" --glob '*.css' | head -40

# Theme / skin attribute locks (non-zxc skins override tokens)
rg -n -- 'data-skin|data-theme|admin-skin|theme-provider|ThemeProvider' \
  "$SRC" ../index.html index.html --glob '*.{html,tsx,jsx,vue,ts,js}' 2>/dev/null | head -40
```

## Four questions (put answers in Plan.Scope)

| # | Question | If “yes”, Plan must state the fix |
|---|----------|-----------------------------------|
| 1 | Do glass nodes still have an opaque **inner panel or ancestor** (`bg-*` / solid `background`)? | Make the inner panel transparent; material lives only on `.skin-sidebar` / `.skin-toolbar` |
| 2 | Does the sticky top bar share the **same** `overflow-y-auto` (or equivalent) as main content? | Restructure chrome per [shell.md](shell.md) |
| 3 | Does CSS set `.skin-toolbar-shine` or glass classes to `display: none` / solid fallback **outside** `prefers-reduced-*`? | Remove or narrow to reduced-media queries |
| 4 | Does the document root carry a **non-target** `data-skin` / theme class with a full override sheet? | Switch to the target skin; sync html, storage, runtime apply; search for legacy selectors |

## Component-library pattern (mode A)

Brand-agnostic structure:

```
outer fixed / drawer (may host .skin-sidebar)
 └── inner panel (often defaults to bg-sidebar / bg-card)  ← most common solid wall
main inset (often defaults to bg-background)
 └── if header is a sibling of scroll → toolbar never reads through content
```

Fix principles:

1. **Inner panel → `background: transparent`** (or drop the solid utility).  
2. **Inset / main column → transparent**; ambient light comes only from `.page-mesh`.  
3. **One primary scroller** wraps sticky toolbar + content.  
4. Class contract and optical values: [shell.md](shell.md) + [recipes/skin-classes.css](../recipes/skin-classes.css).

## Theme attributes (generic)

Many products ship multiple skins or `data-*` themes. When applying zxc:

- Pick **one** target attribute value (or only `html.dark` / no `data-skin`).  
- Sync: static HTML, boot script, preference storage, runtime apply.  
- `rg` the old value: ensure no legacy skin sheet out-specificity `.skin-*`.

Do not hardcode attribute names in this skill—follow the target repo’s convention and
state the intended value in the Plan.

## Relation to Build order

Pass Preflight, then follow SKILL: **tokens → chrome → primitives → domain**.  
If chrome is opaque, do not claim primitives are reskinned.
