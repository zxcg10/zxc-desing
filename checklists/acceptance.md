# zxc-desing — Critique / acceptance

Same habit as frontend-design: critique with screenshots while building—*a picture is worth
1000 tokens*. This file operationalizes that when the agent cannot see the user’s screen.  
Rules are **repo-agnostic**.

---

## Critique order

1. **Look first** (L3) — bright chrome shot; dark if possible. Ask: does the sidebar read the mesh? Does content show under the toolbar? Are cards flat white paper? Are primary buttons flat color blocks?  
2. **Calibrate** (L1 grep / optional L2 computedStyle) when the eye cannot tell if tokens applied.  
3. **Quality floor** — mobile usable, focus visible, reduced-motion / reduced-transparency, dark strokes.  
4. **Remove one accessory** — drop decoration that does not serve the zxc brief.

**Claiming “reskinned”:** valid screenshot (state **evidence level** below) + L1 pass.  
No browser → “static pass, optics unverified” only; **never** claim a full reskin.

---

## Evidence levels (degraded paths)

Use the highest available; Critique must name the level.

| Level | When | What you may claim |
|-------|------|--------------------|
| **L3-shell** | Authenticated (or open) shell with sidebar + toolbar; scroll sticky | Chrome + primitives reskinned (dark called out separately) |
| **L3-auth/primitives page** | Shell needs auth and login is unavailable; page has mesh + card + input + primary btn | **Primitives** reskinned; chrome = “structure landed, optics pending shell entry” |
| **L3-static shell preview** | Temporary same-origin HTML that loads injected global CSS + `.skin-*` skeleton (delete after; do not commit) | Chrome optics OK; note “not a product route” |
| **L1-only** | No browser / screenshot failed | **Do not** claim reskin |

Constraints:

- Do not weaken product auth for critique; do not leave mock tokens or disabled 401 handlers in product code.  
- Static shell previews are local throwaways—delete when done.  
- **Dark:** capture one dark chrome shot when a browser is available; if skipped, list it under residual risks—never imply dark passed by default.

---

## L3 — Screenshots (primary evidence)

Any tool works: Chrome DevTools MCP, Playwright, Puppeteer, headless Chrome, user-supplied shots.

| Capture | Look for |
|---------|----------|
| Bright shell | mesh, sidebar translucency, toolbar, first-screen cards |
| Scrolled toolbar | sticky read-through + shine edge cut |
| Dark shell | light inset strokes do not smear into mush |
| Button / input close-up (optional) | specular, inset |
| Auth / primitives page (degraded) | mesh + card-face + input-inset + btn specular |

Self-ask: without the “reskin” story, does this still look like a default solid admin theme? If yes, fail.

---

## L1 — Static grep (required calibration)

`SRC` = frontend source root.  
**Note:** patterns that start with `-` need `rg -n -- '...'` or the shell treats `--glass-bg` as a flag.

```bash
SRC="${SRC:-frontend/src}"

rg -n -- '--glass-bg:|--card-face:|--elev-1:|--input-inset:' "$SRC" --glob '*.css' || true
rg -n 'page-mesh|skin-sidebar|skin-toolbar|skin-toolbar-shine' "$SRC" --glob '*.{tsx,jsx,vue,svelte,css}' || true
rg -n '\bbg-white\b|\bshadow-sm\b|bg-gray-50' "$SRC" --glob '*.{tsx,jsx,vue,svelte}' || true
rg -n 'active:scale-\[0\.97\]|scale\(0\.97\)|\.pressable' "$SRC" --glob '*.{tsx,jsx,vue,svelte,css}' || true
rg -n '::-webkit-scrollbar|prefers-reduced-transparency|prefers-reduced-motion' "$SRC" --glob '*.css' || true
rg -n 'uppercase|tracking-wider' "$SRC" --glob '*.{tsx,jsx,vue,svelte}' || true

# Optional preflight leftovers
rg -n -- 'bg-sidebar|display:\s*none.*shine|skin-toolbar-shine[\s\S]{0,80}display:\s*none' "$SRC" --glob '*.{tsx,css,jsx,vue}' || true
```

| Check | Pass |
|-------|------|
| Tokens | glass-bg / card-face / elev-1 present |
| Chrome | page-mesh + skin-sidebar + skin-toolbar |
| Press | 0.97 or pressable |
| Reduced prefs | reduced-transparency or reduced-motion |
| Solid leftovers | no large new bg-white/shadow-sm (call out legacy) |

---

## L2 — computedStyle (optional)

```js
() => {
  const pick = (sel) => {
    const el = document.querySelector(sel);
    if (!el) return null;
    const s = getComputedStyle(el);
    return {
      bg: s.backgroundColor,
      bgImage: (s.backgroundImage || '').slice(0, 120),
      backdrop: s.backdropFilter || s.webkitBackdropFilter,
    };
  };
  const root = getComputedStyle(document.documentElement);
  return {
    lang: document.documentElement.lang || null,
    glassBg: root.getPropertyValue('--glass-bg').trim(),
    cardFace: root.getPropertyValue('--card-face').trim(),
    sidebar: pick('.skin-sidebar'),
    toolbar: pick('.skin-toolbar'),
    card: pick('.skin-card, .dash-panel'),
    btn: pick('.skin-btn-primary, .dash-btn-primary'),
    input: pick('.skin-input'),
  };
}
```

Expect: `glass-bg` ~0.28 α; backdrop contains `blur(`; card/btn `bgImage` has `linear-gradient`; input shows inset-like box-shadow.

---

## Critique in the reply (keep short — frontend-design style)

User-facing critique is **a few lines**, not a dump of this checklist. Internally run L1 (and L2/L3 when available); in the reply only:

1. **Evidence level** (name it)
2. **1–2 optical findings** (glass / cards / buttons / dark) — prefer screenshots over prose
3. **Claim** yes|no + **residual risks** only if real

Do **not** paste full L1 grep tables, quality-floor inventories, or Build ledgers into the chat.
