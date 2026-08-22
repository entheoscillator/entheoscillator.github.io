# Entheoscillator — band site

One self-contained file: `index.html`. No build step, no dependencies, no
node_modules. Open it in a browser to see it; push it anywhere to ship it.

## The idea behind the design

The band name already contains the thesis — *entheogen* + *oscillator* — so the
sacred geometry on this site isn't decoration pasted on top. The figure in the
hero is drawn live: three rotating vectors — one oscillator per member of the
trio — summed in the plane, on a canvas, in real time.

It drifts through **just-intonation triads** — 4:5:6 (major), 10:12:15 (minor),
6:7:9 (septimal), 3:4:5, 5:6:7, 2:3:4, 8:9:12, 9:11:12 — holding each for nine
seconds. The triad on screen is printed in the left rail. Those are the same
ratios the statement says the band tunes to, so the geometry and the tuning are
literally the same set of numbers.

The mapping is exact, not decorative. For a triad `a:b:c` the three oscillators
run at frequencies `1`, `1-b` and `1+b`, which produces **b-fold rotational
symmetry** — so the middle voice sets how many petals the figure has (4:5:6
gives five, 2:3:4 gives three) and the outer two set how deep they cut. Integer
frequencies close over a single 2π sweep, so each triad resolves into a finished
rosette; while it eases from one triad to the next the frequencies are
fractional, the curve no longer closes, and it blooms open. Three nested copies
at different scales and phases give the interference texture.

The figure owns the first screen, then fades back to 22% as you scroll so it
never fights the body copy.

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

- `prefers-reduced-motion` is respected: the harmonograph draws one resolved
  3:2 figure and stops, and entry animations are cut to near-zero.
- Keyboard focus is visible (gold outline).
- The canvas is `aria-hidden` — it's atmosphere, not content.
- The page reads correctly with the animation never running; nothing important
  is only revealed by motion.
- The canvas loop pauses when the tab is hidden.
