# I Do Weddings, website changes

**Site:** https://idoweddings.nz
**Period:** 26 August 2026
**From:** `5d33972` (before)  →  **To:** `f913613` (deployed now)
**Scope:** 12 files, 692 insertions, 143 deletions

Everything below is live.

---

## 0. The thing worth knowing first

The site is **not on Wix**. It is five hand-coded HTML pages in a GitHub repo
(`Repo-Riot/ido.nz`) that publish themselves to idoweddings.nz through GitHub Pages
whenever anything is pushed to `main`. There is no platform subscription, no account
to hand over, and nothing to migrate. Any quote to "take over the site" is a quote to
edit five files.

---

## 1. Homepage rebuilt as one scrolling page

**Before:** the homepage was a hero slideshow, two paragraphs, one testimonial and two
buttons. Everything a visitor actually wanted (prices, services, more reviews, how to
get in touch) was behind a click on another page. 135 lines.

**After:** one page you scroll straight through. 596 lines.

| Section | What's in it |
|---|---|
| Hero | Photo carousel, location line, two calls to action |
| Intro | Headline, positioning line, three credibility cards |
| Meet Alex | Photo and bio, links out to the full About page |
| Services & packages | All four packages with prices and expandable detail |
| Love & thanks | Featured testimonial plus three more |
| Book a free intro call | Booking section, currently "coming soon" |
| Contact | Details plus a working enquiry form |

Nav links now scroll down the page instead of loading a new one. The sub-pages still
exist and still work, both for direct links and so Google has more to index.

---

## 2. Contact details

These were wrong on every page.

| | Before | After |
|---|---|---|
| Email | alextofield@me.com | alexidoweddings@gmail.com |
| Instagram | @idoweddings_nz | @idoweddings_matakana |
| Location | "Based in Auckland" | "Based in Matakana" |

---

## 3. Enquiry form (new)

There was no form at all. The contact page said "No forms needed, just reach out
directly", which asks a stranger to compose an email from scratch.

There is now a form on the homepage: name, email, phone, wedding date, which package
they're interested in, and a message.

- The package picker is five pill buttons rather than a dropdown. Dropdowns render
  differently on every browser and are awkward to tap on a phone.
- With no form service configured it opens the sender's email app with everything
  pre-filled and addressed to Alex. Works immediately, costs nothing.
- A Formspree endpoint can be pasted into the config block later if Alex would rather
  submissions arrive automatically.

---

## 4. Booking

New section with "Book a call" buttons in the nav, the hero, and on all four service
cards. All driven by one setting, so the link only ever needs pasting once.

Currently showing **"Online booking coming soon"** with the phone number and enquiry
form underneath, so nobody hits a dead end.

**Outstanding, needs Alex.** Google appointment schedules can only be created in the
Calendar interface, and it has to be her account so confirmations reach her inbox.
Signed in as alexidoweddings@gmail.com: Create → Appointment schedule → "Free intro
call", 20 min → set her hours → collect name, email, phone → turn on email reminders
→ Save → Share → copy link.

Free personal Google accounts get exactly one booking page, which is all that's
needed. Multiple pages, taking payments and spam verification need a paid plan.

Then one line in `index.html`:

```js
window.IDO_CONFIG = { BOOKING_URL: "https://calendar.app.google/...", ... }
```

That switches every booking button on at once.

---

## 5. SEO

This was the weakest part of the site, and most of it was fixable.

### Page titles

The title is the single biggest on-page ranking signal and it's the blue line people
click in search results. Every page said "I Do" and nothing else.

| Page | Before | After |
|---|---|---|
| Home | `I Do - Home` | `Wedding Planner Matakana & Auckland \| I Do Weddings, Alexandra Tofield` |
| About | `I Do - About` | `About Alexandra Tofield \| Wedding Planner Matakana & Auckland` |
| Services | `I Do - Services` | `Wedding Planning Packages & Prices \| Matakana & Auckland` |
| Reviews | `I Do - Love & Thanks` | `Wedding Reviews & Real Couples' Stories \| I Do Weddings Matakana` |
| Contact | `I Do - Contact` | `Contact Alexandra Tofield \| Wedding Planner Matakana & Auckland` |

### Everything else

- **Meta descriptions** on all five pages. There were none, so Google was inventing
  the snippet under each result.
- **Canonical URLs** on all pages.
- **Open Graph and Twitter cards.** Previously absent, so sharing a link anywhere
  produced a blank grey box with no image or description.
- **Structured data** (`ProfessionalService` JSON-LD): business name, founder,
  address, areas served, phone, email, Instagram, and all four services with prices.
  This is what Google reads to decide whether to show a local business.
- **robots.txt and sitemap.xml.** Neither existed.
- **Language** set to `en-NZ` rather than `en`.
- **Logo alt text** was "I Do – Weddings & Events logo by Alex", which is where the
  "Events Logo by Alex" confusion came from. Now describes the business.
- **Descriptive alt text** on every photo, naming the service and the location.

---

## 6. Page speed

Photos were being served at full camera resolution. Page speed is a direct Google
ranking factor and a slow first photo is the most common reason people leave.

| Image | Before | After |
|---|---|---|
| hero1.jpg | 8.3 MB | 430 KB |
| hero2.jpg | 5.7 MB | 462 KB |
| hero3.jpg | 9.1 MB | 238 KB |
| testimonial1.jpg | 14.5 MB | 164 KB |
| Alex Picture.JPG | 1.7 MB | 237 KB |
| **Total** | **39.3 MB** | **1.5 MB** |

96% smaller, no visible quality loss. Resized to a sensible maximum width and
recompressed. The full-resolution originals are untouched in `assets/images/`.

Also added width and height attributes and lazy loading, so the page stops jumping
around while photos load.

---

## 7. Photo cropping

The testimonial and service photos are portrait (427×640) but were being displayed in
short wide boxes. The browser kept the middle and threw away the rest, which cut the
tops off people's heads.

Boxes now match the shape of the photos and anchor to the top, where faces are.
Testimonial cards went from showing roughly 40% of each photo to 84 to 89%.

---

## 8. Mobile

Tested at 360, 375, 390 and 768 pixels wide.

- **Sticky header was 183px, 27% of an iPhone SE screen**, and it stayed there the
  whole time you scrolled, because the nav wrapped onto two rows. Now 107px. Smaller
  logo on phones, one row of links, and the "Book a call" pill hides below 640px since
  the hero and every service card already offer it.
- **Hero location line sat across the couple's faces** and was hard to read against a
  bright sky. Now one line, stronger gradient behind it, smaller buttons.
- Anchor scroll offsets now match the real header height, so tapping a nav link no
  longer lands with the heading hidden behind the header.
- No horizontal overflow at any width tested.

---

## 9. Bug fixed

The homepage carousel threw a JavaScript error on every rotation
(`ReferenceError: di is not defined`), so the position dots never updated. Rewritten,
and the dots are now clickable.

---

## Still outstanding

| # | Item | Owner |
|---|---|---|
| 1 | Google Calendar booking link, section 4 above | Alex |
| 2 | New photos from the recent shoot, including the floral dress ones | Alex |
| 3 | Landscape alternatives for the two portrait service photos, which still lose about half their height | Alex, optional |
| 4 | **Google Business Profile.** Free. For "wedding planner Matakana" searches this matters more than anything on the website, because the map results at the top come from Business Profiles, not sites | Alex |
| 5 | Submit the sitemap in Google Search Console so the new pages get crawled promptly | Charlie |
| 6 | Decide on admin access for Alex: keep routing changes through Charlie, add her to the repo, or add a free CMS layer such as Decap so she gets proper edit boxes | Both |

## What to expect from the SEO work

Not overnight. Titles, structured data, speed and the sitemap are the foundation and
take a few weeks to show. The Google Business Profile is the step most likely to
actually produce enquiries.
