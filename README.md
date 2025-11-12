# HelpStash Website

Static website for the HelpStash iOS app. Required for App Store Connect submission (Privacy Policy and Support URLs).

## Structure

```
website/
├── index.html              # Homepage with features and App Store badge
├── privacy.html            # Privacy Policy (App Store required)
├── terms.html              # Terms of Service
├── support.html            # Support Page (App Store required)
├── accessibility.html      # Accessibility Statement
├── css/
│   ├── reset.css          # CSS normalization
│   ├── style.css          # Main styles (design tokens from iOS app)
│   ├── homepage.css       # Homepage-specific styles
│   └── responsive.css     # Mobile breakpoints
└── assets/
    ├── app-icon.png       # 1024x1024px app icon
    ├── app-store-badge.svg # Official Apple badge
    └── favicon.ico        # 32x32px favicon (TODO)
```

## Deployment

See `/docs/website/deployment-steps.md` for full instructions.

### Quick Deploy to Vercel

1. **Push to GitHub** (separate repo recommended):
   ```bash
   git init
   git add .
   git commit -m "Initial commit: HelpStash website"
   git remote add origin https://github.com/YOUR_USERNAME/helpstash-website.git
   git push -u origin main
   ```

2. **Deploy to Vercel**:
   - Go to [vercel.com](https://vercel.com)
   - Import GitHub repo
   - Framework: Other (static HTML)
   - Deploy

3. **Connect Domain**:
   - Vercel: Add domain `helpstash.app`
   - Update Cloudflare DNS or use Vercel nameservers

4. **Update App Store Connect**:
   - Privacy Policy URL: `https://helpstash.app/privacy`
   - Support URL: `https://helpstash.app/support`

## TODO Before Launch

- [ ] Generate `favicon.ico` from app-icon.png
- [ ] Update App Store link in homepage after submission (line 54)
- [ ] Test all navigation links
- [ ] Verify mobile responsive design
- [ ] Set up email forwarding: support@helpstash.app → gurinderiift@gmail.com

## Local Testing

No build step required - just open HTML files in browser:

```bash
# macOS: Open in default browser
open website/index.html

# Or use Python's built-in server
cd website
python3 -m http.server 8000
# Visit http://localhost:8000
```

## Key Details

- **Contact Email**: support@helpstash.app
- **Pricing**: $1.99/month or $14.99/year
- **Free Trial**: 21 days
- **Free Tier**: 3 pauses/week
- **Age Rating**: 17+

## Legal Review

**Recommendation**: Have a lawyer review Privacy Policy and Terms before public launch, especially:
- GDPR compliance (EU users)
- CCPA compliance (California users)
- Governing law (currently India/Chandigarh)

## License

© 2025 HelpStash. All rights reserved.
