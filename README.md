# classifido-site

The static site at [classifido.com](https://classifido.com). No build step, no
framework, no dependencies, no JavaScript: GitHub Pages serves these files as
they are.

## The lockup is a copy, not an implementation

**The ClassıFido lockup on `index.html` — its markup, the mortarboard cap's
placement and its three `em` steps, the type scale, the spacing variables at
all three breakpoints, the ground and the mark — is a COPY of the output of
`src/render.py` in the product repo, taken from what `render.sign_in_page()`
actually returned.** If the sign-in page's lockup or scale changes there, this
copy goes stale silently and must be re-copied from that file — nothing in
either repo will tell you.

### Why a copy rather than a third drawing of it

The cap's optical scale is already a deliberate duplication. `web/src/
Wordmark.jsx` and `src/render.py` both hold it because Python cannot import
JSX, and `tests/test_wordmark_cap.py` holds the pair together with a shared
ten-input golden table. A third hand-drawn copy here would be a third answer
to the same question with no test behind it, so this repo takes the rendered
artifact instead and draws nothing of its own.

### Re-copying it

From a checkout of the product repo:

```bash
python -c "import sys; sys.path.insert(0,'src'); import render; print(render.sign_in_page('/start'))" > signin.html
```

The values to lift are the `body` variable block and its two `@media` blocks,
the `.frame` / `.watermark` / `.column` / `.mark` / `.type` / `.wordmark` /
`.i` / `.cap` / `.fido` / `.tagline` rules, the three `.cap` size rules that
`render.sign_in_cap_css()` appends, the `<div class="wordmark">` run, and the
three `data:` URIs on the watermark, mark and cap images. The fonts in
`fonts/` are the same files the product serves, from `assets/fonts/`.

## What this repo deliberately does not match

- **Links are 600, where the sign-in page's `.secondary` is not.** That control
  is a de-emphasised way out from under a white button; these are the only
  controls on the page, and `web/src/link.css` states that an action link takes
  the accent and 600. The underline face itself is identical.
- **There is a `padding-bottom` on `.frame` and negative margins on the link
  rows.** The first clears a phone's home indicator; the second buys 44px touch
  targets while giving the copied gaps back to the layout. Neither moves
  anything the sign-in page positions.

## Pages

- `/` — the landing page.
- `/privacy`, `/terms` — placeholders. Real content is pending the legal entity
  name; they carry the page title and "Coming soon." and nothing else.

## DNS

The apex is four `A` records at Porkbun pointing at GitHub Pages
(185.199.108–111.153), with `www` a `CNAME` to `bradley-duitlabs.github.io`.
`CNAME` in this repo is what binds the domain to the site.
