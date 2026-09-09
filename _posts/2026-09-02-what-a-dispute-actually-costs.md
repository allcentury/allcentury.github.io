---
layout: post
title: "Why a dispute costs $229 on a $129 pair of shoes"
series: payments-disputes
series_title: Payments Disputes
series_url: /payments/#disputes
series_order: 2
redirect_from:
  - /2026/09/02/disputes-who-eats-the-loss/
---

Hey, if you're new here, I'm Anthony Ross. I've spent the last 10+ years working in fintech (Brex + Braintree/Venmo). [In my last post](/2026/08/30/disputes-what-happens-when-you-tap-dispute/) I walked through what happens the moment you tap dispute, the five parties, the fraud vs. everything-else fork, how much time everyone actually has, etc. This one answers the question that actually matters to a merchant: once the dispute lands, who pays for it? or rather, who pays and _who gets paid_ and _where did all my money go_...

The answer isn't "the person who did something wrong." It's usually "whoever has the weaker evidence or tech" which is a very different thing, and it's why disputes are so expensive.

## Failed to deliver: the one that actually hurts

A "never received it" dispute is the one that catches merchants off guard, because there's no liability-shift rule for it, no clean policy answer, just evidence or the lack of it. If you can't prove delivery, you lose, full stop, regardless of whether the customer is telling the truth or not.

Here's what that actually costs. Say you sold a pair of shoes for $129, shipping costs you $11, and your card processing fee was 3%, $3.87. A dispute comes in for the full $129, and you don't have delivery evidence, so you lose. Here's the damage:

```
lost sale, reversed by the chargeback    $129.00
shipping, already paid, gone              $11.00
card fee, not refunded on a chargeback     $3.87
cost of the shoes, gone                   $70.00
flat dispute fee, charged either way      $15.00
------------------------------------------------
total loss                               $228.87
```

$228.87 lost on a $129 sale, 1.77x the original transaction, and that's a mild case. Higher margins, or enough of these stacking up that your acquirer drops you into a monitoring program, and the real multiple climbs fast. The chargeback fee alone is worth calling out, it's non-refundable even if the merchant fights the dispute and wins later. Winning gets you your $129 back. It does not get you your $15 back, and on some smaller transactions, $15 might be greater than your actual margin.

## Card-present fraud: the EMV liability shift

Contrast that with card-present fraud, where the networks actually did engineer a clean answer. If you've ever wondered why every card terminal on earth suddenly wanted you to insert your chip instead of swipe, this is why. [EMV](https://www.emvco.com/) stands for Europay, Mastercard, and Visa, the three companies that created the chip standard, and the rule, in plain terms, is that liability falls on whichever side is using the weaker technology.

- When a Merchant doesn't support chip, or has a chip reader but processes it as a swipe anyway, and the transaction turns out fraudulent, the merchant eats it - everytime.
- When a Merchant has a chip-enabled terminal, but the card itself is an old mag-stripe-only card with no chip to insert, so it gets swiped instead, the issuer eats it (usually).
- Both sides are chip-compliant and the transaction still turns out to be counterfeit fraud (the chip got cloned some other way), the issuer is typically still on the hook. Neither party did anything wrong, so the loss falls back to whoever's supposed to be backstopping fraud in the first place.

That's a genuinely elegant piece of policy design, actually, it doesn't try to figure out who's at fault, it just makes upgrading your security the economically rational move for everyone. Once EMV adoption crossed a threshold, this stopped being a live problem for most merchants.  The schemes (Visa, Mastercard, etc) know how to incentivize fraud measures, by pushing the cost back to the weakest link in the payments chain.

## Why non-delivery is the one that stays expensive

The EMV shift is a policy lever, upgrade your tech and the risk moves off you. Non-delivery disputes don't have that lever. The only thing standing between you and this $228.87 problem is whether you can produce a delivery confirmation, a signature, tracking that actually shows it arrived, something. No evidence, no defense, and it doesn't matter that you actually shipped the shoes.

That's the setup for the next post. Merchants aren't defenseless here, they can fight back with evidence, and what actually counts as evidence, what doesn't, and why most merchants don't bother even when they have a winning case, is where representment comes in.
