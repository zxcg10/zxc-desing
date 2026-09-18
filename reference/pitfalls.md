# zxc-desing — Pitfalls + reduced prefs

## Optical / layout failures

| Symptom | Likely cause | Fix |
|---------|--------------|-----|
| Sidebar/toolbar solid gray | glass α ≥ 0.42; weak mesh; opaque ancestor or **inner** `bg-*` | α ≈ 0.28/0.36; left mesh ~32%; transparent provider/inset/inner panel |
| `skin-*` hung but still solid | Outer glass, inner `bg-sidebar`/`bg-card` still opaque | Inner → `bg-transparent`; material only on `.skin-*` ([preflight.md](preflight.md)) |
| Toolbar never reads through | Sticky outside scroll container | Sticky inside `overflow-y-auto` |
| No shine | CSS sets `.skin-toolbar-shine { display: none }` outside reduced prefs | Enable per [skin-classes.css](../recipes/skin-classes.css) |
| Legacy theme covers zxc | `data-skin` / theme class still points at another skin | Switch target; sync html, storage, runtime; search legacy selectors |
| Content under sidebar | Negative margin overlap | Spacer beside content |
| Cards look like white paper | Flat `#fff→#f2` or `bg-card`/`bg-white` covering face | Three-stop `--card-face` + `.skin-card`; hang class on login/settings cards too |
| Primary button flat blue | Plain `bg-primary` | Vertical background-image gradient + inset top highlight |
| Win/loss bars look painted | Short 75%→solid gradient | 28% → 72% → solid |
| Inputs not recessed | Shallow inset or old border/shadow wins | `--input-inset` + `.skin-input` |
| Brand block steals focus | Near-solid white + heavy elev | Brand ~48% translucent + light blur |
| Dark mode mush | Missing light strokes | Sidebar inset light edge; `--card-stroke` ≥ 0.18 |
| Glass on glass | Nested liquid-glass in sidebar; sheet without overlay | Overlay cuts sampling; theme toggles use light elev fills |
| Segment control solid primary | `bg-primary` as segment | `.dash-seg-btn-on` language |
| Heavy table headers | uppercase / too-bold | 11px font-normal muted |
| Fat system scrollbars | Not overridden | Thin thumb + transparent track + Firefox `thin` |
| Press feels dead | No scale or slow duration | `0.97` + `100ms` + `deck-ease` |
| Forced `skin-*` on foreign stack | Vue/Ant without mapping | Mode C: map tokens ([stack-matrix.md](stack-matrix.md)) |
| Business/tests broken | Deleted classes, logic edits, new libs | Visual layer only; override > delete; see SKILL hard bans |
| Chinese headers forced uppercase | English header recipe on zh UI | Branch by lang ([i18n.md](i18n.md)) |
| a11y stripped | Dropped aria/focus rings “for looks” | Keep `aria-*` and `focus-visible` |
| Chinese tofu glyphs | Environment missing CJK fonts; Latin-only webfont preload | Keep CJK fallbacks in the system stack; missing glyphs ≠ token failure ([i18n.md](i18n.md)) |
| False “optics done” | String edits only; or claimed full shell without auth | Run acceptance; state evidence level |

## Reduced preferences (required fallbacks)

`backdrop-filter` + large blurs stutter or die on weak GPUs / WSL / old devices (looks like a solid wall).

### Required media queries

```css
@media (prefers-reduced-transparency: reduce) {
  .skin-sidebar,
  .skin-toolbar,
  .liquid-glass,
  .apple-sheet {
    background: var(--card) !important;
    backdrop-filter: none !important;
    -webkit-backdrop-filter: none !important;
  }
}

@media (prefers-reduced-motion: reduce) {
  .pressable:active { transform: none; filter: brightness(0.96); }
  /* Keep ≤200ms fades; drop exaggerated translate/scale */
}
```

### Optional performance tactics

| Scene | Tactic |
|-------|--------|
| Multi-window / weak GPU | Lower `--glass-blur` (e.g. 20px) or solid-tint sidebar, glass only on toolbar |
| Long scrolling lists | `transform: translateZ(0)` on scroller; no per-row backdrop-filter |
| Janky motion | Press = transform/opacity only; do not change filter while scrolling |
| Blur compositing fails | `transform: translateZ(0)` promotes a layer; **never** ancestor `isolation: isolate` (cuts backdrop sampling) |
| Mobile | Sheet solid **or** strong glass—pick one; pause background motion while open |

### Acceptance wording

- High-end: glass read-through meets brief  
- OS “Reduce transparency”: solid card fallback still readable with strokes  
- Do not pile blur layers on weak devices and claim a perfect reskin
