# zxc-desing — Shell

Chrome makes or breaks the reskin. Same pattern for admin, analytics, and chat consoles.  
Run [preflight.md](preflight.md) first. Optical classes: [recipes/skin-classes.css](../recipes/skin-classes.css).

## Structure

Names vary by stack (Provider / Layout / Shell); structure does not:

```
root.page-mesh                     ← page-mesh only; no solid bg-background
├── sidebar.skin-sidebar           ← fixed / drawer glass; inner panel must be transparent
└── main-column                    ← bg-transparent; h-svh; overflow hidden
    └── scrollContainer            ← overflow-y-auto (sole main scroller)
        ├── header.skin-toolbar    ← sticky top-0 + .skin-toolbar-shine
        └── main#main-content      ← content scrolls under the glass
```

## Hard rules

1. Sticky toolbar must live **inside** the scroll container or it never reads through content.  
2. Sidebar spacer sits beside content; no negative-margin overlap into the sidebar.  
3. Root / main column stay transparent; ambient light is mesh only.  
4. Sidebar samples mesh; toolbar samples scrolling content.  
5. Suggested widths: desktop `16rem`, mobile sheet `18rem`, icon rail `3rem`.  
6. Suggested content padding: `p-4 md:p-6 lg:p-8`; full-bleed routes (chat, embeds) may use `p-0`.

## Class contract

| Class | Role |
|-------|------|
| `.skin-sidebar` | `--glass-bg` + blur; dark inset light stroke |
| `.skin-toolbar` | `--glass-bg-strong` + blur |
| `.skin-toolbar-shine` | Full-width 1px edge cut (required; do not hide outside reduced media) |
| `.skin-brand` | Logo ~48% translucent + light blur; no solid white hero card |
| `.skin-nav-item` | Active primary tint + left indicator; `active:scale(0.97)` |
| `.chrome-edge` | Optional toolbar bottom fade |
| `.apple-overlay` / `.apple-sheet` | Modal dim / thick glass drawer |

## Typical toolbar contents (pick per product)

- Sidebar trigger, breadcrumb, status chip, clock  
- Command palette (⌘K)  
- Account menu (sign out)

## Sidebar footer

Plain text footnote + light/dark toggle. No solid white card and no second liquid-glass layer inside the sidebar.

## Secondary chrome (align if present; otherwise skip)

| Pattern | Use |
|---------|-----|
| Left section rail + right pane | Settings / multi-section pages |
| Top tab grid | Config hub with many categories |
| Full-bleed main column | Chat column, embedded canvas / iframe |
