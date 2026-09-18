# zxc-desing — Tokens

Optical source snapshot. Paste into `:root` / `.dark` as-is when reskinning a repo.

## Color

| Token | Light | Dark |
|-------|-------|------|
| `--background` | `#f5f5f7` | `#000000` |
| `--foreground` | `#1d1d1f` | `#f5f5f7` |
| `--card` | `#ffffff` | `#1c1c1e` |
| `--card-foreground` | `#1d1d1f` | `#f5f5f7` |
| `--popover` | `#ffffff` | `#2c2c2e` |
| `--muted` | `#e8e8ed` | `#2c2c2e` |
| `--muted-foreground` | `#86868b` | `#98989d` |
| `--primary` | `#0071e3` | `#0a84ff` |
| `--primary-foreground` | `#ffffff` | `#ffffff` |
| `--secondary` | `#e8e8ed` | `#2c2c2e` |
| `--accent` | `#34c759` | `#30d158` |
| `--destructive` | `#ff3b30` | `#ff453a` |
| `--success` | `#34c759` | `#30d158` |
| `--warning` | `#ff9500` | `#ff9f0a` |
| `--error` | `#ff3b30` | `#ff453a` |
| `--info` | `#0071e3` | `#0a84ff` |
| `--border` | `rgba(0,0,0,0.08)` | `rgba(255,255,255,0.12)` |
| `--input` | `rgba(0,0,0,0.12)` | `rgba(255,255,255,0.16)` |
| `--ring` | `#0071e3` | `#0a84ff` |
| `--chart-1`…`5` | blue / green / orange / purple / gray | matching dark set |
| `--radius` | `0.875rem` | same |

Status colors use **tints** (~12% fill + vivid text). `--destructive` is for danger only—not “losing” / lag metrics.

## Elevation / card face

| Token | Light | Dark |
|-------|-------|------|
| `--elev-1` | `0 1px 2px rgba(15,23,42,0.06), 0 4px 10px rgba(15,23,42,0.07), 0 12px 28px -6px rgba(15,23,42,0.14)` | `0 1px 3px rgba(0,0,0,0.5), 0 6px 14px -2px rgba(0,0,0,0.45), 0 18px 40px -8px rgba(0,0,0,0.55)` |
| `--elev-2` | `0 6px 16px rgba(15,23,42,0.1), 0 20px 48px -10px rgba(15,23,42,0.18)` | `0 14px 40px -4px rgba(0,0,0,0.65), 0 6px 16px -2px rgba(0,0,0,0.5)` |
| `--card-stroke` | `rgba(0,0,0,0.1)` | `rgba(255,255,255,0.18)` |
| `--card-face` | `linear-gradient(180deg,#fff 0%,#ebebef 55%,#e4e4ea 100%)` | `linear-gradient(180deg,#3a3a3c 0%,#2c2c2e 45%,#1c1c1e 100%)` |

## Glass

| Token | Light | Dark |
|-------|-------|------|
| `--glass-blur` | `32px` | same |
| `--glass-blur-strong` | `48px` | same |
| `--glass-saturate` | `1.9` | same |
| `--glass-bg` | `rgba(255,255,255,0.28)` | `rgba(28,28,30,0.32)` |
| `--glass-bg-strong` | `rgba(255,255,255,0.36)` | `rgba(28,28,30,0.42)` |
| `--glass-stroke-edge` | `rgba(0,0,0,0.1)` | `rgba(255,255,255,0.16)` |
| `--glass-radius` | `1.25rem` | same |

Full `--glass-shadow` / `--glass-specular`: see a landed repo `index.css` or [recipes/vanilla-css.css](../recipes/vanilla-css.css).

## Input / scroll / motion

| Token | Light | Dark |
|-------|-------|------|
| `--input-fill` | `rgba(0,0,0,0.055)` | `rgba(255,255,255,0.08)` |
| `--input-inset` | `inset 0 1px 3px rgba(0,0,0,0.14), inset 0 0 0 1px rgba(0,0,0,0.04)` | `inset 0 1px 3px rgba(0,0,0,0.55), …` |
| `--scrollbar-thumb` | `rgba(0,0,0,0.15)` | `rgba(255,255,255,0.18)` |
| `--scrollbar-thumb-hover` | `rgba(0,0,0,0.28)` | `rgba(255,255,255,0.32)` |
| `--deck-ease` | `cubic-bezier(0.22, 1, 0.36, 1)` | same |
| `--motion-fast` | `100ms` | same |
| `--motion-panel` | `0.35s` | same |

## Fonts

```text
--font-sans: -apple-system, BlinkMacSystemFont, "SF Pro Text", "Segoe UI",
  "PingFang SC", "Hiragino Sans GB", "Microsoft YaHei", "Noto Sans SC", …
--font-display: prefer SF Pro Display; else same as sans
--font-mono: ui-monospace, SFMono-Regular, Menlo, Consolas, monospace
```

`body { font-feature-settings: "tnum" 1; }`

## page-mesh

Large left primary wash (~32% light / ~38% dark) plus chart-4 / accent accents.  
Attach to the provider / root. **Do not stack another solid `bg-background`.**

## Sidebar semantic colors (optional)

`--sidebar-background` and friends may exist for component APIs, but **visible material is
`.skin-sidebar` + `--glass-bg`**. Do not cover glass with opaque `bg-sidebar`.
