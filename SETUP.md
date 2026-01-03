# Pragmatica Labs Site Setup Guide

## 1. Zoho Mail Setup (Free Tier)

### Step 1: Sign up for Zoho Mail
1. Go to https://www.zoho.com/mail/zohomail-pricing.html
2. Click "Forever Free Plan" (up to 5 users, 5GB/user)
3. Choose "Add your existing domain"
4. Enter: `pragmaticalabs.io`

### Step 2: Verify Domain in Cloudflare
Zoho will give you a TXT record to add:

1. Log in to Cloudflare
2. Select `pragmaticalabs.io`
3. Go to DNS settings
4. Add TXT record:
   - Name: `@`
   - Value: (provided by Zoho, looks like `zoho-verification=zb...`)
   - TTL: Auto
5. Wait for verification (can take a few minutes)

### Step 3: Configure MX Records in Cloudflare
Add these MX records (delete any existing MX records first):

| Priority | Name | Mail server |
|----------|------|-------------|
| 10 | @ | mx.zoho.eu |
| 20 | @ | mx2.zoho.eu |
| 50 | @ | mx3.zoho.eu |

### Step 4: Add SPF Record
Add TXT record for email deliverability:
- Name: `@`
- Value: `v=spf1 include:zoho.eu ~all`

### Step 5: Create Email Account
1. In Zoho admin, create user: `sergiy@pragmaticalabs.io`
2. Set password
3. Access mail at: https://mail.zoho.eu

### Step 6: Optional - DKIM
Zoho will provide a DKIM record. Add as TXT:
- Name: `zmail._domainkey`
- Value: (provided by Zoho)

---

## 2. GitHub Pages Deployment

### Step 1: Create Repository
```bash
cd /Users/sergiyyevtushenko/IdeaProjects/pragmaticalabs-site
git init
git add .
git commit -m "Initial site"
git branch -M main
gh repo create pragmaticalabs-site --public --source=. --push
```

### Step 2: Enable GitHub Pages
1. Go to repo Settings → Pages
2. Source: Deploy from a branch
3. Branch: main, folder: / (root)
4. Save

### Step 3: Configure Custom Domain in GitHub
1. In repo Settings → Pages
2. Custom domain: `pragmaticalabs.io`
3. Check "Enforce HTTPS"

### Step 4: Configure DNS in Cloudflare
Add these records:

**Option A: Apex domain (pragmaticalabs.io)**
Add A records pointing to GitHub Pages IPs:
```
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```

**Option B: CNAME (if using www subdomain)**
```
CNAME www → your-username.github.io
```

### Step 5: Create CNAME file
Create a file named `CNAME` in site root:
```
pragmaticalabs.io
```

### Step 6: Verify
- Wait for DNS propagation (can take up to 24 hours, usually faster)
- Check https://pragmaticalabs.io

---

## 3. Cloudflare Settings

### Recommended Settings
1. **SSL/TLS**: Full (strict)
2. **Always Use HTTPS**: On
3. **Auto Minify**: HTML, CSS, JS
4. **Brotli**: On

### Proxy Status
- For GitHub Pages: DNS only (gray cloud) for A records initially
- After HTTPS working: can enable proxy (orange cloud)

---

## Files in This Repository

```
pragmaticalabs-site/
├── index.html       # Home page
├── jbct.html        # JBCT product page
├── aether.html      # Aether product page
├── contact.html     # Contact page
├── css/
│   └── style.css    # Stylesheet
├── CNAME            # GitHub Pages custom domain (create this)
└── SETUP.md         # This file
```

---

## Checklist

- [ ] Sign up for Zoho Mail free tier
- [ ] Add domain verification TXT record
- [ ] Configure MX records in Cloudflare
- [ ] Add SPF record
- [ ] Create sergiy@pragmaticalabs.io
- [ ] Test sending/receiving email
- [ ] Create GitHub repo
- [ ] Enable GitHub Pages
- [ ] Add custom domain in GitHub
- [ ] Configure A records in Cloudflare
- [ ] Create CNAME file
- [ ] Verify site loads at https://pragmaticalabs.io
- [ ] Update contact page email if different
