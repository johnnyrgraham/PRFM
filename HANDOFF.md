# PRFM Training — Site Handoff
**Version:** Launch-Ready Pre-Release  
**Built by:** Johnny Graham with Claude (Anthropic) across multiple sessions  
**Handed to:** Ben Thompson / PRFM Training

---

## Folder Structure

```
prfm-handoff/
├── index.html          ← The entire site. One file. Start here.
├── The PRFM Method - Athlete Mindset.pdf   ← Lead magnet PDF. Linked from the Typeform End Screen.
├── style/
│   ├── main.css        ← All styling. Edit here to change colours, fonts, layout.
│   └── main.js         ← Scroll animations, count-up stats, launch-day button swap.
├── images/
│   ├── logo-white.png              ← Nav + footer (on black backgrounds)
│   ├── logo-black.png              ← Orange strip + guarantee band (on orange)
│   ├── favicon-tile-black.png      ← Browser tab icon
│   ├── hero-athlete.jpg            ← Hero section background photo
│   ├── ben-lunge-event.jpg         ← Coach section photo (Ben competing)
│   ├── testimonial-josh.jpg        ← Josh P. headshot
│   ├── testimonial-sara.jpg        ← Sara headshot
│   ├── testimonial-liam.jpg        ← Liam F. headshot
│   └── [other Ben photos]          ← Unused for now, keep for future pages
└── HANDOFF.md          ← This file
```

> **All three files (index.html, style/main.css, style/main.js) plus the images/ folder must be in the same place for the site to work. Never move one without the others.**

---

## Deploying to Vercel

1. Push this entire folder to a GitHub repository (everything in it, including `images/`)
2. In Vercel, connect the repo. No build settings needed — it's a static site.
3. Vercel will serve `index.html` automatically from the root.
4. Every push to your main branch auto-deploys. That's it.

**Critical:** Vercel's servers are Linux and **filenames are case-sensitive**. `ben-lunge-event.jpg` and `Ben-Lunge-Event.jpg` are different files. The filenames in `index.html` and `main.css` must match the actual files in `images/` exactly.

---

## The One Thing to Do Before Launch: Wire the Forms

Every "Join Waitlist" / "Send me the guide" button on the page (nav, hero, orange strip, final CTA) opens the **same Typeform popup** — form ID `mMQa0Jt5` (`https://form.typeform.com/to/mMQa0Jt5`). There are no HTML `<form>` elements left on this page; Typeform handles the name + email collection entirely inside its own popup.

**How it's wired:** each button carries `data-tf-popup="mMQa0Jt5"` plus `data-tf-opacity` and `data-tf-iframe-props`. A single script tag near the bottom of `index.html` — `<script src="//embed.typeform.com/next/embed.js"></script>` — auto-detects any element with `data-tf-popup` and makes it clickable. No custom JavaScript required.

**If you change the Typeform form ID:** search `index.html` for `mMQa0Jt5` — it appears in all 4 buttons. Replace every instance.

**Sending the PDF ("The PRFM Method - Athlete Mindset.pdf"):**
The PDF lives at the root of the repo, alongside `index.html`. Once deployed, it's served at:
`https://yourdomain.com/The%20PRFM%20Method%20-%20Athlete%20Mindset.pdf`
(swap `yourdomain.com` for the real domain once live; the `%20`s are just spaces in the filename, which is normal).

To actually get that link in front of someone who signs up, add an **Ending** to the Typeform (Content panel → Endings → End Screen), and put the PDF link on the End Screen's button. Worth knowing: **linking that button to a custom URL is a Typeform Plus-plan-and-above feature** — on the free/Basic plan you can still customize the End Screen's text, but the button will link back to Typeform's own default page, not your PDF. Check current plan names/pricing at [typeform.com/pricing](https://www.typeform.com/pricing), as these change. If Ben doesn't want to pay for Plus, the fallback is to add the PDF link directly in the End Screen's body text (a plain link works on all plans) rather than as a styled button.

Test by clicking each of the 4 buttons on the live site, submitting the popup with a real name and email, and confirming the submission shows up in the Typeform dashboard (and the End Screen link, once set up, actually opens the PDF).

---

## Launch Day: Flipping All Buttons


Every "JOIN WAITLIST" button on the page will become "START NOW" or "APPLY NOW" in one edit.

Open `style/main.js`. At the very top, find:

```javascript
const LAUNCH_MODE = false;
```

Change `false` to `true`. Save. Push to GitHub. Done.

Every button on the page — nav, hero, orange strip, all 7 programme cards, final CTA — updates automatically. Each button already has its launch-day label stored in a `data-launch-text` attribute in the HTML.

To go back to waitlist mode, set it to `false` again.

---

## Things Not Yet Built (Open Items)

- **PDF delivery** — the site collects signups via Typeform, but the actual PDF file still needs a linked End Screen (requires Typeform Plus or above for a custom button link — see "Wire the Forms" above for the free-plan workaround).
- **Live spots counter** — the "100 spots" number is currently static, updated manually. A live counter requires tracking real submissions (e.g. via the Typeform dashboard) to know the real count.
- **Thank-you page / redirect** — after someone submits the waitlist form, they currently see the browser's default behaviour. Add a `_next` hidden input pointing to a thank-you URL once you have one.

---

## Common Edits

### Change a price
Open `index.html`. Find the programme by name (Base, Build, Strong, Capacity, Prime, Pro, Custom). Edit the `<div class="card-price">` block. Example:
```html
<div class="card-price">$149<span class="price-week">/mo · $35/wk</span></div>
```

### Change programme descriptions
Same location — the `<div class="card-detail">` inside each programme card.

### Update the "100 spots" number as your waitlist fills
Search `index.html` for `spots-count`. Every instance of the number is wrapped in `<span class="spots-count">100</span>` — update the number in each place, or remove the scarcity language entirely once the waitlist is closed.

### Add a testimonial
In `index.html`, find section 09 (search `FIELD REPORTS`). Copy one `<div class="quote">` block, paste it after the last one, update the photo src, name, journey line, and quote text. Add the client's photo to `images/` first.

### Change the hero photo
In `style/main.css`, find `.hero-visual` and update the `url('../images/...')` part of `background-image`. Keep the `linear-gradient(...)` in front of it — that's the orange overlay treatment.

### Change the coach photo
In `style/main.css`, find `.ben-photo` and update its `url('../images/...')` the same way.

### Change brand colours
Open `style/main.css`. At the very top, find `:root { ... }`. All colours are defined here:
```css
:root {
  --black:  #111111;
  --cream:  #F9F6F2;
  --white:  #FFFFFF;
  --orange: #FF8210;
}
```
Change a value here and it updates everywhere on the site instantly.

---

## Design System: Rules to Follow When Editing

These were deliberate decisions — breaking them will make the site look inconsistent.

**Colours:** Only ever use the four CSS variables above. Never hardcode a hex value anywhere.

**Fonts:** Three fonts, each with a specific job:
- `Inter` — all body text and headings
- `IBM Plex Mono` — labels, tags, prices, button text, anything monospaced
- `Fraunces italic` — pull quotes and testimonial blockquotes only

**Buttons:** All buttons have `border-radius: 6px`. If you add a new button, use one of the two existing classes:
- `.bracket-btn` — solid orange, black text. Primary CTAs.
- `.ghost-btn` — transparent, thin border. Secondary CTAs on light backgrounds.

**Sections alternate backgrounds** in this order: black → orange → cream → black → cream → orange → cream → black → cream → black → footer. Breaking that rhythm makes the page feel unbalanced.

**Orange accent:** Used for: brand colour, active state, price highlights, eyebrow labels, the orange gradient on photos. Not used for: body text, backgrounds (except the strip sections). Don't add more orange — the current balance is deliberate.

---

## How the "Join Waitlist" Buttons Work (Not All the Same)

Two different behaviours happen depending on which button someone clicks, by design:

1. **Nav bar, hero, orange strip, and final CTA** (4 buttons total) — each one opens the **same Typeform popup** directly via `data-tf-popup="mMQa0Jt5"`. No page navigation, no scrolling — the popup just appears on top of whatever section the person is looking at. This is the fast path: sign up without leaving where you are.
2. **All 7 programme card buttons** — plain anchor links (`href="#top"`) that scroll back up to the top of the page, where the nav bar's and hero's "Join Waitlist" buttons (both of which open the Typeform popup) are visible. They don't open the popup directly themselves.

If you add a new "Join Waitlist" button somewhere on the page, decide which of these two patterns it should follow rather than inventing a third. A new button that should let people sign up immediately should carry the same `data-tf-popup="mMQa0Jt5" data-tf-opacity="70" data-tf-iframe-props="title=PRFM Waitlist"` attributes as the 4 above. A new button further down the page (e.g. a future testimonial CTA) should probably just be `<a href="#top" class="...waitlist-cta" data-launch-text="...">Join Waitlist</a>`, matching the programme cards.

**Changing the Typeform:** search `index.html` for `mMQa0Jt5` (appears in all 4 popup-trigger buttons) and replace it if the form ID ever changes. The embed itself is powered by one script tag near the bottom of the page — `<script src="//embed.typeform.com/next/embed.js"></script>` — don't remove it or the buttons stop working.

---

## How the Scroll Animations Work

Every element with `class="reveal"` starts invisible and fades up when it enters the viewport. This is handled by `main.js` using an `IntersectionObserver` — no library needed.

To make something animate in on scroll, add `class="reveal"` to it.  
To stagger a group of items, wrap them in `class="reveal-group"` and add `style="--i:0"`, `--i:1"`, `--i:2"` etc. to each child.

The three stat numbers (17 years / 3 countries / 1000+ clients) count up from 0 when they scroll into view. To change a stat's final value, change `data-target="N"`. The `data-suffix="+"` attribute appends a symbol after the number finishes counting.

---

## Sections Reference

| # | Section | Background | Notes |
|---|---------|-----------|-------|
| 01 | Nav bar | Black | Sticky. Logo + links + CTA. |
| 02 | Hero | Black | Headline, sub-copy, hero photo, primary CTA. "Join Waitlist" opens the Typeform popup (see "How the Join Waitlist Buttons Work" below). |
| 03 | Waitlist strip | Orange | Button opens the Typeform popup. |
| 04 | Philosophy | Cream | Three pillars: Goal First, Built by Hand, Evolves with You. |
| 05 | How it works | Black | Five-step process. |
| 06 | Programme table | Cream | Two-row card grid. 7 programmes. Pro is the funnel target. |
| 07 | Guarantee | Orange | "It's coached" trust statement. |
| 08 | Coach Ben | Cream | Bio, stats, Ben's competition photo. |
| 09 | Testimonials | Black | Three real clients with photos. |
| 10 | Pull quote | Cream | Ben's founder quote. |
| 11 | Final CTA | Black | Button opens the same Typeform popup as sections 01/02/03. |
| — | Footer | Black | Logo + copyright. |


---

## For the LLM Helping with Future Edits

This site uses a deliberate, well-documented structure. Before making any change:

1. **Read the CSS comment above the rule you're changing.** Every section of `main.css` has a comment block explaining what it controls and what to watch out for.
2. **Read the HTML comment above the section you're changing.** Same — every section in `index.html` is documented.
3. **Never add inline styles except for `--i:N` stagger variables.** All styling belongs in `main.css`.
4. **Never add a `<style>` block inside `index.html`.** The CSS is fully external.
5. **Image paths in CSS use `../images/`** (one level up from `style/`). Image paths in HTML use `images/` (relative to root).
6. **The launch-day button swap is in `main.js` — `const LAUNCH_MODE = false`.** Changing this to `true` flips every CTA on the page. Don't modify individual button text manually.
7. **The programme table is NOT one grid — it's three independent tiers stacked in a flex column (`.prog-tiers`), each with its own sub-grid:** `.tier-entry-grid` (Base/Build, 2 equal columns), `.tier-goals-grid` (Strong/Capacity/Prime, 3 equal columns), and `.tier-access-grid` (Pro/Custom, 3fr + 2fr columns). Adding a card means adding it to the right tier's grid and adjusting that grid's own `grid-template-columns` — not tracking a single 5-column total across the whole table.
8. **All four "Join Waitlist" / "Send me the guide" buttons that open Typeform share one form ID (`mMQa0Jt5`) via `data-tf-popup`.** If you change the Typeform, update all four — search `index.html` for `mMQa0Jt5`.

---

*Best of luck Ben. Good luck with the launch.*
