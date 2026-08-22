# Entheoscillator — band site

One self-contained file: `index.html`. No build step, no dependencies, no
node_modules. Open it in a browser to see it; push it anywhere to ship it.

## The idea behind the design

The band name already contains the thesis — *entheogen* + *oscillator* — so the
sacred geometry on this site isn't decoration pasted on top. The figure in the
hero is drawn live from three oscillators — one per member of the trio — and it
**breathes between two readings of the same numbers**:

- a flat, orderly **mandala**: the three oscillators summed as vectors in the
  plane. Frequencies `1`, `1-b` and `1+b` give exact **b-fold symmetry**, so the
  triad's middle voice sets the petal count.
- a tumbling **3D knot**: a 3D Lissajous with one oscillator per axis — x, y and
  z — projected back to the screen with yaw and pitch.

Both are parametric curves over the same 0→2π sweep, so the morph is a
point-by-point blend before projection. Yaw and pitch scale with the blend too,
which is why the mandala sits flat and square to you at one end and only tumbles
as depth comes in. Round trip is about 65 seconds, smoothstepped so it dwells at
each figure long enough to be read.

It drifts through **just-intonation triads**: 4:5:6 (major), 10:12:15 (minor),
6:7:9 (septimal), 5:6:7, 8:9:12, 9:11:12, 7:9:11 — holding each for nine
seconds. The triad on screen is printed in the left rail. Those are the same
ratios the statement says the band tunes to, so the geometry and the tuning are
literally the same set of numbers.

Integer frequencies close over one 2π sweep, so every triad resolves into a
finished figure; while it eases from one triad to the next they're fractional,
the curve no longer closes, and it opens up. A wider violet copy tumbles the
opposite way underneath as the drone.

### The retrace beam

A vector-display spot runs each curve with a short phosphor tail — one lap every
~7 seconds, slow enough to read as a beam rather than a flicker. Every layer
gets its own: gold on the voice, a dimmer violet one on the drone beneath. It
rides the buffers `trace()` has just filled, so it can never drift off its
curve. Tuned to sit just at the edge of noticeable — it should register as the
figure being *drawn* rather than as a moving dot.

### Choosing a figure

Near the top of the `<script>` block:

```js
var FIGURE = 'morph';   // 'morph' | 'rosette' | 'knot'
```

`rosette` holds the flat mandala, `knot` holds the 3D Lissajous. You can also
override it without editing the file — handy for comparing:

```
https://entheoscillator.com/?figure=rosette
https://entheoscillator.com/?figure=knot
https://entheoscillator.com/?figure=0.5    (pin a point mid-morph)
```

Costs about 0.4ms/frame under software rendering — roughly 40× under budget —
and pauses entirely when the tab is hidden.

## Sharing card and icons

`og.png` (1200×630) is the link preview — the one that shows in iMessage, Slack,
Discord, WhatsApp, Facebook and X. It's generated from **`og-card.html`**, which
runs the site's own figure code, so the preview is the real geometry rather than
a mockup. It's held at the orderly mandala; the 3D knot turns to noise at
thumbnail size in a feed.

To regenerate it:

```sh
python3 -m http.server 8080
google-chrome --headless --disable-gpu --hide-scrollbars \
  --virtual-time-budget=12000 --window-size=1200,630 \
  --screenshot=og.png http://localhost:8080/og-card.html
```

Then **bump `?v=`** on `og:image` and `twitter:image` in `index.html`. Scrapers
cache aggressively by URL — without a new query string, Facebook and Slack will
keep serving the old card for days.

The icon is the same three-circle triad mark as the members section, chosen
because it survives 16px; the hero figure turns to mush at that size. Source of
truth is `favicon.svg`; `favicon.ico`, `apple-touch-icon.png` and `icon-192/512`
are rendered from it. If you change the SVG, re-render the rest.

## Things you need to edit

Search `index.html` for `EDIT:` — there are two.

1. **The show** (`<section id="live">`) — festival name, date, and city are
   placeholders reading "TBA". Replace once the festival is confirmed. Note the
   Live section deliberately still says Santa Cruz — that's where the gig is,
   even though the group is billed as Bay Area.
2. **The email** (`<section id="contact">`) — `hello@entheoscillator.com` is a
   placeholder. **It is not a real address.** Swap in whatever you actually
   want bookings going to before this goes public.

Also worth a look before launch:

- **The instrument lists** (`<section id="instruments">`) — I wrote plausible
  ones for a group doing electronics plus ceremonial instruments. Change them
  to what the three of you actually bring.
- **Member bios** — deliberately absent. The trio section presents the three of
  you as three vertices of one figure, with a line saying bios are coming. When
  you have them, the cleanest move is a short paragraph under each name; the
  `.node` blocks are already positioned for it.

## Adding member bios later

Each name lives in a `.node` div in `<section id="trio">`:

```html
<div class="node node--a">
  <p class="node__name">Soumendra Barman</p>
  <p class="node__role">Oscillator I</p>
</div>
```

`node--a` / `node--b` / `node--c` are positioned at the triangle's vertices.
Below 760px the triangle is replaced by a plain stacked list (`.trio__list`),
so if you add bios, add them in **both** places or switch the mobile list to
show the same markup.

## Deploying

Any static host. It's one file.

- **GitHub Pages** — commit `index.html` to the repo root, then Settings →
  Pages → deploy from branch. Point a custom domain at it with the
  `namecheap-dns` skill if the domain is on Namecheap.
- **Netlify / Vercel** — drag the folder onto the dashboard.
- **DigitalOcean App Platform** — static site component, no build command.

Only external request is the Google Fonts stylesheet. If you'd rather have zero
external requests, download Eczar, Spectral, and IBM Plex Mono and swap the
`<link>` for a local `@font-face` block.

## Type and colour

| Role | Face | Why |
|---|---|---|
| Display | **Eczar** | A Latin serif drawn as a companion to Devanagari — carries the harmonium/tanpura side honestly, with enough energy to hold a wide-tracked wordmark |
| Body | **Spectral** | Screen-first serif, low contrast, comfortable at long reading lengths |
| Data | **IBM Plex Mono** | The instrument-panel voice: ratios, dates, labels |

```
--ground     #0B0A14   warm indigo-black, never pure black
--gold       #C9A24A   the voice
--gold-lit   #E8CE8A   highlight
--violet     #6E5AA8   the drone, underneath
--bone       #EDE6D6   type, never pure white
```

## Accessibility

- `prefers-reduced-motion` is respected: the figure draws one resolved 4:5:6
  state and stops, and entry animations are cut to near-zero. Reduced motion
  gets the calm, orderly mandala rather than the tumbling knot.
- Keyboard focus is visible (gold outline).
- The canvas is `aria-hidden` — it's atmosphere, not content.
- The page reads correctly with the animation never running; nothing important
  is only revealed by motion.
- The canvas loop pauses when the tab is hidden.
