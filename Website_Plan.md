# HelpStash Website Developer Guide

**Last Updated:** November 12, 2025
**Version:** 1.1
**Repository:** helpstash-website (public, separate from iOS app)
**Production URL:** https://helpstash.app (deployment pending)
**Status:** Production-ready with Coming Soon App Store badge

---

## Project Context

### HelpStash iOS App
**Purpose:** Prevent impulse purchases through behavioral interventions
**Target Users:** Adults 25-45 with impulse spending issues
**Core Features:** Timer-based pauses, breathing exercises (4-7-8 technique), alternative activities, streak tracking, money saved visualization
**Platform:** iOS 18.0+, Swift 6.2, SwiftData, Live Activities, StoreKit 2
**Business Model:** Freemium - 3 free pauses/week, $1.99/month or $14.99/year premium, 21-day trial

### Website Purpose
**Primary:** App Store compliance (Privacy Policy + Support URL required)
**Secondary:** Marketing homepage to drive app downloads
**Content:** 5 pages (index, privacy, terms, support, accessibility)
**Tech:** Static HTML/CSS, no JavaScript, no build process

### Repository Separation
**HelpStash** (iOS app) → Private repo, Swift/SwiftUI, App Store deployment
**helpstash-website** (this) → Public repo, HTML/CSS, Vercel deployment

**Why Separate:**
- Different tech stacks (Swift vs HTML/CSS)
- Different deployment pipelines (App Store vs Vercel)
- Different skill sets (iOS dev vs web dev)
- Independent versioning and release cycles
- Simpler CI/CD management

### Tech Stack
**Frontend:** Static HTML5 + CSS3 (no frameworks, no build process)
**Hosting:** Vercel (FREE tier, unlimited bandwidth, 119 global edge locations)
**Domain:** Cloudflare (helpstash.app, $15/year, purchased Nov 7, 2025)
**SSL:** Let's Encrypt (automatic via Vercel, auto-renews every 90 days)
**Version Control:** GitHub (helpstash-website public repository)

**Why Static:**
- Zero cost hosting (only domain: $15/year)
- Instant page loads (<1s)
- Maximum reliability (99.99% uptime)
- No security vulnerabilities (no server-side code)
- Simple maintenance (HTML/CSS only)

### File Structure
```
HelpStash_Website/
├── index.html              # Homepage (hero + 6 features)
├── privacy.html            # Privacy Policy (GDPR/CCPA compliant)
├── terms.html              # Terms of Service
├── support.html            # 17 FAQ questions
├── accessibility.html      # WCAG 2.1 AA statement
├── css/
│   ├── reset.css          # Browser normalization
│   ├── style.css          # Design tokens + global styles
│   ├── homepage.css       # Homepage-specific styles
│   └── responsive.css     # Mobile breakpoints
├── assets/
│   ├── app-icon.png           # 1024×1024px app icon (original)
│   ├── app-icon-optimized.png # 248KB optimized version (used in HTML)
│   ├── app-store-badge.svg    # Official Apple badge
│   ├── favicon.ico            # Browser tab icon
│   ├── favicon-*.png          # Multiple sizes (16px, 32px)
│   ├── apple-touch-icon.png   # iOS home screen icon
│   ├── android-chrome-*.png   # Android icons (192px, 512px)
│   ├── Screenshots/           # Local-only (gitignored)
│   └── Screengrabs/           # Local-only (gitignored)
├── vercel.json            # Deployment config (cleanUrls, security headers)
├── .gitignore             # macOS, logs, local screenshots
├── server.js              # Local testing server (Node.js)
├── README.md              # Quick deploy instructions
├── Website_Plan.md        # This document
└── CLAUDE.md              # Web developer constitution (patterns, best practices)
```

**Total Size:** ~1.6 MB (20+ files excluding gitignored screenshots)
**Load Time:** <1 second (actual page load ~500KB with optimized images)

**Note:** Screenshots in `assets/Screenshots/` and `assets/Screengrabs/` folders (~12.7 MB) are excluded from version control via .gitignore and kept locally for reference only.

---

## 1. Quick Start (5 Minutes)

### Prerequisites
✅ Domain purchased: helpstash.app (Cloudflare, Nov 7, 2025)
- GitHub account (free)
- Code editor (VS Code recommended)
- Git installed
- Terminal access

### Local Testing
```bash
# Navigate to directory
cd /Users/gurindersingh/Documents/Cursor/HelpStash_Website

# Start local server (choose one)
python3 -m http.server 8000      # Python (pre-installed macOS)
node server.js                    # Node.js (if installed)

# Open browser
open http://localhost:8000

# Test all pages
open http://localhost:8000/privacy
open http://localhost:8000/terms
open http://localhost:8000/support
open http://localhost:8000/accessibility
```

### Quick Edits
**Update pricing:** Find/replace `$1.99` across all files (index, terms, support)
**Add FAQ:** Copy `<details>` block in support.html, update question/answer
**Update features:** Edit feature cards in index.html (6 cards, emoji + title + description)

---

## 2. Design System (HelpStash Brand)

### Colors (from iOS App)
```css
:root {
  --tranquil-blue: #5b9bd5;  /* Primary CTA, links, header gradient */
  --deep-blue: #2c5f8d;      /* Gradient end, hover states */
  --sage-green: #81b622;     /* Success, money saved */
  --soft-blue: #b4d7f0;      /* Calm backgrounds */
  --text-primary: #1a1a1a;   /* Body text */
  --text-secondary: #666666; /* Captions, secondary text */
}
```

### Typography
**Font Stack:** `-apple-system, BlinkMacSystemFont, 'SF Pro Display', 'Segoe UI', sans-serif` (system fonts, 0 KB download)
**Type Scale:** 48px hero → 32px section → 24px card → 16px body → 14px small
**Mobile:** Hero reduces to 32px on <768px screens

### Spacing (8pt Grid)
```css
--space-xs: 8px; --space-s: 16px; --space-m: 24px;
--space-l: 32px; --space-xl: 48px; --space-xxl: 64px;
```

### 6 Feature Cards (Homepage)
1. ⏱️ **Set a Pause** - Timer creation before purchase decisions
2. 🧘 **Behavioral Interventions** - Breathing exercises, alternative activities
3. 🔥 **Build Streaks** - Gamification, consecutive-day tracking
4. 💰 **Track Savings** - Money not spent visualization
5. 📊 **Understand Patterns** - Spending trigger insights
6. 🔔 **Gentle Reminders** - Timer completion notifications

---

## 3. Architecture Overview

### HTML Structure
**All pages follow this pattern:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>[Page Title] - HelpStash</title>

    <!-- Stylesheets (ORDER MATTERS) -->
    <link rel="stylesheet" href="/css/reset.css">      <!-- 1. Reset -->
    <link rel="stylesheet" href="/css/style.css">      <!-- 2. Global -->
    <link rel="stylesheet" href="/css/homepage.css">   <!-- 3. Page-specific -->
    <link rel="stylesheet" href="/css/responsive.css"> <!-- 4. Mobile -->
</head>
<body>
    <header>
        <nav>
            <a href="/" class="brand">HelpStash</a>
            <a href="/privacy">Privacy</a>
            <a href="/terms">Terms</a>
            <a href="/support">Support</a>
        </nav>
    </header>

    <main>
        <!-- Page content -->
    </main>

    <footer>
        <p>&copy; 2025 HelpStash. All rights reserved.</p>
        <nav>
            <a href="/privacy">Privacy</a>
            <a href="/terms">Terms</a>
            <a href="/support">Support</a>
            <a href="/accessibility">Accessibility</a>
        </nav>
    </footer>
</body>
</html>
```

**Critical:** Use absolute paths (`/css/style.css` not `css/style.css`) for Vercel cleanUrls

### CSS Architecture (4 Layers)
1. **reset.css** - Browser normalization (Meyer Reset)
2. **style.css** - Design tokens (`:root` variables) + global styles
3. **homepage.css** - Page-specific components (hero, feature cards)
4. **responsive.css** - Mobile overrides (`@media (max-width: 768px)`)

### Vercel Configuration (`vercel.json`)
```json
{
  "version": 2,
  "cleanUrls": true,           // /privacy not /privacy.html
  "trailingSlash": false,      // /privacy not /privacy/
  "headers": [
    {
      "source": "/(.*)",
      "headers": [
        {"key": "X-Content-Type-Options", "value": "nosniff"},
        {"key": "X-Frame-Options", "value": "DENY"},
        {"key": "X-XSS-Protection", "value": "1; mode=block"}
      ]
    }
  ]
}
```

### No JavaScript Philosophy
**Why:** Instant loading, accessibility, security, simplicity, reliability
**Interactivity:** FAQ accordions use native `<details>` HTML element (no JS needed)

---

## 4. Page Reference

### Homepage (`index.html`)
**Sections:** Hero (headline + subheadline + App Store badge) + Features (6 cards) + Footer
**Hero Headline:** "Pause Impulse Purchases"
**CTA:** Official App Store badge with "Coming Soon" overlay (placeholder state)
**Current Status:** Badge displays with 60% opacity, grayscale filter, and "Coming Soon" pill overlay - not clickable until app approved
**Update When:**
- After App Store approval: Remove `.coming-soon` class and `<span class="coming-soon-pill">` element from index.html:58-60
- Update href from `#` to actual App Store URL: `https://apps.apple.com/app/helpstash/[APP_ID]`
- Test button is clickable and opens App Store listing
- Also update: Pricing changes, new features added

### Privacy Policy (`privacy.html`)
**Key Sections:** Data Collection, Data Usage, Data Sharing, User Rights (GDPR/CCPA), Contact
**Critical Points:** No PII collected, local-only storage, no analytics, no servers
**Update When:** Data practices change (rare), GDPR/CCPA regulations update
**Legal:** Last Updated date at top, must be live before App Store submission

### Terms of Service (`terms.html`)
**Key Sections:** Service Description, Subscription Terms (pricing), Disclaimers (no medical advice), Limitation of Liability, Contact
**Critical Clauses:** Pricing ($1.99/mo, $14.99/yr, 21-day trial), Auto-renewal, No medical advice disclaimer
**Update When:** Pricing changes, subscription terms change, policy updates
**Legal:** Last Updated date, material changes require user notification

### Support Page (`support.html`)
**Structure:** Contact info (support@helpstash.app) + 17 FAQ questions (5 Getting Started, 5 Using App, 4 Subscription, 3 Troubleshooting)
**FAQ Format:** Native `<details>` HTML element for accordions
**Update When:** User sends same question 3+ times, new feature launches, App Store review mentions confusion
**Add FAQ:** Copy existing `<details>` block, update `<summary>` (question) and `<p>` (answer)

### Accessibility Statement (`accessibility.html`)
**Key Sections:** Our Commitment, WCAG 2.1 AA Conformance, Website Features (semantic HTML, keyboard nav, contrast), iOS App Features (VoiceOver, Dynamic Type, Reduce Motion), Feedback
**Update When:** New accessibility features added, WCAG guidelines update, user reports issue
**Testing:** VoiceOver (Cmd+F5), keyboard-only navigation, 200% zoom, color contrast checker

---

## 5. Deployment Guide

### Pre-Deployment Checklist
- [ ] All HTML files validated (no syntax errors)
- [ ] CSS loads correctly (check browser DevTools)
- [ ] All links work (navigation + email)
- [ ] Images load (app-icon.png, badge.svg)
- [ ] Mobile responsive (test real device or DevTools)
- [ ] Pricing correct ($1.99/mo, $14.99/yr, 21-day trial)
- [ ] Email correct (support@helpstash.app)
- [ ] Domain purchased (helpstash.app ✅)

### Step 1: GitHub Repository Setup

**Create Repository:**
1. github.com → New repository
2. Name: `helpstash-website`
3. Visibility: **Public** (required for Vercel free tier)
4. Description: "Official website for HelpStash iOS app"
5. **Don't** initialize with README (already exists)

**Push Code:**
```bash
cd /Users/gurindersingh/Documents/Cursor/HelpStash_Website
git init
git add .
git commit -m "Initial commit: HelpStash website"
git branch -M main
git remote add origin https://github.com/[USERNAME]/helpstash-website.git
git push -u origin main
```

**Verify:** github.com/[USERNAME]/helpstash-website shows all 14 files

### Step 2: Vercel Deployment

**Create Account:**
1. vercel.com → Sign Up
2. **Choose: "Continue with GitHub"**
3. Authorize Vercel to access repositories

**Import Project:**
1. Dashboard → "Add New..." → "Project"
2. Find `helpstash-website` → "Import"
3. **Configure:**
   - Framework Preset: **Other** (static HTML)
   - Root Directory: `.` (default)
   - Build Command: (leave empty)
   - Output Directory: (leave empty)
   - Install Command: (leave empty)
4. Click **"Deploy"**

**Result:** Site live at `https://helpstash-website-[random].vercel.app` in ~30 seconds

**Test:** Visit Vercel URL, check all 5 pages load, CSS applied, mobile responsive

### Step 3: Custom Domain Configuration

**Add Domain in Vercel:**
1. Project → Settings → Domains → "Add"
2. Enter: `helpstash.app` (no www, no https://)
3. Click "Add"
4. **Select: "Use Vercel Nameservers"** (simplest method)
5. Copy 2 nameservers shown:
   ```
   ns1.vercel-dns.com
   ns2.vercel-dns.com
   ```

**Update Cloudflare:**
1. dash.cloudflare.com → Domain Registration → Manage Domains
2. Click helpstash.app → Nameservers → "Change"
3. Remove existing: `ns1.cloudflare.com`, `ns2.cloudflare.com`
4. Add Vercel: `ns1.vercel-dns.com`, `ns2.vercel-dns.com`
5. Save (warning about CDN is expected)

**Wait for DNS Propagation:** 2-24 hours (usually <2 hours)

**Check Progress:**
```bash
dig helpstash.app NS
# Expected: ns1.vercel-dns.com, ns2.vercel-dns.com
```

**Vercel Status:** Settings → Domains → Status changes "Pending" → "Active"

**Add WWW Redirect (Optional):**
1. Vercel → Settings → Domains → "Add"
2. Enter: `www.helpstash.app`
3. Choose: **"Redirect to helpstash.app"**
4. Type: **Permanent (301)**

### Step 4: SSL Verification

**Once Domain Status = "Active":**
1. Visit `https://helpstash.app`
2. Check padlock icon 🔒 in address bar
3. Certificate details:
   - Issued to: helpstash.app
   - Issued by: Let's Encrypt
   - Valid: 90 days (auto-renews)

**Test Redirects:**
```bash
curl -I http://helpstash.app
# Should redirect to https://helpstash.app

curl -I https://www.helpstash.app
# Should redirect to https://helpstash.app
```

### Step 5: App Store Connect Integration

**CRITICAL: Required for App Store submission**

1. appstoreconnect.apple.com → My Apps → HelpStash
2. App Information → URLs section:
   - **Privacy Policy URL:** `https://helpstash.app/privacy`
   - **Support URL:** `https://helpstash.app/support`
3. Click "Save"

**Verify:** Click each URL in App Store Connect to test (Apple reviewers will check)

### Continuous Deployment

**How it works:**
1. Edit files locally
2. Commit: `git add . && git commit -m "Update pricing"`
3. Push: `git push origin main`
4. Vercel auto-deploys in ~30 seconds
5. Live site updates at helpstash.app

**Rollback:** Vercel Dashboard → Deployments → Previous deployment → "Promote to Production"

---

## 6. Content Management

### Update Pricing
**Files:** index.html, terms.html, support.html
**Find/Replace:** `$1.99` → new price, `$14.99` → new annual price
**Update:** "Last Updated" date in legal pages
**Commit:** `git commit -m "Update pricing to $2.99/month"`

### Add FAQ Question
**File:** support.html
**Copy this block:**
```html
<details>
    <summary>Your new question here?</summary>
    <p>Your answer (2-3 sentences max).</p>
</details>
```
**Place:** In relevant section (Getting Started, Subscription, Troubleshooting)

### Update Features (Homepage)
**File:** index.html
**Add feature card:**
```html
<div class="feature-card">
    <div class="feature-icon">🎯</div>
    <h3>Feature Title</h3>
    <p>One-sentence description (15-20 words).</p>
</div>
```

### Update Legal Pages
**When:** Privacy = data practices change; Terms = pricing/policy changes
**Process:** Edit content → Update "Last Updated" date → Commit
**Notify Users:** If material changes (pricing increases, liability changes), notify via app update notes

### Activate App Store Download Button
**When:** App approved and live on App Store
**Files:** index.html (line 58), homepage.css (no changes needed)
**Steps:**
1. Open index.html, find line 58: `<a href="#" class="app-store-badge coming-soon">`
2. Replace `href="#"` with actual App Store URL: `href="https://apps.apple.com/app/helpstash/[APP_ID]"`
3. Remove `.coming-soon` class: change to `class="app-store-badge"`
4. Delete lines 60: `<span class="coming-soon-pill">Coming Soon</span>`
5. Update aria-label: remove "- Coming Soon" text
6. Test locally: Button should be fully opaque, colored, and clickable
7. Commit: `git commit -m "Activate App Store download button"`
8. Push to deploy

**Result:** Button becomes fully visible, clickable, opens App Store listing

---

## 7. Maintenance Schedule

### Weekly
- Check Vercel deployments (green checkmark = success)
- Monitor support@helpstash.app (respond within 24 hours)
- Test pages: `curl -I https://helpstash.app` (should return HTTP/2 200)

### Monthly
- Content audit (typos, pricing accuracy)
- Performance test (gtmetrix.com: <1s load time)
- Broken link check (deadlinkchecker.com)

### Quarterly
- Legal page review (Privacy, Terms)
- Accessibility audit (wave.webaim.org: 0 errors)
- Competitor research (update features if needed)

### Annual
- Domain renewal (Nov 7, Cloudflare auto-renews, $15)
- Copyright year update (footer: `© 2026 HelpStash`)
- Full content refresh (rewrite copy, update screenshots)

---

## 8. Troubleshooting

### 404 on /privacy
**Cause:** File paths or Vercel config
**Fix:** Use `/privacy` not `/privacy.html`, verify `vercel.json` has `"cleanUrls": true`

### CSS Not Loading
**Cause:** Relative paths
**Fix:** Use `/css/style.css` (absolute) not `css/style.css` (relative)

### SSL Not Working
**Cause:** DNS not propagated or domain not verified
**Fix:** Check `dig helpstash.app NS` shows `ns1.vercel-dns.com`, wait 24 hours, click "Refresh" in Vercel

### Images Broken
**Cause:** Case sensitivity (macOS ≠ Linux)
**Fix:** Use lowercase filenames (`app-icon.png` not `App-Icon.png`)

### Changes Not Showing
**Cause:** Browser cache or CDN delay
**Fix:** Hard refresh (Ctrl+Shift+R / Cmd+Shift+R), try incognito, wait 60 seconds for CDN

### DNS Not Resolving
**Cause:** Propagation delay
**Fix:** Wait 2-24 hours, clear DNS cache: `sudo dscacheutil -flushcache && sudo killall -HUP mDNSResponder` (macOS)

### Deployment Failed
**Cause:** Build error or config issue
**Fix:** Vercel Dashboard → Deployments → Click failed deployment → Check "Build Logs", fix error, push again

### Domain Shows Wrong Site
**Cause:** Old DNS records or cache
**Fix:** Verify nameservers in Cloudflare match Vercel, clear browser cache, try different device

### App Store Badge Not Clickable
**Cause:** Missing link or invalid URL
**Fix:** Update `<a href="https://apps.apple.com/app/helpstash/[APP_ID]">` with actual App Store ID after submission

### Email Link Not Working
**Cause:** Typo in mailto: link
**Fix:** Verify `<a href="mailto:support@helpstash.app">`, test in browser

### Mobile Layout Broken
**Cause:** Missing responsive CSS or viewport meta
**Fix:** Verify `<meta name="viewport" content="width=device-width, initial-scale=1.0">`, check responsive.css loaded

### Security Headers Missing
**Cause:** Vercel config not deployed
**Fix:** Verify `vercel.json` in repository root, redeploy, test at securityheaders.com

### Vercel Preview URL Different from Production
**Cause:** Preview deployments use different URL
**Fix:** Check Vercel Dashboard → Deployments tab, find deployment marked "Production"

### SSL Certificate Expired
**Cause:** Auto-renewal failed (rare)
**Fix:** Vercel handles renewal automatically, if issue persists contact Vercel support (chat in dashboard)

### WWW Redirect Not Working
**Cause:** WWW domain not configured
**Fix:** Vercel Settings → Domains → Add `www.helpstash.app` → Select "Redirect to helpstash.app"

---

## 9. Resources

### Essential Documentation
- This Guide: `Website_Plan.md` (deployment, content, troubleshooting)
- Web Dev Constitution: `CLAUDE.md` (patterns, best practices, CSS mastery)
- HelpStash PRD: `../HelpStash/docs/product/HelpStash_PRD.md` (product requirements)
- Engineering Plan: `../HelpStash/docs/tech/Engineering_Plan.md` (iOS app roadmap)

### Tools & Services
- Vercel Dashboard: https://vercel.com/dashboard
- Cloudflare Dashboard: https://dash.cloudflare.com
- GitHub Repository: https://github.com/[USERNAME]/helpstash-website
- App Store Connect: https://appstoreconnect.apple.com

### Testing Tools
- Performance: https://gtmetrix.com (target: <1s load)
- Accessibility: https://wave.webaim.org (target: 0 errors)
- Security Headers: https://securityheaders.com (target: A grade)
- DNS Checker: https://whatsmydns.net
- SSL Test: https://www.ssllabs.com/ssltest

### Quick Reference Commands
```bash
# Local testing
python3 -m http.server 8000

# Git workflow
git add . && git commit -m "Update" && git push origin main

# DNS check
dig helpstash.app NS

# HTTP status
curl -I https://helpstash.app

# Deploy to Vercel (automatic on push)
# Just push to GitHub, Vercel auto-deploys
```

### Key URLs
- Production: https://helpstash.app
- Privacy Policy: https://helpstash.app/privacy
- Support: https://helpstash.app/support
- Contact: support@helpstash.app

### Browser Support
- Chrome 90+, Safari 14+, Firefox 88+, Edge 90+
- Mobile: iOS Safari 14+, Chrome Android

### Performance Targets
- Load time: <1 second
- Page size: <500 KB
- Lighthouse score: 95+
- WCAG: Level AA compliant

---

## 10. Deployment Checklist

### Pre-Launch
- [ ] All HTML validated (W3C Validator)
- [ ] All CSS loads correctly
- [ ] All links tested (navigation + email)
- [ ] Images load (app-icon.png, badge.svg, all favicons)
- [ ] App Store badge displays "Coming Soon" state (grayed, not clickable)
- [ ] Mobile responsive (iPhone SE to Pro Max)
- [ ] Pricing correct across all pages ($1.99/mo, $14.99/yr)
- [ ] Email correct (support@helpstash.app)
- [ ] Domain purchased (helpstash.app ✅)
- [ ] Screenshots excluded from git (.gitignore configured)

### GitHub Setup
- [ ] Repository created: helpstash-website
- [ ] Visibility: Public
- [ ] All 14 files pushed
- [ ] Verify on github.com

### Vercel Deployment
- [ ] Account created (GitHub auth)
- [ ] Project imported
- [ ] Framework: Other (static)
- [ ] First deployment successful
- [ ] Test preview URL (all pages load)

### Domain Configuration
- [ ] Domain added in Vercel
- [ ] Nameservers: Vercel (not Cloudflare)
- [ ] Cloudflare updated with Vercel nameservers
- [ ] DNS propagated (dig shows ns1.vercel-dns.com)
- [ ] Domain status: Active
- [ ] WWW redirect configured

### SSL Verification
- [ ] HTTPS loads (padlock icon 🔒)
- [ ] Certificate: Let's Encrypt
- [ ] HTTP → HTTPS redirect works
- [ ] WWW → apex redirect works
- [ ] Security headers: A grade

### Post-Deployment
- [ ] All 5 pages accessible
- [ ] Mobile test (real device)
- [ ] Performance test (GTmetrix: A grade)
- [ ] Accessibility test (WAVE: 0 errors)
- [ ] App Store Connect URLs updated
- [ ] "Coming Soon" badge visible and styled correctly

### Post-App Store Approval
- [ ] Update index.html: Remove .coming-soon class
- [ ] Update index.html: Replace href="#" with App Store URL
- [ ] Update index.html: Delete "Coming Soon" pill span
- [ ] Test button clickability and App Store redirect
- [ ] Deploy updated homepage

### Go Live
- [ ] Final review with team
- [ ] Announce to stakeholders
- [ ] Monitor Vercel deployments
- [ ] Set up email alerts
- [ ] Begin weekly health checks

---

**End of Website Plan**

**Document Version:** 1.1
**Last Updated:** November 12, 2025
**Next Review:** December 12, 2025 (1 month post-deployment)
**Maintainer:** Web Development Team

**Questions?** Read `CLAUDE.md` for patterns/best practices, this doc for deployment/content specifics.

**Recent Changes (v1.1):**
- Added "Coming Soon" App Store badge implementation
- Documented screenshot folder structure (local-only, gitignored)
- Added activation instructions for App Store button post-approval
- Updated file structure and asset inventory
- Added Post-App Store Approval checklist section
