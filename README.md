# HelpStash Website

Official website for the HelpStash iOS app - a mindful approach to preventing impulse purchases.

## About

Static HTML/CSS website providing:
- Privacy Policy (GDPR/CCPA compliant)
- Terms of Service
- Support & FAQ
- Accessibility Statement
- Marketing homepage

**Live Site:** [helpstash.app](https://helpstash.app) (coming soon)

## Tech Stack

- **Frontend:** Static HTML5 + CSS3 (no JavaScript, no frameworks)
- **Hosting:** Vercel (global CDN, auto-deploy from main branch)
- **Domain:** helpstash.app
- **SSL:** Let's Encrypt (automatic)

## Project Structure

```
helpstash-website/
├── index.html              # Homepage
├── privacy.html            # Privacy Policy
├── terms.html              # Terms of Service
├── support.html            # Support & FAQ
├── accessibility.html      # Accessibility Statement
├── css/
│   ├── reset.css          # CSS normalization
│   ├── style.css          # Design system & global styles
│   ├── homepage.css       # Homepage-specific styles
│   └── responsive.css     # Mobile-first responsive breakpoints
├── assets/
│   ├── app-icon*.png      # App icons (multiple sizes)
│   ├── app-store-badge.svg # Official Apple badge
│   └── favicon*.png       # Favicons for all platforms
└── vercel.json            # Deployment configuration
```

## Local Development

### Option 1: Node.js Server (Recommended)
```bash
node server.js
# Visit http://localhost:8000
```

### Option 2: Python HTTP Server
```bash
python3 -m http.server 8000
# Visit http://localhost:8000
```

### Option 3: Direct File Opening
Simply open `index.html` in your browser (links between pages will work).

## Deployment

This repository auto-deploys to [Vercel](https://vercel.com) on every push to `main` branch.

### Deploy Your Own

1. Fork this repository
2. Connect to Vercel: [vercel.com/new](https://vercel.com/new)
3. Import your fork
4. Framework: **Other** (static HTML)
5. Deploy (no build configuration needed)

## Browser Support

- Chrome 90+
- Safari 14+
- Firefox 88+
- Edge 90+
- iOS Safari 14+
- Chrome Android

## Performance

- Load time: <1 second
- Page size: ~500 KB
- Lighthouse score: 95+
- WCAG 2.1 AA compliant

## Contact

- **Email:** support@helpstash.app
- **Website:** [helpstash.app](https://helpstash.app)

## License

© 2025 HelpStash. All rights reserved.
