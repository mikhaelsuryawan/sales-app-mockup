## Visual checklist (design-system.md, theme.css, index.css, index.html, favicon, LogoMark)

| ID | Source | Requirement (CSS terms) | Applicability | Status in TARGET |
|---|---|---|---|---|
| V-01 | theme.css @theme / design-system Typography | `--font-sans: 'DM Sans', ui-sans-serif, system-ui, -apple-system, 'Segoe UI', Roboto, Arial, sans-serif` | Terapkan | Sebagian: `--font` L32 has no -apple-system/Segoe UI/Roboto |
| V-02 | theme.css | `--font-mono: 'JetBrains Mono', ui-monospace, SFMono-Regular, Menlo, Consolas, monospace` | Terapkan | Sebagian: `--mono` L32 has only ui-monospace,monospace |
| V-03 | admin index.html / DS intro | Google Fonts: DM Sans with italics, opsz 9..40, wght 300..800, plus JetBrains Mono 400 and 500, `display=swap` | Terapkan | Sebagian: L11 loads only 400–700 with no italics, and Mono 500 only. `.stat b` uses 800 (L209), so that weight is synthesized |
| V-04 | admin index.html | preconnect to fonts.googleapis.com and fonts.gstatic.com (crossorigin) | Terapkan | Sudah L9–10 |
| V-05 | admin index.html | viewport `width=device-width, initial-scale=1.0, viewport-fit=cover` | Terapkan | Sebagian: L5 has no `viewport-fit=cover`, yet `env(safe-area-inset-bottom)` is used at L418/459 |
| V-06 | admin index.html | `theme-color #f1f0f1` | Terapkan; Adaptasi: add a dark-scheme variant | Sudah L7, light only |
| V-07 | admin index.html | SVG favicon `<link rel="icon" type="image/svg+xml">` | Terapkan | Belum: no icon link |
| V-08 | favicon.svg | 64×64 ink tile rx18 `#101112`, white stacked-ingot strokes (w5, round joins), accent dot r5.5 at 49,18 | Adaptasi: SIM mark or initial on ink tile, dot in `#0A92F2` | Belum |
| V-09 | LogoMark.tsx / DS Brand mark | Mark in `currentColor` plus accent dot `fill: var(--accent)`, in a 44px `border-radius:16px; background:rgb(255 255 255/.05)` tile on the rail | Adaptasi: keep the accent dot, swap the paths for the SIM mark | Sebagian: `.sb-logo` L110 is a white tile with the raster SIMGROUP logo; `.sb-mark` L112 is a solid accent-strong "S" with no accent-dot pattern |
| V-10 | admin index.html | `<title>` product name | Terapkan | Sudah L6 |
| V-11 | index.css | Tailwind import, theme import, `@source` UI kit | Tidak berlaku: no build; tokens are inlined in `:root` | n/a |
| V-12 | theme.css | `@import 'tw-animate-css'` (overlay fade/zoom/slide) | Adaptasi: hand-written keyframes | Sudah: `rise/fade/slidein/slideinr/sheetup` L61, L417 |
| V-13 | Palette `accent` | `#ed1c24` as the single highlight | Adaptasi: `--accent:#0A92F2` | Sudah L19 |
| V-14 | Palette `accent-strong` | Solid fill behind small white text: primary button, count badge, active chip/rail/tab | Adaptasi: `#0A6FE0` | Sudah L19; used in `.b-accent` L178, `.nl.on` L119, `.bell i` L189, `.bb-i.on` L462 |
| V-15 | Palette `accent-dark` | Hover/pressed state of accent-strong | Adaptasi: `#0857B8` | Sebagian: defined L19 but only used in `.bb-act:hover` L464. `.b-accent:hover` uses `opacity:.92` L178 |
| V-16 | Palette `accent-soft` | Tint for danger badges, icon tiles and urgent tiles | Adaptasi: `#E7F2FE` for blue tints. Danger tint goes to a separate `--danger-soft` because red is not accent | Sudah L19/22 |
| V-17 | Palette `info`/`info-soft` | The only second hue: in-progress, info banners, second chart series | Adaptasi: tan `#EFA477`/`#FDF1E8`, plus `--info-text #9A4E24` for AA text | Sudah L21 |
| V-18 | Palette ink ladder | `--ink #101112`, `--ink-2 #1b1c1e`, `--ink-3 #26282b` | Terapkan | Sudah L18 |
| V-19 | Palette on-ink | `#fff` / `#b8b8b8` | Terapkan | Sudah L18 |
| V-20 | Palette surface | `surface #f1f0f1` (canvas), `surface-2 #f5f4f6` (nested) | Terapkan | Sudah: `--canvas`, `--raised` L15 |
| V-21 | Palette card | `#ffffff` | Terapkan | Sudah L15 |
| V-22 | Palette border | `#e6e5e7` | Terapkan | Sudah `--line` L16 |
| V-23 | Palette foreground/body | `#101112` / `#333333` | Terapkan | Sudah L17 |
| V-24 | Palette muted | `#686868`, AA on card and canvas | Terapkan | Sudah L17 |
| V-25 | Palette silver | `#c0c0c0`, disabled marks and timestamps only, never body text | Terapkan | Belum as a token. `#C0C0C0` survives only as `--s0` L24 |
| V-26 | Palette success | `#047857` / `#d1fae5` | Terapkan | Sudah L22 |
| V-27 | Palette warning | `#975000` / `#fef3c7` | Terapkan | Sudah L22. `TONEC.warning` L559 uses `--st-t #F2A11F` instead of `--warning` |
| V-28 | Palette danger | Alias of accent | Adaptasi: separate red `--danger #C9141B` / `#FDECED` | Sudah L22 |
| V-29 | Palette chart-muted | `#d4d3d6` for grey context marks | Terapkan | Belum: no token. Columns use hatch plus `--raised`, the donut uses `--s0 #C0C0C0` |
| V-30 | DS Rules | Do not invent greys; use the ladder only | Terapkan | Belum: invented `--void #E4E3E6`, `--control #ECEBEE`, `--hover`, `--line-strong #D9D8DC`, `--subtle #8B8B8B` (L15–17) are used widely |
| V-31 | DS Rules | One accent, one dark, everything else grey; the next emphasis in a region is ink | Terapkan | Belum: extra hues `--s2..s5` (orange, navy, teal, purple) L24, `--st-*` status hues L25, `--pop-tan/sky` L23 |
| V-32 | DS Rules | Status colours are soft tints on cards and badges. Solid colour only on dots, bars, chart marks, timeline bars and the single most urgent card | Terapkan | Sudah: `.tag .t-*` L228–229, `.spill` dot L226 |
| V-33 | DS Rules | No gradient fills | Terapkan (house rule) | Belum: `--accent-grad` on `.cc-col.sel` L338, `.thumb` gradient L429 |
| V-34 | DS Rules | Ink card decorative blob: accent at 30%, `blur(64px)` (blur-3xl) | Adaptasi: blue | Sudah `.card.ink::before` L383 |
| V-35 | DS Rules | Admin canvas glow: three radial gradients at 6–9% accent plus a light tint, `blur-xl`, `opacity:.7` | Adaptasi: blue/sky | Sudah `--glow` L27, `.glow::before` L102 (blur 24px ≈ xl) |
| V-36 | DS Contrast | White on bright accent fails AA, so small white text sits on `accent-strong` | Adaptasi: same rule with blue | Sudah: white text sits only on `--accent-strong` |
| V-37 | theme.css base html | `-webkit-font-smoothing:antialiased; -moz-osx-font-smoothing:grayscale; text-rendering:optimizeLegibility; font-feature-settings:'ss01','cv11'; -webkit-tap-highlight-color:transparent` | Terapkan | Sebagian: body L51 has antialiased and ss01/cv11 only; missing moz smoothing, optimizeLegibility, tap-highlight |
| V-38 | theme.css base body | `background: surface; color: foreground` | Terapkan | Sebagian: `color:var(--body)` L51, not `--fg` |
| V-39 | theme.css base | `*, ::before, ::after { border-color: var(--border) }` | Terapkan | Belum |
| V-40 | theme.css base | `::selection { background: accent; color: #fff }` | Adaptasi: use `--accent-strong` (white text AA) | Belum |
| V-41 | theme.css base | `button:not(:disabled), [role=button] { cursor:pointer }` | Terapkan | Sebagian: `button{cursor:pointer}` L53 has no `:not(:disabled)` (`.btn:disabled` overrides with not-allowed L174) |
| V-42 | theme.css utilities | `.safe-b { padding-bottom: env(safe-area-inset-bottom) }`, `.safe-t` | Terapkan | Sebagian: inline env() in `.drawer` L418 and `.bbar` L459; no top inset |
| V-43 | theme.css utilities | `.no-scrollbar` (scrollbar-width:none plus webkit hide) | Terapkan | Sudah inline on `.sb-scroll` L113–114 and `.utabs` L216 |
| V-44 | Typography | Page title 24px/700/tight tracking | Terapkan | Sudah `h1.pt` L156 |
| V-45 | Typography | Header page title 18px/700/leading-tight | Terapkan | Belum: header uses the 24px title (22px mobile L457) |
| V-46 | Typography | Phone screen title 28px/700 plus accent period | Tidak berlaku (phone app) | n/a |
| V-47 | Typography | Sign-in/brand title 30px (36px on sm brand card) 700 tight, plus accent period | Terapkan | Sebagian: `.login-art h2` 44px L442; accent period not verified |
| V-48 | Typography | Card title 16px/600/leading-tight | Terapkan | Sudah `.ch h2` L198 |
| V-49 | Typography | Dialog/sheet title 18px/600 | Terapkan | Sebagian: `.panel-h h2` is 20px/700 L413 |
| V-50 | Typography | Body 14px | Terapkan | Sudah body L51 |
| V-51 | Typography | Secondary 12px muted | Terapkan | Sudah `.xs.muted` L60 |
| V-52 | Typography | Kicker 11px/600/uppercase/tracking-wider (.05em) muted; on-ink-muted on ink | Terapkan | Sudah `.kick` L381 (.08em), `.ink .kick` L385 |
| V-53 | Typography | Table header 12px/600/uppercase/tracking-wide (.025em) muted | Terapkan | Sebagian: `.tbl th` is 11.5px, .04em L304 |
| V-54 | Typography | Identifier `mono` 12px | Terapkan | Sudah `.mono` L57 |
| V-55 | Typography | Stat value 28px/800/lh 1.15/-0.5px | Terapkan | Sudah `.stat b` L209 |
| V-56 | Typography | Hero metric 30px/700 tight tabular | Terapkan | Sebagian: `.hero-n` 44px L210 |
| V-57 | Typography | Featured number 48–72px/700 tabular | Terapkan | Sudah `.hero-big` 60px L387, `.card.acc .big` 48px L397 |
| V-58 | Typography | Every number is tabular-nums | Terapkan | Sebagian: widely used, but `.cc-axis` L330, `.lc text` L364, `.gantt th` L367 and `.cc-labels` lack it |
| V-59 | Typography | Unit beside number in 600 muted: top-aligned in stat (flex start, unit pt 6px), bottom-aligned on hero (flex end, gap 6px, unit pb 2px) | Terapkan | Sebagian: `.hero-unit` L388 defined; stat unit inline 16px L1272, not top-aligned |
| V-60 | Typography | Inputs 16px on phones, 14px from md (no iOS zoom) | Terapkan | Belum: `.inp input` fixed 14px L239, `.sel select` 13px L244 |
| V-61 | Typography | `@media (pointer:coarse){input,textarea,select{font-size:16px}}` | Adaptasi (phone app rule, but helps the dashboard on phones) | Belum |
| V-62 | Shape | Cards and dialogs 24px | Terapkan | Sudah `--r-card` L26, `.card`, `.modal` |
| V-63 | Shape | Ink hero, rail and right sheet 28px | Terapkan | Sudah `--r-hero`: `.sidebar`, `.card.ink`, `.panel` |
| V-64 | Shape | Bottom sheets top 28px | Terapkan | Sudah `.drawer` L418, mobile `.panel` L467 |
| V-65 | Shape | Nested panels, rows, inputs and bottom-bar tiles 16px | Terapkan | Sebagian: `--r-panel` 16 on tile/rowlink/`.bb-i`. Inputs are pills (matches the "every control is a pill" rule) |
| V-66 | Shape | Menu rows, small tiles, tooltips 12px | Terapkan | Sebagian: `.mi`/`.dd-opt` 12px; `.cc-tip` 8px L342 |
| V-67 | Shape | Medium icon tile 13px | Terapkan | Sudah `.stat .si` L207 |
| V-68 | Shape | Every control, tab, chip, badge and search field is a full pill | Terapkan | Sudah `.btn`, `.pills`, `.fchip`, `.tag`, `.inp`, `.sel` |
| V-69 | Elevation shadow-card | `0 1px 2px rgb(16 17 18/.03), 0 8px 24px -12px rgb(16 17 18/.12)` | Terapkan | Sudah L28 |
| V-70 | Elevation shadow-float | `0 12px 40px -12px rgb(16 17 18/.25)` on rail, bar, dialogs, sheets, popovers, toasts, tooltips, ink hero | Terapkan | Sebagian: L29 used on most; `.cc-tip` has no shadow L342 |
| V-71 | Elevation shadow-glow | `0 8px 20px -8px` accent at 60% | Adaptasi: blue | Sebagian: L30 is `0 10px 24px -8px rgb(10 120 240/.5)`, values differ |
| V-72 | Elevation shadow-up | `0 -10px 30px -14px rgb(16 17 18/.25)` | Terapkan | Sebagian: only a fallback inside `.drawer` L418; no token |
| V-73 | Elevation rule | Cards never have borders; borders only on inputs, table rows, dividers and floating panels | Terapkan | Sebagian: cards have no border; `.cc-col` has a border L336 |
| V-74 | Spacing | Canvas padding 12px (lg 16px) | Terapkan | Sudah `.shell` 16px, 12px under 1024 L450 |
| V-75 | Spacing | 16px between cards; stat rows 12px (sm+ 16px); page body stacks 16px; PageHeader mb 24px | Terapkan | Sebagian: `.grid` 16 L191, `.stats` 16px always (no 12px on phones) L203 |
| V-76 | Spacing | Card padding 20px (16 strips, 24 dialogs/sign-in, 32 brand card) | Terapkan | Sudah `.card` 20 L196, `.attn` 16, `.modal` 24; `.login-form .card` 28 (Sebagian) |
| V-77 | Spacing | Phone app spacing | Tidak berlaku (phone app) | n/a |
| V-78 | Icons | Lucide icons, stroke 2; controls 16px; nav and large tiles 20px | Terapkan | Sebagian: inline path icons `I()` L542 at stroke 2, default 18px (not 16/20) |
| V-79 | Icons | IconTile: soft tones for status, solid ink/accent only for the one primary tile | Terapkan | Sebagian: `statIcon`/`POPS` L554 use pop-blue/tan/ink/sky/mist solid tiles by index, not by status |
| V-80 | Motion | Hover `transition-colors`; `active:scale(.98)` on cards/buttons/chips; .95 round header buttons; .99 large tiles | Terapkan | Sebagian: `.btn:active scale(.97)` L173, `.at` .98, `.rowlink` .99; chips have no active scale |
| V-81 | Motion | Overlays fade/zoom/slide at 200ms | Terapkan | Sebagian: `.overlay` .2s; `.panel`/`.modal` .25s |
| V-82 | Motion | `animate-pulse` on live dots only; `ping` for waiting reader | Terapkan | Belum (no live-dot pulse) |
| V-83 | Motion | Rail width transition 200ms; nothing else moves | Terapkan | Sebagian: `.sidebar` L106 matches, but extra motion (`.stat` hover lift L89, `.cc-col` grow/lift L91–92, `.lc circle` scale L95, shimmer skeleton) goes beyond the house rule |
| V-84 | Ink card | Secondary text on-ink-muted; inner panels 16px radius, white/5, 12px padding | Terapkan | Sudah `.ink .muted` L385, `.ink-panel` L389 (white/.06) |
| V-85 | Ink card | Chips: border white/10, bg white/10, white text | Terapkan | Sudah `.ink-chip` L392 (bg .08) |
| V-86 | Ink card | Buttons `onInk` variant | Terapkan | Sudah `.b-onpop` L183 |
| V-87 | Ink card | Notes: `accent/20` risk, `warning/20` paused, `white/10` locked | Adaptasi: risk note uses danger/20 | Belum |
| V-88 | Ink card | Progress 6px, white/15 track, white fill | Terapkan | Sudah `.ink .bar` L391 |
| V-89 | Accent card | Label 13px/600 white/80; number 48px/700; context 14px white/80; progress white/25 with white fill; onInk sm button | Adaptasi: blue | Sudah `.card.acc` L395–399 (context 13px) |

## Chart checklist (charts.md, charts/*.tsx, ProductionAnalyticsPage)

| ID | Source | Requirement | Applicability | Status in TARGET |
|---|---|---|---|---|
| C-01 | charts.md intro | Hand-drawn SVG, no chart library | Terapkan | Sudah |
| C-02 | charts.md / useElementSize | Each chart measures its container (ResizeObserver) and draws at real pixel width; `height` prop includes the x labels | Terapkan (vanilla ResizeObserver or re-render) | Belum: `lineChart` uses a fixed 640×220 viewBox scaled by `width:100%` (text scales) L1025; `columnChart` uses HTML flex with a fixed 220px plot L332 |
| C-03 | Colour | One highlighted value in accent; the rest ink or chart-muted grey; second series = info | Adaptasi: blue highlight, tan second series | Sebagian: columns use hatch grey plus a gradient highlight; line series use `--s1..s5` hues |
| C-04 | ColumnChart colour | Plain columns ink; `highlight` on rule-breaking columns; `tone="muted"` greys the others; highlighted column prints its value above it | Terapkan | Belum: selection-driven highlight (`sel`), not rule-driven; value printed inside the column (`.val` L340), not above |
| C-05 | ColumnChart tones | `fills = ink / info / chart-muted` | Terapkan | Belum |
| C-06 | BarStrip | Small axis-free columns, grey context (`chart-muted`, or `white/22` on ink), last or `highlightIndex` in accent, default 56px | Terapkan | Belum: no equivalent |
| C-07 | LineChart/Sparkline colour | One series ink (info/accent by meaning, white on ink), 10% area when `area`, `highlightLast` accent end dot only on risk rows | Terapkan | Sebagian: area .1 opacity L1030; series colour by `--s` slot; multiple series in one chart (L1707) |
| C-08 | BarList colour | Main ranking ink, `emphasis` row accent (filtered), secondary ranking `tone=info` | Terapkan | Belum: `.bar i` defaults to accent L319; top sales/product mix bars all blue |
| C-09 | Part-to-whole order | Donut/SegmentBar categorical order: ink, info, accent, warning, success, chart-muted | Adaptasi: ink, tan, blue, warning, success, grey | Belum: donut slots map to `--s4 teal / s2 orange / s1 blue / s0` (L1286) |
| C-10 | Quality/band ramps | Four-band best→worst: success, info, warning, accent | Adaptasi: last band = danger red, not blue | Belum |
| C-11 | Allocation ramp | info from stock, ink from production, silver unresolved | Tidak berlaku (no MES allocation in Sales App) | n/a |
| C-12 | Colour rule | Plans/targets/limits are reference lines, never a second series | Terapkan | Sebagian: column target is per-column `.cc-cap` ticks L341 (not one line); line chart L1707 plots three series |
| C-13 | Axes niceScale | Nice ticks on 1, 2, 2.5, 5, 10 steps, 4 ticks, min/max from data (negatives allowed), flat series handled | Terapkan | Sebagian: `niceScale` L995 matches steps, but zero-based only and forces `max≥1e3` (wrong for small counts like 9–14 leads) |
| C-14 | ticksWithin / yDomain | Fixed domain (percent 0–100) | Terapkan | Belum |
| C-15 | Gridlines | Solid 1px `--border`, `shape-rendering:crispEdges`, one per tick; no axis lines or tick marks | Terapkan | Sebagian: line chart solid `var(--line)` without crispEdges L1028; column grid dashed L334 |
| C-16 | Tick labels | muted 11px tabular, right-aligned, band width = longest label × 6.2px + 10px | Terapkan | Sebagian: right-aligned, 11px, but colour `--subtle`, fixed 40px band L330/L1025, no tabular |
| C-17 | format prop | One formatter for ticks and tooltips | Terapkan | Sudah `fmt` param in both helpers |
| C-18 | xAxisLabels | Every n-th label anchored on the last, edge labels clamped inside, labels that would touch skipped | Terapkan | Belum: column labels ellipsized L346; line labels pre-thinned by caller L965 |
| C-19 | X labels style | muted 11px tabular, centred | Terapkan | Sebagian: `.cc-labels` 11.5px muted, active label bold (extra) |
| C-20 | ReferenceLine | 1px `stroke-muted` (warning/accent for limits), label right-aligned muted 11px, 5px above | Adaptasi: limit = danger | Belum |
| C-21 | Zones | LineChart bands at 8% (success/warning/danger) | Terapkan | Belum |
| C-22 | Column mark | 4px rounded data end, square baseline, ≤24px wide, 2px gap, negatives grow down | Terapkan | Belum: `.cc-col` radius 14px all corners, max 56px, gap 10px, border, min-height 12px / 6% floor L336/L1002 (misrepresents small values) |
| C-23 | Line mark | 2px, round caps/joins, clipped to plot | Terapkan | Sebagian: 2px, no round caps/joins, no clipPath; uses a monotone smooth curve `smooth()` L1021 (skill draws straight segments) |
| C-24 | Null gaps | `null` breaks the line, no drop to zero | Terapkan | Belum |
| C-25 | Points | Last point always gets a dot (r4, 2px card ring) plus a 11px/600 value label; `showDots` marks all; isolated points get a dot | Terapkan | Sebagian: every point has a dot r4 with card ring L1030; no last-value label |
| C-26 | Donut geometry | 148px, 16px ring, starts at 12 o'clock, 2px gaps along the mid-ring, butt ends | Terapkan | Sebagian: 196px, 24px ring, round caps, gap 30 units L1013; starts at 12 via rotate(-90) L353 |
| C-27 | Donut centre | Value 24px/700/lh1/tight over unit 11px/500 muted, mt 4px | Terapkan | Sebagian: `.donut .ctr b` 26px/800, small 12px L357–358 |
| C-28 | Donut empty | Empty donut = full surface-coloured ring | Terapkan | Belum (zero sum yields an empty SVG) |
| C-29 | Donut legend | Caller renders the legend | Terapkan | Sebagian: the helper bakes in `.dleg` L1018 |
| C-30 | Tooltip | HTML bubble over SVG: value first (600 tabular), label second (on-ink-muted); ink bg, 12px radius, 10px/6px padding, 12px text, shadow-float, 10px above (below if no room), clamped to chart width, `ring white/15` on dark cards | Terapkan | Sebagian: `.cc-tip` L342 is label·value on one line, 8px radius, `--invert` bg, no clamp, no shadow; line/donut use native `<title>` only |
| C-31 | Crosshair | Line/sparkline: pointer snaps to nearest x, 1px line (fg/25; border in sparkline; white/30 on ink), focus starts on the last point, arrows step, Home/End jump | Terapkan | Belum |
| C-32 | Discrete marks hit target | Transparent hit area full slot height (≥24px on donut ring) | Terapkan | Sebagian: column button only as tall as the bar; donut has no widened band |
| C-33 | Keyboard roving | One mark in tab order; arrows move focus | Terapkan | Sebagian: `ccKey` L1009 arrows on columns (also changes selection); no Home/End; donut segments not focusable (legend buttons are) |
| C-34 | Hover feedback | Column `opacity:.8`; donut segment lifts 3px outward | Terapkan | Belum: column bg change and translateY(-3px) L92/L337; donut dims others to .45 and widens stroke L355 |
| C-35 | a11y | SVG `role=img` with an `aria-label` naming the measure | Terapkan | Sudah: line L1027, donut L1016; column uses `figure` plus radiogroup L998 |
| C-36 | a11y live echo | sr-only `aria-live=polite` echoes hovered/focused value | Terapkan | Belum |
| C-37 | a11y focus ring | Focusable SVG: 6px radius, no outline, `focus-visible` 2px ring accent/40 | Adaptasi: blue | Sebagian: global `:focus-visible` outline L55 |
| C-38 | Empty state | "No data" centred 14px muted at chart height; rankings/legends use compact EmptyState that says how to get data | Terapkan (Indonesian copy) | Sebagian: caller text L1330, L1336; not at chart height; lineChart/donut unhandled |
| C-39 | Chart card | One chart per card; title names measure and grain | Terapkan | Sebagian: "Sell-out" card L1281 carries a hero number, delta, legend, chart and callout; titles lack grain ("Penjualan per hari") |
| C-40 | Chart card header | Picker in header action (inline combobox `w-full sm:w-64` or small pill tabs); method line `mt 2px 12px muted` under title | Terapkan | Sebagian: `card()` L568 supports acts; no subtitle/method-line slot |
| C-41 | Chart card footnote | `mt 12px 12px muted` note under chart | Terapkan | Sebagian: uses `.callout.blue` L1284 instead |
| C-42 | Legend (Donut/SegmentBar) | Rows flex space-between 14px; 8px round swatch plus label left; muted tabular value plus 600 fg count right; exit link `mt 12px 12px/600 hover:accent` | Terapkan | Sebagian: `.dleg` L359 13px, single value; exit via `link` onclick, not a text link |
| C-43 | Chart on stat card | Sparkline h40 or BarStrip h56 as StatCard children, `mt 16px` full width | Terapkan | Belum |
| C-44 | Chart on ink card | Sparkline `white`, BarStrip `onInk` | Terapkan | Belum (ink hero has only a progress bar) |
| C-45 | Drill-down | Clickable BarList row adds a filter chip and regroups one level finer; label may link to the record (stopPropagation, hover accent) | Terapkan | Sebagian: `drillTo` rows L1336 drill; chip/link behaviour not verified |
| C-46 | SegmentBar / ProgressBar | Stacked part-to-whole bar; progress tones ink / warning (>80%) / accent (>100%) | Adaptasi: >100% = danger or blue per meaning | Sebagian: `.bar` L318 (8px, control track); `achBar` L903 tones success/accent/`--st-t`; no SegmentBar |
| C-47 | Analytics layout 1 | PageHeader with window pill tabs (7/14/30) in actions | Terapkan | Sebagian: Overview card pills Harian/Mingguan/Bulanan L1285 inside the card |
| C-48 | Analytics layout 2 | Filter bar flex-wrap space-between gap 8px: "group by" label 12px/600 muted plus small pills; active drill as filter chip with X | Terapkan | Sebagian: `.fchip` exists L221; not verified as a drill chip |
| C-49 | Analytics layout 3 | KPI grid 2 cols, gap 12px (sm 16px), 4 cols from md; tone from `toneByThreshold`; odd last tile spans 2 (xl 1) | Terapkan | Sebagian: `.stats` 4→2 cols L203/451; no threshold tones |
| C-50 | Analytics layout 4 | Time series grid: 1 col, 2 cols from xl (1280) | Terapkan | Sebagian: `.g2` 2 cols, 1 below 1024 L451 |
| C-51 | Analytics layout 5 | Ranking plus stacked side cards `1.4fr / 1fr` from xl | Terapkan | Belum (`.g-side` is 1fr/340px) |
| C-52 | Analytics layout 6 | Detail table plus summary `1fr / 20rem` from xl | Terapkan | Sebagian: `.g-side` minmax(0,1fr) 340px, collapses ≤1279 L195/448 |
| C-53 | Example: bottleneck rows | Rank chip 36px, 12px radius, card bg, shadow; row on surface-2, 16px radius, 12px padding; progress plus 11px muted meta | Terapkan | Sebagian: `.rowlink` with rank number in top sales L1292; no rank chip |
| C-54 | Example: variance colouring | >5% accent, positive warning, else success | Adaptasi: >limit = danger | Tidak diverifikasi / n/a |

**Skill chart types or helpers with no target equivalent:** `Sparkline` (with crosshair), `BarStrip`, `BarList` as a component (target only has ad-hoc `.bar` rows), `SegmentBar`, `ReferenceLine`, LineChart `zones`, `ticksWithin`/`yDomain`, `xAxisLabels` thinning, `axisWidth`, `columnPath` (4px top-rounded path), `ChartTooltip` (clamped HTML bubble), `useCrosshair`, `LiveValue` (aria-live), `useActiveMark` roving focus with Home/End, empty-donut ring, `toneByThreshold`.

**Target chart helpers with no skill equivalent:** `.gantt` (attendance calendar grid with hatched off-days, L365–370/L1474), `meter()` (36-tick score meter, L1033/L371), `.funnel` (solid coloured stage bars, L373–374/L1290), `smooth()` monotone-cubic curve L1021, the selectable-column `radiogroup` with `badge` and `.val` inside the column, per-column target caps `.cc-cap` plus "Target on/off" toggle, `.hatch` fill pattern, `dHi()` donut/legend linked highlight that swaps the centre value, `.legend`/`.capkey`, and the shimmer skeleton for charts (`main.loading .cc/.lc/.donut`).