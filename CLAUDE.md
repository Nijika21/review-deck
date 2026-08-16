# review-deck — context for Claude / Claude Code

Static, single-file review deck (`index.html` + `shots/*.webp`) for the site owner to
read on their phone/laptop and give feedback on the CV Mitra Agung Teknik and PT Mitra
Agung Elektrindo websites via linked Google Forms. No build step, no framework.

- Live: https://review.ganiworks.my.id/
- Repo: https://github.com/nijika21/review-deck (public, personal account — **not** the
  `PKL-Website` org; moved there deliberately, see Decisions log below)
- Deploy: git-linked to Vercel project `review-mat-mae`. Push to `main` → auto-deploys.
  Do **not** fall back to manual `vercel --prod` unless the git integration is broken —
  that was the old way before this repo existed and it's a dead end (see log).

## Status right now (2026-08-16)

The deck itself (`index.html`, structure/content/design below) is **final** — it went
through many rounds of revision from the original blueprint and the current committed
version is the correct one, not the blueprint. Don't re-derive structure from the
blueprint section below; it's kept only as a backlog reference.

**Forms are migrating from Google Forms to Jotform.** The 7 Google Forms (still live,
still linked from `index.html` right now) had drifted from the questions shown on the
page — some had extra questions the page never mentioned, some were missing questions
the page implied existed. Investigated by comparing each live Google Form against the
page's `qlist` text. Resolved: **the page (`index.html`) is the source of truth**, not
the old Google Forms. 7 new Jotform forms were built from the page's question text (not
the old Google Forms' extra/missing content) and manually corrected afterward — see
"Pending web edits for Claude Code" below for the exact swap needed in this repo.

## Current structure (source of truth — treat this as correct, not the blueprint below)

Order of sections, each with its own Google Form via a "Isi Tanggapan →" button:

0. Cover (`#ringkasan`) — title, read-time estimate, sticky topnav with jump links:
   `Ringkasan / CV / PT / Handover / Penutup` (5 fixed links, no search).
1. Intro — "Kenapa halaman ini dibuat" + caveats about what's still placeholder.
2. **CV** (`#cv`) — 2 sub-sections, 2 forms:
   - CV Bagian A: Tampilan Website
   - CV Bagian B: Sistem Pengelolaan & Hak Akses
3. **PT** (`#pt`) — 3 sub-sections, 3 forms:
   - PT Bagian A: Tampilan & Isi Website
   - PT Bagian B: Data Perusahaan yang Perlu Dipastikan
   - PT Bagian C: Sistem Pengelolaan & Hak Akses **(Rencana)** — dashboard not built yet.
     Uses a `badge-planned` note ("🔧 Rencana — belum dibuat") + a hand-coded
     `.flow`/`.flow-step` diagram (not mermaid, not a mockup image) to show the planned
     flow, reusing the same design as CV-B.
4. **Handover, Domain & Hosting** (`#handover`) — ONE shared section for both companies
   (`company-hero shared-hero` / `badge-shared`), explicitly *not* merged into the CV or
   PT sections above. Internally split into two cards ("Untuk CV" / "Untuk PT", 4
   questions each = 8 total) but funnels into **one combined Google Form**, not two.
5. Penutup (`#penutup`) — final catch-all "Tanggapan Keseluruhan" form.

**Total: 7 Google Forms**, all created manually (not Apps-Script-generated), each
presumably already wired to its own tab in a Google Sheet. Screenshot annotations
(`.callout` / `.callout-label`) are absolute-positioned divs baked directly into the
HTML per shot — not a separate reusable annotation layer.

## Decisions log (don't redo/undo these without being asked)

- **QR codes next to feedback buttons: tried, then reverted.** Added once (commit "Add
  QR codes next to each feedback form button"), then removed in the very next commit
  ("Remove QR codes from feedback buttons — confusing on same-device reading") because
  the page is read solo on the same phone that would scan the code — pointless. Don't
  re-add without an explicit new ask.
- **Handover/domain/hosting is one combined section+form for both companies**, not
  split into CV-C and PT-D. This was a deliberate simplification vs. the original
  blueprint (see below) — keep it combined unless told otherwise.
- **Repo lives under the personal GitHub account (`nijika21`), not the `PKL-Website`
  org**, even though the CV/PT app repos are in that org. Explicit owner request:
  wanted personal-profile visibility. The old `PKL-Website/review-deck` copy was
  deleted after confirming the new one deployed correctly — don't recreate it there.
- Deploy pipeline was manual (`vercel --prod` via Claude Code CLI) until it got a real
  git repo + `vercel git connect`. If a session ever falls back to manual deploy again
  when git deploy is available, that's a regression — fix the git link instead.

## Pending web edits for Claude Code (do these in this repo)

Not done yet — needs an actual edit to `index.html` in this repo, then commit + push
(auto-deploys). Do this next time a session has access to the repo files:

**Swap all 7 "Isi Tanggapan →" button hrefs from Google Forms to Jotform.** Old →
new, one-to-one by section (find each old URL in `index.html`'s `btn-feedback` links
and replace with the matching new one — do not change anything else in those `<a>`
tags):

| Section | Old (Google Form) | New (Jotform) |
|---|---|---|
| CV Bagian A | `https://docs.google.com/forms/d/e/1FAIpQLScXAheva_3PlK5706mMhu9rJF3xghnDJ3NsswqN2VRdY5iNOg/viewform` | `https://form.jotform.com/262272783854063` |
| CV Bagian B | `https://docs.google.com/forms/d/e/1FAIpQLSemKfMEg1S9alKi5eYXrLy5ca2fSU3s44NCb7k18f629P81oQ/viewform` | `https://form.jotform.com/262272737096060` |
| PT Bagian A | `https://docs.google.com/forms/d/e/1FAIpQLSdE3Hvq4RDPEoTOw44M_y6kRVQIHm3ZJHKK9nBglBdCm2eG2A/viewform` | `https://form.jotform.com/262273278757065` |
| PT Bagian B | `https://docs.google.com/forms/d/e/1FAIpQLSd4HE51-MuxjeRum34DnNphuhNU7ZpbzaFi5tXlRngUSqauKg/viewform` | `https://form.jotform.com/262272947582063` |
| PT Bagian C | `https://docs.google.com/forms/d/e/1FAIpQLSd7_wGGRqaUp72t9-INLsfeEVySwMNTqU7fNCjdFQD92MM1Wg/viewform` | `https://form.jotform.com/262272728467062` |
| Handover | `https://docs.google.com/forms/d/e/1FAIpQLScgLj9xyRwonOjCTLtPd9bvAUd_b6A6q_Q3xgvCGj-YE2dJCQ/viewform` | `https://form.jotform.com/262272464755059` |
| Penutup | `https://docs.google.com/forms/d/e/1FAIpQLScAXGETgMqxWttIXSdUL0d_irqekzn5LQaa0nawL6eGkmdhQA/viewform` | `https://form.jotform.com/262272840402046` |

Don't touch the old Google Forms themselves — leave them as-is (unlinked, not
deleted), same as the earlier `PKL-Website` org repo situation: safer to leave orphaned
than to delete something that might still matter.

After swapping, verify: 7 `btn-feedback` hrefs all point to `form.jotform.com`, zero
`docs.google.com/forms` references left in `index.html`, div balance unchanged, then
commit + push (git-auto-deploys per the Deploy section above — do not `vercel --prod`
manually).

**Already fixed directly in Jotform (no page edit needed for these):** PT Bagian B's
"Jam operasional" question now shows a proper text field for the actual hours when
"Lainnya" is picked (was just a generic explain box before); the Handover form's
"email pengirim" question option was reworded from "Sementara email pribadi" to "Akun
pribadi" (it's the real intended choice, not a stopgap — for both the CV and PT copies
of that question); the two "Nama domain" questions (CV's and PT's) were moved to sit
next to each other in the Handover form instead of being split across the two company
sections. None of this needed a corresponding `index.html` change since the page's
prose descriptions of Handover/PT-B don't quote exact field wording.

## Waiting list — from an earlier, more ambitious blueprint, not yet built

An earlier full blueprint proposed a bigger version of this deck. Some of it shipped in
a simplified form (see structure above); the rest below was never built. Keep it here
so it isn't reproposed from scratch or silently forgotten — but nothing here should be
built without the owner explicitly asking again.

- **Apps Script to auto-generate all Google Forms + a linked Sheet in one run.**
  Current forms were created manually one at a time; there's no script that recreates
  them or keeps them in sync with the questions in `index.html`.
- **CV Bagian C and PT Bagian D as separate per-company Domain/Hosting/Handover
  sections+forms**, so answers land in company-specific rows. Current implementation
  intentionally combined these into the one shared Handover section/form instead
  (see Decisions log) — revisit only if per-company tracking turns out to matter.
- **Mockup diagrams/images (not just flow charts) for not-yet-built features**, e.g. an
  actual dashboard mockup for PT Bagian C instead of the current text + flow-diagram
  treatment.
- **Screenshots pulled live via browser automation** with annotations kept as a
  separate reusable layer (so re-shooting a page doesn't require redrawing callouts).
  Current callouts are hand-placed percentage coordinates baked into each shot's HTML.
- **Flow diagrams authored in Mermaid and rendered**, vs. the current hand-coded
  `.flow`/`.flow-step`/`.flow-branch` CSS blocks (which work fine, just aren't
  Mermaid-sourced).
- **Sticky nav with search/jump-to-category**, vs. the current fixed 5-link topnav
  (`Ringkasan/CV/PT/Handover/Penutup`), which has no search.
