# Auralis AI — AI Data Automation Landing Page

A premium, high-converting, fully responsive SaaS landing page for **Auralis AI**, an AI-driven data
automation platform. Built to the PRD *"Next-Gen AI Platform Landing Page Speed Run"*.

> **Headline:** Automate Data Workflows with Next-Generation AI
> **Stack:** Vanilla JavaScript + Custom CSS (zero UI/animation dependencies)

---

## ✨ Highlights

| PRD Requirement | Implementation |
| --- | --- |
| **Matrix-driven pricing** | Multi-dimensional `pricingMatrix` (currencies × billing × tiers). No prices hardcoded in markup. |
| **Performance-isolated currency/billing switch** | Only price **text nodes** update on change. Cards are built once and never remounted — no global re-render or layout reflow. |
| **Annual 20% discount** | `annual.discount = 0.8` applied via `base × tariff × multiplier × discount`. |
| **Currencies** | INR `₹`, USD `$`, EUR `€` via tariff conversion. |
| **Bento → Accordion** | Desktop bento grid; mobile accordion. Both render from one shared `activeIndex`. |
| **State persistence on resize** | `activeIndex` is layout-independent — hovering a desktop card and resizing to mobile keeps the same panel open. |
| **No banned libraries** | No Shadcn / Radix / HeadlessUI / Framer Motion / Tailwind UI / runtime animation engines. |
| **Native motion only** | CSS transitions & keyframe animations. Entry sequence completes < 500ms. |
| **Semantic HTML** | `header`, `nav`, `main`, `section`, `article`, `footer` landmarks. |
| **SEO** | Title, meta description, viewport, Open Graph, Twitter Card, JSON-LD structured data. |
| **Accessibility** | Keyboard-operable controls, `aria-pressed` / `aria-expanded`, accessible names, reduced-motion support. |
| **Responsive** | 320px → 1440px+ with no horizontal overflow. |

---

## 🎨 Design Tokens

Defined as CSS variables in `:root`:

- **Font:** Manrope (400–800)
- **Palette:** Pumpkin `#fe7f2d` + Charcoal `#233d4d` (with derived shades), accent green `#4cc9a3`
- **Motion:** hover/toggle 180ms ease-out · layout 340ms ease-in-out · entry < 500ms

## 📊 Charts & data viz (all hand-built — no chart libraries)

Every visualization is drawn from scratch with SVG + CSS + a little vanilla JS, so no banned
dependencies are introduced:

- **Hero area chart** — smooth Catmull-Rom → Bézier curve with animated stroke draw + gradient fill
- **Line chart** — events processed vs. errors caught, dual series, animated on scroll
- **Donut** — % automated, animated `stroke-dashoffset` ring with counting label
- **Bar chart** — weekly hours saved per quarter, CSS height transition
- **Progress gauges** — customer-rating bars, animated width
- **Animated counters** — KPIs and metrics count up via `requestAnimationFrame` easing

Charts reveal lazily via `IntersectionObserver` so they never block Time to Interactive.

---

## 🚀 Run locally

It's a single static file — no build step.

```bash
# any static server works
npx serve auralis-ai
# or
python3 -m http.server 8000 --directory auralis-ai
```

Then open <http://localhost:8000>.

## ☁️ Deploy

Drop the folder on any static host (Vercel, Netlify, GitHub Pages, Cloudflare Pages).
The site is fully static, so deployment never returns 404/500.

```bash
# Vercel
vercel deploy auralis-ai --prod
# Netlify
netlify deploy --dir auralis-ai --prod
```

---

## 🧮 Pricing logic

```js
const pricingMatrix = {
  currencies: { INR:{symbol:'₹',tariff:1}, USD:{symbol:'$',tariff:0.012}, EUR:{symbol:'€',tariff:0.011} },
  billing:    { monthly:{multiplier:1,discount:1}, annual:{multiplier:12,discount:0.8} },
  tiers:      { starter:{baseRate:2999}, growth:{baseRate:7999}, enterprise:{baseRate:19999} }
};

function calculatePrice(tier, currency, billingCycle) {
  const base   = pricingMatrix.tiers[tier].baseRate;
  const tariff = pricingMatrix.currencies[currency].tariff;
  const cycle  = pricingMatrix.billing[billingCycle];
  return base * tariff * cycle.multiplier * cycle.discount;
}
```

Annual cards display the **effective monthly** price (`annualTotal / 12`) plus the billed-annually total.

---

## 📁 Structure

```
auralis-ai/
├── index.html      # entire app: markup + CSS + JS
├── package.json    # scripts only — zero runtime deps
├── README.md
├── LICENSE
└── .gitignore
```

## ✅ Dependency audit

`package.json` declares **no dependencies** — guaranteeing no banned UI/animation libraries are bundled.

## 📄 License

MIT
