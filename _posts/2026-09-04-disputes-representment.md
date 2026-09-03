---
layout: post
title: "Representment: How to win a dispute"
series: payments-disputes
series_title: Payments Disputes
series_url: /payments/#disputes
series_order: 3
---

Hey, if you're new here, I'm Anthony Ross. I've spent the last 10+ years working in fintech and ecommerce, most recently on disputes at Braintree. [Last post](/2026/09/02/disputes-who-eats-the-loss/) I walked through the actual costs of buying and selling shoes online, and what a dispute actually costs on top of that. This post is the other half of that story: what evidence gives merchants their best chance to win it back.

## Evidence isn't generic

The instinct most merchants have is to send everything they've got, the invoice, a screenshot of the order confirmation, maybe an angry email explaining they definitely shipped it. Most of that doesn't matter. What wins a dispute is evidence that speaks directly to what the customer is actually claiming, and that's different for every dispute type. If you try to prove answers to "did this arrive" but the claim is "I never authorized this," the issuer and network are not going to award you anything. Sending the wrong kind of strong evidence loses exactly as often as sending no evidence at all.

Two examples make this concrete.

## Physical goods: proof of delivery, not proof of shipping

Back to the shoes example. The losing version of that dispute had no delivery evidence, just an invoice and a shipping label. The winning version has tracking that shows delivered status, ideally with a signature or a photo at the door, tied to the actual address on the order. "I shipped it" and "it arrived" are different claims, and only the second one is evidence. A carrier scan showing the package left your warehouse proves you did your job, it does not prove the customer got it, and "never received it" disputes are specifically about whether it got there, not whether it left.  The reason a signature goes a long way is because of porch pirates, a buyer can claim the goods never made it to their hands a lot of the time, and even with a picture of the package on a porch, the buyer will win.

## Digital goods: proof of use, not proof of purchase

Now take a fraud claim on a digital good, say you sell a SaaS product and a customer says they never made the purchase. An invoice doesn't help here either, of course there's an invoice, that's not in dispute, but whether they authorized it is. What actually moves the needle: IP address and device info at the time of purchase matching their normal account activity, login and usage records showing the account kept using the product with the same device data before and after the "unauthorized" charge. If someone's account logged in from the same device the day after a charge they're now disputing as fraud, and used the thing they supposedly never bought, that's a very different story than a stolen card used once and never touched again. Same underlying question as the shoes, does the evidence match the actual claim, but the evidence itself looks nothing alike.

## Why this is hard to do well

Every network has slightly different rules for what counts, Visa's compelling evidence requirements aren't Mastercard's, and they change. Knowing which category of evidence actually matters for a given reason code, before you spend time assembling the wrong packet, is most of the battle. That's specific and repetitive enough that it's a real machine learning problem, not just a checklist, and it's what [Raymond Buhr](https://patents.google.com/patent/US20240289807A1/en) and I ended up patenting at Braintree, [now live as Dispute Evidence Recommendations](https://developer.paypal.com/braintree/articles/risk-and-security/chargebacks-retrievals/disputing-chargebacks#dispute-evidence-recommendations). I'll go deeper on how that system actually works in the next post, what it's ranking, and why the recommendations get better the longer it runs.
