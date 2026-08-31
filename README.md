# AR Blog

This blog uses the Hyde theme for Jekyll.  It is a fork of the original Hyde theme by Mark Otto.  The original Hyde theme can be found [here](https://www.github.com/poole/hyde).


## Local Dev

Reset baseurl because github pages is hosted at the root of the domain.

```
bundle exec jekyll serve --baseurl '/'
```

## Series (e.g. Payments Index)

Jekyll has no native "series" concept, only tags/categories. Series here are hand-rolled with front matter and one hub page per top-level topic (e.g. `payments.md` -> `/payments/`).

To add a post to an existing series:

```yaml
---
layout: post
title: Some post title
series: payments-disputes       # groups posts together, used to count "Part N of M"
series_title: Payments Disputes # display name for the "Part N of M in ..." link
series_url: /payments/#disputes # where that link points, an anchor on the hub page
series_order: 2                 # sort order within the series
---
```

`_layouts/post.html` reads `page.series`/`series_order`/`series_title`/`series_url` and renders a "Part N of M in ..." line automatically, no changes needed there for a new post in an existing series.

To start a brand new series/category:

1. Add a new `##`/`###` section to `payments.md` (or a new top-level hub page if it's not a payments topic) under the right category.
2. Under that heading, add a post-listing loop, copy the pattern already used for Disputes:
   ```liquid
   {% assign my_posts = site.posts | where: "series", "some-series-slug" | sort: "series_order" %}
   <ol>
   {% for post in my_posts %}
     <li><a href="{{ post.url }}">{{ post.title }}</a></li>
   {% endfor %}
   </ol>
   ```
3. Kramdown auto-generates heading IDs from the heading text (`### ACH` -> `id="ach"`), so `series_url: /payments/#ach` just works, no extra markup needed.
4. Write posts with matching `series: some-series-slug` and an incrementing `series_order`, per the front matter block above.

A hub page only shows up in the left sidebar automatically if it uses `layout: page` and has a `title` (see the loop in `_includes/sidebar.html`). Keep that layout reserved for actual top-level nav destinations (like `payments.md`, `about.md`), not per-series sub-pages, or the sidebar gets cluttered with one link per series.
