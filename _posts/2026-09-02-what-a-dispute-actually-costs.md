---
layout: post
title: "What a dispute actually costs"
series: payments-disputes
series_title: Payments Disputes
series_url: /payments/#disputes
series_order: 2
redirect_from:
  - /2026/09/02/disputes-who-eats-the-loss/
---

Hey, if you're new here, I'm Anthony Ross. I've spent the last 10+ years working in fintech (Brex + Braintree/Venmo). [In my last post](/2026/08/30/disputes-what-happens-when-you-tap-dispute/) I walked through what happens the moment you tap dispute, the five parties, the fraud vs. everything-else fork, how much time everyone actually has, etc. This one answers the question that actually matters to a merchant: once the dispute lands, who pays for it, and how much.

The answer to who loses isn't "the person who did something wrong." It's usually "whoever has the weaker evidence or tech" which is a very different thing, and it's why disputes are so expensive.

## Proof of delivery

A "I never received the goods" dispute is very common, especially for e-commerce companies and the dispute is extra costly because there's no liability-shift rule for it and no clean policy answer, just evidence or the lack of it. If you can't prove delivery, you lose, full stop, regardless of whether the customer is telling the truth or not.

Here's what that actually costs. Say you sold a pair of shoes for $129, shipping costs you $11, and your card processing fee was 3%, $3.87. If that sale goes through clean, here's your profit:

```
revenue                                   $129.00
cost of the shoes                         -$70.00
shipping                                  -$11.00
card processing fee                        -$3.87
------------------------------------------------
profit                                     $44.13
```

Now a dispute comes in, and you don't have delivery evidence, so you lose. The chargeback claws back the full $129, not the $125.13 you actually netted, the processor doesn't hand back their cut either. You already spent the shipping, you never get the shoes back, and there's a flat dispute fee on top of all of it:

```
cost of the shoes, gone                   -$70.00
shipping, already spent                   -$11.00
card processing fee, not refunded          -$3.87
flat dispute fee, charged either way      -$15.00
------------------------------------------------
net loss                                  -$99.87
```

The number that actually matters is the swing between those two outcomes, what the dispute cost you relative to the sale just working, $144.00. That's more than the sale price itself.

> Correction: an earlier version of this post added the cost of the shoes on top of the reversed sale price and put the total loss at $228.87. That's wrong, I had miscalculated it, the shoes are already inside the $129 that got reversed, not a separate loss stacked on top of it.

With enough of these, your acquirer will put you into a monitoring program that can raise your processing rate and dispute fees on top of everything else. The dispute fee itself is worth calling out, it's non-refundable even if the merchant fights the dispute and wins later. Winning gets you your $129 back. It does not get you your $15 back, and on some smaller transactions, $15 might be more than your entire margin.

## Card-present fraud: the EMV liability shift

Contrast that with card-present fraud, where the networks actually did engineer a more obvious answer. If you've ever wondered why every card terminal on earth suddenly wanted you to insert your chip instead of swipe, this is why. [EMV](https://www.emvco.com/) stands for Europay, Mastercard, and Visa, the three companies that created the chip standard, and the rule, in plain terms, is that liability falls on [whichever side](/2026/08/30/disputes-what-happens-when-you-tap-dispute/#the-parties) is using the weaker technology.

- When a Merchant doesn't support a chip reader, or has a chip reader but processes it as a swipe anyway, and the transaction turns out fraudulent, the merchant eats it - everytime.
- When a Merchant has a chip-enabled terminal, but the card itself is an old mag-stripe-only card with no chip to insert, so it gets swiped instead, the issuer eats it (usually).
- Both sides are chip-compliant and the transaction still turns out to be counterfeit fraud (the chip got cloned some other way), the issuer is typically still on the hook. Neither party did anything wrong, so the loss falls back to whoever's supposed to be backstopping fraud in the first place.

That's a genuinely elegant piece of policy design, actually, it doesn't try to figure out who's at fault, it just makes upgrading your security the economically rational move for everyone. Once EMV adoption crossed a threshold, this stopped being a live problem for most merchants.  The schemes (Visa, Mastercard, etc) know how to incentivize fraud measures, by pushing the cost back to the weakest link in the payments chain.

## Why non-delivery is the one that stays expensive

The EMV shift is a policy lever, upgrade your tech and the risk moves off you. Non-delivery disputes don't have that lever. The only thing standing between you and that $144 swing is whether you can produce a delivery confirmation, a signature, tracking that actually shows it arrived, something. No evidence means no defense and it doesn't matter that you actually shipped the shoes.

That's the setup for the next post. Merchants aren't defenseless here, they can fight back with evidence, and what actually counts as evidence, what doesn't, and why most merchants don't bother even when they have a winning case, is where representment comes in.
