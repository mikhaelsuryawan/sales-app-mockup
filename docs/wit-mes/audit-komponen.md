## Checklist: kit `packages/ui` vs `mockup/dashboard/index.html`

The target is `/Users/mikhaelsuryawan/WIT/Project/Sales App/mockup/dashboard/index.html`. CSS is on lines 12–489, helpers on 548–576, table on 1038–1051, overlays on 1056–1065, shell on 1086–1132, and the dropdown enhancer on 3293–3344.

**How kit tokens map to the target** (this applies to every row):
- `surface` → `--canvas`
- `surface-2` → `--raised`
- `border` → `--line`
- `silver` → `--s0` / `--line-strong`
- `black/5` → `rgb(16 17 18/.05)`
- `ink-2` / `ink-3` / `on-ink-muted` already exist in the target.

**How "accent" translates.** The kit uses `accent` (red in MES) in two ways:
- **Brand emphasis** stays SIM blue: primary button, active rail item, active underline, switch on, combobox check, active default chip, RailAction.
- **Danger or urgency** becomes `--danger` / `--danger-soft`: danger badge, danger IconTile, destructive menu item, a number past its limit, a late date, the danger toast mark, the accent ProgressBar when it means "held/late", a failed check, the danger SegmentedControl option, the remove action.

**Size scale used in the table:** h-8=32px, h-10=40, h-11=44, h-12=48; p-5=20; rounded-xl=12, rounded-2xl=16, rounded-card=24, rounded-hero=28; text-xs=12, text-sm=14, text-base=16.

| ID | Component / variant (source) | Requirement (translated to CSS) | Applicability | Status in TARGET |
|---|---|---|---|---|
| K-01 | `cn()` merge (lib/cn.ts) | Merge classes so the last radius/shadow wins | Tidak berlaku: no Tailwind. Helpers build class strings by hand | n/a |
| K-02 | Token mapping (components.md foundations) | surface/surface-2/border/silver/ink-2/ink-3/on-ink-muted defined, with dark overrides | Adaptasi: alias the kit names onto the existing vars | Sebagian: everything exists under other names (lines 14–47). No aliases, so kit CSS cannot be ported 1:1 |
| K-03 | Focus ring (all interactive) | `box-shadow:0 0 0 2px rgb(10 146 242/.4)`, no outline | Terapkan | Sebagian: global `:focus-visible{outline:2px solid var(--accent);outline-offset:2px}` (l.55). Different look, same purpose |
| K-04 | Field focus | `border-color:var(--accent);box-shadow:0 0 0 2px rgb(10 146 242/.2)` | Terapkan | Belum: `.inp:focus-within` and `.ta:focus` use an outline (l.238, 283) |
| K-05 | Press feedback | `transform:scale(.98)` on buttons, chips, tappable cards; round header buttons `.95` | Terapkan | Sebagian: `.btn:active scale(.97)` (l.173), `.rowlink .99` (l.322), `.at/.ws-btn .98`. Not applied to chips or stat cards |
| K-06 | Icon size from parent | 16px in buttons and chips (`.btn svg{width:16px;height:16px}`), 20px in rail, bar, large tiles | Terapkan | Sebagian: size is passed per call; `btn()` uses 18 for md and 15 for sm (l.562); rail uses 20 ✓ |
| K-07 | Overlay (lib/overlay.ts) | `position:fixed;inset:0;z-index:50;background:rgb(16 17 18/.5);backdrop-filter:blur(2px)`, fade in and out | Terapkan | Sebagian: `.overlay` is .4 alpha with no blur and only fades in (l.410) |
| K-08 | Close button (overlay.ts) | Absolute top 16 / right 16, 32px round, 16px icon, muted → hover canvas bg + fg; on dark `on-ink-muted` → hover white/10 | Terapkan | Sebagian: the panel puts a soft `ibtn('x')` inside its header (l.1059). `openModal` has no close button |
| K-09 | `useIsPhone` (<768px) | Menus, pickers and notifications become a bottom sheet below 768px | Terapkan | Sebagian: `.panel` turns into a bottom sheet at ≤767 (l.467). `.pop` and `.dd-pop` stay anchored popovers on phones |
| K-10 | `useMediaQuery` | `matchMedia` listener | Adaptasi: vanilla `matchMedia` | Sudah: 1280px listener (l.586) |
| K-11 | Roving tabindex (lib/roving.ts) | Arrow keys move and select, Home/End jump, disabled skipped, wraps, one `tabindex=0` | Terapkan: needs a shared `roving(e,list)` helper | Sebagian: only chart columns via `ccKey` (l.1004). Tabs, pills and radio groups have none |
| K-12 | `sameKey` / reset keys | Reset the page to 1 when the filter key changes (arrays compared by value) | Adaptasi: compare the query string per table | Belum: `ST.tb[key].page` survives filter changes and is only clamped (l.1039) |
| K-13 | `useElementSize` | ResizeObserver size | Tidak berlaku: only feeds SignaturePad and charts | n/a |
| K-14 | `useLazyList` + LazySentinel | Phone lazy list, 6 per page, "N more · Load", IntersectionObserver at 120px margin | Tidak berlaku: dashboard lists paginate | n/a |
| K-15 | index.ts barrel | Single export surface | Tidak berlaku: one file; keep helpers grouped in one HELPERS block | n/a |
| K-16 | Button base (button.tsx) | inline-flex, gap 8, radius 999, 14px **600**, no wrap, colour transition, disabled `opacity:.5;pointer-events:none`, svg 16 | Terapkan | Sebagian: `.btn` (l.172) uses weight 500 and `cursor:not-allowed` instead of `pointer-events:none` |
| K-17 | Button primary | `bg accent-strong`, white, `--shadow-glow`, hover `bg accent-dark` | Terapkan | Sebagian: `.b-accent` (l.178) hovers with `opacity:.92`, not `--accent-dark` |
| K-18 | Button secondary | `bg ink`, `on-ink`, hover `ink-3` | Terapkan | Sebagian: `.b-ink` (l.182) hovers with opacity .85. `.b-solid` uses `--invert`, which flips in dark mode; the kit keeps ink |
| K-19 | Button outline | `1px solid --line`, bg card, fg, hover canvas | Terapkan: Cancel / "Lihat semua" | Sebagian: `.b-soft` (l.179) has a `--line-strong` border on `--raised` |
| K-20 | Button ghost | fg text, hover `rgb(16 17 18/.05)` | Terapkan | Sebagian: `.b-ghost` (l.181) uses muted text and hovers `--control` |
| K-21 | Button link | `accent-strong` text, underline on hover (offset 4px) | Terapkan | Belum |
| K-22 | Button danger | `bg danger-soft`, text **--danger** (translated), hover `bg --danger` + white | Adaptasi: red token instead of the kit's accent | Sebagian: `.b-danger` (l.184) has hover only inside `.tbl` (l.187) |
| K-23 | Button card | bg card, fg, `shadow-card`, hover canvas | Terapkan | Sudah: `.b-card` (l.180), hover `--raised` |
| K-24 | Button soft | bg canvas (no border), fg, hover `--raised` | Terapkan | Sebagian: `.b-soft` has a border and acts as outline. A true soft variant is missing |
| K-25 | Button onInk | `rgb(255 255 255/.1)`, white, hover `/.2` | Terapkan: buttons on ink or accent cards | Sebagian: `.b-onpop` (l.183) is solid white with ink text |
| K-26 | Size sm | h32, px12, 12px | Terapkan | Sudah: `.btn.sm` (l.175) |
| K-27 | Size md | h40, px16 | Terapkan | Sudah: `.btn` |
| K-28 | Size lg | h48, px24, 16px | Terapkan | Belum |
| K-29 | Size icon | 40×40 | Terapkan | Sudah: `.btn.icon` (l.176) |
| K-30 | Size icon-sm | 32×32 | Terapkan | Sudah |
| K-31 | Size icon-lg | 44×44 | Terapkan | Sebagian: `.btn.icon-lg` is 48 |
| K-32 | `loading` | Disables the button, sets `aria-busy`, swaps the first icon for a spinning loader (or prepends one) | Terapkan: save actions | Belum: `btn()` has no `loading` option |
| K-33 | `asChild` (link styled as a button) | `<a class="btn b-…">` keeps the exact classes | Adaptasi: `btn()` takes an `href` and emits `<a>` | Sebagian: links are written by hand as `<a class="btn b-onpop sm">` (l.1093) |
| K-34 | Operator size (h-14 full width) | Single primary 56px, full width | Tidak berlaku: operator screens only | n/a |
| K-35 | Card default (card.tsx) | radius 24, bg card, `shadow-card` | Terapkan | Sudah: `.card` (l.196), also padding 20 |
| K-36 | Card ink | radius 28, ink, `on-ink`, `shadow-float`, `isolation:isolate`, blob 288px at right/top -96px, accent/30, blur 64px | Terapkan | Sudah: `.card.ink` + `::before` (l.382–384). Blob is 280px, about equal |
| K-37 | Card accent | radius 24, accent bg, white, glow; one urgent metric | Terapkan (SIM blue) | Sudah: `.card.acc` (l.395) with `--accent-strong` |
| K-38 | Card soft | radius 16, bg `--raised`, no shadow | Terapkan | Sudah: `.tile` (l.201) |
| K-39 | CardHeader (no action) | Column, gap 4, padding 20 | Terapkan | Sebagian: `.ch` (l.197) is a row with min-height 40 and margin-bottom 16 inside the padded card |
| K-40 | CardHeader with action | wrap, space-between, gap 8; title column `flex:1;min-width:0`; actions wrap, `max-width:100%` | Terapkan | Sudah: `.ch` wraps, `.ch .acts` (l.200). Gap is 12 |
| K-41 | CardTitle | 16px 600, line-height 1.25 | Terapkan | Sebagian: `.ch h2` (l.198) matches but adds an accent icon, which the kit has no rule for |
| K-42 | CardDescription | 14px muted | Terapkan | Belum: `card()` has no `desc` option |
| K-43 | CardContent / CardFooter | padding 20, top 0; footer flex | Adaptasi: the card's own padding stands in | Sebagian: no footer slot |
| K-44 | Single-block card | `.card` with p20, or p16 for a strip | Terapkan | Sudah: `.card` p20, `.attn` p16 (l.400) |
| K-45 | Title with count | Title + CountBadge inline | Terapkan | Belum |
| K-46 | EmptyState inside an ink card | Text `on-ink`, paragraphs `on-ink-muted` | Terapkan | Belum (no EmptyState) |
| K-47 | IconTile tones (icon-tile.tsx) | default canvas/body · danger danger-soft/danger · success · warning · info · ink · accent (solid). Only ink and accent are solid | Terapkan | Sebagian: `statIcon()` (l.1295) picks from the `POPS` decorative palette by index, not by status tone |
| K-48 | IconTile sizes | sm 36/r12/icon16 · md 42/r13/icon18 · lg 48/r16/icon20 | Terapkan | Sebagian: only `.stat .si` 40/r13 (l.207) |
| K-49 | IconTile round | `border-radius:50%` | Terapkan | Belum |
| K-50 | Number tile | 16px bold tabular inside the tile | Adaptasi: ranks | Belum |
| K-51 | Shared Tone union | One status-to-tone map feeds tile, stat, dot and badge | Terapkan | Sebagian: `tag` tones and `TONEC` (l.559) exist, but statIcon ignores them |
| K-52 | StatCard layout (stat-card.tsx) | p20, radius 24; label 13px 600 body/.8; value 28/800/-.5px truncate; unit 14 600 muted; hint 12 muted mt4; IconTile on the **right** in a row | Terapkan | Sebagian: `.stat` (l.204) has padding 18/20, a 12px muted label, and the icon on top |
| K-53 | StatCard clickable | Renders as button/a, full width, hover `--raised`, active .98, focus ring | Terapkan | Sebagian: `a.stat` lifts with translateY(-2) and a float shadow (l.89) |
| K-54 | Tone follows number | `count?danger:success`; ink for the one primary stat | Terapkan | Belum: tone comes from position in `POPS` |
| K-55 | StatCard children slot | Child below, margin-top 16, full width | Terapkan | Belum |
| K-56 | Stat row grids | 2 columns, gap 12 → 16 at ≥640, 4 columns at ≥1280; overview 2/3/6 | Terapkan | Sebagian: `.stats` is 4 columns → 2 at <1024 (l.203, 451) with gap 16 |
| K-57 | Badge base + dot (badge.tsx) | radius 999, 2px 10px, 12px **500**, gap 6, svg 12; dot 6px `currentColor` | Terapkan | Sebagian: `.tag` (l.227) is h22, padding 0 9, 11px 600, no dot. `spill()` has a dot but with a border and h28 |
| K-58 | Badge default | canvas bg, body text | Terapkan | Sebagian: `t-neutral` is control bg with muted text |
| K-59 | Badge accent | solid `accent-strong`, white (the one "act now" value) | Terapkan (blue) | Sebagian: `t-accent` is soft blue (l.228) |
| K-60 | Badge success | success-soft / success | Terapkan | Sudah: `t-success` |
| K-61 | Badge warning | warning-soft / warning | Terapkan | Sudah: `t-warning` |
| K-62 | Badge danger | danger-soft / **--danger** | Terapkan | Sudah: `t-danger` |
| K-63 | Badge info | info-soft / info-text (SIM tan) | Terapkan | Sudah: `t-info` |
| K-64 | Badge muted | canvas / muted | Terapkan | Sudah: `t-neutral` matches |
| K-65 | Badge outline | 1px `--line`, fg; draft/open/pending | Terapkan | Belum |
| K-66 | Badge ink | ink / on-ink; released/assigned/confirmed | Terapkan | Belum |
| K-67 | Status → variant map | One map per status union (draft=outline, planned=default, released=ink, in progress=info, paused=warning, failed=danger, done=success, act-now=accent, closed=muted) | Terapkan: approval, order, leads statuses | Sebagian: tones are chosen inline per page; no shared map |
| K-68 | Dot and priority convention | Dot on live or blocked states; critical=accent (→danger), high=danger, normal=default, low=muted | Adaptasi: critical → `--danger` | Belum |
| K-69 | Locked "Released" badge | Muted badge with a lock icon | Tidak berlaku: no locked documents in the dashboard | n/a |
| K-70 | Badge on ink | `rgb(255 255 255/.1)`, white, unless accent | Terapkan: hero card | Sebagian: `.ink-chip` (l.392) is similar but is a chip |
| K-71 | CountBadge | h18, min-width 18, padding 0 4, 10px bold tabular, `box-shadow:0 0 0 2px var(--card)`, accent-strong/white or white/ink on ink. Hidden at 0, caps at 99+ | Terapkan | Sebagian: `.nl .cnt` is 20px/11px (l.122); `.bb-i .cnt` 18px ✓ (l.465). No ring, no 99+ cap |
| K-72 | Header bell badge | top/right -4, h19, min-width 19, 2px canvas border, accent bg, 10.5px bold | Terapkan | Sudah: `.bell i` (l.189) is 20px/11px with a canvas ring |
| K-73 | StatusDot | 8px round, tone bg, optional pulse; `role="img"` + `aria-label` when it stands alone | Terapkan | Sebagian: `.dotc` 8px (l.225). No pulse, no label |
| K-74 | StatusText | Phone card status line | Tidak berlaku: phone app | n/a |
| K-75 | Input base (input.tsx) | h44, full width, px16, 16px (14px from 768), placeholder muted, disabled `opacity:.6;cursor:not-allowed` | Terapkan | Sebagian: `.inp` (l.237) is h44 at 14px fixed with no disabled style |
| K-76 | Input default | radius **16**, 1px `--line`, bg card, accent focus border + ring; disabled bg canvas | Terapkan | Sebagian: `.inp` is a pill (r99) with a line-strong border on `--raised` |
| K-77 | Input soft | radius 16, no border, canvas bg; used inside white cards and sheets | Terapkan: fields inside panels and cards | Belum |
| K-78 | Input pill | r999, no border, card bg, `shadow-card`; canvas search | Terapkan | Sudah: `.inp.card` (l.240), header search (l.1194) |
| K-79 | Left icon | Absolute at left 14px, 16px, muted; input padding-left 40 | Terapkan | Sudah: flex icon inside `.inp` (l.242) |
| K-80 | Right slot | Unit or clear at right 8px; input padding-right 44 | Terapkan: Rp / qty units | Belum |
| K-81 | Input sizes | Sign-in h48; quantity h48 18px 600 tabular; measurement h56 20px bold | Adaptasi: sign-in h48; quantity in order forms | Belum (`.inp.h40` only, l.241) |
| K-82 | Invalid | default: `border-color:--danger`; soft/pill: `box-shadow:0 0 0 2px rgb(danger/.25)`; `aria-invalid` | Terapkan | Belum: no `aria-invalid` anywhere |
| K-83 | Textarea default (textarea.tsx) | min-height 112, r16, p12/16, 1px line, card bg, accent focus | Terapkan | Sebagian: `textarea.ta` (l.282) is min 90, line-strong on raised, outline focus |
| K-84 | Textarea soft + short | No border, canvas bg; short notes min-height 80 | Terapkan | Belum |
| K-85 | NativeSelect default (native-select.tsx) | h44, r16, 1px line, card bg, padding 0 40 0 16, 14px; chevron right 14, 16px, `opacity:.6` | Terapkan for fixed enums of 6 or fewer | Sebagian: `sel()` (l.564) is pill h40/13px. Every select is replaced by the dd enhancer anyway |
| K-86 | NativeSelect soft | h44, r16, no border, canvas bg | Terapkan | Belum |
| K-87 | NativeSelect inline | h32, r999, transparent, padding 0 32 0 12, 500, hover `black/.05` | Terapkan: filter bars, rows-per-page | Sebagian: `.sel.sm` is h32 but bordered (l.245) |
| K-88 | Select placeholder | Empty value shows muted text; placeholder option disabled when required | Terapkan | Sebagian: the enhancer shows "Pilih" (l.3298) in normal colour |
| K-89 | FormField label | Block label, 14px 500 fg, margin-bottom 6; required mark `*` with 2px left margin | Adaptasi: render the mark in `--accent-text`, since red is reserved for danger | Sebagian: `.field>span` is 12.5px (l.236). Required mark is an inline red `<b>` (l.1423) |
| K-90 | FormField message + wiring | 12px message margin-top 4; error in `--danger` replaces the muted hint; `for`/`id`, `aria-describedby`, `aria-invalid` | Terapkan | Sebagian: `.ferr` (l.294) is 600 weight with no hint swap, no `aria-describedby`, no `aria-invalid` |
| K-91 | Combobox rule (combobox.tsx) | Every data-backed dropdown is searchable; a fixed enum of 6 or fewer stays a native select | Terapkan | Sebagian: the enhancer (l.3293) makes **every** select searchable, including tiny enums |
| K-92 | Combobox API / named pickers | items, label, description, keywords, icon, disabled reason, clearable, create; named pickers write a secondary line | Adaptasi: `combo(id,{items,desc,reason,clearable,onCreate})` | Sebagian: options come only from `<option>` text; no description, icon or reason |
| K-93 | Trigger default | h44, r16, 1px line, card bg, px16, 14px; open → `border-color:accent`; chevron 16 `opacity:.6` | Terapkan | Sebagian: `.dd-btn` (l.250) is pill h40/13px on `--raised`. Open rotates the chevron instead of tinting the border |
| K-94 | Trigger soft | h44, r16, canvas bg | Terapkan | Belum |
| K-95 | Trigger inline | h32, r999, px12, 500, hover/open `black/.05` | Terapkan: filters | Sebagian: `.sel.sm .dd-btn` (l.256) is bordered |
| K-96 | Multi trigger chips | Up to 3 chips (h24, max-width 160, 12px 500, canvas bg) then +N; inline variant shows first label + N | Terapkan where a multi filter is added | Belum |
| K-97 | Panel ≥768 | Modal popover, align start, offset 6, collision 12px; width 288, min = trigger width, max `100vw-24`; r16, 1px line, card, float shadow, fade+zoom | Terapkan | Sebagian: `.dd-pop` (l.260) has min 260, flips up/down, rises in. Not modal (no outside inert) |
| K-98 | Panel on phones | Bottom sheet, max-height 70dvh, safe-area padding, sr-only title, rows min 48 | Terapkan | Belum |
| K-99 | Search row | h44, bottom border, search icon, transparent input (16px, 14px from 768), autocomplete/autocorrect/spellcheck off; "N of M" counter once M > 20 | Terapkan | Sebagian: search is an `.inp.card` pill (l.3319) with autocomplete off only. A paged footer (8 per page) replaces the counter |
| K-100 | List and rows | Scroll list max-height 256, padding 4, overscroll contain; row r12, padding 8/12, 14px, active bg canvas; label + secondary 12 muted; check 16 accent at the right | Terapkan | Sebagian: `.dd-opt` (l.263) is 13px. A selected row turns bold accent-text, which the kit does not do. Paging instead of scroll |
| K-101 | Disabled rows | Stay visible; reason appended to the secondary line; `cursor:not-allowed`; muted label; skipped by arrow keys | Terapkan | Sebagian: `aria-disabled` + opacity (l.267). No reason shown, and the highlight can land on a disabled row |
| K-102 | Empty state | px12 py24, centred 14 muted, `Tidak ada hasil untuk "q"` | Terapkan | Sebagian: `.dd-empty` "Tidak ada hasil" without the query (l.268) |
| K-103 | Clear row | Top row with X icon, muted, "Hapus pilihan" when clearable and a value is set | Terapkan: optional filters | Belum |
| K-104 | Create row | Pinned under the list, top border, 600 accent text, plus icon, `Buat "q"` | Adaptasi: only where creating inline makes sense | Belum |
| K-105 | Multi footer | "N dipilih", ghost sm "Hapus", secondary sm "Selesai" | Terapkan with multi | Belum |
| K-106 | Behaviour | Every typed word must match label/description/keywords; highlight starts on the selection; arrows skip disabled; Enter picks; Esc closes; query resets on close; rows block mousedown so focus stays in the input; highlight scrolls the list only, never the page | Terapkan | Sebagian: Enter/Esc/arrows/Tab and initial highlight work (l.3332–3340). Match is one substring, not per word. `scrollIntoView` can scroll the page. Disabled rows not skipped. Options are focusable buttons |
| K-107 | Combobox a11y | Trigger `role=combobox aria-haspopup=listbox aria-expanded aria-controls`; input `aria-autocomplete=list aria-activedescendant`; `role=listbox` (+multiselectable); rows `role=option aria-selected aria-disabled` | Terapkan | Sebagian: trigger has haspopup + expanded; input `role=combobox`; listbox/option/selected present. Missing `aria-controls`, `aria-activedescendant`, `aria-autocomplete`, option ids |
| K-108 | MultiCombobox rows | Checkbox 16px, r5, border; checked accent bg + white check; panel stays open | Terapkan where multi values are needed | Belum: `multiple` selects are skipped (l.3299) |
| K-109 | Switch md (switch.tsx) | 48×28, off `rgb(silver/.6)`, on accent; knob 24px at left 2, `translateX(20px)`, `shadow-sm`; `role=switch aria-checked` | Terapkan (blue on) | Sebagian: `.switch` (l.284) has a 20px knob at inset 4, off `--control` + inset line |
| K-110 | Switch sm | 36×20, knob 16, `translateX(16px)` | Terapkan: table rows | Belum |
| K-111 | Switch disabled / label | `opacity:.5;cursor:not-allowed`; `disabled`; `aria-label` or field id | Terapkan | Sebagian: `swc()` sets `aria-label` (l.570). No disabled support; toggling is an inline attribute flip |
| K-112 | SegmentedControl (segmented-control.tsx) | Track: canvas bg, p4, gap 4, r999. Option h40, px12, 14px 600, `flex:1`; off body + hover `black/.05`; selected tone default ink, success, warning, danger→`--danger` | Terapkan: one-answer choices (theme Light/Dark, master/free-text source) | Sebagian: theme uses `pills()` with `role=tab` (l.3012). Source choice uses `.fchip` radio labels (l.1420) |
| K-113 | Segmented sm / touch | h32; touch h48 full width | Terapkan sm | Belum |
| K-114 | Radiogroup roving | `role=radiogroup`; buttons `role=radio aria-checked`; roving tabindex; arrows select | Terapkan | Sebagian: native radios inside `.fchip` (l.1420) work by keyboard but look different |
| K-115 | Choice grid | >4 options: 2-column grid, gap 8, min-h48, r16, px16, 14px 600; selected ink, others canvas | Adaptasi: reason pickers with more than 4 options | Belum |
| K-116 | Tabs shared (tabs.tsx) | `role=tablist aria-orientation`; trigger 600, gap 6, svg 16; roving keys; count pill 10px bold, padding 1/6, canvas/body (white/.2 when active) | Terapkan | Sebagian: `pills()`/`utabs()` (l.565, 567) set `role=tab aria-selected` but have no roving or tabindex, and pills have no count |
| K-117 | PillTabs md | List: card bg, p4, gap 4, `shadow-card`, scrolls sideways with hidden scrollbar, pr24. Trigger h36, px16, 14px; active ink/on-ink | Terapkan | Sebagian: `.pills` (l.212) adds an inset line, gap 2, h32/13px, active `--invert` (flips in dark), does not scroll |
| K-118 | PillTabs sm | h32, px12, 12px | Terapkan | Belum (single size) |
| K-119 | UnderlineTabs | List full width, sideways scroll, baseline as `box-shadow:inset 0 -1px 0 var(--line)`. Trigger 2px transparent bottom border, padding 10/12, 14px 600 muted, inset focus ring; active accent border + fg | Terapkan | Sebagian: `.utabs` (l.216) draws `border-bottom` (clips the active underline), gap 20, padding-bottom 8, only active is 600 |
| K-120 | SegmentedTabs + urgent | Phone list tabs, equal width; urgent tab turns accent when active | Tidak berlaku: phone app. The dashboard uses PillTabs on phones | n/a |
| K-121 | Right fade on scrolling tabs | Absolute right fade 28px, card→transparent, inside a relative min-width:0 wrapper | Terapkan with K-117/119 | Belum |
| K-122 | Chip base (chip.tsx) | h40, r999, 600, gap 8, svg 16, press .98, `aria-pressed` + data-active, disabled .5 | Terapkan | Sebagian: `.fchip` (l.221) is h32, 12.5px 500. `aria-pressed` only in some uses (l.1523, 2215) |
| K-123 | Chip default (toggle inside cards) | 1px line, card bg, px14, 12px body; active accent-strong border + bg + white | Terapkan (blue) | Sebagian: `.fchip` uses line-strong on raised; active is `--invert` |
| K-124 | Chip filter (canvas filter rows) | card bg, px16, 14px, `shadow-card`; active ink/on-ink | Terapkan | Belum (single chip style) |
| K-125 | Chip count + "Semua" | Count pill 10px bold (white/.2 active, canvas off); "Semua" carries the total; clicking the active chip clears | Terapkan | Sebagian: `.fchip .n` is opacity .7 text, not a pill |
| K-126 | ChipRow | flex, gap 8, nowrap, sideways scroll, hidden scrollbar, padding-bottom 4 | Terapkan | Sebagian: `.chips` wraps (l.220) |
| K-127 | Steps container (steps.tsx) | Horizontal `<ol>` scrolls sideways, gap 8, pb8, pr40; right fade 40px canvas | Terapkan: order lifecycle | Belum: `.steps` is a vertical timeline (l.430) |
| K-128 | Step states | Pill h40, px16, 14px 600. current ink (`aria-current=step`); done card + shadow + 20px success-soft check; upcoming `--raised` muted; skipped `--raised` silver | Terapkan | Sebagian: `.sd.done/.cur/.up` dots (l.434) carry the same meaning, different shape. No `aria-current`, no skipped state |
| K-129 | Step index / hint / sr-only | Index 12px tabular `opacity:.6`; hint 11px 500 `opacity:.7`; sr-only state text | Terapkan | Sebagian: sub-text present (`.step small`); no index, no sr-only state |
| K-130 | Steps placement / derivation | Own `.card` p20; states derived from a flow array; cancelled → all skipped | Terapkan: Pesanan (l.1371) | Sebagian: derived inline. Insentif rules (l.1670) are a check list (K-219), not steps |
| K-131 | ProgressBar (progress.tsx) | `role=progressbar` + valuemin/max/now/valuetext + label; track canvas (white/.25 on dark); heights 4/6/10; fills ink (default), accent (blue, or `--danger` when it means late/held), success, warning, info, white | Terapkan | Sebagian: `.bar` 8px on `--control` (l.318), `achBar` (l.903). No ARIA. Default fill is accent |
| K-132 | Progress caption | 11px muted, margin-top 4 | Terapkan | Sebagian: side caption 12px (l.904) |
| K-133 | SegmentBar + legend | flex, gap 2, r999, h10 (sm 6); segments min-width 4, `flex-grow:value;flex-basis:0`; zero skipped; `role=img` "label: value"; legend margin-top 12, 12px muted, 8px dots | Terapkan: attendance mix, lead status | Belum (donut and funnel only; `.legend` l.348 exists) |
| K-134 | Avatar (avatar.tsx) | Round, 600, white initials (first two words), `role=img aria-label` + title; colour = stored or name hash into the muted 10-colour palette (#4c5a78 … #6b6f73) | Terapkan | Sebagian: `av()` (l.557) hashes into the 5 `POPS` colours, some light with dark text, no `title` |
| K-135 | Avatar sizes | xs 24/9px · sm 28/10 · md 36/12 · lg 48/14 · xl 80/24 | Terapkan | Sebagian: 32 / 40 / 48 (l.231–232) |
| K-136 | Avatar ring | `box-shadow:0 0 0 4px var(--card),var(--shadow-card)` | Terapkan: profile | Belum |
| K-137 | AvatarStack | `margin-left:-8px` overlap, each with a 2px card ring, "+N" on canvas | Terapkan: assigned teams (penugasan) | Belum |
| K-138 | DataTable shell (data-table.tsx) | `border-collapse`, 14px, inside `relative overflow-x:auto`. th h40, px16, 12px 600 uppercase `.025em` muted. td 12/16. Row bottom border `--line`, last none, hover `--raised`, clickable pointer | Terapkan | Sebagian: `.tbl` (l.303) is 13px, th 11.5px with padding 10/12, td 12px, `.tbl-wrap` margin -4 |
| K-139 | Sortable headers | th button: pill, gap 4, padding 4/6; up-down icon (`opacity:.4`), up or down 14px; `aria-sort`; nulls last; `localeCompare(…,{numeric:true})` | Terapkan | Belum |
| K-140 | Clickable row open button | First cell gets a 32px round button with ↗ icon, `aria-label="Buka <key>"`, hover card; clicks from nested controls or portals never open the row | Terapkan | Sebagian: `tr onclick` only (l.1043), so the keyboard cannot open rows. Nested guard is `stopPropagation` on select/action cells |
| K-141 | Pagination | Top border, padding 12/16, 14px muted; "1-12 dari 52" tabular; two **outline** icon-sm chevrons | Terapkan | Sebagian: `.pager` (l.313) has margin-top 14, no border, a rows-per-page select, soft icons, and "n/N" |
| K-142 | Empty body | One full-width cell with a compact EmptyState ("Belum ada data") | Terapkan | Sebagian: plain centred muted text (l.1044) |
| K-143 | pageSize / resetPageKey / restore | Default 12; 0 turns paging off; filter change → page 1; sort and page restored after Back | Terapkan | Sebagian: per-page 10 (+select); state kept in `ST.tb` ✓; no reset on filter change; no paging-off option |
| K-144 | First column convention | Id in mono 12px 600 over a truncated name; phone-only badge, meta and meter lines | Terapkan | Sebagian: `.mono` exists (l.57); `c.prim` (l.1043) only for the phone card |
| K-145 | hideBelow | Hide columns progressively at sm / md / lg / xl so a phone keeps 2 columns | Adaptasi: keep `.tbl.resp` stacked cards or switch to breakpoints | Sebagian: one breakpoint `.hide-m` plus stacked phone rows (l.475–484) |
| K-146 | Numbers / units / limits / dash | Right-aligned tabular; unit `<span class=xs muted>`; past a hard limit 600 **--danger**, soft limit 600 warning; late date `--danger`; missing value muted "–" | Adaptasi: limit colour → `--danger` | Sebagian: `.r` alignment, en dash used (l.931). No limit-colour rule |
| K-147 | Meter column | Width 144–176px; value 12px 600 above a ProgressBar, or bar + 11px caption; tone by state | Terapkan | Sebagian: `achBar` / `shareBar` (l.903–904) put the caption to the side |
| K-148 | Trend cell | Sparkline 28px high | Adaptasi: decide in the charts audit | Belum |
| K-149 | Aging row tint | `background:rgb(warning-soft/.4)` | Terapkan: overdue, pending tasks | Belum |
| K-150 | Row actions | Master data: trailing ghost icon-sm edit/remove (remove in `--danger`, disabled with `title` = blocker), column 88px. Operational: outline sm verbs, secondary for the one-click resolve | Terapkan | Sebagian: `rowActs()` (l.566) uses labelled soft "Edit" + danger "Hapus" sm |
| K-151 | Sort defaults | Initial sort on severity or date, descending | Terapkan | Belum |
| K-152 | Table placement | List-page main table sits on the canvas without a card; a table that is one section of a detail page goes in a card | Adaptasi: list pages can drop the card | Sebagian: tables sit inside `card()` |
| K-153 | KeyValue (key-value.tsx) | `<dl>` with row dividers; standalone r24, card, padding 4/20, shadow; row grid gap 12, py12, 14px, label column 96/120/150; dt muted; dd `min-width:0;word-break`; hidden rows | Terapkan: detail panels | Belum: `.kv` (l.324) is a 3-column metric tile grid, which matches K-225 instead |
| K-154 | EmptyState (empty-state.tsx) | Centred column, gap 8, padding 48/24 (compact 32/16); 48px round canvas icon tile (24px icon); title 14px 600 fg; description max-width 320 muted; action margin-top 8 | Terapkan | Belum: no helper |
| K-155 | Empty copy rules | Title names what is missing or the good outcome; description says how records arrive; action offers the next step | Terapkan | Sebagian: copy like "Belum ada data." only |
| K-156 | Not-found page | Card + EmptyState + back button | Terapkan | Sebagian: `pageNotFound` exists (l.1183); not checked against this pattern |
| K-157 | Banner structure (banner.tsx) | wrap, gap 12, r24, padding 12/16; 36px round tile (card bg, 16px icon); text min-width 192, 14px; title 600 fg over body/.7; actions wrap; dismiss ghost icon-sm | Terapkan | Sebagian: `callout()` (l.569, CSS 288) is r16, padding 12/14, bare bulb icon, no title/actions/dismiss |
| K-158 | Banner info | info-soft box, tile card + info-text | Terapkan | Sebagian: `.callout` default (tan info) |
| K-159 | Banner warning | warning-soft, warning icon | Terapkan | Belum |
| K-160 | Banner danger | danger-soft, `--danger` alert icon | Terapkan | Belum |
| K-161 | Banner success | success-soft, check icon | Terapkan | Belum |
| K-162 | Banner neutral | card + shadow, canvas tile | Terapkan | Sebagian: `.callout.blue` is an extra blue tone (l.291), keep as info-blue |
| K-163 | Banner uses | Blocker explaining a disabled action; rule that escalates info→warning; scope link with "Tampilkan semua" | Terapkan | Sebagian: `callout` is used 51× as a static hint only |
| K-164 | Inline blocker note | `.card` p16 14px with a bold "Belum bisa …:" lead | Terapkan | Belum |
| K-165 | SplitStats (split-stats.tsx) | Margin -20 / bottom -20, top border, 2–4 equal columns with dividers; cell padding 12/8 centred; label 10.5px 500 muted truncate; value 15px 800 tabular; last child of a p20 card | Terapkan: hero / sales cards | Belum |
| K-166 | PageHeader (page-header.tsx) | margin-bottom 24, wrap; eyebrow kicker; h1 24px 700 tight; description 14 muted max-width 672 (one sentence); actions wrap, `max-width:100%` | Adaptasi: title stays in the shell header | Sebagian: `h1.pt` 24px 700 + breadcrumb (l.156, 1193). No description sentence; actions are per page |
| K-167 | Kicker | 11px 600 uppercase `.05em` muted (`on-ink-muted` on ink) | Terapkan | Sudah: `.kick` (l.381) at `.08em`; `.ink .kick` ✓ |
| K-168 | Dialog content + sizes (dialog.tsx) | Fixed, centred, max-height `100dvh-32`, width `100%-32`, r24, p24, card, float shadow, fade + zoom 95; sizes 384 / 512 / 672 / 896 | Terapkan | Sebagian: `.modal` (l.416) is 520px only, max-height `100dvh-40`, rise animation |
| K-169 | Dialog header / title / desc / footer | Header margin-bottom 16, gap 4, pr24; title 18px 600; description 14 muted; footer margin-top 24, `column-reverse` on phone → row at ≥640, end-aligned | Terapkan | Sebagian: modals use `h2.pt` 22px with a dot + `.sm.muted` (l.1447); footer is an inline flex end, not reversed on phone |
| K-170 | Radix dialog behaviour | **Focus trap** (Tab cycles inside), Esc closes, focus returns to the trigger, `aria-labelledby`/`describedby`, body scroll lock, inert background | Terapkan (by hand) | Sebagian: Esc ✓ (l.1064), return focus ✓ (`lastFocus`), `aria-modal` ✓. No focus trap. `openModal` has no label. No scroll lock |
| K-171 | Entity dialog state | One opener per entity (edit or create); form built fresh per open; errors shown only after the first submit | Terapkan | Sebagian: `openPanel`/`openModal` rebuild each time ✓; error timing varies per form |
| K-172 | Dialog form layout | 1 column → 2 at ≥640, gap 16; main picker and notes span both; number rows 2→4 columns, gap 12 | Terapkan | Sebagian: `.grid.g2` gap 10–12 (l.1380) |
| K-173 | Description states the rule | Description names the governing rule; edit mode names what stays fixed, shown as `readonly` | Terapkan | Belum |
| K-174 | Error / optional / default copy | Full-sentence instructions; "Opsional sampai …" hints; default value as placeholder with its source | Terapkan | Sebagian: some "(opsional)" labels; `doReject` error "Alasan wajib diisi." ✓ |
| K-175 | Boolean row | flex, space-between, gap 12, r16, `--raised` bg, p12; 14px 500 title + 12 muted line + Switch | Terapkan | Sudah: `.rowlink` + `swc()` in Pengaturan (l.3013) |
| K-176 | Multi-choice chips | Filter chips with a check icon when active | Terapkan | Sebagian: `.fchip aria-pressed` day picker (l.1523), no check icon |
| K-177 | Dependent pickers | Changing the parent clears the child; child disabled with "Pilih … dulu" | Terapkan | Belum: only callout copy (l.2603) |
| K-178 | Preview / blocking live check | Preview box margin-top 16, r16, `--raised`, p12, 14px. Blocking note r12, accent-soft, padding 8/12, 12px 500 → for SIM use danger-soft/`--danger`; submit disabled while it shows | Adaptasi: blocking note in danger tokens | Belum |
| K-179 | Quantities | Unit in the hint; starts at the remaining amount; `inputmode="decimal"` | Terapkan | Sebagian: `inputmode="numeric"` (l.1382) |
| K-180 | One dialog, several verbs | Copy table keyed by verb | Adaptasi where verbs share a form (approve/reject) | Sebagian: reject and approve are separate code paths (l.1555–1559) |
| K-181 | Dialog footer | Outline "Batal" + primary named with the verb | Terapkan | Sebagian: "Batal" is `b-card`, not outline |
| K-182 | ConfirmDialog base (confirm-dialog.tsx) | `role=alertdialog`, max-width 448; title 18px 600 as a question with the record code; description margin-top 6, 14 muted; children margin-top 16; outline Cancel + primary verb | Terapkan | Sebagian: delete modals (l.1447, 1539) ask a question with the code but are `role=dialog` at 22px |
| K-183 | `destructive` | Confirm button `bg --danger`, white, no shadow, darker on hover | Terapkan | Sebagian: delete confirm button not checked in full; uses the `b-danger` soft variant |
| K-184 | `confirmDisabled` | Confirm held until a required field is filled | Terapkan | Belum |
| K-185 | Approve / reject shared state | One deciding state; reject = destructive and disabled until a note is typed (note textarea min-height 80) | Terapkan | Sebagian: reject validates only on click (l.1559) |
| K-186 | Blocked delete | Destructive + disabled, description names the blocker | Terapkan (house rule: data still in use cannot be deleted) | Sebagian: `inUse` text exists (l.2087); dialog form not checked |
| K-187 | Queue pending state | One pending `{kind,id}`, the matching dialog mounted | Adaptasi | Sudah (implicitly): one `#layer` |
| K-188 | Remove vs Delete copy | Master data "Hapus {nama}?"; documents use the lifecycle verb | Terapkan | Sudah: "Hapus …?" ✓ |
| K-189 | Sheet right (sheet.tsx) | Fixed, top/bottom/right 12, width `100%-24`, r28, float shadow, column; widths 448 / 576 / 672; slides from the right | Terapkan | Sudah: `.panel` (l.411) at 480 with slide-in. Single size |
| K-190 | Sheet header / body / footer | Header padding 20 20 12, pr56; body flex scroll px20 pb20; footer end, gap 8, top border, padding 16/20 | Terapkan | Sebagian: `.panel-h/.panel-b/.panel-f` (l.412–415) use 24px padding, gap 10, and the close button sits in the header flow |
| K-191 | Sheet bottom (phone) | Bottom 0, max-height 85dvh, scroll, top radius 28, safe-area padding; slides up; grab handle 40×6 `--line` (white/.2 dark) | Terapkan | Sebagian: `.panel` on phone (l.467) has 92dvh and no handle. `.drawer` has a handle (l.419) |
| K-192 | Sheet tones | Light card; dark ink reserved for navigation | Terapkan | Sudah: `.drawer` ink for nav (l.418), panel light |
| K-193 | Every phone menu is a bottom sheet | Overflow, user, bell, workspace, search and pickers become sheets below 768 | Terapkan | Belum: `.pop` stays anchored on phone (l.421) |
| K-194 | DropdownMenu / ActionMenu ≥768 (dropdown-menu.tsx, action-menu.tsx) | Content r16, 1px line, card, p4, min-width 192, float shadow, offset 8; item r12, padding 8/12, 14px, highlighted canvas, disabled .5, svg 16, destructive `--danger`; label 11px 600 uppercase muted; separator 1px line, margin 4 | Terapkan | Sebagian: `.pop` (l.421) is fixed 280px with p8; `.mi` (l.422) is padding 10/12, 13px. No label or separator class (`hr.sb-hr` reused) |
| K-195 | ActionMenu phone sheet rows | min-height 48, r16, padding 6/12, 14px 500, IconTile sm (danger when destructive), 12px muted description; sr-only title when none | Terapkan | Belum |
| K-196 | Record overflow trigger | Outline icon button "Aksi lain"; items filtered by what the current state allows | Terapkan: detail panels | Belum |
| K-197 | Radix menu behaviour | `role=menu/menuitem`, `aria-haspopup=menu` + `aria-expanded`, arrow-key roving, Home/End, typeahead, Esc closes and refocuses the trigger, focus moves into the menu on open | Terapkan (by hand) | Sebagian: Esc closes and outside click closes (l.1064–1065). `aria-haspopup="true"` with no expanded state; no roles, no arrows, no focus move or return |
| K-198 | Popover (popover.tsx) | r16, 1px line, card, p0, float shadow, offset 8, collision 8; notification panel 400px, end-aligned | Terapkan | Sebagian: bell uses `.pop` 280px (l.1126) |
| K-199 | Tooltip (tooltip.tsx) | ink bubble r12, padding 6/10, 12px 500, on-ink, float shadow, max-width 320, offset 6; 200ms delay; shows on focus and hover; collapsed rail labels at offset 22 | Terapkan: collapsed rail, icon buttons | Sebagian: native `title` only (l.563, 1092). Not keyboard-visible, not styled |
| K-200 | Toaster stack (toast.tsx) | Fixed; phone: left/right 16, bottom 96, centred; ≥768: bottom/right 16, end-aligned; z 60; gap 8; max 3; 3.5s; `role=region aria-label aria-live=polite` | Terapkan | Sudah: `.toasts` (l.424, 486) right/bottom 24, max 3, 3.6s, `aria-live` (l.493) |
| K-201 | Toast item | max-width 384, r16, ink bg, on-ink, padding 12/16, float shadow; 20px round tone mark (success+check / `--danger`+alert, none for default); 500 message; 12px on-ink-muted description; action pill white/.1; 24px dismiss X | Terapkan | Sebagian: `.toast` (l.425) uses `--invert` (flips in dark), shows a bare icon (check even on default), has no action or dismiss button |
| K-202 | Toast copy and tones | Past-tense result; description with quantity and code; success = create/save/advance, default = pause/hold/cancel/delete, danger = failures | Terapkan | Sebagian: past tense ✓; `toast()` defaults to success, so deletes show a check |
| K-203 | PhotoInput (photo-input.tsx) | 3-column grid, gap 8; square thumbs r16 canvas, cover image, remove button 28px ink/.7; add tile = label around a hidden file input (`image/*`), camera icon; hidden at max | Adaptasi: product photo field | Sebagian: `fotoField` (l.2778) with a hidden file input + `.foto-lg`; `.thumbs` 84px gradient placeholders (l.429) |
| K-204 | SignaturePad (signature-pad.tsx) | Canvas signature with Clear | Tidak berlaku: no signing flow on the dashboard (no "ttd" or "tanda tangan" in the file) | n/a |
| K-205 | Rail container (rail.tsx) | r28, ink, py16, float shadow, width transition 200ms; collapsed 76 centred, expanded 240 px12; nav margin-top 12, p4, scrolls with hidden scrollbar; workspace and footer margin-top 12 | Terapkan | Sudah: `.sidebar` (l.106–108) at 248 expanded, padding 16/12; `.sb-scroll` p2 |
| K-206 | RailItem | r16, on-ink-muted, hover white/.1 + white; svg 20; active accent bg + white + glow; collapsed 44 square with a right tooltip + sr-only label and count; expanded h44, gap 12, px12, 14px 500; sub-item h36, pl44; trailing chevron; white CountBadge | Terapkan | Sebagian: `.nl` (l.117) is h40 at 13.5px; active `--accent-strong` ✓; collapsed count is a 10px dot (l.125); `title` instead of a tooltip; no sub-items |
| K-207 | RailAction (create) | Collapsed 44 round accent + glow, hover lift + scale 1.05; expanded h44 full width pill | Adaptasi: only if a global "create" exists | Belum (none) |
| K-208 | RailWorkspace | Expanded: r16, 1px white/.1 border, ink-2, p10, hover ink-3, press .98; 36px card tile; kicker 10.5px; name 12.5px 700 white; up-down icon. Collapsed: 44 card tile + tooltip | Terapkan | Sudah: `.ws-btn` / `.ws-tile` (l.134–141) |
| K-209 | RailCollapse | Expanded h40, r16, 1px white/.1, 12px 600, "Ciutkan menu"; collapsed 44 + "Buka menu" tooltip; `aria-expanded`; only at ≥1280 | Terapkan | Sudah: `.sb-collapse` (l.147, 1099) with `aria-expanded` and the ≥1280 gate |
| K-210 | BottomBar (bottom-bar.tsx) | Fixed left/right/bottom 12, safe-area margin, h68, r22, ink, px12, float shadow, hidden from 768 | Terapkan | Sudah: `.bbar` (l.459) |
| K-211 | BottomBarItem | 44 square r16, on-ink-muted, hover white/.1, press .98, svg 20; active accent-strong + glow; white CountBadge at -4/-4; count in `aria-label`; `aria-current` | Terapkan | Sudah: `.bb-i` (l.460–465) |
| K-212 | BottomBarAction | 48 round accent-strong, glow, hover accent-dark | Terapkan | Sudah: `.bb-act` (l.463) for Approval |
| K-213 | Nested list row | r16, `--raised`, p12, gap 12, hover canvas; title 14px 600 truncate; meta 12 muted joined with " · "; right column (badge over value) hidden on phone, with a phone copy below | Terapkan | Sebagian: `.rowlink` (l.320) hovers to `--control`; no standard right column or phone copy |
| K-214 | Row with its own action | div row + inner link (`flex:1`, hover accent) + trailing sm verb button | Terapkan | Belum |
| K-215 | Sequence tile | 40px, r12, card, 12px bold tabular, card shadow | Adaptasi: ranks or route order | Belum |
| K-216 | Selected state | Compact picker row ink fill; selectable white card `0 0 0 2px ink` ring; deep-linked record accent ring + scroll to centre | Terapkan | Sebagian: `.stat.on` inset 2px fg ring (l.206); no deep-link highlight |
| K-217 | Ranked row | Nested row + 36px rank tile; head baseline split; meter line with ProgressBar + 11px caption | Terapkan: grading / top sales | Belum |
| K-218 | History row | items start, gap 12, r16, padding 8/12, hover `--raised`; time column 12px muted tabular (width 64 relative or 48–56 clock); 14px text; 11px meta with person + mono event type; newest first | Terapkan: activity log, approval history | Belum: `.step` timeline (l.431) is the nearest |
| K-219 | Check list | 1 column → 2 at ≥640, gap 6; rows r12, `--raised`, padding 8/12, 12px; 16px round mark success / `--danger` (blocking) / warning (advisory) / muted (unknown); bold label over muted note; failed check links to its fix | Terapkan: insentif rules (l.1670) | Sebagian: rendered as vertical `.steps` with done/up dots |
| K-220 | Link chips | Entity chip: name 500 truncate over 11px mono code, hover accent. Inline code link: mono 12px 500, hover underline. PersonChip: xs avatar + name + 12px muted hint, "Belum ditugaskan" when empty. Stop propagation inside clickable rows | Terapkan | Belum: names are plain text |
| K-221 | Revision timeline | Left border `--line`, pl20, gap 12; 12px dot at left -26 with a 4px card ring (success / warning / line); current gets an ink "Terkini" badge | Adaptasi: change history (Usulan) | Sebagian: `.steps` vertical line + dots (l.430–435) |
| K-222 | Tree row | Rows with toggle chevron, kind tile, children indented with a 2px left border | Adaptasi: region / branch drill-down hierarchy | Sebagian: breadcrumb drill chips (l.1334), no tree |
| K-223 | Flow chain | Sideways scroll; steps h40 r999 `--raised` px16 14px 600 with a count pill (card + shadow); chevron between steps | Terapkan: lead funnel stages | Sebagian: `.funnel` bars (l.374, 1290) and drill chips with chevrons (l.1334) |
| K-224 | Metric on ink | Kicker in on-ink-muted, then baseline row: value 30px bold tight tabular + unit 12px 600 on-ink-muted | Terapkan | Sudah: `.hero-big` / `.hero-unit` (l.387–388), 60px hero size |
| K-225 | Reading tile | r16, canvas, padding 10/12; kicker; value 24px bold tabular + unit 12px 600 muted; "n/a" when empty; 2-column grid, gap 8 | Terapkan | Sudah: `.kv>div` (l.325) r16 `--raised` p12 with 14px/700 values, 3 columns; `.tile` stats (l.931) |
| K-226 | Settings / navigation list | Row dividers, r24, card, shadow; rows min-height 64, padding 12/16, 44 round icon, 14px 600 label + 12 muted hint, chevron | Terapkan: Pengaturan | Belum: Pengaturan uses cards + `.rowlink` |
| K-227 | Total footer in a card | margin-top 12, space-between, r16, `--raised`, padding 12/16; muted label + 16px bold tabular value | Terapkan: order totals | Belum |

### Target helpers that need to change
- **`btn()` (l.562) and `.btn*` CSS (l.172–187):** weight 600; svg 16; press .98; add `outline`, `link`, `onInk` and a true border-less `soft`; current `b-soft` becomes outline; hovers use `--accent-dark` / `--ink-3` instead of opacity; `lg` size; icon-lg 44; `loading` option; `href` option that emits `<a>`.
- **`ibtn()` (l.563):** icon 16.
- **`card()` (l.568), `.ch`:** description option, count badge in the title, column header when there is no action.
- **`.stat` + `statIcon()` (l.204, 1295):** icon on the right, 13px 600 label, `tone` from the number instead of `POPS`, children slot, hover/press per K-53.
- **`tag()` / `spill()` (l.558–560):** 12px 500 with padding 2/10; solid accent, outline and ink variants; `dot` flag; one shared status → tone map.
- **`av()` (l.557):** muted 10-colour palette with white initials, xs/sm/md/lg/xl sizes, `ring`, `title`.
- **`.inp` / `.ta` / `sel()` / `.field` / `.ferr`:** default r16 with card bg and border + ring focus; soft variant; right slot; `aria-invalid`; hint/error swap with `aria-describedby`; required mark in accent-text.
- **Dropdown enhancer (l.3293–3344):** skip enums of 6 or fewer (or style them as native selects); r16 trigger, open border; per-word match; skip disabled rows; scroll only the list; `aria-controls` / `aria-activedescendant` / option ids; `Tidak ada hasil untuk "q"`; scroll list instead of 8-per-page paging (or keep paging as an Adaptasi); bottom sheet below 768; clearable, create, disabled reason and multi support.
- **`swc()` (l.570):** 24px knob at left 2, silver off, `sm` size, `disabled`.
- **`pills()` / `utabs()` (l.565, 567):** roving keys + tabindex; ink active instead of `--invert`; sm/md sizes; count pill; horizontal scroll + right fade; underline baseline as an inset shadow.
- **`.fchip` / `.chips`:** h40; default and filter variants; `aria-pressed` everywhere; count pill; no-wrap scrolling row.
- **`dataTable()` (l.1038):** sortable headers with `aria-sort`; first-cell open button; page reset on filter key; outline pager buttons with a top border; EmptyState compact; progressive column hiding; ghost icon row actions for master data.
- **`openModal` / `openPanel` / `closeLayer` (l.1057–1063):** focus trap, `aria-labelledby` / `aria-describedby`, body scroll lock, close X in modals, overlay .5 + blur 2px, modal sizes, title 18px 600, phone footer reversed, panel grab handle on phone.
- **`userPop` / `bellPop` / `wsPop` / `searchPop` + `.pop` / `.mi` (l.1102–1132):** `role=menu/menuitem`, `aria-expanded`, arrow-key roving, focus into the menu and back to the trigger, label/separator classes, bottom sheet below 768, bell panel 400px.
- **`toast()` (l.571):** ink bg (not `--invert`), round tone mark, no mark for the default tone, action + dismiss button, region role; callers pass `default` for delete/cancel.
- **`callout()` (l.569):** grow into Banner (tone, title, round tile, actions, dismiss).
- **`.bar` / `achBar` / `shareBar` (l.318, 903):** progressbar ARIA, xs/sm/md heights, ink default, tone param.
- **`.steps` (l.430):** keep as a vertical timeline for history and revisions; add a horizontal pill stepper for order lifecycle; convert insentif rules to a check list.
- **`.kv` (l.324):** rename to reading tile; add a real KeyValue.
- **Rail `.nl` (l.117):** h44 at 14px, collapsed count as a real white CountBadge, styled tooltip instead of `title`.

### New helpers to create
- `badge()` and a shared `STATUS_TONE` map; `countBadge(n,{tone})` (0 → empty, 99+ cap); `statusDot(tone,{pulse,label})`.
- `iconTile(icon,{tone,size,round})`.
- `emptyState({icon,title,desc,action,compact})`.
- `banner({tone,title,body,action,dismiss})`.
- `keyValue(items,{labelWidth,bare})`.
- `splitStats(items)`.
- `segmented(opts,val,fn,{size})` with radiogroup roving.
- `chip(label,{variant,active,count,icon})` and `chipRow()`.
- `stepper(steps)` (horizontal pill stepper with sr-only state).
- `progressBar(v,{tone,size,label})` and `segmentBar(segments,{legend})`.
- `avatarStack(people,{max,size})`.
- `confirmDialog({title,desc,confirmLabel,destructive,disabledUntil,body,onConfirm})` with `role=alertdialog`.
- `actionMenu(trigger,items,{title})`: dropdown at ≥768, bottom sheet below.
- Shared `trapFocus(el)`, `roving(e,selector)` and `isPhone()` (matchMedia 767.98px) utilities.
- `tooltip` (CSS bubble shown on hover and focus, `aria-describedby`).
- `formField({label,required,hint,error})` with id/describedby/invalid wiring.
- Composite row helpers: `listRow`, `rankedRow`, `historyRow`, `checkList`, `entityChip` / `codeLink` / `personChip`, `flowChain`, `settingsList`, `totalFooter`, `sequenceTile`.