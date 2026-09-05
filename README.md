# nicogallardoramos-glitch.github.io

A single-page professional portfolio. Plain HTML, CSS and JavaScript — no
build step, no dependencies, no `npm install`. Open `index.html` in a browser
and it works.

**Live at:** <https://nicogallardoramos-glitch.github.io/>

> ⚠️ **Content is currently synthetic** — a fictional operations manager
> called "Alex Rivera". Replace it before sharing the URL with anyone.
> Every editable spot is marked with an `EDIT ME` comment.

---

## Contents

```
.
├── index.html              ← all page content lives here
├── 404.html                ← shown for bad URLs (self-contained, inlined CSS)
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
- [x] Author identity configured — Nico Gallardo Ramos <nicogallardoramos@gmail.com>
- [x] Repo initialized on `main`, all files committed
- [ ] Pushed to GitHub
- [ ] Pages enabled (**Settings → Pages → Source → "GitHub Actions"**)
- [ ] Real content swapped in

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
| Title, description, social preview | `<head>` — the `EDIT ME` block |
| Name and initials | `.brand`, and `assets/favicon.svg` |
| Headline, intro, stats | `<section class="hero">` |
| About text | `<section id="about">` |
| Jobs and education | `<section id="experience">` — copy an `<li>` to add one |
| Projects | `<section id="work">` — copy an `<article class="card">` |
| Skills and certifications | `<section id="skills">` |
| Quotes | `<section id="testimonials">` — delete the section if you have none |
| Email, LinkedIn, location | `<section id="contact">` |
| Colour scheme | `css/styles.css`, `--accent-h` at the very top |

**Photo.** Save a square image (800×800 or larger) as `assets/portrait.jpg`,
then replace the `<div class="portrait-placeholder">…</div>` block with:

```html
<img src="assets/portrait.jpg" alt="Nico Gallardo Ramos" width="800" height="800"
     style="border-radius:calc(var(--radius) - 5px); display:block">
```

**Résumé.** Drop a PDF at `assets/resume.pdf` — the About section already
links to it. Remove that link if you would rather not publish one.

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
4. Also update the fallback email in `js/main.js` (search for
   `hello@example.com`).

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

- [ ] Replace every `EDIT ME` block — no "Alex Rivera" anywhere
- [ ] Searching the project for `example` returns only things you meant to keep
- [ ] Add `assets/portrait.jpg` and `assets/resume.pdf`
- [ ] Configure or remove the contact form
- [ ] Add `assets/og-image.png` (1200×630) so shared links show a preview
- [ ] Test on your phone
- [ ] Toggle dark mode and check both themes
- [ ] Print the page (Ctrl+P) — it is styled to come out as a clean résumé
- [ ] Run it through <https://pagespeed.web.dev>
