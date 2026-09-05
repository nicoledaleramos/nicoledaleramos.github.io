# nicogallardoramos-glitch.github.io

A single-page professional portfolio. Plain HTML, CSS and JavaScript — no
build step, no dependencies, no `npm install`. Open `index.html` in a browser
and it works.

**Live at:** <https://nicogallardoramos-glitch.github.io/>

Personal portfolio for **Nicole Dale Ramos** — executive assistant, project
manager and automations specialist. Content is real, drawn from the EA/VA and
Automations résumés. Two things are still placeholders and are listed under
[Still to do](#still-to-do).

---

## Contents

```
.
├── index.html              ← all page content lives here
├── 404.html                ← shown for bad URLs (self-contained, inlined CSS)
├── resume/
│   ├── ea-va.html          ← Executive Assistant / VA résumé, as a live page
│   └── automations.html    ← Automations / PM résumé, as a live page
├── css/styles.css          ← all styling; design tokens at the top
├── js/main.js              ← theme toggle, nav, scroll reveal, form
├── assets/
│   └── favicon.svg         ← browser tab icon (edit initials + colour)
├── .github/workflows/
│   └── deploy.yml          ← auto-deploys to GitHub Pages on push
├── .nojekyll               ← required; stops Pages from mangling files
├── .gitattributes          ← forces LF endings so the Linux runner is happy
├── CNAME.example           ← rename to CNAME when you add a domain
├── robots.txt
├── sitemap.xml
└── .gitignore
```

---

## Setup status

- [x] Git installed (2.55.0.windows.5)
- [x] Author identity configured
- [x] Repo initialized on `main`
- [x] Pushed to GitHub
- [x] Pages enabled — Source set to "GitHub Actions"
- [x] First deploy succeeded; site live
- [x] Real content in place

<a id="still-to-do"></a>
### Still to do

Two placeholders remain. Both are marked `EDIT ME` in `index.html`.

1. **LinkedIn URL** — `YOUR-LINKEDIN` in the contact section. For EA/VA
   applications this is usually the second thing a recruiter clicks, so it is
   worth filling in. Delete the whole `<li>` if you would rather not link one.
2. **Contact form endpoint** — `YOUR_FORM_ID`. See
   [The contact form](#the-contact-form). Until it is set, the form shows a
   "not configured" message instead of silently discarding messages.

Optional but worth doing:

- `assets/portrait.jpg` — a square photo, 800×800 or larger. The hero shows a
  lettered placeholder until you add one.
- `assets/og-image.png` — 1200×630. Controls the preview card when the link is
  shared on LinkedIn or in a message.

### Privacy decisions already made

- The **full street address** from the source résumés is **deliberately
  omitted from every page here** — this repo and the site are public and get
  scraped. City and province only. Keep the full address in the copy you send
  directly to an employer.
- Your **phone number is published** in the contact section and on both résumé
  pages. That was your call, and it is easy to reverse: delete the `<li>` in
  `index.html` marked with the phone comment and redeploy.
- **No testimonials section.** Both résumés say "references available on
  request" and no real quotes were supplied. Inventing praise on a job-seeking
  site is not worth the risk; the section was removed rather than filled.

---

## Deploying

The workflow in `.github/workflows/deploy.yml` publishes on every push to
`main`. Nothing to run locally.

```powershell
cd "C:\Users\My PC\portfolio"
git add .
git commit -m "Describe what changed"
git push
```

Watch it under the repo's **Actions** tab. The first deploy takes about a
minute; later ones are faster. If a deploy fails, the Actions log says why —
it is almost always Pages not being set to "GitHub Actions" as its source.

---

## Making it yours

Work through these in order. All of it is in `index.html` unless noted.

| What | Where |
|---|---|
| Title, description, social preview | `<head>` of `index.html` |
| Name and initials | `.brand`, and `assets/favicon.svg` |
| Headline, intro, stats | `<section class="hero">` |
| About text | `<section id="about">` |
| Jobs | `<section id="experience">` — copy an `<li>` to add one |
| Highlighted work | `<section id="work">` — copy an `<article class="card">` |
| Skills and certifications | `<section id="skills">` |
| Email, phone, LinkedIn, location | `<section id="contact">` |
| Colour scheme | `css/styles.css`, `--accent-h` at the very top |
| Résumé content | `resume/ea-va.html`, `resume/automations.html` |

**Photo.** Save a square image (800×800 or larger) as `assets/portrait.jpg`,
then replace the `<div class="portrait-frame">…</div>` block with:

```html
<img src="assets/portrait.jpg" alt="Nicole Dale Ramos" width="800" height="800"
     style="border-radius:calc(var(--radius) - 5px); display:block">
```

**Keeping the résumés in sync.** The two files under `resume/` are standalone
— they carry their own CSS and share nothing with the main site. Editing a job
description in `index.html` does **not** update them, or vice versa. When a
role changes, edit all three, or the site will contradict itself in front of
someone reading both.

Each résumé page has a screen-only toolbar — a back link, a link to the other
version, and a "Save as PDF" button that opens the browser print dialog. It is
hidden on print, so the printed sheet is exactly the résumé.

**Colour.** `--accent-h: 172` is a hue on the 0–360 wheel. Try `212` for
blue, `268` for purple, `24` for a warm orange. Light and dark themes both
follow it automatically.

> `404.html` carries its own inlined copy of the accent colour, because it
> must work without loading a stylesheet. If you change `--accent-h`, update
> `--accent` in `404.html` too, or the two pages will drift apart.

---

## The contact form

Static hosting has no server, so a plain `<form>` cannot email you. The form
is pre-wired to **Formspree**'s free tier (50 submissions/month):

1. Sign up at <https://formspree.io> and create a form.
2. Copy your form ID.
3. In `index.html`, replace `YOUR_FORM_ID`:
   ```html
   <form action="https://formspree.io/f/xyzabcde" method="POST">
   ```
The fallback email shown if the form errors is already set in `js/main.js`.

Until you do this the form shows a "not configured yet" message rather than
pretending to send. Alternatives: **Web3Forms** (unlimited, free),
**Netlify Forms** (free, but only if you host on Netlify), or delete the form
and keep the email link.

---

## Custom domain

### Getting one cheaply

| Option | Cost | Notes |
|---|---|---|
| GitHub Student Developer Pack | **Free** | Free `.me` for a year via Namecheap, plus `.tech` / `.software`. Best option if you are a student — <https://education.github.com/pack> |
| Cloudflare Registrar | ~$10/yr | Sold at wholesale cost, no markup, no first-year gimmick |
| Porkbun / Namecheap | $1–3 first year | `.xyz`, `.site`, `.online`. Renewal jumps to $12–15 — check before buying |
| `.eu.org` | Free forever | Legitimate, but approval takes days–weeks and DNS setup is manual |

Avoid Freenom (`.tk`, `.ml`, `.ga`) — domains get reclaimed without warning
and recruiters recognise them.

### Connecting it

1. Rename `CNAME.example` to `CNAME`, put your domain in it (one line, no
   `https://`, no trailing slash), commit and push.
2. At your registrar's DNS panel, add:

   **For `www.yourdomain.com`** (recommended — simpler and more resilient):
   ```
   Type: CNAME   Name: www   Value: nicogallardoramos-glitch.github.io
   ```

   **For the bare `yourdomain.com`**, four A records:
   ```
   Type: A   Name: @   Value: 185.199.108.153
   Type: A   Name: @   Value: 185.199.109.153
   Type: A   Name: @   Value: 185.199.110.153
   Type: A   Name: @   Value: 185.199.111.153
   ```
3. GitHub → **Settings → Pages → Custom domain** → enter it → Save.
4. Wait for the DNS check to pass (minutes to a few hours), then tick
   **Enforce HTTPS**. The certificate is free and automatic.
5. Update the domain in `index.html` (`og:url`, `canonical`), `robots.txt`
   and `sitemap.xml`.

Because this is a *user site* (`username.github.io`), it is served at the
domain root both before and after you attach a domain — so no paths need to
change. That is the main reason this repo name was worth choosing.

---

## Hosting alternatives

The site is plain static files, so it runs anywhere. Point the service at
this repo — nothing in the code needs to change.

- **Cloudflare Pages** — faster CDN than GitHub Pages, free custom domain and
  SSL, preview builds per commit. Leave the build command blank, output
  directory `/`.
- **Netlify** — free tier, drag-and-drop deploy, built-in form handling that
  would replace the Formspree step entirely.
- **Vercel** — free hobby tier; best choice if you later rebuild in Next.js.

You can run several at once — same repo, multiple hosts, one domain pointed
at whichever you prefer.

---

## Previewing locally

Double-click `index.html`. That is genuinely all this site needs.

Python is not installed on this machine, so `python -m http.server` will not
work. If you later want a real local server (only necessary if you start
fetching files with JS), the easiest option is the **Live Server** extension
in VS Code — right-click `index.html` → "Open with Live Server".

---

## Before you share the URL

- [ ] Fill in or delete the LinkedIn link (`YOUR-LINKEDIN` in `index.html`)
- [ ] Configure or remove the contact form (`YOUR_FORM_ID`)
- [ ] Add `assets/portrait.jpg` — the hero shows a lettered placeholder without it
- [ ] Add `assets/og-image.png` (1200×630) so shared links show a preview
- [ ] Read the whole page once for typos and anything you would not say aloud
- [ ] Check both résumé pages render and the "Save as PDF" button works
- [ ] Test on your phone
- [ ] Toggle dark mode and check both themes
- [ ] Print the main page (Ctrl+P) — it is styled to come out as a clean résumé
- [ ] Run it through <https://pagespeed.web.dev>
