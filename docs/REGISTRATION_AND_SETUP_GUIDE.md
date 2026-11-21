# PEACX AB — Domain Registration, Hosting & Bi-Directional Email Setup Guide

**Entity:** PEACX AB (Malmö, Sweden)  
**Primary Domain:** `peacx.eu`  
**Backup / Redirect Domain:** `peacx.se`  
**Target Infrastructure Cost:** ~50 SEK Total Year 1 (~270 SEK/year ongoing across both domains); **0 SEK/month for hosting & email**.

---

## 1. Executive Cost & Architecture Summary

| Component | Architecture / Provider | Cost Year 1 | Cost Year 2+ |
| :--- | :--- | :--- | :--- |
| **`peacx.eu` (Primary)** | Loopia / Inleed / Namecheap | ~40 SEK | ~95 SEK/yr |
| **`peacx.se` (Redirect)** | Polar55 / Inleed / Loopia | ~5–10 SEK | ~179 SEK/yr |
| **Web Hosting** | GitHub Pages or Cloudflare Pages | **0 SEK** | **0 SEK** |
| **Inbound Email** | Cloudflare Email Routing (forward to your Gmail) | **0 SEK** | **0 SEK** |
| **Outbound Email (Send as `peter@peacx.eu`)** | Brevo Free SMTP Relay (300 mails/day) + Gmail "Send mail as" | **0 SEK** | **0 SEK** |
| **Total Operational Cost** | — | **~50 SEK** | **~274 SEK/yr** (~23 SEK/mo) |

*(Optional Alternative: If you prefer a 100% standalone corporate IMAP mailbox rather than routing via Gmail, **Zoho Mail Lite EU** is €0.90/month (~125 SEK/year) and handles both inbound and outbound with custom webmail and mobile apps).*

---

## 2. Step-by-Step Execution Plan

### Step 1: Register `peacx.eu` and `peacx.se`
1. Go to a reputable Swedish or European registrar:
   - **Recommendation (Convenience):** [Loopia.se](https://www.loopia.se) or [Inleed.se](https://inleed.se) allows registering both `.eu` and `.se` in a single cart.
   - **Recommendation (Lowest price):** Polar55.se (~5 kr for `.se`) and Loopia.se (~40 kr for `.eu`).
2. Search and add to cart:
   - `peacx.eu`
   - `peacx.se`
3. Registrant Details: Register under your personal name/personnummer (or company under formation). When `PEACX AB` receives its Swedish Org.nr from Bolagsverket, you can do a free 1-click owner transfer to the legal entity.
4. Uncheck any expensive upsells (web hotel, email hosting, site builders) — you do not need them because we provide global edge hosting and email routing for free.

---

### Step 2: Set Up Free Cloudflare DNS (The Traffic Controller)
1. Sign up for a free account at [Cloudflare.com](https://dash.cloudflare.com/sign-up).
2. Click **Add a Site** and enter `peacx.eu`. Select the **Free** plan.
3. Cloudflare will give you two nameservers (e.g. `ada.ns.cloudflare.com` and `bob.ns.cloudflare.com`).
4. Log into your domain registrar control panel (Loopia/Inleed), find DNS / Nameservers for `peacx.eu`, and change them to Cloudflare's nameservers.
5. Repeat for `peacx.se`.

---

### Step 3: Deploy the Website (GitHub Pages or Cloudflare Pages)

#### Option A: Cloudflare Pages (Recommended — Fast & Zero Configuration)
1. Log into your Cloudflare Dashboard.
2. Go to **Compute (Workers) > Pages > Connect to Git**.
3. Select the GitHub repository `peacpeac/peacx-eu`.
4. Framework preset: None (HTML/Static). Build command: *leave empty*. Output directory: `/`.
5. Click **Save and Deploy**. Your site is now live on `https://peacx-eu.pages.dev` with an automated SSL certificate.
6. Under **Custom Domains**, click **Set up a domain**, enter `peacx.eu` and `www.peacx.eu`. Cloudflare will automatically configure the DNS records!

#### Option B: GitHub Pages
1. In your GitHub repo `peacpeac/peacx-eu` → **Settings > Pages**.
2. Under **Build and deployment > Source**, select `Deploy from a branch` → `main` → `/ (root)` → **Save**.
3. Under **Custom domain**, enter `peacx.eu` and check **Enforce HTTPS**.
4. In Cloudflare DNS for `peacx.eu`, import `dns/peacx.eu.zone.txt` or add the GitHub Pages IP addresses (`185.199.108.153`, etc.).

---

### Step 4: Configure Bi-Directional Email (`@peacx.eu`)

You want to **both receive and send** emails as `peter@peacx.eu` (and `info@peacx.eu`). Here is how to achieve this seamlessly inside your regular Gmail for **0 SEK**:

#### A. Inbound Routing (Receiving Mails to Gmail)
1. In Cloudflare Dashboard for `peacx.eu`, go to **Email Routing**.
2. Click **Get Started**. Cloudflare will add the MX and SPF records automatically.
3. Under **Routing Rules**, add rule:
   - Custom address: `peter@peacx.eu` → Action: Send to `your-personal-email@gmail.com`.
   - Custom address: `info@peacx.eu` → Action: Send to `your-personal-email@gmail.com`.
   - Catch-all rule (optional): Send any unlisted `@peacx.eu` address to your Gmail.
4. Cloudflare will send a verification link to your Gmail. Click to confirm.
5. Now, all emails sent to `peter@peacx.eu` arrive directly in your regular Gmail inbox.

#### B. Outbound Relay (Sending Emails from Gmail as `peter@peacx.eu`)
To send emails from `peter@peacx.eu` directly inside Gmail without getting flagged as spam or showing "via gmail.com":
1. Sign up for a free account at [Brevo.com](https://www.brevo.com) (formerly Sendinblue — 300 emails/day free forever) or [Resend.com](https://resend.com) (3,000 emails/month free).
2. In Brevo → **Senders, Domains & Dedicated IPs > Domains > Add a Domain**: enter `peacx.eu`.
3. Brevo will show you 2 DNS TXT records (DKIM and Brevo validation code). Add these to Cloudflare DNS. Brevo will verify the domain with a green checkmark.
4. In Brevo → **SMTP & API**: Click **Generate a new SMTP key**. Note down:
   - **SMTP Server:** `smtp-relay.brevo.com`
   - **Port:** `587`
   - **Login / Username:** your Brevo account email
   - **Password:** the generated SMTP key
5. Open your Gmail in browser:
   - Go to **Settings (gear icon) > See all settings > Accounts and Import**.
   - Under **Send mail as**, click **Add another email address**.
   - Name: `Peter Ac | PEACX AB` (or `Peter Ac`)
   - Email address: `peter@peacx.eu`
   - Uncheck *"Treat as an alias"*. Click **Next Step**.
   - **SMTP Server:** `smtp-relay.brevo.com`
   - **Port:** `587`
   - **Username:** your Brevo SMTP login
   - **Password:** your Brevo SMTP key
   - Secured connection using TLS: **Checked**.
   - Click **Add Account**.
6. Gmail will send a confirmation code to `peter@peacx.eu` (which Cloudflare immediately delivers to your Gmail inbox!). Enter the code to verify.
7. **Result:** When composing or replying in Gmail, you can now select `peter@peacx.eu` in the **From:** dropdown. Your outbound email goes through Brevo with 100% DKIM + SPF cryptographic verification. The recipient sees a legitimate email from `peter@peacx.eu`.

---

### Step 5: Configure `peacx.se` to 301 Redirect to `peacx.eu`

You want `peacx.se` to act as an automatic redirect to `peacx.eu`.
1. In Cloudflare Dashboard for `peacx.se`, go to **Rules > Redirect Rules**.
2. Click **Create rule**:
   - Rule name: `Redirect peacx.se to peacx.eu`
   - When incoming requests match: **All incoming requests**
   - Type: **Dynamic** (or Static)
   - Expression / URL Redirect:
     - Type: **301 (Permanent Redirect)**
     - Target URL: `concat("https://peacx.eu", http.request.uri.path)`
     - Preserve query string: **Yes**
3. In Cloudflare **Email Routing** for `peacx.se`:
   - Set a Catch-all rule: `*@peacx.se` → forward to `peter@peacx.eu` (or your Gmail).
4. **Result:** Anyone typing `peacx.se` or sending an email to `@peacx.se` is instantly and cleanly routed to your `.eu` domain.

---

## 3. Ready-to-Paste Bolagsverket Registration Package (`PEACX AB`)

When filing on [verksamt.se](https://www.verksamt.se) using your BankID:

### A. Företagsnamn (Priority Order)
1. **1:a handsval:** `PEACX AB`
2. **2:a handsval:** `PAXIO AB`
3. **3:e handsval:** `PEACX i Malmö AB`

### B. Säte
- **Kommun:** `Malmö` (Skåne län)

### C. Verksamhetsbeskrivning (Exact Swedish Legal Text)
> *"Bolaget ska bedriva konsultverksamhet inom företagsledning (management), försörjningskedjor (supply chain), informationsteknologi (IT), automation och artificiell intelligens (AI), samt utveckla, tillverka och sälja digitala lösningar, mjukvara och hårdvara inom automation och därmed förenlig verksamhet."*

### D. SNI-koder (Branschkoder)
- `62020` – Datakonsultverksamhet (Primär)
- `70220` – Konsultverksamhet avseende företags organisation (Management & Supply Chain)
- `62010` – Dataprogrammering (Digitala lösningar & mjukvara)
- `71122` – Teknisk konsultverksamhet inom industriteknik (Automation & hårdvara)

### E. Meddelande till handläggaren (Message to Case Officer)
Paste this in the comments box to prevent any name disputes or confusion:
> *"Företagsnamnet PEACX är ett nybildat fantasiord härlett från grundarens namn (Peter Ac) kombinerat med bokstaven X för execution och automation. Ordet har ingen särskild språklig betydelse på svenska eller engelska och är fullt särskiljande gentemot befintliga bolagsnamn i Malmö kommun och Sverige. Verksamhetsbeskrivningen omfattar både konsulttjänster och praktisk utveckling av hård- och mjukvara inom automation."*
