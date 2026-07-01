# Intelligence Room — build guide

How to reproduce this prospect "intelligence room" for the next prospect, fast.
The reference implementation is [`index.html`](./index.html) in this repo — a single,
self-contained file (all CSS/JS inline, fonts from Google Fonts). Clone it, swap the
prospect-specific content, redeploy. This guide marks exactly what's reusable vs. what
changes.

---

## 1. What the room is

A private, per-prospect page that delivers Impactable's free **Competitor Intel Report**
(two hand-built docs: a competitive white-space report + a signal-composed audience
worksheet) and shows the path to engage, styled to **blend into impactable.marketing**.

It's organized as four tabs (client-side view switching, no framework):

| Tab | What it holds | Reuse |
| --- | --- | --- |
| **Overview** | Hero, "what's in this room" cards linking to the other tabs, doctrine/resources | Structure reusable; copy per prospect |
| **Competitor Intel** | The full competitive white-space report (thesis, 10 competitors, matrix, white spaces, pains, targeting, hooks, takeaways) | Content is 100% per prospect |
| **Audience Worksheet** | Efficiency ladder, LinkedIn tiers, signal overlays, keyword architecture, negatives, budget, 90-day plan | Content is 100% per prospect |
| **Roadmap** | The path, the one-time launch strategy offer, ongoing management options, channel motions, supporting documents, pricing | Structure reusable; numbers/links per prospect |

**The 90% insight:** two inputs drive almost all the content — the **Competitive
White-Space report** and the **Audience Targeting Worksheet**. Produce those two for the
new prospect first, then this room is mostly a paste-and-relabel job.

---

## 2. Inputs to gather before you start

- Prospect **name**, primary **contact**, and **date**
- The **Competitive White-Space report** (10 competitors, matrix, 5 white spaces, hooks, takeaways)
- The **Audience Targeting Worksheet** (personas, signal overlays, keyword themes, negatives, budget, 90-day plan)
- The two **shareable Google Docs** (competitor intel + audience targeting) — **set link-sharing to "anyone with the link can view"** or the prospect can't open them
- The prospect's **Book-a-Call Landbot URL**
- **Pricing/discount** specifics (see §5)

---

## 3. Design system — keep it aligned with impactable.marketing

Fonts (already linked in `<head>`): **Archivo Black** (display), **Fraunces** (italic accents),
**Inter** (body), **JetBrains Mono** (labels).

Palette lives in `:root`. These must match the marketing site — pull the current values
from `https://impactable.marketing/pricing.html` (view source, copy its `:root`) if the
brand shifts. As of this build:

```
--asphalt:#0A0A0C; --concrete:#121217; --court:#1B1B22;
--line:rgba(244,241,234,0.10); --line-soft:rgba(244,241,234,0.06);
--bone:#F4F1EA; --chalk:#D8D4CB; --chalk-dim:#A8A49B;
--blue:#0099D1; --blue-soft:#5BB8E0; --canopy:#D4FF3D; --clay:#FFB627; --moss:#7BAA3E;
```

- **No navy.** The marketing site has none. Feature panels use the charcoal gradient
  `linear-gradient(160deg,#14141a 0%,#0c0c10 100%)`, not a navy gradient.
- Primary action color is **canopy** (`--canopy`); **blue** (`--blue`) is the secondary/brand accent.

---

## 4. Per-prospect swap checklist

Search `index.html` for each and replace:

- [ ] **Prospect name** — "Clear Raven" throughout (nav logo `.doc` label, hero copy, footers, meta)
- [ ] **Contact name** — "Luke Nelson" (Competitor Intel hero meta; worksheet budget note)
- [ ] **Dates** — "June 30, 2026" / "June 2026"
- [ ] **Hero copy** on each tab (thesis, positioning line, sub-copy)
- [ ] **Competitor Intel view** (`id="view-competitor"`) — replace the whole report body with the new prospect's white-space report
- [ ] **Audience Worksheet view** (`id="view-worksheet"`) — replace personas, signal table, keyword tables, negatives, budget splits, 90-day timeline (note the small JS data objects: `ladderData`, `budgetData`, `tlData` near the bottom)
- [ ] **Google Doc URLs** (2) — competitor intel doc + audience targeting doc (appear in the Competitor Intel view and the Roadmap "supporting documents")
- [ ] **Book-a-Call Landbot URL** — appears in the nav CTA and the Roadmap CTAs (search `landbot.online`)
- [ ] **Pricing + discount** on the Roadmap (see §5)
- [ ] **Channel links** — `linkedin-launch.html`, `google.html`, `pricing.html` usually stay (they're Impactable pages)

Everything else — the component CSS, the 4-tab nav, the `showView` JS, the panel layouts — carries over untouched.

---

## 5. Offer / pricing framing (keep these distinct)

Pulled from `impactable.marketing/pricing.html`. Do **not** conflate the one-time strategy
with ongoing management — an earlier draft did and it confused the offer.

- **One-time launch strategy** = Foundational Strategy, normally **$1,500**. If the free
  Competitor Intel Report (the two delivered docs) is already done, discount it — Clear
  Raven got **$999**.
- **Ongoing management** = from **$3,000/mo**:
  - One channel (LinkedIn *or* Google): up to **$15k/mo** ad spend
  - LinkedIn **+** Google (one system): up to **$10k/mo** combined
  - Rate flexes 15% → 7% of spend as budgets scale (custom/scale tier).
- Always link the full breakdown: `https://impactable.marketing/pricing.html`.

---

## 6. Gotchas we already solved (don't repeat them)

- **Link-styled `<button>` elements need `background:transparent`.** A `<button>` sharing a
  link's class inherits the browser's light-gray default and renders unreadable
  (canopy-on-gray). See `.rc-link`.
- **Never color text with `--court`** (`#1B1B22`) — it's invisible on the dark background.
  Use `--chalk-dim` for muted text/numerals (see `.take-n`).
- **No navy** — swap any navy gradients to the charcoal panel gradient.
- **Set the two Google Docs to "anyone with the link can view"** before handing off.
- Verify competitor-teardown specifics (tiers/prices) against each vendor's **current**
  pricing before anything goes into ad copy.

---

## 7. Deploy

It's one self-contained `index.html`. Give the new prospect their own repo + Vercel project
(same as this one), drop in the swapped file, and push. No build step.

---

## Fast path

1. Produce the two docs (white-space report + audience worksheet) for the new prospect.
2. Copy `index.html`, run the §4 checklist.
3. Paste the two reports into the Competitor Intel and Audience Worksheet tabs.
4. Update names, dates, Google Doc links, Landbot URL, and Roadmap pricing.
5. Confirm the palette still matches `pricing.html`; deploy.
