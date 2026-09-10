# The Feeling Expert — homepage

Trauma therapy practice site for Elyce, Boca Raton FL. Single self-contained
`index.html`, no build step. Open it directly or serve the folder.

## Architecture

Built on the scroll system behind [huts.com](https://huts.com). One decision
underpins the whole thing:

> **Lenis is the only smoothing layer.** Every scroll-linked animation is
> declared linear and bound rigidly to scroll position. The softness you feel
> is the scroll value being interpolated once, and it propagates into every
> animation downstream. Easing a scrub on top of an already-eased scroll is
> what produces the "swimming" feel most smooth-scroll sites have.

Section heights are **derived, never authored**, so vertical scroll maps ~1:1
to horizontal travel:

```js
sectionHeight = panelHeight + Math.max(0, trackWidth - sectionWidth)
```

### Design tokens

| | |
|---|---|
| Type | Fraunces (display) · Karla (body) |
| Headings | `#92205C` · body `#6A696F` · accent `#D15AA3` |
| Rules | heading colour at 16% alpha — never a stray grey |
| Easing | `cubic-bezier(.33,1,.68,1)` everywhere, broken once for the card flip |
| Space | every value a `clamp()`; no breakpoint spacing overrides |
| Grid | 12 columns desktop / 8 mobile, one reusable `calc()` |

### Gotchas worth keeping

These were all real bugs during the build — don't reintroduce them.

- **Never put `overflow-x` on `html`/`body`.** Any value but `visible` makes
  body the scrollport and every `position: sticky` on the page silently dies.
  Horizontal overflow is clipped at source instead.
- `--vw` is set from `clientWidth` in JS because `100vw` includes the
  scrollbar and pushes full-width spans into overflow.
- `.scroll-content` needs `grid-template-columns: minmax(0,1fr)` or the auto
  column sizes to the track's max-content and stretches the section.
- `.viz-art` needs explicit `height:auto` — with only `max-height`, a replaced
  element sized by `width:100%` inside a grid resolves height to the max and
  letterboxes.
- On touch there is no hover, so the flip cards **merge** front and back
  rather than hiding the back — otherwise the qualifier copy vanishes on
  mobile.

## Layout

```
index.html                 the site
images/                    optimised, web-ready assets
icons/                     original icon pack (source)
Elyce website content/     drop originals here; they get optimised into images/
_source/                   original site copy + markup, and the authority band
                           section removed for later reuse
```

Images are resized to ~2× display size and converted to WebP, except the three
pillar illustrations, which are PNG for alpha (their cream ground is cut out
so they sit on the pink panels).

## Still needs Elyce

Search the markup for `Needs Elyce` — flagged inline:

- Two client stories (one is written, two are placeholders)
- Fees: `$[FEE]` for `[LENGTH]` minutes, and the sliding-scale line
- "What actually happens in a session"
- Years in practice + women supported, for the authority band
- Blog post titles and dates

The `.needs` badges are author-only and should be deleted before launch.

## Accessibility

Improves on the reference site in four places it falls short: `prefers-reduced-motion`
is honoured throughout (Lenis is never mounted and every scrub is skipped),
focus rings are authored, images are lazy-loaded below the fold, and heading
levels don't skip. Mobile has no tap target under 44px.
