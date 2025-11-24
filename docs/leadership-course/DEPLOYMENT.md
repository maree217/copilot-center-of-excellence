# Landing Page Deployment Guide

Your professional landing page (`index.html`) is ready to deploy via GitHub Pages!

## 🚀 Quick Setup (5 minutes)

### Step 1: Enable GitHub Pages

1. Go to your repository on GitHub: `https://github.com/maree217/copilot-center-of-excellence`
2. Click **Settings** (top menu)
3. Scroll down to **Pages** (left sidebar under "Code and automation")
4. Under **Source**, select:
   - **Branch**: `claude/leadership-course-assessment-01EyEPWATs4gao7eAcYYmcrY` (or merge to `main` first)
   - **Folder**: `/docs`
5. Click **Save**

### Step 2: Access Your Landing Page

After 2-3 minutes, your site will be live at:

```
https://maree217.github.io/copilot-center-of-excellence/leadership-course/
```

**Note:** The URL structure is:
- `https://[username].github.io/[repo-name]/[path-to-index]`
- In your case: `https://maree217.github.io/copilot-center-of-excellence/leadership-course/`

### Step 3: Test All Links

Once deployed, verify:
- ✅ Hero section loads properly
- ✅ All ROI cards display correctly
- ✅ Pricing table is readable
- ✅ Email form works (opens email client)
- ✅ "Book Consultation" buttons work
- ✅ Mobile responsive design (test on phone)
- ✅ All internal links work (README.md, tools, etc.)

---

## 🎨 Customization Options

### Update Contact Email

The landing page uses `ram@aicapabilitybuilder.com` in multiple places:
1. Navigation "Book Consultation" button
2. Footer contact section
3. Email form action
4. All pricing tier CTA buttons

**To change the email:**
```bash
# Replace all instances in index.html
sed -i 's/ram@aicapabilitybuilder.com/YOUR_EMAIL@example.com/g' docs/leadership-course/index.html
```

### Update LinkedIn URL

Current: `https://linkedin.com/in/rammaree`

**To change:**
Edit line 407 in `index.html`:
```html
<a href="https://linkedin.com/in/YOUR_LINKEDIN" target="_blank">LinkedIn</a>
```

### Update Pricing

Pricing is in the "Pricing Section" (~lines 350-450):
- **Self-Paced**: £1,995 (line ~370)
- **Cohort**: £12,500 (line ~390)
- **Enterprise**: £35k-£50k (line ~410)

### Update ROI Metrics

ROI cards are in the "ROI Section" (~lines 200-300). Each card follows this structure:
```html
<div class="roi-card">
    <div class="company-name">Company Name</div>
    <div class="industry">Industry · Employee Count</div>
    <div class="metric">2,000% ROI</div>
    <p class="description"><strong>Metric</strong>. Details here.</p>
</div>
```

### Update Color Scheme

Colors are defined in CSS variables (lines 14-24):
```css
:root {
    --primary: #0078D4;        /* Microsoft Blue */
    --primary-dark: #005A9E;   /* Darker Blue */
    --secondary: #50E6FF;      /* Light Blue */
    --success: #10893E;        /* Green */
    --text-dark: #1F1F1F;      /* Near Black */
    --text-light: #605E5C;     /* Gray */
    --bg-light: #F5F5F5;       /* Light Gray */
    --white: #FFFFFF;
    --gradient: linear-gradient(135deg, #0078D4 0%, #50E6FF 100%);
}
```

**To use your brand colors:**
1. Change `--primary` to your primary brand color (hex code)
2. Change `--gradient` to match your brand
3. All buttons, links, and accents will update automatically

---

## 📊 Recommended Next Steps

### 1. Set Up Professional Email Capture

Replace the basic `mailto:` form with a real email capture service:

**Option A: Tally.so (Free)**
- Create form at https://tally.so
- Replace the `<form>` section (lines ~620-625) with Tally embed code
- Example: `<iframe src="https://tally.so/embed/YOUR_FORM_ID"></iframe>`

**Option B: Mailchimp**
- Create audience at https://mailchimp.com
- Get embedded form code
- Replace email form section

**Option C: ConvertKit**
- Create landing page at https://convertkit.com
- Use their email capture form
- Replace email form section

### 2. Add Analytics

**Google Analytics:**
```html
<!-- Add before </head> tag (line ~12) -->
<script async src="https://www.googletagmanager.com/gtag/js?id=GA_MEASUREMENT_ID"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'GA_MEASUREMENT_ID');
</script>
```

**Plausible (Privacy-friendly alternative):**
```html
<!-- Add before </head> tag -->
<script defer data-domain="yourdomain.com" src="https://plausible.io/js/script.js"></script>
```

### 3. Add Custom Domain (Optional)

Instead of `maree217.github.io/copilot-center-of-excellence`:

1. Buy domain (e.g., `aicoursehub.com`) from Namecheap, GoDaddy, etc.
2. In GitHub Pages settings, add custom domain
3. Configure DNS records:
   - Add `CNAME` record pointing to `maree217.github.io`
4. Enable HTTPS (automatic via GitHub)

Your site will then be: `https://aicoursehub.com`

### 4. Add Calendly for Booking

Replace "Book Consultation" buttons with Calendly:

1. Create account at https://calendly.com
2. Set up 30-min consultation event
3. Get your Calendly link (e.g., `https://calendly.com/yourname/30min`)
4. Replace all `mailto:ram@aicapabilitybuilder.com?subject=Book%20Consultation` links with your Calendly URL

**Better UX:** Use Calendly inline embed instead of popup:
```html
<!-- Replace CTA button with this -->
<a href="https://calendly.com/yourname/30min" target="_blank" class="btn-primary">Book Consultation</a>
```

---

## 🔧 Troubleshooting

### Issue: Page shows 404 error

**Fix:**
- Ensure GitHub Pages is enabled (see Step 1)
- Check branch selection (should be your feature branch or `main`)
- Folder must be `/docs`
- Wait 2-3 minutes for GitHub to build the site

### Issue: Styles not loading

**Fix:**
- CSS is embedded in `<style>` tags (lines 13-350)
- View page source - if you see the CSS, it's loading correctly
- Try hard refresh: `Ctrl+Shift+R` (Windows) or `Cmd+Shift+R` (Mac)

### Issue: Links don't work

**Fix:**
- Internal links (README.md, tools) are relative paths
- They work on GitHub Pages but might not work locally
- Test on the live GitHub Pages URL, not by opening `index.html` directly

### Issue: Email form doesn't work

**Fix:**
- `mailto:` links open the user's default email client
- This is expected behavior for the basic version
- For better UX, integrate with Tally.so, Mailchimp, or ConvertKit (see above)

---

## 📈 Performance Tips

### Optimize for SEO

Add to `<head>` section (after line 5):
```html
<meta name="keywords" content="Microsoft Copilot, AI training, enterprise AI, Copilot deployment, AI leadership">
<meta property="og:title" content="Microsoft Copilot Leadership Course | Enterprise AI Implementation">
<meta property="og:description" content="Deploy Microsoft Copilot without the £59.6M mistakes. 2,000%+ ROI. 90-day roadmap.">
<meta property="og:image" content="https://yourdomain.com/og-image.png">
<meta property="og:url" content="https://yourdomain.com">
<meta name="twitter:card" content="summary_large_image">
```

### Add Favicon

1. Create 32x32 favicon.ico file
2. Place in `docs/leadership-course/` directory
3. Add to `<head>`:
```html
<link rel="icon" type="image/x-icon" href="favicon.ico">
```

### Test Mobile Responsiveness

Use Chrome DevTools:
1. Right-click page → "Inspect"
2. Click device toolbar icon (top-left)
3. Test on iPhone, iPad, Android sizes

---

## 🎯 Landing Page Conversion Checklist

Once deployed, verify these conversion elements are working:

- [ ] Hero section clearly states the value proposition
- [ ] ROI metrics are visible (2,000%+ ROI, 80-95% adoption)
- [ ] "Book Consultation" button is prominent (nav + hero)
- [ ] Pricing tiers are clear with CTAs for each
- [ ] Testimonials build trust and credibility
- [ ] Email capture form is easy to find
- [ ] All CTAs lead to the right action (email, booking, enrollment)
- [ ] Mobile design is clean and readable
- [ ] Page loads in < 3 seconds
- [ ] No broken links or images

---

## 📧 Support

If you need help customizing or deploying the landing page:

- **Email**: ram@aicapabilitybuilder.com
- **LinkedIn**: https://linkedin.com/in/rammaree

---

**Version:** 1.0 (January 2025)
**Last Updated:** 2025-11-24
**File**: `/docs/leadership-course/index.html` (750 lines)
