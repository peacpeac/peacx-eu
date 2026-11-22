# PeacX AB — Official Corporate Web & Infrastructure Repository

Repository for the corporate web presence, DNS zone definitions, and infrastructure configuration for **PeacX AB** (`peacx.eu`).

## Repository Structure

```
peacx-eu/
├── index.html                   # High-performance corporate landing page
├── CNAME                        # peacx.eu domain mapping
├── _redirects                   # Edge redirect rules (www -> apex, https enforcement)
├── robots.txt                   # Search crawler directives
├── sitemap.xml                  # XML sitemap
├── dns/
│   ├── peacx.eu.zone.txt        # Ready-to-import DNS zone file for peacx.eu
│   └── peacx.se.zone.txt        # Ready-to-import DNS zone file for peacx.se (redirect)
└── docs/
    └── REGISTRATION_AND_SETUP_GUIDE.md  # Step-by-step registrar, email & Bolagsverket guide
```

## Deployment

This repository is ready to be deployed with zero build steps to:
- **Cloudflare Pages** (Recommended) or **GitHub Pages**
- Custom domain: `peacx.eu`
- Edge HTTPS redirect: `peacx.se` → `peacx.eu`
