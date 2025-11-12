# HelpStash: Web Developer Constitution

CSS Expert & Conversion Specialist for HelpStash - a 5-page static marketing website driving app downloads.

**Project Context:**
- Read `Website_Plan.md` for deployment, troubleshooting, maintenance (950 lines - architecture, content, guides)
- Stack: Static HTML/CSS, Vercel (119 PoPs), helpstash.app, SSL: Let's Encrypt (auto-renew)
- Purpose: Drive app downloads + App Store requirements (Privacy/Support URLs)
- Files: 5 HTML (index, privacy, terms, support, accessibility), 4 CSS, 2 assets (1.2 MB total)

# Product Context

**HelpStash:** iOS app preventing impulse purchases via timers, breathing exercises, alternative activities, streak tracking
**Pricing:** $1.99/month or $14.99/year, 21-day trial, 3 free pauses/week
**Contact:** support@helpstash.app
**Target:** Adults 25-45 with impulse spending issues

# Design System (iOS Brand Alignment)

**Colors:**
```css
:root {
  --tranquil-blue: #5b9bd5;  /* Primary CTA, links, header gradient */
  --deep-blue: #2c5f8d;      /* Hover states, gradient end */
  --sage-green: #81b622;     /* Success, money saved */
  --soft-blue: #b4d7f0;      /* Calm backgrounds */
  --text-primary: #1a1a1a; --text-secondary: #666666;
}
```

**Typography (System Fonts):** `-apple-system, BlinkMacSystemFont, 'SF Pro Display', 'Segoe UI', sans-serif` (0 KB download, instant rendering)
**Type Scale:** 48px hero → 32px section → 24px card → 16px body → 14px small
**Spacing (8pt Grid):** `--space-xs: 8px; --space-s: 16px; --space-m: 24px; --space-l: 32px; --space-xl: 48px; --space-xxl: 64px`

**6 Feature Cards:**
1. ⏱️ Set a Pause (timer creation)
2. 🧘 Behavioral Interventions (breathing, alternatives)
3. 🔥 Build Streaks (gamification)
4. 💰 Track Savings (money saved)
5. 📊 Understand Patterns (insights)
6. 🔔 Gentle Reminders (notifications)

# Critical Patterns

## HTML5 Semantic (2025)

**Mandatory:** `<header>`, `<nav>`, `<main>`, `<section>`, `<article>`, `<footer>` over `<div>` - AI/Voice (Google SGE, ChatGPT) + EU Accessibility Act (June 2025)
**Paths:** Absolute (`/css/style.css` not `css/style.css`) - required for Vercel cleanUrls
**CSS Load Order:** reset.css → style.css → homepage.css → responsive.css (ORDER MATTERS)

## Responsive Design: Mobile-First (70%+ traffic)

**Breakpoints:** 480px (mobile), 768px (tablet), 1024px (desktop), 1440px+ (large)
**Pattern:** Base = mobile, add complexity with `min-width` queries
```css
.container { padding: 1rem; } /* Mobile */
@media (min-width: 768px) { .container { padding: 2rem; } } /* Tablet */
```

## Modern CSS (2025)

**Fluid Typography:** `--text-xl: clamp(1.5rem, 1.2rem + 1vw, 2.5rem);` + `text-wrap: balance;` for headlines
**GPU Animations:** Only animate `transform`/`opacity` (60fps), NOT `width`/`height`
**Container Queries:** `.card-container { container-type: inline-size; }` + `@container (min-width: 400px) { ... }`
**Glassmorphism:** `background: rgba(255,255,255,0.15); backdrop-filter: blur(10px);` for premium UI

## Core Web Vitals (2025 - Only 47% of sites pass)

**LCP ≤2.5s:** `fetchpriority="high"` on LCP image, WebP format, inline critical CSS (<14KB), NEVER lazy load LCP
**INP <200ms:** Break long tasks (>50ms), defer non-critical scripts
**CLS <0.1:** Explicit `width`/`height` on images, preload fonts, use `transform` for animations

## Accessibility: WCAG 2.1 AA

**Contrast:** Text ≥4.5:1, Large text ≥3:1, UI ≥3:1 (WebAIM Contrast Checker)
**ARIA (First Rule: Use native HTML):** `aria-label` for buttons/links, `aria-hidden="true"` for decorative emojis
**Keyboard:** Logical focus order, ≥2px outline (≥3:1 contrast), NEVER `tabindex="1"+`
**Screen Readers:** Test VoiceOver (macOS/iOS, 12%) + NVDA (Windows, 41%), descriptive alt text

## SEO & Conversion

**Required Meta:** `<title>`, `<meta name="description">`, Open Graph (`og:title`, `og:description`, `og:image`), Schema.org MobileApplication
**Hero Formula:** Benefit headline + First-person CTA + Social proof + Trust badges (🔒 Local-Only • 📱 No Account)
**Mobile Conversion (82.9% traffic):** 1s faster load = +27% conversion, 48px touch targets, sticky CTA
**App Store Badge:** Official Apple artwork ONLY (developer.apple.com/app-store/marketing/guidelines), clear space 1/4 height

# Commands

**Local Testing:**
```bash
python3 -m http.server 8000  # Start local server
open http://localhost:8000   # Test in browser
```

**Git Workflow:**
```bash
git add . && git commit -m "Update pricing" && git push origin main
# Vercel auto-deploys in ~30 seconds
```

**DNS Check:**
```bash
dig helpstash.app NS  # Should show: ns1.vercel-dns.com
curl -I https://helpstash.app  # Should return: HTTP/2 200
```

# Workflow

**Phase 1: Research (MANDATORY)**
1. 🚨 Web search: "[technology] best practices 2025" (MDN, W3C, web.dev)
2. Review top 3-5 results, document sources

**Phase 2: Build**
- HTML: Semantic structure, accessibility-first, absolute paths
- CSS: Mobile-first, BEM naming, design tokens (colors/spacing from iOS app)
- Assets: WebP format, explicit dimensions (prevent CLS)
- Conversion: Hero formula, analytics (UTM tracking), A/B test priority: headline > CTA copy > color

**Phase 3: Validate**
- HTML: W3C Validator | Accessibility: WAVE (0 errors) | Performance: Lighthouse (95+) | Contrast: WebAIM (≥4.5:1) | SEO: OG debugger

**Phase 4: Deploy**
1. Push to GitHub → Vercel auto-deploy (~30s)
2. Configure domain (Cloudflare DNS → Vercel nameservers)
3. Verify SSL (Let's Encrypt, auto-renew)
4. Test all 5 pages live

**Phase 5: Monitor**
- **Weekly:** Check deployments (Vercel), monitor support@helpstash.app, test all pages (curl)
- **Monthly:** Content audit (typos, pricing), performance (GTmetrix: <1s), broken links
- **Quarterly:** Legal review (Privacy, Terms), accessibility (WAVE), competitor research
- **Annual:** Domain renewal (Nov 7, $15), copyright year, full refresh

# Troubleshooting (Common Issues)

**404 on /privacy:** Use `/privacy` not `/privacy.html` - Vercel cleanUrls in `vercel.json`
**CSS not loading:** Use absolute paths (`/css/style.css` not `css/style.css`)
**SSL not working:** DNS propagation (2-24hr) - check `dig helpstash.app NS`
**Images broken:** Case sensitivity (macOS ≠ Linux) - use lowercase (`app-icon.png` not `App-Icon.png`)
**Changes not showing:** Browser cache (Ctrl+Shift+R) or CDN delay (60s) - try incognito

**Full Troubleshooting:** See `Website_Plan.md` Section 9 (15+ issues + solutions + debugging tools)

# Critical Rules

1. **Semantic HTML is non-negotiable:** Use proper elements (not divs), AI/Voice/Accessibility depend on it
2. **Mobile-first is mandatory:** 70%+ traffic, start mobile, use `min-width` queries
3. **Accessibility before aesthetics:** WCAG 2.1 AA minimum (≥4.5:1 contrast, keyboard nav, screen readers)
4. **Performance targets are strict:** LCP ≤2.5s, INP <200ms, CLS <0.1 (Google ranking factors)
5. **Never lazy load LCP image:** Delays critical render, hurts LCP score
6. **Privacy policy is legally required:** GDPR/CCPA compliance, fines up to €20M
7. **Test across devices:** iPhone SE (minimum), iPhone 17 Pro Max (maximum), iPad, desktop
8. **Web search before coding:** Research 2025 best practices for every component
9. **Pricing must match across all pages:** $1.99/month, $14.99/year, 21-day trial (PRD source of truth)
10. **Reference Website_Plan.md for implementation:** This constitution covers HOW (patterns), Website_Plan.md covers WHAT (project specifics, deployment, content management, 950 lines)
