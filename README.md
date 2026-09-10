# Vels Mobiles — landing page

Single static page, white/professional theme. No build step, no framework, no npm install.
One `index.html` (inline CSS + JS) plus static assets.

**71 KB HTML (~17 KB gzipped). Zero third-party requests on load** — no web fonts, no
libraries, no analytics. Google Maps loads only if the visitor taps the map; the Meta
Pixel loads only once you paste an ID.

---


## Run it locally

```bash
python3 -m http.server 8080
```

http://localhost:8080

## Deploy to Netlify (free tier)

Drag this folder onto https://app.netlify.com/drop — live in about ten seconds. Or:

```bash
npx netlify-cli deploy --prod --dir .
```

`netlify.toml` handles publish dir, caching and security headers. No build command, so
nothing can fail at build time.

---

## What I verified (and where it came from)

| Fact | Source |
|---|---|
| Apple iPhone specialist service | Their own Instagram reel |
| **Doorstep service, all of Chennai** | Owner-confirmed (reel: "DOOR STEP SERVICE IN YOUR CONVENIENT PLACE") |
| Established **2015** (→ "10 years", "Since 2015") | JustDial listing |
| Landmark: **opposite Unlimited Family Showroom** | JustDial listing |
| Address: 9/12, Girija Nagar West, Red Hills Main Road | Their Instagram bio |
| Instagram: **@velsmobiles** (Vels Mobiles_official_) | Direct |
| JustDial 4.9 ★ / 121 ratings | Owner-confirmed |
| Google 4.9 ★ / 113 reviews | You |
| 3 customer reviews (Gayathri Jayaraj, Levins A, Sharath R) | You, from Google |

So the "spot service" you mentioned is **doorstep pickup and delivery, covering all of
Chennai**. Since that's the widest-reach offer, the page is positioned city-wide rather
than as a Kolathur-only shop: the H1 says "anywhere in Chennai", the doorstep section
lists 24 Chennai localities, and the schema declares Chennai as the service area. The
Kolathur shop stays visible as the credibility anchor — ten years, a real address, a real
bench — which is what makes a city-wide doorstep promise believable.

---

## ⚠️ Before you run ads — must do

### 1. Is doorstep limited to iPhone 14–17?
Their own doorstep promo graphic (reel `DcNc9UPx7bI`, now `reel-1.mp4`) reads:

> Apple iPhone SERVICE — **ONLY FOR** — iPhone 14 Series to 17 Series
> DISPLAY Replacement · BATTERY Replacement · BACK DOORS

That "ONLY FOR" is the line that was cut off in the caption — and it's a **device
restriction, not a geographic one**, so it doesn't contradict Chennai-wide coverage.
But the page currently offers doorstep for any device. **Check whether doorstep pickup
really is limited to iPhone 14–17 series**, or whether that was just one promo.

If it is limited, say so on the page — it will save wasted ad spend on people with an
iPhone 11 who message and then get turned down.

### 2. "6 months warranty" — does it apply to everything?
The same graphic advertises **6 MONTHS WARRANTY** and **CERTIFIED SPARES**. The page
currently says only "warranty on every repair". A concrete "6-month warranty" is a much
stronger ad hook — but I left it generic because the graphic is for the 14–17 series
promo and I can't tell if it covers all repairs. Confirm the scope and I'll make it
specific.

### 3. A storefront photo is the one gap left
Every photo on the page is now a real frame pulled from their own reels — but **none of
their 12 reels contains a shot of the shop front, the signage, or anyone at the counter.**
It's all bench footage.

So the hero image is currently the repair bench (an opened iPhone, screw trays, tooling)
rather than the storefront. It works — it says "real workshop" immediately — but a photo
of the shop front with signage would do more for a local ad, because it proves there's a
physical place to walk into.

**Ask the owner for two phone photos:** the shop front with the board visible, and the
counter with a technician at it. Drop them in as `assets/shop-1.jpg` (900×765, landscape)
and `assets/shop-2.jpg` (800×800, square) and the page picks them up with no code change.
Update the two `alt` attributes to match.

### 4. Domain
The site is configured for **https://velsmobiles.com** — canonical URL, Open Graph,
Twitter card, sitemap, robots.txt and JSON-LD all point there. Set `velsmobiles.com`
(the apex, not `www`) as the **primary domain** in Netlify so it matches.

### 5. Meta Pixel — for the ₹300/day campaigns
In `index.html`, near the top of the `<script>` block:

```js
var META_PIXEL_ID = "";   // ← paste your Pixel ID
```

Once set:

| Event | Fires on |
|---|---|
| `PageView` | Load |
| `Lead` | Any WhatsApp click, and form submit — tagged `wa_form_doorstep` or `wa_form_walkin` |
| `Contact` | Any phone click |
| `FindLocation` | "Get directions" |
| `ViewContent` | Instagram / reel clicks |

Optimise for **Lead**. All 22 CTAs carry a `data-ev` name (`wa_hero`, `wa_doorstep`,
`call_bar`, …) passed as `content_name`, so Events Manager will tell you which button and
which offer — walk-in vs doorstep — is actually producing leads.

### 6. Add map coordinates to the JSON-LD (optional, helps local SEO)
Left out deliberately rather than guessing a pin. Right-click the shop in Google Maps to
copy the lat/long, then add inside the JSON-LD:

```json
"geo": { "@type": "GeoCoordinates", "latitude": 13.xxxx, "longitude": 80.xxxx },
```

---

## Photos — all real, all from their reels

There are no generated placeholders left. Each slot is a frame extracted from one of their
own reels, cropped to the slot's aspect ratio:

| File | Shows | Source reel |
|---|---|---|
| `shop-1.jpg` | Hero — opened iPhone on the bench, screw trays, tooling | `DVBGPOxkYL1` @13s |
| `shop-2.jpg` | Fitting a replacement back glass | `DVBGPOxkYL1` @21s |
| `shop-3.jpg` | Replacement display running a colour test | `DZ1h3rSKyi8` @14s |
| `shop-4.jpg` | iPhone opened to the logic board | `DZ1h3rSKyi8` @7.5s |
| `shop-5.jpg` | Sealed replacement battery before fitting | `DZ1h3rSKyi8` @20s |
| `shop-6.jpg` | iOS parts screen confirming a genuine battery | `DZ1h3rSKyi8` @22s |

Captions and `alt` text describe what is actually in each frame, so nothing on the page
claims something the photo doesn't show. `shop-6` is the strongest of them — an on-device
"genuine Apple part" confirmation is a trust signal most repair shops never put in front
of customers.

To re-cut any of them, the frame extractor and crop script is in the scratchpad
(`gen`/crop helpers), or just grab a frame:

```bash
ffmpeg -ss 13 -i reel.mp4 -frames:v 1 frame.png
```

## Instagram reels — self-hosted

The four reels in that row are **their own reels, downloaded and re-encoded locally**, not
Instagram embeds. No third-party script, no iframe, no tracking, and they work even if
Instagram is slow or the post is later deleted.

| Slot | Instagram shortcode | Shows | Size |
|---|---|---|---|
| `reel-1.mp4` | `DcNc9UPx7bI` | Doorstep service promo | 161 KB |
| `reel-2.mp4` | `DVYwzzskbfk` | iPhone 15 Pro Max restored | 417 KB |
| `reel-3.mp4` | `DVNZhzHkekz` | Apple Watch SE display replaced | 535 KB |
| `reel-4.mp4` | `DZ1h3rSKyi8` | Display repair on the bench | 1.1 MB |

**How they behave:** nothing downloads on page load. An IntersectionObserver fetches and
plays a clip only once it scrolls near the viewport, and pauses it when it leaves. Each
card falls back to its poster JPEG and downloads **zero** video bytes if the visitor has
Data Saver on, has reduced-motion set, or the browser blocks autoplay. Tapping a card
opens the real reel on Instagram — which is also where the audio is.

**Audio is deliberately stripped.** Instagram reels usually carry trending music licensed
for use *on Instagram*, not for rehosting on a commercial website. Silent loops sidestep
that entirely, autoplay reliably (browsers block sound), and cut file size by about 15%.

**To swap a clip**, drop a new MP4 in as `assets/reel-N.mp4`, refresh the poster, and
update the `href` and `<cite>` label. Encoder settings used:

```bash
ffmpeg -i input.mp4 -vf "scale=-2:960:flags=lanczos,fps=24" \
  -c:v libx264 -profile:v main -pix_fmt yuv420p -crf 33 -preset veryslow \
  -movflags +faststart -an output.mp4
```

Eight more of their reels were downloaded and are sitting in the scratchpad if you want
different ones — including a Samsung Flip 3 inner-display repair and two back-glass jobs.

## Reviews

The three reviews on the page are **real Google reviews**, reproduced word for word. Don't
paraphrase or tidy them — altering a quoted review is what turns a testimonial into a
fabricated one.

Deliberately **not** in the JSON-LD: individual `Review` markup. Google treats reviews a
business republishes on its own site as "self-serving" and won't show rich results for
them on a LocalBusiness — and marking them up anyway risks a structured-data manual
action. The `aggregateRating` (4.9 / 113, sourced from the Google listing) is the part
Google actually uses, and that's already there.

To add more later, copy an existing `<figure class="rev">` block. The avatar circle takes
the reviewer's first initial and the first three cards are tinted blue / green / amber.

## Targeting notes for the Chennai campaigns

The page is now built to take city-wide traffic:

- **H1 matches a city-wide ad promise** — "anywhere in Chennai", not "in Kolathur".
- **Two offers, tracked separately.** The quote form's "where should we service it?" field
  tags leads `wa_form_walkin` or `wa_form_doorstep`. After a few days you'll see whether
  Chennai-wide doorstep or local walk-in is cheaper per lead, and can split budget or
  audiences accordingly.
- **24 localities listed** in the doorstep section — makes "all of Chennai" concrete for
  someone in Velachery or Tambaram who'd otherwise assume a Kolathur shop won't serve them.
  Also useful organic surface area for "iPhone repair <area>" searches.
- **Radius suggestion:** target Chennai city rather than a radius around Kolathur — a
  radius pin would under-serve the south and the OMR belt, which is where doorstep pickup
  is worth the most.

## Page structure

Top bar (hours · phone · doorstep) → header → hero (photo + floating badges, dual ratings,
live open/closed) → 4 stat tiles → 12 services → **doorstep section** → shop gallery →
**Instagram reels** → 4-step process → reviews → **WhatsApp quote form** → address + hours
+ click-to-load map → CTA band → footer → sticky mobile Call/WhatsApp bar.

The quote form has no backend — it collects name, phone, device, issue, walk-in vs
doorstep, and area, then opens WhatsApp with the message pre-written. Nothing to host.

## Business details

- **Vels Mobiles** — Apple specialist, Kolathur, since 2015
- 9/12, Girija Nagar West, Red Hills Road, Kolathur, Chennai, TN 600099
  (opposite Unlimited Family Showroom)
- 098400 81414
- Google 4.9 ★ (113) · JustDial 4.9 ★ (121)
- https://share.google/2mtY5ieSRkj06kvcX
- https://jsdl.in/DT-20Y2MI2I6QQ
- https://www.instagram.com/velsmobiles/
