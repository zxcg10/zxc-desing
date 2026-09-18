# zxc-desing — Stack matrix

Matches frontend-design **Skin Read**: name the product and constraints before editing.
Optical target stays fixed; **implementation follows the stack**.

Step 0: this table. **Do not** dump unmapped Tailwind utilities into a non-Tailwind app
before declaring the path.  
Step 0.5 (existing chrome components): [preflight.md](preflight.md).  
Mode A/B paste source for `.skin-*`: [skin-classes.css](../recipes/skin-classes.css).

## 1. Framework

| Stack | Mode | Approach |
|-------|------|----------|
| React + Vite + Tailwind + Radix/shadcn | A | Hang `skin-*` / `dash-*`; chrome per shell.md |
| React + Tailwind (no shadcn) | A/B | Tokens + equivalent classes; copy shell structure |
| **Next.js App Router** | A | Same as React; `html.dark` on root layout; **sticky still inside a client scroll container** (not a sibling of scroll in a Server Layout) |
| **Vue 3 + Tailwind** | A/B | Pure classes / global CSS only; own layout markup for shell; **no** React components or JSX wrappers |
| **Svelte / SvelteKit + Tailwind** | A/B | Same as Vue: `.skin-card` in global or `:global`; Svelte markup for shell |
| Vue + Element Plus / Naive | C→B | CSS variables → theme override; then layout containers |
| React + **MUI** / **Ant Design** | C→B | **Do not fork the library**; map theme tokens; glass only on Layout / Card / chrome DOM |
| Plain PHP / multi-page / no component lib | C | [vanilla-css.css](../recipes/vanilla-css.css) |

### Vue / Svelte hard rules

- Drop React-only assumptions (`forwardRef`, mandatory shadcn `cn`).  
- Deliverable = token file + global `.skin-*` / `.dash-*` + classes on `.vue` / `.svelte`.  
- Do not install React or Radix just to reskin.

## 2. Tailwind v3 vs v4 (required)

| Signal | Version | Recipe |
|--------|---------|--------|
| `tailwind.config.js/ts` + `@tailwind base` | **v3** | [tailwind-v3.config.js](../recipes/tailwind-v3.config.js) + tokens in `index.css` |
| `@tailwindcss/vite` or `@import "tailwindcss"`; config often absent | **v4** | [tailwind-v4.css](../recipes/tailwind-v4.css): register with **`@theme`**; do **not** put primary tokens only in a legacy `tailwind.config.js` |
| No Tailwind | none | [vanilla-css.css](../recipes/vanilla-css.css) |

```bash
rg -n 'tailwindcss|@tailwindcss/vite' package.json
rg -n '@tailwind |@import \"tailwindcss\"|@theme' --glob '*.css' .
ls tailwind.config.* 2>/dev/null
```

**v4:** utility colors come from `@theme` `--color-*`; semantic tokens (`--glass-bg`, `--card-face`) stay on `:root`; skin classes use `var(--glass-bg)` directly.

**Ban:** editing only `tailwind.config.js` on a v4 app so `bg-primary` dies, then papering over with ad-hoc CSS.

## 3. CSS-in-JS / MUI / Ant Design

Goal: **do not rewrite the library**—inject variables + restyle chrome.

### Principles

1. Mount zxc `:root` / `.dark` (vanilla or recipes).  
2. Point the library Theme / ConfigProvider `primary/bg/text/border/radius` at `var(--*)`.  
3. Add glass classes (or equivalent `sx` / `styles`) **only** on Layout / Sider / Header / page-level Card shells.  
4. Theme Button/Input when possible; otherwise `:root` + light global overrides; **keep original DOM**.  
5. Say in the reply: “theme override + chrome DOM; library source not forked.”

### MUI

```ts
createTheme({
  palette: {
    primary: { main: 'var(--primary)' },
    background: { default: 'var(--background)', paper: 'var(--card)' },
    text: { primary: 'var(--foreground)', secondary: 'var(--muted-foreground)' },
    error: { main: 'var(--destructive)' },
  },
  shape: { borderRadius: 14 },
})
```

AppBar/Drawer → glass; Paper → `var(--card-face)` + `var(--elev-1)`.

### Ant Design

```ts
ConfigProvider({
  theme: {
    token: {
      colorPrimary: 'var(--primary)',
      colorBgBase: 'var(--background)',
      colorBgContainer: 'var(--card)',
      colorText: 'var(--foreground)',
      colorBorder: 'var(--border)',
      borderRadius: 14,
      fontFamily: 'var(--font-sans)',
    },
  },
})
```

Override the default solid Sider (`#001529`); table headers follow [i18n.md](i18n.md).

## 4. Decision tree

```
Tailwind present?
  ├─ v3 → recipe v3 + :root tokens
  ├─ v4 → recipe v4 @theme + :root tokens
  └─ no → vanilla-css.css
MUI / AntD / Element?
  └─ yes → mode C theme map; promote to B if chrome DOM is editable; never fork the lib
Vue / Svelte?
  └─ yes → pure classes; do not port React components
Can edit sidebar/toolbar DOM?
  ├─ yes → shell.md hard rules
  └─ no → palette only; do not claim glass chrome is done
```

## 5. Class strategy

| Environment | Strategy |
|-------------|----------|
| Global CSS allowed | `skin-*` / `dash-*` / `pressable` |
| CSS Modules / scoped | `:global(.skin-card)` or bake recipes into the module |
| MUI `sx` / AntD `classNames` | Tokens first; hang glass on containers |
| DOM frozen | Theme variables only |
