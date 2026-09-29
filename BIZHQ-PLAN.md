# BizHQ Plan

**Owner:** Terrence Lee (TeeLee Biz)
**Last updated:** 2026-09-29
**Purpose:** Single source of truth for BizHQ work. Any Claude session working on BizHQ or this site reads this file first, then verifies live state before acting.

---

## 1. How we work

1. **Claude carries the work.** Terrence gives direction and yes/no decisions. Claude does the building, testing, committing, deploying and verifying.
2. **Evidence first.** Check the live state before acting: read the actual files, query Supabase, check logs. Never rely on memory or an old summary. No guessing, no assuming.
3. **Recommend, don't survey.** Present one clear recommendation with its trade-off, and ask for a yes or no.
4. **Test before shipping.** Syntax-check and render every page change at phone width before it goes live. After deploy, confirm it's live.
5. **Say where things live.** Give the exact file, URL or screen path for every change.
6. **Only hand Terrence steps Claude cannot do**, such as GitHub, Apple or Stripe account-owner actions or his own Windows PC. When that happens, give exact click-by-click steps, the next 2–3 steps together, and one clean copy-paste block with nothing extra in it.
7. **Two strikes, new path.** If a route fails twice, stop and find a different route.
8. **No Expo Go for BizHQ.** Terrence moved off it on purpose. Do not suggest it unless he raises it.
9. **Customer-facing AI never gets autonomous write access to production.** Use human-in-the-loop escalation.

---

## 2. Where everything lives

| Piece | Location |
|---|---|
| BizHQ mobile app (Expo SDK 54, custom EAS dev client) | Terrence's PC: `C:\Users\teele\bizpilot` (folder still named `bizpilot`; app brand is BizHQ) |
| Website + client portal | GitHub `teeleebiz-web/invoicespro-site` (this repo, served from `main`) |
| Backend (shared with InvoicePro) | Supabase project `bbdlhioqdlgnwbdpejvj` |
| iOS bundle ID | `com.teebiz.bizpilot` |
| EAS project ID | `34be8390-f178-485e-940a-e131b5525213` |

**BizHQ tables** (all `bp_` prefix): `bp_clients`, `bp_proposals`, `bp_contracts`, `bp_invoices`, `bp_invoice_line_items`, `bp_installments`, `bp_reminders`, `bp_ai_commands`, `bp_documents`, `bp_portal_access`, `bp_expenses`, `bp_appointments`, `bp_time_entries`.

**Shared with InvoicePro, so changes affect both apps:** Edge Functions `send-invoice` and `send-receipt`. **Always pass `verify_jwt: false` explicitly when redeploying these two.**

**Brand palette:**
- Olive `#6B7A2F`: chrome only (headers, nav, status bar, borders).
- Gold `#E8A020`: every button, CTA and active state, always with dark ink text `#2D1B0E`, never white.
- Secondary text: `#5A4A3A` and `#6B5B4E`.
- Dark olive status bar: `#3E471B`.

---

## 3. Done (verified)

| Date | Item | Evidence |
|---|---|---|
| 2026-09-25 | App color pass to olive/gold; portal-invite email redeployed; 7 missing FK indexes added; function search paths pinned | Prior session handoff |
| 2026-09-26 | `send-invoice` (v12) and `send-receipt` (v25) email palettes updated, `verify_jwt: false` kept | `list_edge_functions` 2026-09-28 |
| 2026-09-28 | **Client portal data leak fixed.** `portal.html` looks up `bp_portal_access` for the signed-in user only (`auth_user_id`), shows a "no account found" state when none is linked, and filters invoices, proposals and contracts by `client_id`. Live on `main`. | [PR #1](https://github.com/teeleebiz-web/invoicespro-site/pull/1), commit `fecaa5a` |
| 2026-09-28 | **Payment confirmation page** (`payment-complete.html`) moved to the olive/gold palette. Live on `main`. | Same PR |
| 2026-09-28 | `bizhq-transcribe` Edge Function (voice to text via Whisper) confirmed **deployed** (v1, requires login) | `list_edge_functions` |
| 2026-09-28 | Claude GitHub App installed, so Claude can push to this repo | Push succeeded |
| 2026-09-29 | `.claude/settings.json` added to `main`, pre-approving Claude's PR merges and live-site checks on this repo | Commit `f183d1b`, JSON validated |

---

## 4. Open items, in order

| # | Item | Status / what's needed |
|---|---|---|
| 1 | **Add `CLAUDE.md`** with the working rules from Section 1, so every session loads them automatically | Next step. Claude does it. |
| 2 | **`client-portal.html`** still has the old clay-brown palette (`--text-mid: #8B4A20`, `--text-mute: #9B8574`) | Convert to olive/gold, test at phone width, merge, confirm live. Claude does it. |
| 3 | **Delete the leftover `claude/settings.json`** (created without the dot; inactive) | Claude does it. |
| 4 | **Remove Edge Function `temp-stripe-mode-check`** (leftover diagnostic, still ACTIVE) | Supabase tools can't delete functions. Terrence clicks delete in the Supabase dashboard; Claude gives the exact steps. |
| 5 | **Stray `App.js` in this public website repo** (older BizHQ app copy) | Recommend removing it from the website repo. Needs Terrence's yes. |
| 6 | **Voice mic feature: `eas build --profile development --platform ios` fails.** The exact error was never captured. | Needs Claude running on Terrence's PC (Claude Desktop app, or `claude remote-control` in `C:\Users\teele\bizpilot`). Then Claude runs the build and reads the real error. |
| 7 | **`OPENAI_API_KEY` Supabase secret** for `bizhq-transcribe` | Unconfirmed. Check with a test call after the build works. |
| 8 | **Stripe is in LIVE mode.** Payment chain untested. | Terrence decides: small real charge, or a Stripe test key. |
| 9 | `sms_suppressions` table has RLS enabled but no policy | Needs Terrence's direction. |
| 10 | "Proposals label spacing" bug | Can't be seen in the code. Needs a screenshot from Terrence. |
| 11 | Supabase advisor housekeeping (unused indexes, duplicate permissive policies, `auth_rls_initplan`, leaked-password protection) | Low priority. |

---

## 5. Exact next step

**In a new Claude Code session on `teeleebiz-web/invoicespro-site`, Terrence types:**

continue the BizHQ plan

**Claude then:**
1. Reads this file.
2. Confirms `.claude/settings.json` loaded, meaning merges and live-site checks aren't blocked. If it didn't load, Claude says so plainly.
3. Does open item #1: writes `CLAUDE.md`, commits it, pushes it and merges it.
4. Does open items #2 and #3, verifies both are live, and updates this file's **Done** table.
