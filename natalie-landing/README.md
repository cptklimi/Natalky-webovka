# Natalie Sebestova — landlord landing page

Static one-page site for Natalie Sebestova, Property Manager & BDM at Edwards Real Estate Co, Brisbane. Built for GitHub Pages. No build step, no dependencies.

```
index.html
assets/
  logo-navy.png          Edwards lockup, navy on transparent
  logo-white.png         Edwards lockup, white on transparent (for dark backgrounds)
  natalie-hero.jpg       hero portrait, 760×1140
  natalie-portrait.jpg   square crop, 620×620
  og-image.jpg           social share image, 1200×630
robots.txt
sitemap.xml
```

---

## 1. Deploy

1. Create a repo, upload everything in this folder to the root.
2. Settings → Pages → Source: `main` branch, `/ (root)`.
3. Add the custom domain in Settings → Pages. GitHub writes a `CNAME` file for you.
4. At the registrar, point the apex to GitHub's four A records and `www` to `<username>.github.io` via CNAME.
5. Tick **Enforce HTTPS** once the certificate provisions (can take an hour).

---

## 2. Wire up the forms

Both forms (rental review + selling enquiry) post to Web3Forms and stay on the page. Three values to set, all in the `CONFIG` block near the bottom of `index.html`:

```js
var CONFIG = {
  endpoint: 'https://api.web3forms.com/submit',
  accessKey: 'REPLACE-WITH-YOUR-WEB3FORMS-ACCESS-KEY',
  recaptchaSiteKey: 'REPLACE-WITH-YOUR-RECAPTCHA-V2-SITE-KEY'
};
```

**Web3Forms access key** — free at web3forms.com. Enter the destination email (`natalie@edwardsrealestateco.com.au`), they email you a key. Submissions land in that inbox. Set a CC for the sales enquiry so Adam gets a copy.

**reCAPTCHA** — create a **v2 "I'm not a robot" checkbox** key pair at google.com/recaptcha. Site key goes in `CONFIG`; secret key goes in the Web3Forms dashboard so the token actually gets verified server-side.

> ⚠️ **Important:** reCAPTCHA verification is a **Web3Forms Pro feature**. On the free plan the widget will render and block submissions client-side, but the token isn't verified on their server — which means a determined bot posting directly to the endpoint can bypass it.
>
> Two ways to deal with that:
> - **Pay for Web3Forms Pro** and paste the reCAPTCHA secret into their dashboard. You get the reCAPTCHA Natalie asked for, properly verified.
> - **Use hCaptcha instead** — Web3Forms includes it free with zero config, no keys needed. Swap the two `<div class="captcha" id="recaptcha-…">` elements for `<div class="h-captcha" data-captcha="true"></div>`, drop the reCAPTCHA `<script>` tag and the `onRecaptchaLoad` function, and add `<script src="https://web3forms.com/client/script.js" async defer></script>`.
>
> The honeypot field (`botcheck`) is already in both forms and works on every plan.

Alternatives if you'd rather not use Web3Forms: Formspree, Netlify Forms, or a Cloudflare Pages Function. Only `CONFIG.endpoint` and the hidden fields change.

**Test both forms before sending the link to Natalie.** Submit each one and confirm the email arrives, including from a phone.

---

## 3. Fill the placeholders

Everything in `[SQUARE BRACKETS]` is a real gap. Search the file for `[` and work through it.

| Placeholder | Where | Notes |
|---|---|---|
| `[MOBILE]` | header, bio, CTA, footer, schema, error message | Also update the `tel:+61400000000` hrefs |
| `[OFFICE ADDRESS]`, `[POSTCODE]` | footer, schema | Street address, not a PO Box — it costs local map visibility |
| `[LICENCE NUMBER]` | footer | |
| `[X]`, `[X] hrs`, `[X]%`, `[X] yrs` | stats band | Real numbers or delete the band — vague stats are worse than none |
| `[MONTH YEAR]` | stats caption | Dated proof beats undated proof |
| Bio paragraphs | About Natalie | Two or three sentences background, one or two personal |
| `[ONE SHORT LINE IN NATALIE'S OWN WORDS]` | pull quote | Her actual promise, in her voice |
| `[SUBURB]` × 12 | service area, schema `areaServed` | The suburbs she really covers |
| `[INNER NORTH / INNER SOUTH]` | service area H2 | |
| `[MANAGEMENT FEE]`, `[LETTING FEE]` | FAQ, schema | |
| Lock-in answer | FAQ, schema | |
| Two reviews | testimonials | Named, with suburbs |
| `[GOOGLE / RATEMYAGENT]` | testimonials caption | |
| `[SALES CONTACT]` | selling modal | Presumably Adam |

**Keep the FAQ answers in the schema identical to the visible text.** If they drift, Google ignores the markup.

---

## 4. Before launch

- Replace `https://www.nataliesebestova.com.au/` everywhere — `<title>`, canonical, OG tags, `sitemap.xml`, and all five `@id` fields in the JSON-LD. There are about a dozen.
- Validate the structured data at search.google.com/test/rich-results.
- Verify the Queensland legal claims against the RTA before publishing. Two statements carry risk: the 12-month rent increase limit attaching to the property rather than the tenancy, and minimum housing standards applying to general tenancies. Both are accurate as I understand them, but they're the page's differentiator and they need to be right. The footer disclaimer is not a substitute for checking.
- Get Adam's sign-off on Natalie being the named face for owner acquisition. The Edwards brochure tells landlords they deal with the principals; this page tells them they deal with Natalie.
- Submit to Google Search Console and Bing Webmaster Tools.
- Set up the Google Business Profile — practitioner listing in her own name only, website field pointing here, not to the Edwards homepage.
- Ask Adam for a link from the Edwards team page to this domain. Highest-value thing you can do for a new domain, and it's free.

---

## 5. What this page can't do alone

A single page will rank for her name and some long-tail phrases. It won't rank for "property management Brisbane" — that's owned by competitors running 27 to 150 suburb pages. The SEO layer that follows:

```
/property-management-<suburb>/     × 10–15, written properly
/switch-property-managers-brisbane/
```

Each reuses the hero form and the Queensland section with genuinely local content on top. Ten good pages beat forty thin ones.

---

## Notes

- Fonts load from Google Fonts: Lora (headings) and IBM Plex Sans (body), matching the Edwards brochure.
- Brand colours sampled from the brochure PDF: navy `#0D1E38`, cream `#FAF7F2`, sand `#EBE4D9`, blue accent `#2F5E8C`.
- No cookies are set and no analytics are included. If you add GA4 or Plausible, Australian Privacy Act obligations and a privacy policy page come with it.
- Tested layout breakpoints: 1000px and 720px. The nav collapses at 1000px — if Natalie wants a hamburger menu rather than hiding the links, that's a small addition.
