---
name: zxc-desing
description: >-
  Applies the zxc Apple HIG / Liquid Glass dashboard skin across React/Vue/Svelte,
  Next.js, Tailwind v3/v4, MUI/AntD, or vanilla CSS (tokens, chrome, primitives;
  patterns proven in production dashboards). Use ONLY when the user explicitly
  enables or names this skill (zxc-desing / Liquid Glass reskin / 复刻前端风格).
  Does not auto-run.
disable-model-invocation: true
---

# zxc-desing

> **Primary rule**  
> Reskin the visual layer only (tokens, chrome, primitives). **Do not refactor business
> logic. Do not add new third-party UI libraries.**  
> If a component library cannot be fully covered, keep its DOM, override with CSS
> variables, and say so in the reply. Preserve `aria-*` and keyboard / `focus-visible`.

Cross-repo **reskin** kit. The aesthetic is fixed (macOS System Settings–style data
console). Alignment with frontend-design-style skills is **process shape**
(plan → brief review → build → screenshot critique)—not “invent a new look each time.”

`disable-model-invocation: true` — do not apply unless the user names this skill.

## Package layout (never hardcode absolute paths)

Open materials relative to this `SKILL.md`:

- [reference/tokens.md](reference/tokens.md)
- [reference/shell.md](reference/shell.md)
- [reference/preflight.md](reference/preflight.md) — probe before editing (opaque ancestors / sticky / theme lock)
- [reference/elements.md](reference/elements.md)
- [reference/stack-matrix.md](reference/stack-matrix.md)
- [reference/i18n.md](reference/i18n.md)
- [reference/pitfalls.md](reference/pitfalls.md)
- [recipes/skin-classes.css](recipes/skin-classes.css) — pasteable `.skin-*` / `.page-mesh` source
- [recipes/](recipes/) — Tailwind v3 config / v4 `@theme` / vanilla bundle
- [checklists/acceptance.md](checklists/acceptance.md)

Optional reference: a landed `DESIGN.md` in the workspace (if present).

---

## Process: Skin Read → plan → review → build → critique

Two-pass workflow (same idea as frontend-design): **plan against the brief first, then
code; critique with screenshots while building.**

### 1. Skin Read (one line, before editing)

Read the product and stack, then declare (do not skip):

> **Reading this as:** \<product shape: admin / chat / analytics / …\> **for** \<audience\>**,
> stack** \<React|Vue|Svelte|Next + TW v3|v4|vanilla|MUI|AntD\>**, mode** \<A|B|C\>**,
> lang** \<zh|en|mixed\>, **leaning toward** zxc Liquid Glass chrome + card-face
> (not a marketing landing page).

If ambiguous, ask **one** question (e.g. mode A vs C). Infer when you can.

Stack details: [stack-matrix.md](reference/stack-matrix.md). **Do not** dump unmapped
Tailwind utilities into a non-Tailwind project before declaring the stack path.

### 2. Plan (short plan against the brief)

Compact plan. Structure matches frontend-design token/type/layout/principles—but
content comes from **zxc sources**, not a newly invented aesthetic:

| Block | Write |
|-------|--------|
| **Color** | Cite tokens: background / primary / glass α / three-stop card-face (4–6 named values) |
| **Type** | System stack + display/metric; header rules from [i18n.md](reference/i18n.md) |
| **Layout** | One-line chrome + ASCII (`page-mesh` → sidebar glass → sticky toolbar in scroll) |
| **Principles** | Translucency must be obvious; **chrome before primitives**; override beats deleting classes; no business changes |
| **Stack / recipe** | Mode A/B/C + recipes to use (include `skin-classes.css`) |
| **Scope** | Preflight answers; shared files to touch; explicit **non-goals** (pages/tests) |

Run [preflight.md](reference/preflight.md) before planning; put the four answers in Scope.

### 3. Review against brief (before code)

Self-check against zxc brief (tokens + shell + primary rule):

- Drifted into “generic SaaS white cards / solid top bar / glass α ≥ 0.42”? Fix the plan and say what changed.  
- About to strip aria, add libraries, or touch 50 business pages? Return to shared primitives.  
- Stack path matches recipe (v3 config vs v4 `@theme` vs vanilla)?  
- Preflight: opaque inner panel / sticky outside scroll / shine disabled / legacy theme lock—each has a fix?

**Only then start coding.**

### 4. Build (required order)

1. Tokens (+ Tailwind recipe if needed)  
2. Inject [recipes/skin-classes.css](recipes/skin-classes.css) (or equivalent rules)  
3. **Chrome** ([shell.md](reference/shell.md): transparent sidebar inner panel, sticky-in-scroll)  
4. Shared primitives (Card / Button / Input / Dialog… hang `skin-*`)  
5. Domain surfaces that **already exist** ([elements.md](reference/elements.md))  
6. i18n table headers  

Conflicts: prefer variable / skin-class **overrides** over deleting utilities.  
Do not claim primitives are “done” in Critique while chrome is still opaque.

### 5. Critique (acceptance)

Screenshot critique is the primary optical evidence; static checks calibrate.
Evidence levels: [acceptance.md](checklists/acceptance.md).

Quality floor (default, do not boast):

- Usable on mobile  
- Visible keyboard focus  
- Honor `prefers-reduced-motion` / `prefers-reduced-transparency`  
- Visual a11y (contrast, dark-mode strokes)  

“Remove one accessory”: drop decoration that does not serve the zxc brief (neon, stacked glass, extra glow).

**Claiming “reskinned”:** valid screenshot (state the evidence level) + L1 pass.  
No browser → say “optics not verified”; **do not** claim a full reskin.  
User reply stays short — see Delivery shape; do not narrate the whole acceptance checklist.

---

## Hard bans

**Visual:** stacked glass; negative-margin overlap into the sidebar; sticky toolbar outside the scroll container; glass α ≥ 0.42; card-face collapsed to flat `#f2`; neon / HUD / border-beam.

**Engineering:** no business/API/route/test “drive-by” refactors; no new UI libraries; no a11y stripping; do not delete classes still relied on; do not force unmapped `skin-*` onto foreign stacks; do not restyle unless this skill is enabled.

---

## Delivery shape (like frontend-design — short)

Same habit as frontend-design: **compact plan → brief review note → build → screenshot critique**.
Do **not** dump L1/L2 checklists, quality-floor inventories, or long Build ledgers into the user reply.
Run those internally via [acceptance.md](checklists/acceptance.md); surface only what the user needs.

**Before code** (keep tight — one Skin Read line + a short plan block):

```text
Reading this as: <shape> for <audience>, stack <…>, mode <A|B|C>, lang <zh|en|mixed>.

Plan
- Color: <4–6 named token values>
- Type / lang: <system stack + i18n header rule>
- Layout: <one line or tiny ASCII>
- Principles / scope: <chrome-first; non-goals>
```

If the plan drifted (SaaS cards, glass α ≥ 0.42, solid bar…), say **what changed and why** in one sentence, then code.

**After code** (a few lines — picture > tokens):

```text
Changed: <tokens → chrome → primitives → …> (skip list only if material)
Critique: <evidence level>; <1–2 optical findings>; dark <ok|not tested>
Claim: yes|no — residual: <only real risks>
```

Evidence levels stay in [acceptance.md](checklists/acceptance.md). No browser → “optics not verified”; do not claim a full reskin.

---

## Element families (pick by product shape—not a mandatory checklist)

| Family | Examples | When |
|--------|----------|------|
| Chrome | sidebar / toolbar / cmdk | Almost always |
| Primitives | Button / Input / Card / Dialog | Almost always |
| Analytics | panels, charts, win/loss bars | Analytics / dashboards |
| Admin | config hub, wizards, sync rails | Ops / back-office |
| Chat | bubbles, composer, knowledge base | Support / messaging |

Details: [elements.md](reference/elements.md). Only restyle surfaces that **already exist**.
