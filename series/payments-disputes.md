---
layout: page
title: Payments Disputes
---

A short series on how card disputes actually work, from the moment a cardholder taps "dispute this charge" through chargebacks, representment, and arbitration. Written from my time working on dispute resolution systems, including some [patented work](https://patents.google.com/patent/US20240289807A1/en) on evidence recommendations.

{% assign series_posts = site.posts | where: "series", "payments-disputes" | sort: "series_order" %}
<ol>
{% for post in series_posts %}
  <li><a href="{{ post.url }}">{{ post.title }}</a></li>
{% endfor %}
</ol>

{% if series_posts.size == 0 %}
<p class="message">First post coming soon.</p>
{% endif %}
