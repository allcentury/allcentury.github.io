---
layout: page
title: Payments Index
---

Posts on payments, from ten years in fintech working credit card systems (Braintree) and banking rails (Brex, Venmo).

## Credit Cards

### Disputes

How card disputes actually work, from intake through chargebacks, representment, and arbitration. Some of it [patented work](https://patents.google.com/patent/US20240289807A1/en) from my time as a staff engineer and engineering manager on the disputes team at Braintree.

{% assign disputes_posts = site.posts | where: "series", "payments-disputes" | sort: "series_order" %}
<ol>
{% for post in disputes_posts %}
  <li><a href="{{ post.url }}">{{ post.title }}</a></li>
{% endfor %}
</ol>

{% if disputes_posts.size == 0 %}
<p class="message">First post coming soon.</p>
{% endif %}

## Banking

### ACH

Coming soon.
