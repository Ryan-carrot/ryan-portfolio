# Ryan — Portfolio

Static site for local SEO and site management work in Roseburg, OR.
Live at https://ryan-portfolio-tau.vercel.app

Lighthouse mobile, live URL: 100 / 100 / 100 / 100. Verified on the
deployed site, not a local build — the score is part of the pitch, so
it's held to the same standard as client work.

## Stack

HTML, CSS, vanilla JS. No framework, no build step, no dependencies.

A portfolio for local businesses doesn't need React. A single HTML file
loads faster, has nothing to go stale, and can be handed to anyone. The
same reasoning drives the client sites this portfolio shows.

## Performance decisions

Each of these came from a measurement on a client site, not a best-
practices list.

**Self-hosted fonts, preloaded, `font-display: swap`.** Google Fonts
adds a three-hop chain (document → CSS → woff2) before any text can
render. On one client site that chain cost ~800ms. Two woff2 files in
`/fonts` cost one request each.

**Above-the-fold content paints without JavaScript.** The hero
animation is CSS `@keyframes` firing at first paint, not a JS class
toggle on an `opacity: 0` element. A JS-gated hero on a client site
added 2,040ms of element render delay — Lighthouse doesn't count
transparent text as painted. Below-the-fold reveals still use
IntersectionObserver; the hero doesn't need it.

**Transform and opacity only.** Every animation runs on the compositor
thread. Nothing animates `width`, `height`, `top`, or `left`.

**`prefers-reduced-motion` is honored** for the full motion system, and
reduced-motion users get content in its final state — never stuck at
`opacity: 0` because the animation was suppressed.

**Deferred script.** Safe only because the hero is JS-independent.
Deferring first would have made LCP worse.

## Case studies

The site's case studies are before/after PageSpeed comparisons on real
local business sites. Every score shown was captured in the PageSpeed
Insights UI on the live URL. Local Lighthouse runs don't count — they
vary from PSI by enough to matter, and the client can run PSI themselves.

## Running locally

Open `index.html`. That's it.

## Deploying

Vercel, on push to `main`.

## What's not here

Planning docs, client notes, and pricing live outside the repo. Git
history is permanent; internal notes don't belong in a public one.
