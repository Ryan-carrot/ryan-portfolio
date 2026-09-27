# Hexry Development: Ryan's Portfolio

The site I show local business owners when they ask what I do.
Live at https://ryan-portfolio-tau.vercel.app

It scores 100 / 100 / 100 / 100 on Lighthouse mobile, measured on the
deployed URL. That matters more than it sounds: the site's whole
argument is "I make sites fast," so it had better be one. Local
Lighthouse runs don't count. I've watched the same page score 37, 50,
and 41 on three consecutive days in PageSpeed Insights, and any client
can run that test themselves.

Business owners in the Douglas County area will no longer have to pay
premium prices for sub-par site performance. With Hexry Development,
nothing goes out unless all PSI are green, SEO is tight, and the page/site
captures the essence and quality of the products or services the business
offers.

## Stack

HTML, CSS, a little vanilla JavaScript. No framework, no build step,
nothing in `node_modules` because there is no `node_modules`. There is
no need to power a daily driver with jet fuel.

This is a page with words and pictures on it. It doesn't need React.
Every dependency is something that can break, go stale, or need
explaining to whoever inherits the site, and the client sites I build
follow the same rule, because at the end of the day a business owner
should be able to drive their site if they so choose.

## Things I got wrong first

Every rule below exists because I measured something on a client site
and didn't like the number.

**Fonts are self-hosted:** I shipped a site that loaded two fonts from
Google. Fine, everyone does it. Then I looked at the request chain:
HTML → Google's CSS → Google's font files, three round trips before a
single word could render, about 800ms on a throttled phone. Two `.woff2`
files in `/fonts`, preloaded, `font-display: swap`. One hop.

**The hero paints before JavaScript runs:** A client site was stuck at
91 and I couldn't see why. The LCP breakdown showed 2,040ms of "element
render delay" on a paragraph. The paragraph was sitting at `opacity: 0`
waiting for a script to add a class and fade it in, and Lighthouse
doesn't count invisible text as painted. The fade-in is CSS now, firing
at first paint. Same animation, no gate. Below the fold I still use
IntersectionObserver; above the fold I don't need it.

**Only `transform` and `opacity` animate:** Those run on the compositor
thread and cost essentially nothing. Animating `width`, `height`, `top`,
or `left` triggers layout, and layout is where frame budgets go to die.
If an effect can't be done with transform and opacity, I don't do it.

**`prefers-reduced-motion` is honored everywhere:** Not just the hero,
the whole motion system. And reduced-motion users get content in its
final state, not stuck at `opacity: 0` because the animation that would
have revealed it got suppressed. That second part is the one people
miss.

**`script.js` is deferred:** Safe only because of the hero fix above.
I tried deferring first, once. LCP got worse.

## Case studies

Before-and-after PageSpeed comparisons on real local business sites.
Every number was captured in the PageSpeed Insights UI on the live URL,
because a score I got in a local terminal is a score I can't show a
client.

## Running it

Open `index.html`.

## Deploying

Vercel, on push to `main`.

## What isn't here

Planning notes, client details, and pricing live in a folder next to
this one, not in it. Git history is permanent. I learned that the easy
way, by checking before the first push.
