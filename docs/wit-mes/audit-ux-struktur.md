## Checklist: wit-mes-frontend (UX + structure + mobile) against `mockup/dashboard/index.html`

Line numbers point to `/Users/mikhaelsuryawan/WIT/Project/Sales App/mockup/dashboard/index.html`. "n/a" = the rule does not apply, so there is nothing to verify.

### UX (references/ux.md)

| ID | Source | Requirement | Applicability | Status in TARGET |
|---|---|---|---|---|
| U-01 | ux Hierarchy 1 | Each region has one accent element. The second emphasis is ink (dark card, secondary button, active pill). | Terapkan (accent = SIM blue) | Sebagian. Overview has one accent card (1270) and ink pills (215). The phone bar shows two accent items at once: `.bb-act` Approval is always accent (462, 1116) and so is the active `.bb-i.on` (461). |
| U-02 | ux Hierarchy 2a | Attention card lists only non-empty queues, sorted by count, each linking to its filtered list. | Terapkan | Sudah. `pageOverview` attn: filter `>0` and sort (1249-1258), links such as `#/leads?f=unassigned`. |
| U-03 | ux Hierarchy 2b | Role homes list the next six items for the role. | Tidak berlaku. There is one demo user, Admin SIM (1198). Applies only if a role switcher is added. | n/a |
| U-04 | ux Hierarchy 3 | A record's identity sits in an ink hero at the top of its detail view (code, name, status, progress numbers). | Adaptasi: details are side panels, so add a compact ink header block at the top of `openPanel` | Sebagian. The panel header is a plain title plus a muted `sub` line "label · code · status" (1059, 2226). No ink hero. |
| U-05 | ux Actions 1 | One primary button per region. On a detail view it is the next lifecycle step; other steps are outline; edit/copy/cancel/delete go into an overflow menu. | Adaptasi: panel footer = primary next step, with a "⋯" overflow pop for Hapus/Nonaktifkan | Sebagian. The `openForm` footer has one accent Simpan but a visible danger Hapus (2224). `rowActs` puts Edit and Hapus inline on every row (566). |
| U-06 | ux Actions 2 | Build menus from `cond ? item : null`, so a menu never lists an action that state or role forbids. | Terapkan | Sebagian. Hapus always renders, even when `E.inUse` blocks it; the block appears only after the click (2256-2257). |
| U-07 | ux Actions 3 | Hide what the role cannot do. A viewer without rights sees an EmptyState naming who does the step. | Adaptasi: `PERM` map plus `can()` and a demo role switch (UI guard is convenience only; AGENTS says the backend decides) | Belum. No `can(` anywhere. The login text claims role-based access (1223) but nothing enforces it. |
| U-08 | ux Actions 4 | Disable only for state blockers and say why, using the same sentence the action uses to refuse. | Adaptasi: `xxxBlocker(r)` returning an Indonesian sentence, used both in the button `title`/callout and inside the action | Sebagian. `E.inUse`/`E.lockStatus` sentences (2058-2087, 2165, 2173, 2797) are shared by `delAsk` (2257) and `toggleStatus` (2254). Buttons stay enabled and the reason only shows after the click. |
| U-09 | ux Actions 5 | Every button does something real. | Terapkan (also AGENTS) | Sebagian. Nearly all buttons dispatch, navigate or toast. The Pengaturan > Notifikasi switches only flip `aria-checked` with no toast (3012). |
| U-10 | ux Feedback 1a | Every change ends in a past-tense toast whose description names the record. | Terapkan (also AGENTS) | Sebagian. 109 `toast(` calls. `saveForm` names the record (2252), as does `doDel` (2261). `approve` gives a generic description, "Notifikasi terkirim ke pengaju." (1555), and the Notifikasi switches show no toast at all. |
| U-11 | ux Feedback 1b | Tones: success for create/save/advance; default for pause/cancel/remove/delete; danger for reject/fail/block. | Adaptasi: add a `default` (neutral icon) tone to `toast()` | Sebagian. `toast()` only knows check vs alert (573). Delete and cancel use the success check icon (`doDel` 2261, `ordCancel` 1375). |
| U-12 | ux Feedback 1c | A blocked attempt shows a danger toast with the blocker as its description. | Terapkan | Sudah. `toggleStatus` → `toast('Tidak bisa diubah', lk, 'danger')` (2254). |
| U-13 | ux Feedback 2 | A rule the current input is about to trigger shows as a Banner whose tone escalates live (info → warning). | Adaptasi: a callout updated on `oninput` (e.g. claim over limit, canvassing limit) | Sebagian. Over-limit shows in `klaimPanel` (2500) as static text. No live escalation while typing. |
| U-14 | ux Feedback 3 | If storage fails, a danger banner at the top of `main` says changes will not survive a reload; the session keeps working in memory. | Adaptasi: `shSet`/`store.set` return a boolean; on false set `ST.storageErr` and render a danger callout first in `main` (1200) | Belum. `shSet` (594) and `store.set` (553) swallow errors silently. |
| U-15 | ux Forms 1 | One dialog per entity; its form mounts only while open, so every open starts clean. | Terapkan | Sudah. `ENT` config plus `openForm` rebuilds `#layer` on each open (2220); `closeLayer` clears it (1063). |
| U-16 | ux Forms 2 | Errors appear after the first submit, then update live. Long forms keep submit enabled. | Terapkan | Sebagian. `saveForm` shows errors on submit with Simpan enabled (2243-2245), but errors don't update live afterwards; `doReject` is the same (1559). |
| U-17 | ux Forms 3 | Errors are full-sentence instructions; required fields get an accent asterisk; optional fields say so; defaults show as placeholders with their source. | Terapkan (asterisk in SIM blue, which also satisfies AGENTS "merah hanya untuk bahaya") | Sebagian. Errors are fragments: "Wajib diisi", "Isi dengan angka", "Pilih minimal satu" (2232, 2237). The asterisk is red `var(--danger)` (2210). "opsional" hints exist (2697, 2803). |
| U-18 | ux Forms 4 | Units go in the hint or right slot; quantity starts at the remaining amount; inputMode decimal/numeric. | Terapkan | Sebagian. `inputmode` numeric/decimal present (1382, 1419, 2217). No units in a right slot, no remaining-amount defaults. |
| U-19 | ux Forms 5 | Dependent pickers clear children when the parent changes; the disabled child says "Pilih X dulu". | Terapkan | Sebagian. Children re-sync (`mptSync` 2772, `pmKatSync` 2729, `katcSync` 2885). No disabled "Pilih … dulu" state found. |
| U-20 | ux Forms 6 | In edit mode the description names what stays fixed, and that field renders read-only. | Terapkan | Sebagian. `fd.lock`/`roIf` render a read-only tile (2209, 2213). No description naming the locked field. |
| U-21 | ux Forms 7 | Submit names the verb; Cancel is outline. | Terapkan | Sebagian. Generic "Simpan"/"Simpan perubahan" (2224-2225); specific "Tolak" and "Ajukan hapus" (1558, 1627); Batal is `b-card`. |
| U-22 | ux Pickers 1 | Every data-backed choice is a searchable combobox; a native select only for enums of six or fewer. | Terapkan | Sudah. A global IIFE swaps every `<select>` for a searchable, paged listbox (3292-3345). |
| U-23 | ux Pickers 2 | Options that cannot be chosen stay visible with the reason. | Terapkan | Sebagian. `dd` supports `aria-disabled` (3327); no option carries a reason. |
| U-24 | ux Pickers 3 | List filters are chips with counts plus inline pickers; clicking the active chip clears it; stat tiles double as filters. | Terapkan | Sebagian. Chips with counts (2199, 1614, 1712) use a "Semua" chip rather than toggle-off. Stat tiles link to pages (1273) but don't filter lists. |
| U-25 | ux Confirm 1 | Confirm title is a question naming the record; the description states the consequence in one sentence; required inputs sit inside with confirm disabled until filled. | Terapkan | Sebagian. `delAsk` "Hapus X?" (2258), `djDelAsk` (1447) and `promoDelAsk` (1627) render as "?." because `<i>.</i>` follows the question mark. `rejectModal` "Tolak pengajuan." is not a question, and Tolak is enabled before a reason is entered (1556-1559). |
| U-26 | ux Confirm 2 | Destructive confirms use the destructive style; reject requires a note; approve does not. | Terapkan | Sudah. `btn v:'danger'` (2259); `doReject` requires `#rj` (1559); `approve` has no note (1555). |
| U-27 | ux Confirm 3 | Released or posted records never change in place; offer copy, reversal or a change request. | Adaptasi (approval request = change request) | Sudah (sampled). `ordCancel` becomes a "Batal pesanan" approval (1375); `promoDel` → "Menunggu approval" (1628); `dupKonten` copy (2971). |
| U-28 | ux Nav 1 | Rail and phone bar read one NAV definition; below xl, list pages show their section as pill tabs. | Terapkan | Sebagian. Rail and drawer come from `NAV` (1070, 1086); `bottomBar` is hard-coded (1115-1118). |
| U-29 | ux Nav 2 | Back returns to the previous in-app page; a bookmark goes to the list. | Adaptasi: put the open panel in the URL (`?id=`) so Back closes it | Sebagian. Hash routing makes Back work between pages (582-585). Panels are not in history, so Back leaves the page instead of closing the panel. |
| U-30 | ux Nav 3 | Lists keep search, filters, sort and page across Back, and `main` restores its scroll. | Adaptasi: put `page` in the URL or in a `sessionStorage` map keyed by hash | Sebagian. Search and filters live in the URL (`setQ` replaceState, 584). Table page is in-memory `ST.tb` (1039), not tied to a history entry. Scroll is kept only on a same-path re-render (1187, 1201). |
| U-31 | ux Nav 4 | Linked filters and selections live in the URL with replace; `?new=1` opens the create dialog, optionally prefilled. | Terapkan | Sebagian. `setQ` uses replace (584); `?tab=`, `?f=`, `?q=`, `?u=` are linked (1251-1254, 1292). No `?id=` and no `?new=1` (grep: none). |
| U-32 | ux Nav 5 | Per-viewer preferences persist under `<ns>.<app>.<thing>`. | Adaptasi: `salesapp.dashboard.sidebar` / `.theme` | Sebagian. `sim-sidebar` (1123) and `sim-theme` (8, 1124) persist under un-namespaced keys. Read notifications are not persisted. |
| U-33 | ux Empty 1 | An EmptyState has a title, how records arrive, and the next action. | Terapkan | Sebagian. `crudList` empty has a "Hapus filter" action (2200). The `dataTable` default is "Belum ada data." with no how or next step (1038). |
| U-34 | ux Empty 2 | Phone lists reveal 6 at a time with an "N lagi · Muat" sentinel. | Adaptasi (optional; tables page instead) | Belum. Pager on every width (1045-1046). |
| U-35 | ux Empty 3 | Charts show "No data" at their own height; rankings use a compact EmptyState. | Terapkan | Sebagian. `columnChart` has no empty branch (996-1008). Text-only empties at 1321 and 3254. |
| U-36 | ux Empty 4 | A detail view for a missing record shows "… tidak ditemukan" with a Back action. | Terapkan | Sebagian. Unknown routes go to `pageNotFound` (1226). `trxPanel` (1348) has no guard; `prodTempPanel` does (2870). |
| U-37 | ux Phones 1 | Every page works on phones: the rail becomes a floating ink bottom bar (Home, 3 shortcuts, round Create, More), and the More sheet lists every page behind a search. | Adaptasi (round centre = Approval) | Sebagian. Ink `bbar` (459-463, 1112-1118) and a bottom-sheet drawer with NAV (418, 1125). The drawer has no search field. |
| U-38 | ux Phones 2 | Every menu, picker, filter and notification panel opens as a bottom sheet with a grab handle and safe-area padding; no anchored dropdowns. | Adaptasi: at ≤767px restyle `.pop` and `.dd-pop` as fixed bottom sheets | Sebagian. Drawer is a sheet with a handle (418-419); `.panel` becomes a sheet (467) without a handle. `userPop`, `bellPop`, `searchPop` and `wsPop` (1102-1126, CSS 421) and `dd-pop` (`place` 3313) stay anchored on phones. |
| U-39 | ux Phones 3 | Touch targets 44px minimum, 48px for choice rows, 56px for the primary action; inputs render at 16px. | Terapkan (≤767px) | Sebagian. `.bb-i` 44 (460), `.bb-act` 48 (462), `.inp` 44 (237). `.btn.sm`, `.fchip`, `.pills` and `icon-sm` are 32px (175, 221, 213, 176). Inputs are 14px (239), selects 13px (244), so iOS zooms. |
| U-40 | ux Phones 4 | Operator app hides its tab bar on detail, uses a sticky action bar and one-step sheets. | Tidak berlaku (operator app) | n/a |
| U-41 | ux Operator 1 | Identify by badge/PIN with a simulated scan. | Tidak berlaku (dashboard uses username login, 1217) | n/a |
| U-42 | ux Operator 2 | Work comes from the operator's own queue; "Not on your board". | Tidak berlaku | n/a |
| U-43 | ux Operator 3 | Pre-start checks, steppers, loss reason, quality gate. | Tidak berlaku | n/a |
| U-44 | ux Copy 1 | Plain and specific; no exclamation marks, filler adverbs or em dashes. | Terapkan (Indonesian) | Sebagian. No "!" in UI strings. 8 em dashes "—" used as placeholders or empty values (1283, 1774, 2146, 2181, 2883, 3023, 3024, 3327). |
| U-45 | ux Copy 2 | Meta joins with " · "; ranges read "start → end" or "→ berjalan". | Terapkan | Sebagian. " · " is widespread (1261, 1375, 2226); "→" only at 932 and 2370. |
| U-46 | ux Copy 3 | Empty values read "Tidak ada/Belum/Belum di-assign"; an empty cell is a muted en dash. | Terapkan | Sebagian. "Belum di-assign" (697); the read-only tile uses "–" (2213); em dashes elsewhere (U-44). |
| U-47 | ux Copy 4 | Labels come from `*_LABEL` maps, never hard-coded. | Adaptasi: one label/tone map per status union | Sebagian. `STTONE` 779, `ORD_TONE` 1365, `FM_ST_TONE` 1488, `TR_TONE` 2508, `HK_ST` 3154. Many statuses are inline strings ("Menunggu batal" 1375, "Dihapus" 1448). |
| U-48 | ux A11y 1 | Tokens pass WCAG AA (muted, white on accent-strong, success/warning on tints). | Adaptasi (blue tokens) | Sebagian, by calculation only. White on `--accent-strong #0A6FE0` ≈ 4.8:1, passes; `.b-accent` uses it (178). Raw `--accent #0A92F2` with white would be ≈ 3.3:1, so keep it off small white text. Muted/tints not checked. |
| U-49 | ux A11y 2 | Accent focus ring everywhere; roving arrow keys in tabs, segmented controls and charts; combobox arrows/Enter/Esc. | Terapkan | Sebagian. `:focus-visible` accent outline (55); chart arrows via `ccKey` (1009); `dd` handles arrows/Enter/Esc (3335-3339). `pills`/`utabs` are `role=tablist` without arrow keys (565, 567). |
| U-50 | ux A11y 3 | Icon-only controls carry an `aria-label` with their count; nav sets `aria-current`; the stepper uses `aria-current="step"`. | Terapkan | Sebagian. Bell "Notifikasi, n perlu perhatian" (1197); bbar counts (1113, 1116); nav `aria-current` (1092). Steps (1774) have no `aria-current="step"`. |
| U-51 | ux A11y 4 | Charts, avatars and segment bars are `role="img"` with labels; charts echo the focused value in a live region. | Terapkan | Sebagian. `av` (557), donut (1016) and line chart (1027) are labelled `role=img`; the column chart is a radiogroup (1001). No live-region echo (only `#toasts` is live, 493). |
| U-52 | ux A11y 5 | Clickable table rows also offer a real open button in the first cell. | Terapkan | Sebagian. `tr onclick` (1043) is not focusable. CRUD rows have an Edit button (566); read-only tables can't be opened from the keyboard. |
| U-53 | ux Responsive QA | At 375 and 768, `main.scrollWidth == clientWidth`; `grid-cols-1` base, `minmax(0,…)` tracks, wrapping headers, no bleed. | Terapkan (AGENTS also requires 1440 and light/dark) | Sebagian, not run in the browser. `minmax(0,1fr)` (448-473); wrapping rows (1261, 2197); small bleed `.tbl-wrap{margin:0 -4px}` (302). |

### Structure, adapting, SKILL, conventions, fixtures, state, tooling

| ID | Source | Requirement | Applicability | Status in TARGET |
|---|---|---|---|---|
| S-01 | structure Workspace | pnpm/Turbo monorepo: apps/*, packages/*. | Tidak berlaku (AGENTS: one HTML file, no build) | n/a |
| S-02 | structure deps rule | `apps → ui/fixtures → types`; ui imports no domain data. | Adaptasi: keep file order tokens → domain-free helpers → data/derive → pages | Sebagian. Helpers at 548-570 are domain-free, but derive and data blocks (1811, 2053) are mixed in among pages. |
| S-03 | structure App layering | main/router/auth/state/layouts/pages/components/lib. | Adaptasi as commented sections | Sebagian. ROUTER & STATE (578), DATA (588), SHELL (1067), PAGES (1208). No auth section. |
| S-04 | structure Rules 1 | A page composes and owns no business rules; derivations live in derive/lib. | Adaptasi | Sebagian. Derive functions exist (`repRange` 857, `deriveInc` 977, `refreshDerived` 985), but pages mutate data directly (`approve` 1555, `ordAdvance` 1374). |
| S-05 | structure Rules 2 | One `<Entity>Dialog` (open/editing/onSaved). | Adaptasi (`ENT` config) | Sebagian. Master data uses `ENT` + `openForm`/`saveForm` (2053-2253); ad-hoc modals elsewhere (`visForm` 1515, `promoForm` 1629, `posmModal` 2575). |
| S-06 | structure Rules 3 | Shared per-record actions come from one hook (`items`/`open`/`dialogs`). | Adaptasi: one `actions(kind, rec)` per entity used by both rows and panel footer | Sebagian. `rowActs` is generic (566); panel footers differ per entity. |
| S-07 | structure Rules 4-5 | Promote components on second use; `useX` naming; .ts/.tsx split. | Tidak berlaku (one file); the intent is to reuse helpers | n/a |
| S-08 | structure Domain types | Unions with `*_LABEL`, `*_FLOW` and filter subsets. | Adaptasi (JS consts) | Sebagian. `ORD_NEXT` (1366), `TASK_FLOW` (2341); not systematic. |
| S-09 | structure IDs/codes | Prefixed ids; sequential human codes (`nextCode`). | Adaptasi: `newId(p)` and `nextCode(list, 'ORD-2026-', 4)` | Sebagian. `'ap'+Date.now()%1e6` (1375, 1535, 1604, 2249, 2550, 2584, 2643) can collide and gives no sequential codes. |
| S-10 | structure Store | One reducer with `entity/verb` actions and an envelope stamped with actor and clock. | Adaptasi: one `act(type, payload)` that mutates, stamps `{by, at}`, writes `audit`, persists and renders | Sebagian. `audit()` stamps who and when (2049), but mutations are scattered across about 60 functions. |
| S-11 | structure cascades | Cascades run inside the reducer, never in pages. | Adaptasi (AGENTS: soft delete, in-use blocks rather than cascades) | Sebagian. `inUse` guards (2058-2087); rename propagation `onRename` (2248). |
| S-12 | structure guard | A failed guard leaves state unchanged, so a stale button can't corrupt data. | Terapkan | Sebagian. Guards in `toggleStatus` (2254), `assignSave` (1768), `cvAssignSave` (2404). `doDel` doesn't recheck `inUse` (2261). |
| S-13 | structure Blockers | One `<entity><Verb>Blocker` shared by the action and the UI. | Adaptasi | Sebagian. See U-08; `doDel` skips the blocker. |
| S-14 | structure Permissions | `Permission` union, `BY_ROLE`, `can()`; UI gates rendering. | Adaptasi (convenience guard) | Belum. The IAM matrix data exists (`MODULES`/`ACTS` 1792-1793) but nothing enforces it. |
| S-15 | structure Clock | `FIXTURE_NOW` plus a ticking `nowMs`/`nowIso`. | Adaptasi: `NOW='2026-10-26T…+07:00'` offset clock used by `nowStamp` and new records | Sebagian. The date is fixed ("26 Okt" 1195, `LD_TODAY` 785, `HK_TODAY` 3151), but `nowStamp` uses the real wall-clock time (2048) and `ordCancel` uses real time (1375). |
| S-16 | structure format/dates | Central format helpers; JSX never calls `toLocaleString`. | Adaptasi (Indonesian `fmt*`) | Sebagian. `rp`/`jt`/`pct` are central (550-552), but `toLocaleString` is also called inline at 931, 955, 965 and 1172. |
| S-17 | structure Persistence | Versioned envelope `{version, state}`; fall back to the seed on mismatch or corruption; save returns a boolean so a banner can show. | Adaptasi on `salesapp.shared.v1` (needs a matching change in `mobile/`) | Sebagian. Corrupt JSON falls back to `{}` (592). No version check; save returns nothing (594). Only shared slices persist, so `D`/`MD` edits are lost on reload. |
| S-18 | structure Reset | "Reset demo data" clears the app's keys and reloads. | Terapkan | Sebagian. Pengaturan Reset calls `shReset` and toasts "muat ulang" (3011) but doesn't reload, and leaves the `sim-*` keys. No confirmation, although it is destructive. |
| S-19 | structure Scoped views | `useAppState` stamps the actor; `useScoped` gives site-filtered collections, id maps and name helpers. | Adaptasi (`live()`, `byU`, workspace filter) | Sebagian. `live()` and `byU` (610) exist. The workspace switch `wsPop` doesn't filter anything ("Data mockup tetap sama", 1121). |
| S-20 | structure MobileScopeProvider | Narrow the store to the operator. | Tidak berlaku | n/a |
| S-21 | structure UI state 1 | Per-viewer preferences behind try/catch. | Terapkan | Sudah. `store` wrapper (553). |
| S-22 | structure UI state 2 | List state survives Back. | Adaptasi | Sebagian (see U-30). |
| S-23 | structure UI state 3 | Linked state lives in the URL, set with replace, deleted at its default. | Terapkan | Sudah. `setQ` deletes empty values and uses replaceState (584). |
| S-24 | structure UI state 4 | Notifications are derived on render; only read ids persist. | Terapkan | Sebagian. `bellPop` derives (1126); the "+1" in the bell count is hard-coded (1197); no read state. |
| S-25 | structure UI state 5 | Never copy store state into local state. | Terapkan | Sudah (sampled). `ST.asg`/`ST.cvPick` hold selections only (1754, 2397). |
| S-26 | structure Seed | Deterministic seeded PRNG plus consistency checks. | Adaptasi: inline mulberry32 if dummy data is ever generated | Sudah. Data is static or derived; `Math.random` appears only in animation timing (1142). |
| S-27 | structure Seed chunk/CI diff | `manualChunks` seed chunk; CI fails if regeneration changes the seed. | Tidak berlaku | n/a |
| S-28 | structure Tests | `node:test` reducer and journey tests. | Tidak berlaku (AGENTS: no tests). Intent: a manual journey checklist in `docs/CONTEXT.md` | n/a |
| S-29 | structure Quality gates | Strict TS, ESLint, Prettier, CI; effects only for subscriptions. | Tidak berlaku. Intent = AGENTS "Selesai" (no console errors, no overflow) | n/a |
| S-30 | structure Naming | Storage `<ns>.<app>.<thing>`, permission `<area>.<verb>`, code `<PREFIX>-<year>-<seq>`, deep-link params. | Adaptasi (`salesapp.dashboard.*`) | Sebagian. `salesapp.shared.v1` follows it; `sim-theme`/`sim-sidebar` don't; no codes or permissions. |
| S-31 | structure Backend arrives | Keep action names as REST verbs, actor and time on the server, blockers as service guards. | Adaptasi: name `act()` types after `docs/03-contracts` | Belum |
| S-32 | adapting §1 Copy as-is | Copy root config, packages/ui, tailwind-config, tsconfig, scripts, fixtures helpers, app shells. | Tidak berlaku as files; intent ported in the other rows | n/a |
| S-33 | adapting §1 nav.ts | `NAV` sections and badge keys. | Terapkan | Sudah. `NAV` (1070). |
| S-34 | adapting §1 useNavCounts | One count per badge key. | Terapkan | Sudah. `counts()` used by bbar and bell (1113, 1197). |
| S-35 | adapting §1 notifications.tsx | `useNotifications` rules plus a button with the count. | Terapkan | Sebagian (see S-24). |
| S-36 | adapting §1 GlobalSearch | Grouped `searchAll` results. | Adaptasi | Sebagian. `doSearch` jumps to the first match (1122); no grouped results. |
| S-37 | adapting §1 CreateMenu | Global create menu filtered by permission. | Adaptasi (optional "Buat" pop) | Belum. Create buttons are per page only. |
| S-38 | adapting §1 PhoneNav | Role shortcuts and a fallback. | Adaptasi | Sebagian. Fixed shortcuts (1115-1118). |
| S-39 | adapting §1 Header | Date, role label, "Switch demo user". | Adaptasi | Sebagian. Hard-coded date chip (1195) and "Admin SIM" (1198); no user switch. |
| S-40 | adapting §1 SideRail | Product name and site/workspace list. | Terapkan | Sudah. `sidebarHTML` + `wsPop` (1086, 1121). |
| S-41 | adapting §1 AdminLayout GLOW | Accent glow written out by hand. | Terapkan (blue) | Sudah. `.glow` (1188). |
| S-42 | adapting §1 auth.tsx | Demo session key, sign-in/out, `can`. | Adaptasi | Belum. Login just calls `go('/')` (1217); Keluar → `/login` (1102); no session. |
| S-43 | adapting §1 store.tsx key | `STORAGE_KEY` for the whole state. | Adaptasi | Sebagian (see S-17). |
| S-44 | adapting §1 scoped.ts | Collections, maps, name helpers. | Adaptasi | Sebagian (see S-19). |
| S-45 | adapting §1 mobile auth/store/scope/tabs/router/paths/time | Mobile shell adaptations. | Tidak berlaku to the dashboard (see M rows) | n/a |
| S-46 | adapting §1 permissions/persistence/clock | Adapt these three helpers. | Adaptasi | See S-14, S-17, S-15. |
| S-47 | adapting §1 Write new | types, store, derive, router, badges, links, pickers, create. | Adaptasi in-file | Sebagian. `ROUTES` (3290); badges `tag`/`spill` (558-560); no `paths` builder (hrefs are inline strings); no create provider. |
| S-48 | adapting §2 Rename scope | `@mes/` → `@scope/`. | Tidak berlaku | n/a |
| S-49 | adapting §2 Storage prefix | `mes.` → project namespace. | Adaptasi (`salesapp.`) | Sebagian (see S-30). |
| S-50 | adapting §2 Product strings | Titles, rail name, sign-in copy. | Terapkan | Sudah. "SIM Sales Console" (6, 1215), Indonesian. |
| S-51 | adapting §2 Locale | Offset, label and currency for the market. | Terapkan | Sudah. `id-ID` rupiah (550-551), `lang="id"` (2). |
| S-52 | adapting §3 Palette | Set accent tokens, keep the token names, check contrast. | Terapkan (SIM blue) | Sudah. Tokens at 19-22 and dark at 41-43; contrast see U-48. |
| S-53 | adapting §3 Hand-written colours | GLOW, `theme-color`. | Terapkan | Sebagian. `theme-color #F1F0F1` (7) with no dark-theme variant. |
| S-54 | adapting §4 gap 1 | No second hand-drawn blob inside ink cards. | Terapkan | Sudah. Only `.card.ink::before` (383). |
| S-55 | adapting §4 gap 2 | No raw accent behind small white text. | Terapkan | Sudah. `.b-accent`, `.bb-i.on` and `.bb-act` use accent-strong (178, 461-462). |
| S-56 | adapting §4 gap 3 | Sticky header inside overflow-x never sticks. | Tidak berlaku (no Gantt) | n/a |
| S-57 | adapting §4 gap 4 | Hide rather than disable for permission. | Adaptasi when U-07 lands | n/a today |
| S-58 | adapting §4 gap 5 | Empty cell = muted en dash. | Terapkan | Sebagian (see U-44, U-46). |
| S-59 | adapting §4 gap 6 | Empty tables get an EmptyState with the next action. | Terapkan | Sebagian (see U-33). |
| S-60 | adapting §4 gap 7 | Cards carry no borders. | Terapkan | Sudah for cards (196). Note `.pop` has a border (421). |
| S-61 | adapting §4 gap 8 | Show zero counts on filter chips. | Terapkan | Sudah. Chip counts include 0 (2199). |
| S-62 | adapting §4 gap 9 | Phone master/detail scrolls the detail into view. | Adaptasi | Sudah in effect. Details open as a bottom-sheet panel (467). |
| S-63 | adapting §4 gap 10 | Drop unused dependency. | Tidak berlaku | n/a |
| S-64 | adapting §5 Verify | pnpm gates, then screens at 375/768. | Adaptasi: AGENTS manual check at 375/768/1440, light and dark | Not verified in this read-only audit. |
| S-65 | SKILL Stack | React, Vite, Tailwind, Radix. | Tidak berlaku | n/a |
| S-66 | SKILL Workflow, new product 1-6 | Copy, palette, types, shells, pages, gates. | Tidak berlaku, except palette (S-52) | n/a |
| S-67 | SKILL New screen | Closest recipe plus the finish checklist. | Terapkan | Process rule; not verifiable in the file. |
| S-68 | SKILL UI essentials | Grey canvas, borderless cards, one ink, one red accent, pills, DM Sans plus JetBrains Mono, titles ending in an accent period. | Adaptasi (blue accent per AGENTS) | Sudah. Fonts (11), page title period `<i>.</i>` (1193), panel period (1059). |
| S-69 | SKILL UX essentials | Summary of the U rows. | Terapkan | See U rows. |
| S-70 | SKILL Structure essentials | Summary of the S rows. | Adaptasi | See S-10, S-13, S-14, S-15, S-17. |
| S-71 | SKILL Before you finish | `patterns.md` checklist plus gates. | Adaptasi (AGENTS "Selesai") | Not verified. |
| S-72 | conventions Where: enums | Use `*_LABEL`. | Adaptasi | Sebagian (see U-47). |
| S-73 | conventions Where: store dispatch | Change data only through dispatch. | Adaptasi | Sebagian (see S-10). |
| S-74 | conventions Where: derive / lib | Shared derivations in derive, page-local in lib. | Adaptasi | Sebagian (see S-04). |
| S-75 | conventions Where: format/dates | Use the helpers; never `toLocaleString` inline. | Adaptasi | Sebagian (see S-16). |
| S-76 | conventions Where: scoped | Pages never filter the raw store by site. | Adaptasi (workspace) | Sebagian (see S-19). |
| S-77 | conventions Where: auth `can` | Gate mutating buttons. | Adaptasi | Belum (see U-07). |
| S-78 | conventions Where: badges | One badge per status union. | Adaptasi | Sebagian. `spill`/`tag` with tone maps (558-560). |
| S-79 | conventions Where: links/paths | Route builders and link chips. | Adaptasi (`paths` object) | Belum. Inline `#/...` strings. |
| S-80 | conventions Where: pickers | Searchable combobox for data choices. | Terapkan | Sudah (see U-22). |
| S-81 | conventions Where: create.tsx | Shared create dialogs. | Adaptasi | Belum. |
| S-82 | conventions Where: history-state | List state survives Back. | Adaptasi | Sebagian (see U-30). |
| S-83 | conventions Where: storage | `usePersistentState` under namespaced keys. | Adaptasi | Sebagian (see U-32). |
| S-84 | conventions Where: kit barrel | One kit of components. | Adaptasi | Sudah. Helpers `btn`, `card`, `dataTable`, `pills`, etc. (562-570, 1038). |
| S-85 | conventions Reference: dashboard | Attention row, ink hero, accent card, stats, lists. | Terapkan | Sudah (1257-1293). |
| S-86 | conventions Reference: list page | PageHeader with description, search, primary; stats; chips; table. | Terapkan | Sebagian. `crudList` has search, chips, table and an accent add button (2197-2201). No one-sentence page description under the title (1193). |
| S-87 | conventions Reference: detail page | Back, actions, ink hero, Steps, KeyValue, related lists, history. | Adaptasi (panel) | Sebagian (see U-04, U-05). |
| S-88 | conventions Layout: grid base | `grid-cols-1` base on every breakpoint grid. | Terapkan | Sudah (448-473). |
| S-89 | conventions Layout: wrap rows | Rows with a right-side button wrap. | Terapkan | Sudah (1261, 2197). |
| S-90 | conventions Layout: no bleed | No negative-margin bleed. | Terapkan | Sebagian (302). |
| S-91 | conventions Layout: tables | Hide secondary columns; key badges in the first cell on phones. | Terapkan | Sudah. `hide-m` and `prim` card rows (1042, 476-481). |
| S-92 | conventions Layout: cards | No border; nested rows on surface-2. | Terapkan | Sudah (196). |
| S-93 | conventions Layout: numbers / mono ids | Big numbers with a muted unit; identifiers in mono. | Terapkan | Sudah. 1273; `.mono` (57). |
| S-94 | conventions Layout: toast tones | `default \| success \| danger`. | Adaptasi | Sebagian (see U-11). |
| S-95 | conventions Checklist 1 | PageHeader with a one-sentence description. | Terapkan | Belum. The header has breadcrumb and title only (1192-1193). |
| S-96 | conventions Checklist 2-3 | Stats; chip filters in the URL. | Terapkan | Sudah (2199, 584). |
| S-97 | conventions Checklist 4 | Table with row key, row click and `resetPageKey`. | Terapkan | Sebagian. Page only clamps (1039); it does not reset to 1 when filters change. |
| S-98 | conventions Checklist 5-6 | `tried` dialogs; deep links. | Terapkan | See U-16, U-31. |
| S-99 | conventions Checklist 7 | typecheck, prettier. | Tidak berlaku | n/a |
| S-100 | conventions Prose | Plain copy, no em dash, no "!". | Terapkan | Sebagian (see U-44). |
| S-101 | fixtures clock.ts | Offset clock from `FIXTURE_NOW`. | Adaptasi | Sebagian (see S-15). |
| S-102 | fixtures dates.ts | Plant-time (WIB) math and date-input conversion. | Adaptasi (only where dates are input) | Sebagian. `isoOf` and `new Date(...'T00:00:00')` use local time (866, 874, 3163). |
| S-103 | fixtures format.ts | `fmt*`, `fmtIdr`/`fmtIdrShort`, `plural`, `initials`. | Adaptasi | Sebagian. `rp`/`jt`/`pct`/`initials` exist (550-556); no shared date formatter. |
| S-104 | fixtures ids.ts | `newId`, `nextCode`. | Adaptasi | Sebagian (see S-09). |
| S-105 | fixtures json.d.ts | Typed JSON import. | Tidak berlaku | n/a |
| S-106 | fixtures permissions.ts | `BY_ROLE` and `can`. | Adaptasi | Belum. |
| S-107 | fixtures persistence.ts | Versioned load/save with fallback and a boolean save result. | Adaptasi | Sebagian (see S-17). |
| S-108 | admin state/store.tsx | Provider with a `storageError` flag. | Adaptasi (`ST.storageErr`) | Belum. |
| S-109 | admin state/scoped.ts `useAppState` | Dispatch stamps the actor and the clock. | Adaptasi | Sebagian. `audit` uses a hard-coded user 'Dewi Kartika' (2049). |
| S-110 | admin state/scoped.ts `useScoped` | Maps and name helpers with a fallback ("Unknown"). | Adaptasi | Sebagian. `byU` (610), `tokoName`; panels assume the record exists (1348). |
| S-111 | admin state/scoped.ts `useNow` | Re-render on an interval so "x ago" labels move. | Tidak berlaku (no relative-time labels) | n/a |
| S-112 | admin auth/auth.tsx | Session in storage, `RequireAuth`, `can`. | Adaptasi (light) | Belum. No route guard: `#/` works without login (582). |
| S-113 | admin lib/storage.ts | `readStorage`/`writeStorage` that remove on null. | Terapkan | Sudah. `store` (553), `shGet`/`shSet` (592-594). |
| S-114 | admin lib/history-state.ts | Per-history-entry state. | Adaptasi | Sebagian (see U-30). |
| S-115 | admin main.tsx | Provider order plus one Toaster. | Adaptasi: boot order load → auth → render, one `#toasts` | Sudah. `#toasts aria-live` (493), `render(true)` boot (3346). |
| S-116 | package.json | Workspace scripts. | Tidak berlaku | n/a |
| S-117 | turbo.json | Task graph. | Tidak berlaku | n/a |
| S-118 | pnpm-workspace.yaml | Workspace packages. | Tidak berlaku | n/a |
| S-119 | eslint.config.js | Lint rules (`no-console` and others). | Tidak berlaku. Intent: no stray `console.log` | Not checked by a tool. |
| S-120 | .prettierrc / .prettierignore | Formatting. | Tidak berlaku | n/a |
| S-121 | .npmrc / .gitignore | Package and git config. | Tidak berlaku (no deps) | n/a |
| S-122 | .github/workflows/ci.yml | Seed diff, typecheck, lint, test, build. | Tidak berlaku (Vercel static; AGENTS: no deploy without confirmation) | n/a |
| S-123 | scripts/seed/rng.ts | mulberry32 `createRng`. | Adaptasi only if data generation is added | Not needed today (S-26). |
| S-124 | scripts/tsconfig.json | Scripts TS project. | Tidak berlaku | n/a |
| S-125 | packages/*/package.json | Source exports, workspace deps. | Tidak berlaku | n/a |
| S-126 | tsconfig base/library/vite-react + app tsconfigs | Strict compiler flags. | Tidak berlaku | n/a |
| S-127 | admin vite.config.ts | manualChunks seed. | Tidak berlaku | n/a |
| S-128 | admin index.html | `viewport-fit=cover`, `theme-color`, fonts. | Terapkan | Sebagian. The viewport meta lacks `viewport-fit=cover` (5), so `env(safe-area-inset-bottom)` on the bbar (459) resolves to 0 on iOS. Fonts and `theme-color` are present (7, 11). |
| S-129 | admin package.json | App deps. | Tidak berlaku | n/a |
| S-130 | examples journeys.test.ts | Whole flows asserted step by step, each dispatch one minute apart. | Tidak berlaku. Intent: a manual demo-journey script (approval → toast → list) | n/a |
| S-131 | examples repo-README.md | Run, layout, conventions, checks sections. | Adaptasi: AGENTS plus `docs/CONTEXT.md` already cover this | Exists outside the target; not audited. |

### Mobile assets (applied to the dashboard on phones only; `mobile/index.html` keeps its own style per AGENTS)

| ID | Source | Requirement | Applicability | Status in TARGET |
|---|---|---|---|---|
| M-01 | apps/mobile/index.html | PWA metas, `viewport-fit=cover`, apple-touch. | Adaptasi: take only `viewport-fit=cover` | Belum (see S-128). |
| M-02 | apps/mobile/vite.config.ts | PWA manifest and workbox. | Tidak berlaku (static demo, no service worker) | n/a |
| M-03 | apps/mobile/public/* | Icons. | Tidak berlaku (SIM logo in `assets/`) | n/a |
| M-04 | apps/mobile/src/index.css | Inputs at 16px under `pointer: coarse`; safe-area utilities. | Terapkan | Belum. Inputs 14px (239), selects 13px (244). |
| M-05 | mobile main.tsx / router.tsx | Providers; `RequireAuth` around the layout. | Tidak berlaku | n/a |
| M-06 | mobile auth/auth.tsx | `MOBILE_ROLES`, session. | Tidak berlaku | n/a |
| M-07 | mobile state/store.tsx | Own storage key. | Tidak berlaku. The project deliberately shares `salesapp.shared.v1` | n/a |
| M-08 | mobile state/scope.tsx | Operator scope plus stamped dispatch. | Tidak berlaku | n/a |
| M-09 | mobile layouts/tabs.ts + TabBar.tsx | Tab definitions, badge counts in `aria-label`, `aria-current`, 56px items. | Adaptasi for the bbar | Sebagian. bbar labels with counts and `aria-current` (1113-1116); items 44/48px (460, 462). |
| M-10 | mobile layouts/MobileLayout.tsx | Hide the bar on detail screens; ScrollRestoration. | Adaptasi: hide the bbar while a panel sheet is open | Sebagian. Panel overlay z-index 61 covers the bar; no scroll restoration (see U-30). |
| M-11 | mobile layouts/DetailHeader.tsx (BackButton) | Back goes to history if the user came from inside the app, otherwise to the fallback list. | Adaptasi: breadcrumb link exists; add a back button on `page.crumb` pages | Sebagian. Breadcrumb links to the parent (1192). |
| M-12 | mobile layouts/ScreenHeader.tsx | Greeting, title with accent period, avatar. | Tidak berlaku (dashboard header) | n/a |
| M-13 | mobile layouts/LogoMark.tsx | Product mark. | Tidak berlaku (SIM logo, 543) | n/a |
| M-14 | mobile components/ActionSheet.tsx | One-step bottom sheet whose body unmounts on close. | Adaptasi for phone pops (U-38) | Sebagian. `.panel` is a sheet on phones (467); pops are not. |
| M-15 | mobile components/StickyBar.tsx | Primary actions pinned above the home indicator, with a note line. | Adaptasi: `.panel-f` acts as the sheet footer | Sebagian. The footer exists (1059) but has no safe-area padding and no note slot. |
| M-16 | mobile components/Stepper.tsx | 48px ± steppers. | Tidak berlaku (no piece counting) | n/a |
| M-17 | mobile components/StatTile.tsx | Linked big-number tiles. | Adaptasi | Sudah. `.stat` links (1273-1274). |
| M-18 | mobile components/Section.tsx | Titled block with a count. | Adaptasi | Sudah. `card()` with title and `utabs` counts (567-568). |
| M-19 | mobile lib/paths.ts | Path builders. | Adaptasi | Belum (see S-79). |
| M-20 | mobile lib/time.ts | Greeting on the app clock. | Tidak berlaku | n/a |
| M-21 | mobile lib/storage.ts `clearAppStorage` | Reset removes every key with the app prefix. | Terapkan | Sebagian. `shReset` removes only the shared key (595). |
| M-22 | ex mobile pages/more/MorePage | "Reset demo data" behind a confirm, then reload. | Terapkan | Belum. No confirm and no reload (3011). |
| M-23 | ex mobile pages/work/WorkListPage | Chips, lazy list with sentinel, EmptyState. | Adaptasi | Sebagian (see U-24, U-34, U-33). |
| M-24 | ex mobile pages/work/WorkOrderPage + lib/wo.ts | `startBlocker` mirrors the reducer guard and becomes a danger toast. | Adaptasi (blocker pattern only) | Sebagian (see U-08). |
| M-25 | ex mobile pages/work/sheets.tsx | Action sheets with toasts that name the record. | Adaptasi (toast pattern) | Sebagian (see U-10). |
| M-26 | ex mobile pages/auth/LoginPage | Badge or PIN with a simulated scan. | Tidak berlaku | n/a |
| M-27 | ex mobile pages/home/HomePage, WoHero, WoCard | Hub tiles, ink hero, list cards. | Tidak berlaku (operator domain); the dashboard hero is already covered | n/a |
| M-28 | ex mobile pages/quality/InspectionPage, components/InstructionView | Inspection and instruction flows. | Tidak berlaku | n/a |
| M-29 | ex mobile components/badges.tsx, pickers.tsx | Status dot line; operator pickers. | Adaptasi (dot status = `spill`) | Sudah. `spill` (560). |

### Conflicts with AGENTS.md (AGENTS wins on project rules)

1. **Accent colour.** SKILL says one WIT red `#ed1c24`. AGENTS says the accent stays SIM blue and red is only for danger. AGENTS wins; the target is already blue (19). Related: the required-field asterisk is red (2210). Making it the accent colour satisfies both the skill (U-17) and AGENTS.
2. **Stack and build.** Monorepo, React/Vite/TS, ESLint/Prettier, `node:test` and CI (structure.md, adapting §1/§5, SKILL Workflow) conflict with AGENTS: one self-contained HTML file per mockup, no build, no dependencies, "Tes, lint, build: tidak ada". AGENTS wins. Only the intent carries over (S-10, S-13, S-14, S-15, S-17).
3. **Persistence key.** The skill gives each app its own copy (`mes.admin.*`, `mes.mobile.*`). The project deliberately shares `salesapp.shared.v1` between the two mockups. The project wins. Versioning or a storage-error flag can be added, but the shape must change in `mobile/index.html` at the same time.
4. **Delete semantics.** The skill's generic `/remove` with `cascadeRemove` conflicts with AGENTS: always soft delete with confirmation, and data still in use cannot be deleted. AGENTS wins. The target follows it (`inUse` block, `r.del=true`, 2256-2261); never port the cascade.
5. **Permissions authority.** The skill's `can()` gates rendering. AGENTS (shared core): the backend decides and a UI guard is only a convenience. These are compatible as long as any role guard is labelled cosmetic and is not treated as the rule.
6. **Mobile assets.** The operator PWA look (white tab bar, red accent, DM Sans) conflicts with the AGENTS mobile style (blue gradient, tan accent, glassmorphic, Plus Jakarta Sans). AGENTS wins. Mobile assets apply only to the dashboard's phone layout, and only behaviourally (M rows).
7. **Copy language.** The skill's English copy examples and maps ("Good morning", `*_LABEL` in English) conflict with AGENTS: Indonesian UI in active, simple sentences. AGENTS wins. The copy rules ("·", "→", en dash, no em dash, no "!") still apply in Indonesian.
8. **QA scope.** The skill checks 375 and 768 only and has no dark mode. AGENTS requires 375/768/1440 in light and dark, plus the chooser page and both cross-links. AGENTS wins as the superset.
9. **Data ids.** The skill's random `newId` versus the AGENTS shared-core invariant of stable operation IDs for deduplication (BR-005/BR-017). These don't conflict for a mockup, but if ids are reworked (S-09), stable sequential ids fit AGENTS better than the current `Date.now()%1e6`.

Other problems found while checking, not conflicts:
- Confirm titles render "?." (2258, 1447, 1627).
- Reset leaves the `sim-*` keys, asks for no confirmation and does not reload (3011).
- The viewport meta lacks `viewport-fit=cover`, so the safe-area padding on the phone bar is ineffective (5, 459).
