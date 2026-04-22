# Afternoon Unfold — Session Working Doc

_Last updated: Apr 20–21, 2026 session handoff_

Paste this (or reference it) at the start of a new Claude session to pick up where we left off.

---

## 1. Live state (what's currently shipped)

- **Domain:** https://www.afternoon-unfold.com is **live** (Cloudflare DNS → GitHub Pages → org repo `Afternoon-Unfold/afternoon-unfold`).
- **Main file:** `/Users/zoraliu/Desktop/web/index.html` (previously `afternoon-unfold-v15.html`; renamed so bare domain loads pre-sale directly).
- **Previous stable backup:** `/Users/zoraliu/Desktop/web/afternoon-unfold-v14.html` (frozen, do not edit).
- **Repo:** public today. **Pending:** migrate to Cloudflare Pages so repo can go private without losing hosting (GitHub Pages requires public on free tier).
- **Videos compressed via HandBrake (Fast 720p30):** total 97.6 MB → 16.9 MB.
  - `wave-survey-bg.mp4` 69 MB → 7.5 MB
  - `au-newspaper-v2.mp4` 12 MB → 2.7 MB
  - `cover.mp4` 9.6 MB → 2.9 MB
  - `unbox.mp4` 7 MB → 3.8 MB
  - Deleted: `palmtree-moves.mp4` (25 MB), `au-newspaper.mp4` (12 MB), `cover.mov` (12 MB), `unbox.mov` (16 MB).

---

## 2. Major v15 changes locked in this session

### Quiz restructure (Q1)
- New Q1: **"Hello! Who's this magical box for?"** with two chips: **Myself** / **Someone Else**.
- When **Myself** → copy uses *your / you*. When **Someone Else** → name field captures recipient, copy uses *`[Name]'s`*.
- Tarot question (Q7) is **skipped when gifting** — progress counter drops from 8 → 7 dynamically.
- Single source of truth: `refreshPersonalization()` rewrites Q1, Q4, Q5, Q6, Q7 strings and clears `dataset.twOrig` + `twDone[n]` so typewriter re-runs with new copy.
- Gift toggle on final slide simplified to only capture a **handwritten note** (recipient name already captured in Q1).

### Copy / microcopy
- Tarot heading: "Pick the card that calls for you" — `" for you"` suffix dropped in self mode; desktop gets `max-width:780px` + `white-space:nowrap` on `#slide-7 .quiz-inner` to prevent 2-line wrap.
- Reading card now tells users the **full reveal is printed inside the physical box**.
- "Your reading gets deeper with every answer" moved to its own line.

### International zip handling
- `zippopotam.us` lookup on zip entry.
- If **non-US**: reservation CTA swaps to email-only mailing list signup with copy:
  > "Drop your email and we'll be in touch when we open international shipping."

### Prize modal
- Fixed iOS Safari click bubbling: added `pointer-events:none` on `.pf-icon`, `.pf-icon *`, `.pf-label` so the parent `onclick="openPrizeModal()"` fires reliably. (Commit `ea1a748`.)
- Prize icon: switched to round question-mark glyph, bigger darker shadow; label "prize" → **bold, non-italic, rosewood**.
- Prize copy: *"Reserve one of the first 50 boxes to win a premium matcha ritual set"* + matcha image.

### Video autoplay fixes
- `IntersectionObserver` calls `.play()` on below-fold videos when they scroll into view.
- **Excludes** `#quiz-video-bg` and `#loading-video` to avoid conflicts with `openQuiz()` explicit play.
- `openQuiz()` also calls `wv.load()` + 250 ms retry as belt-and-suspenders against race conditions.

### Start Over bug
- `restartQuiz()` was throwing `TypeError` on a removed `#input-notes` textarea. All `getElementById` calls now guarded with `if(el)` and iterated defensively.

### Mobile responsive (mobile-only — desktop unchanged)
```css
@media(max-width:768px){
  #main-nav{height:50px;padding:0 2rem;}
  .nav-logo{gap:10px;}
  .nav-logo-img{height:28px;width:28px;}
  .nav-logo-main{font-size:15px;}
  .nav-logo-sub{font-size:8px;}
  .nav-cta{font-size:10.5px;letter-spacing:0.14em;padding:8px 16px;}
  section{padding-top:50px;}
  #cover{padding-top:50px;}
}
```
- Moved inline `<span>` styles into `.nav-logo-main` / `.nav-logo-sub` classes so the media query wins cleanly.
- Verified via Claude Preview at 375×812: nav adapts, quiz scrolls, Q7 tarot row with 8 cards overflows horizontally but Continue button reachable, result page stacks to 1 column, prize float 60×60, reservation modal 327×730 fits, intl modal 327×466 fits.

---

## 3. Strategy decisions locked in

### Domain ownership (currently on boyfriend's Cloudflare)
Multi-step plan that accommodates the 60-day ICANN transfer lock + relationship dynamics:
1. **Venmo ~$12 now** to close the financial ledger.
2. **Wait until June** when the 60-day lock lifts.
3. Frame the ask as an **external requirement** — Wharton Venture Lab needs her to prove domain ownership / LLC alignment.
4. Do an **intra-Cloudflare account transfer** (no registrar move, no downtime, no AUTH code).
5. Eventually register the domain under the **LLC** once formed.
6. If he pushes back, fallback is "add me as Administrator" — but LLC-in-her-name remains the long-game.

### GitHub private repo path
- Free GitHub Pages → public only.
- Plan: **migrate hosting to Cloudflare Pages** (free, private repo supported), keep Cloudflare DNS pointing at the new Pages deployment, then flip the GitHub repo to private.

### F1 / business operation reality (important constraint)
- F1 prohibits **active self-employment** in the US. She's in China until July, so sole-proprietor sales are fine pre-arrival.
- Once on F1 (Aug 2026, Wharton), options:
  - **(Preferred) CPT** via Wharton Venture Lab / entrepreneurship course (MGMT 891 etc.) — work authorization tied to her own venture.
  - Boyfriend (green card holder) runs US-side ops as **paid 1099 contractor** while she stays a passive owner.
  - Pause until OPT — bad option, loses momentum.
- **H1B correction (from earlier assistant overclaim):** her prior H1B (Dec 2022 – June 2023, ~7 months at Goldman NY) + >3 years outside US means 8 CFR 214.2(h)(8)(ii)(B) cap-exempt does **not** apply. She'll re-enter the lottery for Summer 2027 IBD internship / Summer 2028 full-time, but the ~5.5 years of remaining H1B time is preserved once selected.
- Finance major = STEM-designated → **36 months OPT** post-graduation. LLC should enroll in **E-Verify** to qualify for STEM OPT extension employer status.
- Penn ISSS email sent this session — awaiting response (3–7 business days).

### Sourcing narrative (Yiwu)
Never mention Yiwu / wholesale / China publicly. Practical playbook:
- Turn off geotagging on all posts/stories.
- Shoot close-ups / "could-be-anywhere" frames.
- Remove Chinese packaging before shipping; replace with branded sleeves, wax seals, hand-numbered cards.
- Language: "curated from trusted makers" / "hand-selected by the founder" / "sourced worldwide."
- Invest in premium own-branded packaging — that's the perceived-value lever, not the raw item cost.

### Ad spend readiness
- Hold on paid ads until: (1) domain + analytics live, (2) Meta Pixel installed, (3) at least 10 organic posts up with one that's performing, (4) IG bio points to `afternoon-unfold.com`.
- Start with **$10–15/day test budget** against the best organic post once above are true.
- "50 boxes" framing is intentional scarcity for interest-gauging — not a hard inventory cap.

---

## 4. Pending tasks (ordered by priority)

### This week (before Yiwu trip Thu Apr 24)
- [ ] **Venmo boyfriend ~$12** for the domain (closes the ledger before the ask).
- [ ] (Optional) Install Meta Pixel + Cloudflare Web Analytics if time permits.

### Yiwu trip (Thu Apr 24 – weekend)
- Sourcing.

### Immediately after Yiwu
- [ ] Request Cloudflare Admin access from boyfriend (soft step before the full transfer ask).
- [ ] Install **Meta Pixel** + **Cloudflare Web Analytics**.
- [ ] Update **IG bio** with `afternoon-unfold.com`.
- [ ] Apr 28+: start $10–15/day ad test on best-performing organic post.

### Round 1 launch
- [ ] **May 1** — Round 1 opens.
- [ ] Bump `PRE_SALE_RESERVED_BASE` each morning during the window.

### Mid-May onward
- [ ] ISSS response received → **form LLC** (PA, single-member).
- [ ] Migrate hosting to **Cloudflare Pages**; flip GitHub repo to **private**.

### June
- [ ] 60-day ICANN lock lifts → request domain transfer citing **Wharton Venture Lab** requirement.
- [ ] Transfer domain to **LLC name**.
- [ ] Enroll LLC in **E-Verify** (future STEM OPT employer requirement).

### July
- [ ] **July 4** — ship Founding 50 (boyfriend handles US-side packing from CA).

### August 2026
- [ ] Enter US on F1 for Wharton; arrange CPT via Venture Lab.

---

## 5. Key business numbers (frozen)

- Each craft: **$40** (`CRAFTS` array).
- Curation fee: **$15** (`CURATION_FEE`).
- Bundle discount ladder: 2→10%, 3→15%, 4→20%, 5+→25%.
- Pre-sale code: `UNFOLD15` (15% off, 50 redemptions max).
- Free US shipping baked in; US-only audience for Round 1.
- 1 craft retail: **$55** / with UNFOLD15: **$47**.

## 6. Key integrations

- **Formspree:** `https://formspree.io/f/xaqaoebr` → `zoraliu97@gmail.com`. Honeypot `_gotcha` on reservation form. ASCII-only `_subject` prefixes (no emoji → spam).
- **Stripe:** 5 test Payment Links (qty 1–5). Not wired into pre-sale. Switches on post-July-4. TD Bank payout login still needs resolving.
- **GitHub auth:** classic PAT "Afternoon Unfold Mac 2" (scope `repo`, expires Jul 20 2026), stored in macOS Keychain. Pushes silent.

## 7. Code landmarks in `index.html`

- Pre-sale config: `PRE_SALE_TOTAL` / `PRE_SALE_START` / `PRE_SALE_END` / `LAUNCH_DATE` / `PRE_SALE_RESERVED_BASE`.
- Personalization: `refreshPersonalization()`, `pickWhoFor()`, `titleCaseName()`.
- Slide machine: `goToSlide(n)` (handles tarot skip + counter math).
- Quiz data: `var CRAFTS = [` / `var SCENES = {`.
- Bundle ladder: search `discountPct=0.25`.
- Reservation modal: `id="reserve-modal"`.
- Welcome popup: `id="welcome-popup"` (2 s auto-open, 7-day localStorage dismiss).
- Reservation success full page: `id="reserved-page"`.
- Sticky banner: `id="presale-banner"` (hides when `body.overlay-active`).
- Video autoplay: `IntersectionObserver` block; excludes `#quiz-video-bg` + `#loading-video`.

## 8. UX rules (do not violate)

- **No italic fonts.** Use rosewood `#B94F63` for emphasis.
- **No dark buttons.** All CTAs: rosewood gradient pill `linear-gradient(90deg,#8E3A4E,#B94F63)`, `border-radius:999px`.
- **No "Surprise me" scene chip** on Q1 (the blind box IS the surprise).
- **"It's a gift!" chip** stays inside the main 2×3 grid on Q1.
- **30-second promo modal** suppressed during Round 1 (avoid double email-capture).
- **Box # badge** on reservation success: 140 px, wax-seal gold gradient.
- **Full-page** reservation success (not popup), with "Back to main page" button.
- **Flip-digit count-up** on welcome popup stats.
- **Floating IG + TikTok** icons on right edge, scroll-responsive.

## 9. Open questions / things to revisit

- Final call between Formspree Personal ($10/mo autoresponder) vs Zapier Gmail trigger vs manual replies for first ~20 reservations.
- Nurture email sequence (May 10 close → July 4 launch): 5 emails, not written yet.
- Content calendar: 12 posts between now and May 1 launch.
- When / whether to retire v14 backup file.

## 10. Sensitive / do-not-share notes

- Yiwu sourcing is **never** mentioned publicly.
- Domain is currently registered to boyfriend; framing for the ask is **Wharton Venture Lab requirement**, not a trust issue.
- LLC formation is **intentionally timed** to follow ISSS guidance — don't form before the response comes back.

---

_End of working doc. Keep this file alongside `SESSION_CONTEXT.md` (which stays focused on the static project facts) — this one tracks the moving parts._
