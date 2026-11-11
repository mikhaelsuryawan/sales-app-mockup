## 1. Checklist

All line numbers point to `/Users/mikhaelsuryawan/WIT/Project/Sales App/mockup/dashboard/index.html`. Skill sources: `references/layouts.md` (LY), `references/patterns.md` (PT), `assets/repo/apps/admin/src/layouts/*` (SRC).

| ID | Source | Requirement (concrete) | Target routes affected | Applicability | Status |
|---|---|---|---|---|---|
| L-01 | LY Admin shell / AdminLayout.tsx | Shell is `h-dvh flex gap-4 overflow-clip bg-surface`, padding `p-3` that becomes `lg:p-4` from 1024 | all except /login | Terapkan | Sudah. `.shell` L100 uses 16px padding, dropping to 12px below 1024 (L461) |
| L-02 | LY / AdminLayout GLOW | Ambient glow sits in its own clipped `overflow-hidden` box; the gradient RGB values come from the accent | all | Adaptasi: swap the red RGB for SIM blue | Sudah. `.glow` and `::before` with `var(--glow)` at L101-102. Check that `--glow` resolves to blue |
| L-03 | LY | `main` is the only scroll box, with `pb-24` on phone and `md:pb-2` above | all | Terapkan | Sudah. `main` CSS L168; phone gets `padding-bottom:100px` in the 767 media block |
| L-04 | LY / AdminLayout scrollTops | Save `scrollTop` per history entry. PUSH/REPLACE starts at the top; POP (Back/Forward) restores the saved position | all lists | Terapkan | Sebagian. render() L1187/L1201 keeps scroll on the same path and resets on a new path, but never restores on Back |
| L-05 | LY | Page body is `space-y-4`; PageHeader carries `mb-6` | all | Adaptasi: the title lives in the header h1, so only the 16px vertical rhythm applies | Sebagian. Rhythm comes from margin-bottom on `.stats`, `.attn` and `.g-hero`; no wrapper |
| L-06 | LY grids | Hero `1.5fr/1fr`, detail pairs `lg:2`, lists+analysis `1fr 1fr 20rem`, side `2fr/1fr` or `20rem/1fr`, all with a `grid-cols-1` base and `minmax(0,…)` | /, /leads, /live-tracking, /pengaturan | Terapkan | Sudah. `.g-hero` L378, `.g-ov` L192, `.g2` L194, `.g-side` L195 collapse at 1279/1023/767. `.g-side` puts the list on the right (340px), the mirror of `20rem_1fr` |
| L-07 | LY Rail tiers / SideRail | Rail hidden below md; collapsed 76px with no collapse control from md to below xl; expandable from xl with the choice persisted | all | Terapkan | Sudah. `.rail-wrap` hidden ≤767; railCollapsed L1185; canExpand L1087; `sim-sidebar` L1123. Expanded width is 248px (recipe w-60 = 240) |
| L-08 | LY Rail header | Link to `/`, logo tile `size-11 rounded-2xl bg-white/5` (mark size-7); expanded adds product name 15px bold over an 11px tagline | all | Adaptasi: product "Sales Console", tagline e.g. "SIMGROUP" | Sebagian. sidebarHTML L1090 shows the "S" mark when collapsed and the logo image when expanded; no name or tagline |
| L-09 | LY Rail action | `RailAction "Create"` opens the shared CreateMenu (side right, align start) | all | Adaptasi: a "Buat" menu that only aggregates create flows that already exist (ordForm, visForm, promoForm, infoForm, openForm('toko'/'user'/…)) | Belum |
| L-10 | LY Rail nav / SideRail | One row per NAV section. In the expanded rail the current section opens in place with `sub` items; the open row drops its fill and gets a rotating chevron; a closed section shows the sum of its pages' counts | all | Adaptasi: 12 NAV sections as rows, Ringkasan stays a single page | Belum. sidebarHTML L1092 lists every page flat under section kickers. The badge key (4th tuple field, L1072-1079) is ignored and the `.nl .cnt` CSS at L122 is unused |
| L-11 | SRC SideRail | Active row uses the accent fill with glow | all | Terapkan (blue) | Sudah. `.nl.on` L119 |
| L-12 | LY Workspace | `RailWorkspace` shows kicker + name. Dropdown opens top (expanded) or right (collapsed) with a label, rows (icon, name, muted sub, accent check), a separator, and Sign out. Sign out lives here only | all | Adaptasi: the workspace is the client (multi-tenant) | Sebagian. ws-btn L1098 and wsPop L1121 have the check mark, but Sign out sits in userPop, there is a mobile link instead, and the rail also carries a theme switch, Pengaturan and a promo card (L1093, L1096-1097) |
| L-13 | LY Footer | `RailCollapse` renders only where the rail can expand | all | Terapkan | Sudah. L1099 |
| L-14 | LY nav.ts | One NAV source feeds the rail, SectionTabs, phone bar, More sheet and header title; counts come from one hook | all | Terapkan | Sebagian. NAV L1070 drives the rail, More (via sidebarHTML) and the crumb (locate L1085). bottomBar hardcodes its paths (L1115-1117). counts() L1084 is the hook |
| L-15 | LY SectionTabs | Below xl, a leaf list page shows its section's pages as pill tabs (`rounded-full bg-card p-1 shadow-card`, active `bg-ink`, count pill). Never on detail pages | every NAV section with ≥2 pages (Operasional, Penjualan, Kinerja, Leads, Program, Usulan SFA, Client mgmt, Toko, Master data, Master data user, Pengaturan aplikasi) | Terapkan | Belum |
| L-16 | LY Header | `h-14`: phone menu button · title block (leaf label over "site · weekday date") · pill search `md:max-w-md` · Create `sm:inline-flex` · bell · user menu | all | Adaptasi: keep the crumb + h1 with accent period; the date line goes under the title | Sebagian. hdr L1190-1199 has menu-btn, crumb+h1, hsearch, datechip, search icon, bell and user; no Create; the date is a separate chip hidden below 1280 |
| L-17 | LY Header | Title block `md:max-w-[14rem] lg:max-w-[18rem]`, truncating | all | Terapkan | Sebagian. h1.pt has ellipsis but no max width |
| L-18 | LY Header | Phone search icon opens a full-width GlobalSearch row under the header; tablet and up shows the pill | all | Terapkan | Belum. searchPop L1109 opens a 320px anchored pop, and the pill is hidden below 1024 instead of below 768 (L461) |
| L-19 | LY UserMenu | Pill trigger (avatar sm; name/role from xl); ActionMenu titled "Name · Role" with Switch demo user and Sign out | all | Adaptasi: one demo user, so Switch user does not apply | Sudah. user-btn L1198 shows utx at ≥1280; userPop L1102 has Pengaturan, tema, mobile and Keluar |
| L-20 | LY GlobalSearch | Combobox pill. Live results panel below once text is typed, grouped by entity with kickers, 5 hits per group newest first, every word must match, arrow/Enter/Esc, hits `preventDefault` on mousedown, empty state `Tidak ada hasil untuk "q"`. Choosing a hit navigates, clears and closes the phone row | all | Adaptasi: groups Sales, Toko, Leads (plus Produk), using existing data and existing target routes | Belum. doSearch L1122 runs on submit only, jumps to the first match (sales → leads → toko), and toasts on a miss |
| L-21 | LY CreateMenu | Items gated, each with icon, label and one-line description; opens a shared dialog or navigates to `list?new=1`; renders nothing when empty | /pemesanan, /penugasan?tab=jadwal, /promo, /pengumuman, /toko, /user-sales … | Adaptasi (same scope as L-09) | Belum |
| L-22 | LY Notifications | Items derived from current state, never stored; an item clears when its condition ends; `{id,tone,icon,title,body,at,to,mine}` sorted newest first | all | Terapkan | Sebagian. bellPop L1126 derives approvals, leads and usulan, but the rekon row "1 selisih" is hardcoded (L1131) and the badge adds a hardcoded `+1` (L1197) |
| L-23 | LY Bell | `card icon-lg` with numbered badge and aria "Notifikasi, N belum dibaca"; unread = id missing from a read list persisted per user and workspace | all | Terapkan | Sebagian. Badge and aria are present (L1197); no read state |
| L-24 | LY Panel | Popover `w-[400px] p-0` aligned end from md; light bottom sheet on phone | all | Terapkan | Belum. `.pop` is 280px and anchored at every width (L421) |
| L-25 | LY Panel header | "Perlu perhatian" over "N belum dibaca" or "Semua sudah dibaca", plus ghost sm "Tandai semua dibaca" (disabled at 0) | all | Terapkan | Belum. Only a kicker (L1127) |
| L-26 | LY Scope toggle | "For me" / "Site alerts" sm buttons; admin starts on site | all | Tidak berlaku: single Admin SIM demo, no per-user assignment on the dashboard | n/a |
| L-27 | LY List rows | Row with IconTile in item tone, title + unread dot, body, time; opening marks read, closes and navigates; read rows at `opacity-70` | all | Terapkan | Sebagian. `.mi` rows with icon and text navigate via go() (L1128-1131); no tone tile, body, time or read state |
| L-28 | LY Empty | `EmptyState compact` BellOff, "Tidak ada yang perlu ditindak", with a description of what would appear | all | Terapkan | Belum. Rows render even when a count is 0 |
| L-29 | LY BackButton / BackButton.tsx | Ghost sm "Kembali": goes back when `history.state.idx>0`, otherwise navigates to the fallback list | detail views | Adaptasi: there are no detail routes; details are side panels (openPanel L1057) closed by X, overlay or Escape, with focus returned (L1063). Deep links like `/grading?u=` clear themselves with replaceState (L1677) | Sudah (adapted) |
| L-30 | LY list memory / history-state.ts | Search, filters, sort and page kept per history entry so Back restores the list | all list routes | Terapkan | Sebagian. Filters and search live in the hash via setQ/replaceState (L584) and survive Back. Table page/per/selection live in `ST.tb`/`ST.sel` per table key (L1039-1040), not per entry, so they persist even on a fresh nav visit |
| L-31 | LY | Filters that other pages link to live in the URL and update with replace | /leads?f=, /toko?tab=, /approval?type=, /insentif?tab=, /absensi?q= | Terapkan | Sudah. setQ L584; attention tiles L1250-1254 |
| L-32 | LY PhoneNav | Bottom bar with six slots: Home · shortcut 1 · round centre action · shortcut 2 · shortcut 3 · More | all (<768) | Adaptasi: no create menu yet, so the centre slot is the main queue (Approval) | Sudah. bottomBar L1115-1118: Overview, Penjualan, Approval (round), Absensi, Leads, More |
| L-33 | LY PhoneNav | Slot active when path equals `to` or starts with `${to}/` | all | Terapkan | Sebagian. Uses `startsWith(p)` with no `/` boundary (L1113); harmless with the current paths |
| L-34 | LY PhoneNav | More slot stays `active` while the sheet is open | all | Terapkan | Belum. Button at L1118 has no active state |
| L-35 | SRC PhoneNav | Count badges on bar slots | all | Terapkan | Sudah. `.cnt` L1113/L1116 |
| L-36 | LY More | Dark bottom sheet, never a side drawer; hidden close button; sr-only title "Semua menu" | all | Terapkan | Sudah. openDrawer L1125 renders `.drawer` as an ink bottom sheet with handle (L418-420) |
| L-37 | LY More 1 | Sticky search (`sticky top-0 bg-ink`, `h-11 rounded-2xl bg-white/10`) "Cari halaman"; a query opens every section and filters by page or section label | all | Terapkan | Belum |
| L-38 | LY More 2 | Recent: kicker + pills of the last 4 pages opened from the sheet, persisted; shown only without a query | all | Terapkan | Belum |
| L-39 | LY More 3 | Section order: current first, then role-preferred, then NAV order. Collapsible kicker headers with rotating chevron and `aria-expanded`; the current section opens on open | all | Adaptasi: drop role preference (one role) | Belum. Flat `.sb-sec` kickers, everything open, NAV order (reuses sidebarHTML) |
| L-40 | LY More 4 | Open section is a `grid-cols-3` of icon cells (tile size-11 `bg-white/10`; current page `bg-accent shadow-glow`; white count badge; xs label) | all | Terapkan | Belum. Uses rail `.nl` rows instead |
| L-41 | LY More 5 | Footer `border-t border-white/10`: avatar md, name over role, `onInk` sm "Keluar" | all | Terapkan | Belum. Footer is the rail foot (Pengaturan, Tema gelap, Workspace) |
| L-42 | LY More behaviour | Close on backdrop tap, Escape and navigation; record Recent; reset query and open section on each open | all | Terapkan | Sebagian. Overlay and Escape close it (L1064) and links call closeLayer (L1088); no Recent or query |
| L-43 | LY | Every other phone menu (row overflow, create, filter, notifications, pickers) is a light bottom sheet with a `bg-border` handle | all | Terapkan | Sebagian. `.panel` becomes a bottom sheet below 768 (767 media block). `.pop` menus (user, bell, search, workspace) and `dd-pop` stay anchored; `.modal` stays centred |
| L-44 | LY Responsive tables | Phone shows two columns (identifier with phone badge line + one number); `hideBelow` md/lg columns appear on tablet; desktop shows all | every dataTable route | Adaptasi: the existing card-row transform is acceptable, but trim each phone row to identifier + status + one number | Sebagian. `.tbl.resp` L475-484 turns rows into label:value stacks and hides `hide-m` columns, but still prints every other field |
| L-45 | LY | List row status shows as a compact line under the meta on phone | list routes | Terapkan | Sebagian. Status spill appears as a labelled line in the card row |
| L-46 | LY | Captions ("diperbarui 09:41", context lines) are `hidden md:block` | /, live boards | Terapkan | Sebagian. Only the datechip hides (<1280) |
| L-47 | LY | Toasts centred above the bar on phone (`bottom-24`), bottom right elsewhere | all | Terapkan | Sudah. `.toasts` L424 and phone rule `bottom:96px` |
| L-48 | LY | Attention tiles: 2 cols phone, 3 tablet, auto beside the title from xl | / | Terapkan | Sudah. `.attn-g` L373 plus the 1279/767 rules |
| L-49 | LY | Stat rows: `grid-cols-2 gap-3` phone, `sm:gap-4`, `xl:grid-cols-4` | list routes | Terapkan | Sudah. `.stats` L203 (4 cols, 2 below 1024); gap stays 16 on phone (minor) |
| L-50 | LY Hard rule | Every breakpoint-only grid has a `grid-cols-1` base and `minmax(0,…)` fr tracks | all | Terapkan | Sudah. Grid classes declare explicit minmax templates |
| L-51 | LY Hard rule | Flex rows with a right-side button are `flex-wrap justify-between gap-2` | all | Terapkan | Sudah. `.ch` L197, `.attn` L372, crudList toolbar L2196 |
| L-52 | LY Hard rule | Banners are `flex flex-wrap gap-3`, text `min-w-[12rem] flex-1` | callouts, bulk bar | Terapkan | Sebagian. `.bulk` wraps (L315); `.callout` L288 does not wrap (icon + text, works) |
| L-53 | LY Hard rule | No negative-margin bleed inside `main` | all tables | Terapkan | Sebagian. `.tbl-wrap{margin:0 -4px}` L302; it sits inside card padding so it is safe today, but should be 0 |
| L-54 | LY Hard rule | Verify at 375 and 768: `main.scrollWidth===clientWidth`, no element's right edge past innerWidth | all | Terapkan | Not verified in this read-only audit |

| ID | Source | Requirement (concrete) | Target routes affected | Applicability | Status |
|---|---|---|---|---|---|
| P-01 | PT Dashboard 1 | View row: PillTabs view switch persisted per viewer, plus a caption `hidden md:block` ("Semua klien · diperbarui 09:41") | / | Adaptasi: no second view exists, so add the caption only; the period switch already sits on the Sell-out card | Belum |
| P-02 | PT Dashboard 2 | Attention card: title + helper; tiles only for counts >0, sorted; link to the pre-filtered list; urgent tiles in accent-soft; "Tidak ada yang perlu ditindak." when empty | / | Terapkan | Sudah. pageOverview L1249-1258. `.at.d` danger tone on Overdue is fine (overdue is a danger status) |
| P-03 | PT Dashboard 3 left | Ink hero: kicker, title, meta, status top right, giant number + caption, step chips, latest-event panel, footer with `onInk` CTA; EmptyState when there is nothing | / | Adaptasi: the featured "record" is today's sell-out; step chips become breakdown links | Sudah. L1259-1268 (kick, h3, delta, hero-big, target bar, ink-panel, ink-chips, b-onpop) |
| P-04 | PT Dashboard 3 right | Accent card for the one urgent metric with `onInk` "Buka" above two StatCards (ink primary + info) | / | Terapkan | Sebagian. acc card (Approval) L1270 and g-2s L1272-1275; both stats are plain white, with no ink or info tone |
| P-05 | PT Dashboard 4 | Status overview: one StatCard per status in its tone, plus a Delayed card (danger above 0) | / | Adaptasi (optional): lead or order status counts. Avoid duplicating the /absensi stats | Belum |
| P-06 | PT Dashboard 5 | Lists + analysis grid `1fr 1fr 20rem`: two list cards (CountBadge, outline sm "Lihat semua", 5 nested rows, compact empty) and an analysis card with Donut and a ghost full-width link | / | Adaptasi: Top sales + Kehadiran rows as the lists, Leads funnel/donut as the analysis | Sebagian. `.g-ov` L1278-1292 is a 3-col grid with Sell-out span2; headers use soft "Detail"/"Buka leads", no CountBadge |
| P-07 | PT Dashboard 6 | Second view (Shift view) | / | Tidak berlaku: no equivalent view | n/a |
| P-08 | PT RoleHome | Role-aware home for roles without the plant dashboard | / | Tidak berlaku: single Admin SIM demo | n/a |
| P-09 | PT List 1 | PageHeader: title, one-sentence purpose, actions = pill search (`sm:w-72`) + primary "Buat X" | every list route (/leads, /toko, /produk, /absensi, /kunjungan, /pemesanan, /data-penjualan, /promo, /posm, /canvassing, /penagihan, /klaim-biaya, /pengumuman, /overdue, master data …) | Adaptasi: the title stays in the header h1; add a one-line description and a page-actions row (search + primary) above the stats | Sebagian. No description anywhere; search sits inside the card (crudList L2196, pageLeads L1708); primary sits in card acts |
| P-10 | PT List 1 | Live boards add `Diperbarui 09:41 WIB` | /absensi, /live-tracking, /sinkron | Adaptasi | Belum |
| P-11 | PT List 1 | View switch with counts as PillTabs | /penugasan, /aktivitas, /toko, /task-pending | Terapkan | Sebagian. /penugasan uses fchips with no counts (L2417); /aktivitas uses pills (L3287); utabs with counts on /toko, /iam, /task-pending, /grading, /produk |
| P-12 | PT List 2 | Scoping banner when a deep link narrows the list (info, or warning when the record is missing) with outline sm "Tampilkan semua" | /absensi?q=, /leads?q=/f=, /approval?type=, /toko?q=, /grading?u= | Terapkan | Belum |
| P-13 | PT List 3 | Stat tiles `grid-cols-2 xl:4/5`; click applies the filter; tone follows the count; coverage as a unit; odd last tile `col-span-2 xl:col-span-1` | /absensi, /leads, /pemesanan, /kunjungan, /klaim-biaya, /penagihan, /sinkron, /sell-in | Terapkan | Sebagian. Clickable toggles on Absensi L1464, Leads L1700, Pemesanan L1452, Kunjungan L1496, Klaim L2493. Tone never follows the count; /sinkron tiles are non-clickable divs (L2559) |
| P-14 | PT List 4 | Filter bar: ChipRow with counts, "Semua" first, a second click on the active chip clears it; data-backed filters as inline clearable pickers | list routes | Terapkan | Sebagian. fchips with counts on approval L1547, promo L1614, crudList L2197; a second click re-sets instead of clearing (stat tiles do clear); pickers ldFilters L1730 and simcoFilter |
| P-15 | PT List 5 | DataTable on the canvas without a card; rowClick to detail; `resetPageKey`; table history; compact empty | list routes | Terapkan (detail = panel) | Sebagian. dataTable L1038 is wrapped in card() everywhere; rowClick opens panels; page is clamped but not reset on filter change; empty is one sentence (allowed) |
| P-16 | PT List state | Search/filters in history state, linked filters in URL with replace, viewer view choice persisted in storage | list routes | Terapkan | Sebagian. Everything is in the URL (setQ L584); no persisted view choice |
| P-17 | PT List state | Cards/Table toggle persisted, card grid `1/md2/xl3` | – | Tidak berlaku: no card view exists | n/a |
| P-18 | PT Detail 1 | Action bar: Back · next lifecycle step as primary · other steps outline · ActionMenu for edit/copy/cancel/delete; only allowed actions render | panels: ordPanel, leadPanel, aprPanel, promoPanel, arPanel, klaimPanel, usulanPanel, taskPanel, openForm | Adaptasi: the panel footer is the action bar; X is Back | Sebagian. ordPanel L1373 has "Tandai …" primary and state-gated actions; "Ajukan batal" uses the red danger variant even though it is a request, not a destructive delete; openForm footer L2224 OK |
| P-19 | PT Detail 2 | Blocker card or Banner with a bold lead-in ("Belum bisa …:") | panels | Terapkan | Sebagian. Callouts in ordPanel L1372; no bold lead-in |
| P-20 | PT Detail 3 | Ink hero: eyebrow mono code, h1, context line, badges, metric grid, definition line, progress, status notes | record panels (pesanan, leads, insentif, grading, penagihan) | Adaptasi: a compact ink header block at the top of the panel body | Belum. Panels use `.kv` plus tiles |
| P-21 | PT Detail 4 | Lifecycle Steps from the status flow | ordPanel, leadPanel, promo | Terapkan | Sudah. `.steps` in ordPanel L1371 and leadPanel L1773 |
| P-22 | PT Detail 5 | KeyValue cards; empty values read "Tidak ada" / "Belum" / "Belum ditugaskan"; ranges `start → end` | panels | Terapkan | Sebagian. `.kv` used; empty values print "–" (leadPanel L1771) |
| P-23 | PT Detail 6 | Related nested rows, requirement meters, SplitStats | panels | Terapkan | Sebagian. Item rows in ordPanel L1370 |
| P-24 | PT Detail 7 | History card, newest first | panels | Terapkan | Sebagian. Global audit() L2049 and fmLogPanel exist; no per-record history in panels |
| P-25 | PT Detail 8 | Dialogs last: entity dialog and every ConfirmDialog | all | Terapkan | Sudah. delAsk L2255, rejectModal L1556, openModal L1062 |
| P-26 | PT Detail not found | Card EmptyState (title, description) with Back action | unknown route, missing ids | Terapkan | Sebagian. pageNotFound L1226 is text + "Kembali ke overview", no EmptyState title or icon |
| P-27 | PT Master data | PageHeader → domain PillTabs → panel UnderlineTabs (`?tab=` replace) with one entity table per tab | /sektor, /brand, /kompetitor, /kategori-produk, /produk, /status-produk, /satuan, /konversi, /company-sim, /user-admin, /user-sales, /iam, /client, /cabang-client, /region, /kategori-toko, /event, /mitra, /konfigurasi-aktivitas | Adaptasi: domains = NAV sections; domain tabs come from SectionTabs (L-15); in-page utabs stay | Sebagian. utabs + `?tab=` on /iam L2325, /brand, /kategori-produk, /produk, /mitra; no domain tabs |
| P-28 | PT EntityTable | Card header with title, description, pill search `sm:w-56` + Add; row click edits; `_actions` column with ghost icon-sm Edit/Remove; Remove disabled with `title=reason` when referenced; confirm "Hapus {name}?" | master-data routes | Adaptasi: soft delete stays (AGENTS rule) | Sebagian. crudList L2193 puts search + chips in the body; rowActs L566 are text buttons, soft Edit + red danger "Hapus" on every row; "in use" shows a modal after the click (L2257) instead of a disabled button; soft delete L2261 |
| P-29 | PT EntityDialog | Field union (required, hint, span, mono, readOnly), automatic "Kode wajib diisi", case-insensitive code uniqueness, custom validate, errors after first submit | openForm/saveForm | Terapkan | Sebagian. Panel form via fieldHTML L2208; check saveForm L2228 for uniqueness and post-submit errors |
| P-30 | PT Master data | Without manage permission: no Add, no actions column, no row edit | master data | Tidak berlaku: single admin role | n/a |
| P-31 | PT Revisioned 1 | Lifecycle draft→review→released→obsolete; released is read-only | – | Tidak berlaku: no revisioned documents. /promo's Draft→Menunggu approval→Aktif is an approval flow (P-36) | n/a |
| P-32 | PT Revisioned 2 | One shell renders hero, Steps, body and "Dipakai oleh" | – | Tidak berlaku | n/a |
| P-33 | PT Revisioned 3 | Lists mark released docs with a Lock badge | – | Tidak berlaku | n/a |
| P-34 | PT Revisioned 4 | Revision timeline, ECO From/To compare | – | Tidak berlaku | n/a |
| P-35 | PT Revisioned 5 | `?new=1&product=` prefilled change request | – | Tidak berlaku | n/a |
| P-36 | PT Approval queues | Stacked cards, one per queue kind, hidden at 0, CountBadge in the title | /approval (kinds: Absen, Cuti, Hapus penjualan, Batal pesanan, Promo baru), /toko?tab=usulan, /klaim-biaya, /task-pending, /insentif?tab=rekon | Adaptasi: keep the existing bulk select, either as a card per kind or as the current table with type chips | Sebagian. pageApproval L1544 is one table with type chips and bulk bar |
| P-37 | PT Approval queues | Each row has a verb button ("Setujui") | same | Terapkan | Sudah. Tolak/Setujui per row L1554; approveToko L2313; klaimSet L2505 |
| P-38 | PT Approval queues | One `pending` state mounts ConfirmDialog; approve and reject share it; reject requires a note | same | Terapkan | Sebagian. rejectModal requires a reason (L1556-1559); approve() L1555 runs without confirmation |
| P-39 | PT Boards | Dispatch columns scroll sideways (`w-72` column cards, count pill, empty slot) | – | Tidak berlaku: no column board | n/a |
| P-40 | PT Boards | `role=link` cards with AvatarStack + menu; `?wo=` ring and scrollIntoView | – | Tidak berlaku | n/a |
| P-41 | PT Boards | One actions hook shared by board, table and detail | /kunjungan, /penugasan?tab=jadwal | Adaptasi: shared functions | Sudah. visForm/visPanel/visDelAsk reused from both routes (L2431) |
| P-42 | PT Boards | Scoping banner on board deep link | – | Tidak berlaku (see P-12) | n/a |
| P-43 | PT Boards | Phone "Needs assignment" stacked rows | – | Tidak berlaku | n/a |
| P-44 | PT Production control | Machine chips with StatusDot, machine cards with alarm + Sparkline, group-by PillTabs | – | Tidak berlaku: no machine monitoring. /live-tracking follows P-50 instead | n/a |
| P-45 | PT Timelines 1 | Fixed scale in `overflow-x-auto`, sticky label column, no sticky header | /penugasan | Tidak berlaku: /penugasan Jadwal is a table (L2429-2433); there is no Gantt | n/a |
| P-46 | PT Timelines 2 | Group band, day and hour lines, tick labels | – | Tidak berlaku | n/a |
| P-47 | PT Timelines 3 | Bars with solid status fills, selected ring | – | Tidak berlaku | n/a |
| P-48 | PT Timelines 4-5 | Now line, chart card with badge strip and caption | – | Tidak berlaku | n/a |
| P-49 | PT Record-picker 1 | Picker in page actions (`sm:w-80`); selection in URL (`?id=`, replace); EmptyState before a pick | /live-tracking (`?u=`), /mapping-outlet (`?cl=`) | Terapkan | Sebagian. Selection in URL (L2510, L2276); mapping defaults to the first client, so there is no empty-before-pick state |
| P-50 | PT Record-picker 2 | Side-list `xl:grid-cols-[20rem_minmax(0,1fr)]`; list card `max-h-[32rem]`, capped at 60 rows; selected row filled | /live-tracking, /mapping-outlet | Terapkan | Sebagian. /live-tracking uses `.g-side` (map left, 340px list right) L2511 |
| P-51 | PT Record-picker 3 | Search results as mono chips | /mapping-outlet | Adaptasi | Belum |
| P-52 | PT Record-picker 4 | Trace tree / flow chain | – | Tidak berlaku | n/a |
| P-53 | PT Record-picker 5 | Phone: list first; after a pick, scroll the detail into view | /live-tracking | Terapkan | Belum |
| P-54 | PT Analytics layout | Chart cards per `charts.md` | /penjualan, /sell-in, /target, /insentif, /grading, /sku-display, /market-share, /kepatuhan-kunjungan, /aktivitas (Hasil kerja), / | Terapkan | Sebagian. columnChart/lineChart/donut with period pills (pagePenjualan L1307); charts.md is outside this audit's scope |
| P-55 | PT Insight row | `rounded-2xl bg-surface-2 p-3`: factor badge, title, score, reason, meter/sparkline, ≤4 record links then `+N` | / (Ringkasan otomatis), /penjualan drill rows, /grading, /kepatuhan-kunjungan | Adaptasi | Sebagian. rowlinks with bar on Top sales L1291; reasons only in the ovSummary prose |
| P-56 | PT Recommendation card | Kind + impact badges, rationale, evidence pills, Apply/Dismiss, confirm with reason, decision log | – | Tidak berlaku: no recommendation feature (do not invent) | n/a |
| P-57 | PT What-if card | Editable field next to a computed value, Reset/Apply disabled until changed | – | Tidak berlaku: tgtModal is plain entry | n/a |
| P-58 | PT Analytics empty | Empty states name the good outcome and say when rows will appear | analytics routes | Terapkan | Sebagian. e.g. "Semua sales memenuhi syarat insentif" L1238; dataTable empties are generic |
| P-59 | PT Operator station (identify, pick work, station, instruction, quantity, touch) | Kiosk flow inside admin | – | Tidak berlaku: field work lives in the separate mobile mockup | n/a |
| P-60 | PT Measurement entry (blocks, result badge, defects, disposition, footer, permission) | Inspection entry | – | Tidak berlaku. /form-penjualan and /quiz//survey are form builders, not measurement | n/a |
| P-61 | PT Integration 1 | Connection cards with health-tone IconTile, mono endpoint, last sync, Switch + "Sinkron sekarang" | /sinkron | Adaptasi: one card for the SIM (BPR1) employee sync, from existing data only | Belum. "Sinkron dari SIM" exists only as a button on /user-sales (L2318) |
| P-62 | PT Integration 2 | Neutral banner naming which system owns which data | /sinkron, /user-sales | Terapkan | Sebagian. Callouts mention SIM (BPR1) |
| P-63 | PT Integration 3 | Log DataTable in a card (direction tile, time, system badge, mono path, status, duration) | /sinkron | Adaptasi: device table | Sebagian. "Perangkat sales" table L2560 |
| P-64 | PT Integration 4 | Device monitoring: list next to the selected machine, Reading tiles, LineChart | – | Tidak berlaku | n/a |
| P-65 | PT Sign-in 1 | `min-h-dvh bg-surface` with ambient glow; `max-w-5xl` grid `lg:[1fr_1.1fr]` of brand + form | /login | Terapkan | Sebagian. `.login` 2-col full bleed (L428); no glow; brand panel hidden on phone instead of stacking |
| P-66 | PT Sign-in 2 | Brand card `variant=ink p-8`: logo tile + name, kicker, 3xl promise with accent period, pillar rows `bg-white/5`, demo-data note | /login | Adaptasi: the promise period uses SIM blue | Sebagian. login-art uses `--pop-blue` and a tan period (L1213-1216); no pillar rows |
| P-67 | PT Sign-in 3 | Form card: "Masuk." with accent period, helper, inputs with left icons at h-12, `size=lg w-full` submit with trailing arrow | /login | Terapkan | Sebagian. L1217-1223 has the right copy and inputs; password uses the settings icon; submit has no arrow and is default height |
| P-68 | PT Sign-in 4 | Demo accounts grid; redirect to `state.from` | /login | Adaptasi: one demo account row (Dewi Kartika · Admin SIM); return to the previous hash | Belum |
| P-69 | PT Phone PWA (Hub, Record card, List screen, Record screen, Stepper, Operator sign-in, Profile) | Mobile operator app | – | Tidak berlaku: the dashboard is admin; the mobile mockup has its own style rules | n/a |

| ID | Source | Requirement (concrete) | Target routes affected | Applicability | Status |
|---|---|---|---|---|---|
| F-01 | PT Finish | One accent element per region; second emphasis is ink; solid red behind small white text uses accent-strong | all | Adaptasi: accent = SIM blue; red only for danger | Sebagian. The hero region is right (ink + one acc card). Red appears outside danger: lead status "Belum Tertarik" spill (L1717), "Ajukan batal" (L1373), and a red "Hapus" on every master-data row (L566) |
| F-02 | PT Finish | Borderless `rounded-card` cards on `surface`; nested rows `surface-2` hovering to `surface`; a list's main table on the canvas, section tables in cards | all | Terapkan | Sebagian. `.card` L196 and `.rowlink` L320 are correct; list tables sit in cards; `.fchip` has a 1px border (L221) |
| F-03 | PT Finish | Pill controls; lucide icons at 16/20; soft-tint icon tiles except the one ink/accent tile | all | Terapkan | Sudah. `.btn`, `.pills`, `.fchip`, `I(ic,16/20)` |
| F-04 | PT Finish | Big tabular numbers with the unit muted; mono identifiers | all | Terapkan | Sudah. `.stat b` L209, hero-big, mono PO and lead ids |
| F-05 | PT Finish | Every data-backed dropdown is a searchable combobox; plain select only for fixed enums ≤6 | all forms/filters | Terapkan | Sudah. The global dd enhancer (end of script) makes every `<select>` searchable, including small enums |
| F-06 | PT Finish | Errors after first submit; required marks, hints, defaults visible; submit names the verb | forms | Terapkan | Sebagian. rejectModal is correct (L1559); saveForm not audited |
| F-07 | PT Finish | Every mutation ends in a toast; every blocked action says why; actions the role cannot take are hidden | all | Terapkan | Sudah. approve/doDel/toggleStatus toasts; lock reason in toggleStatus L2254; delAsk block modal |
| F-08 | PT Finish | Empty states have title, description and next action; every button does something real | all | Terapkan | Sebagian. dataTable empty is one sentence; crudList offers "Hapus filter"; no bell empty state; the /parameter edit icon only toasts "disimpan" without editing (L2892) |
| F-09 | PT Finish | Lists keep search, filters and table position across Back; linked filters in URL | lists | Terapkan | Sebagian (see L-04, L-30) |
| F-10 | PT Finish | Hover = surface (white/10 on dark); press = scale .98; focus = accent ring; motion limited to fades, slides, live pulse | all | Terapkan | Sebagian. Hover correct; press is .97 on `.btn` and .98 on `.at`; focus-visible ring not checked; fxAnimate L1145 adds count-up, bar growth and stagger beyond "fades/slides" |
| F-11 | PT Finish | Phone-app rules (accent period titles, tab bar hidden on detail, lazy lists, sticky bar) | – | Tidak berlaku (dashboard) | n/a |
| F-12 | PT Finish | Phones: every menu, picker and filter opens as a bottom sheet with grab handle and safe-area padding | all <768 | Terapkan | Sebagian. Panels and More are correct; `.pop` menus and `dd-pop` are not |
| F-13 | PT Finish | `grid-cols-1` base + minmax; header and card rows wrap; checked at 375/768 | all | Terapkan | Sebagian. Code is compliant; not measured in this audit |
| F-14 | PT Finish | Charts: ariaLabel, format, one highlighted value, plans as reference lines, "Belum ada data" state | /, /penjualan, /leads, analytics routes | Terapkan | Sebagian. columnChart has label + targets + selected bar (L996); lineChart has label; no-data state unverified |

## 2. Route map (every key in `ROUTES`, L3290, plus /login handled in render L1182)

| Route | Page fn | Governing MES recipe |
|---|---|---|
| `/login` | pageLogin | Sign-in (admin) |
| `/` | pageOverview | Dashboard (attention, ink hero, accent card, lists + analysis); RoleHome n/a |
| `/penjualan` | pagePenjualan | Analytics (charts.md + insight rows, drill) |
| `/absensi` | pageAbsensi | List page (live-board caption) + panel detail (attPanel) |
| `/kunjungan` | pageKunjungan | List page + panel detail (visPanel) |
| `/approval` | pageApproval | Approval queues |
| `/target` | pageTarget | Analytics (target vs actual tabs) + EntityDialog-style entry (tgtModal) |
| `/promo` | pagePromo | List page + Detail panel (Steps Draft → Menunggu approval → Aktif) |
| `/insentif` | pageInsentif | Analytics; the `?tab=rekon` part is an Approval queue |
| `/grading` | pageGrading | Analytics (insight rows); Formula tab = Master data EntityTable |
| `/leads` | pageLeads | List page (bulk assign) + Detail panel (leadPanel, Steps) |
| `/overdue` | pageOverdue | List page |
| `/company-sim` | pageSimco | Master data console |
| `/sektor` | pageSektor | Master data console |
| `/client` | pageClient | Master data console |
| `/kategori-toko` | pageKategori | Master data console |
| `/toko` | pageToko | List page (Daftar toko); Usulan tab = Approval queue |
| `/iam` | pageIAM | Master data console (UnderlineTabs per entity) |
| `/produk` | pageProduk | Master data console (tabs Master, Mapping, Produk dari Sales; the last is an approval-style queue) |
| `/penugasan` | pagePenugasan | List page (view switch Per sales/Per toko, Jadwal tab); Timelines n/a (no Gantt) |
| `/pemesanan` | pagePemesanan | List page + Detail panel (ordPanel lifecycle) |
| `/sell-in` | pageSellin | Analytics |
| `/user-sales` | pagePengguna | Master data console (+ Integration banner for SIM sync) |
| `/user-admin` | pageAdmin | Master data console |
| `/parameter` | pageParameter | Master data console (settings rows; no exact recipe) |
| `/task-pending` | pageTaskPending | Approval queues (assign/close) + panel detail |
| `/aktivitas` | pageAktivitas | Analytics (Hasil kerja per sales) + List page (Log aktivitas) |
| `/canvassing` | pageCanvassing | List page |
| `/posm` | pagePosm | List page |
| `/region` | pageRegion | Master data console |
| `/brand` | pageBrand | Master data console |
| `/kategori-produk` | pageKatProd | Master data console |
| `/kompetitor` | pageKompetitor | Master data console |
| `/cabang-client` | pageCabang | Master data console |
| `/mapping-outlet` | pageMapOutlet | Record-picker (client picker + outlet side list) |
| `/data-penjualan` | pageDataJual | List page (ledger) + panel detail (trxPanel) |
| `/penagihan` | pagePenagihan | List page + panel detail (arPanel) |
| `/klaim-biaya` | pageKlaim | Approval queues (stats tiles as filter) + panel detail |
| `/live-tracking` | pageTracking | Record-picker side list (live-board caption) |
| `/kepatuhan-kunjungan` | pageKepatuhan | Analytics |
| `/pengumuman` | pagePengumuman | List page |
| `/sinkron` | pageSinkron | Integration console (adapted: device sync log + stats) |
| `/satuan` | pageSatuan | Master data console |
| `/konversi` | pageKonversi | Master data console |
| `/konfigurasi-aktivitas` | pageAktCfg | Master data console |
| `/form-penjualan` | pageFormJual | Master data console (form builder; no exact recipe) |
| `/status-produk` | pageStatProd | Master data console |
| `/event` | pageEvent | Master data console |
| `/mitra` | pageMitra | Master data console (tabs) + Approval-style queue tab |
| `/sku-display` | pageSkuDisplay | Analytics |
| `/market-share` | pageMarketShare | Analytics |
| `/quiz` | pageKonten(q,'Quiz') | List page (+ builder in EntityDialog style) |
| `/survey` | pageKonten(q,'Survey') | List page (+ builder in EntityDialog style) |
| `/pengaturan` | pagePengaturan | No recipe (settings card grid); only finish checklist and grid rules apply |