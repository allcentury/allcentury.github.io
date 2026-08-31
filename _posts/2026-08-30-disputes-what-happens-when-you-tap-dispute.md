---
layout: post
title: What actually happens when you tap "dispute this charge"
series: payments-disputes
series_title: Payments Disputes
series_order: 1
---

I've spent the last 10 years in fintech, focused on credit card payments (Braintree, Brex) and banking rails (Brex, Venmo). I'm going to write a series on topics I've spent a lot of time on, and this one is specifically on disputes, some of it [patented work](https://patents.google.com/patent/US20240289807A1/en) from my time as a staff engineer and engineering manager on the disputes team at Braintree.

I recently had to dispute a charge that was clearly fraudulent, someone in South Korea had used my credit card to take cash out of an ATM. So, I thought I'd walk through the technical pieces today, the APIs and systems involved, from the dispute itself to all the money movements it triggers.

From your side, you click dispute in your bank or credit card app, and that's it, you're done. Behind the scenes though, you just kicked off a process with four separate companies, a rulebook you've never read, and a clock that started the second you hit submit. Almost nobody outside of payments knows what actually happens next, and it's helpful to know who's in charge at each step so you can figure out why yours might be stuck.

## The parties

Five parties, minimum, are involved in every card transaction, and every one of them is involved again when you dispute it.

1. **You**, the cardholder.
2. **Your bank**, the issuer, the one who gave you the card and who you're actually talking to when you tap "dispute."
3. **The network**, Visa, Mastercard, Amex, Discover. The network doesn't hold your money or the merchant's, it's the rulebook and the message router in the middle. It defines what a dispute even means, how it's coded, and how fast everyone has to respond.
4. **The merchant's bank**, also known as the acquirer, who's on the hook to relay this to the merchant and, eventually, pull the money back.
5. **The merchant** the provider of the good or service that charged your card.

When you tap dispute, you're not messaging the merchant. You're messaging your issuer, who translates that into a formatted message and sends it through the network to the acquirer, who hands it to the merchant. Five hops, and the merchant is usually the last to know, sometimes days after you already have your money back.

```
[ YOU ]
   |  tap "dispute", instant ack
   v
[ ISSUER ]         your bank (Chase, Citi, etc)
   |  same day
   v
[ NETWORK ]        Visa / Mastercard
   |  usually <48h
   v
[ ACQUIRER ]       merchant's bank (Chase, Citi, BofA, etc)
   |  <1 day, batched
   v
[ MERCHANT ]       sees it last

..................................
response + evidence travel back
up this same chain, and can happen
more than once (disputes, pre-arbitration)
..................................
```

Only that first hop is synchronous, you tap dispute and your bank acks it instantly, that's the confirmation screen you see. Everything after that is async and mostly batched, not because anyone's being slow, that's just how message volume this large gets processed. The issuer typically gets its message to the network the same day. The network relays it to the acquirer usually well within 48 hours, often faster. The acquirer notifies the merchant same day or the next, almost always as part of a batch rather than a live push. None of that needs to be instant, the rules that actually govern this are measured in days, not seconds.

## The first decision

When you file a dispute, your bank (credit card issuer) will likely give you a prompt like this one from Chase:

![Chase's dispute reason picker: recurring charge after cancellation, no credit for a return, item never received, dissatisfied with the item, overcharged, charged twice, or an unauthorized purchase](/public/imgs/chase-dispute-reason-picker.png)

The selection you make here has a big determination in what VCR (Visa Compelling Evidence) code gets sent all the way back to the merchant. In fact, what you select has a huge impact on the evidence the merchant _must_ provide to attempt to win back the transaction.

Broadly it splits two ways:

**Fraud.** You didn't make this charge, someone else did, using your card number without your permission. This is coded differently than everything else, it moves faster, and the evidence that matters is about *authentication*, was this really you, not about the transaction itself.

**Everything else.** You made the charge, but something about the transaction is wrong. The item never showed up. It wasn't as described. You were charged twice, or charged more than you expected. You returned something and never got credited. You canceled a subscription and got billed anyway. This bucket is much bigger than people assume, most disputes aren't "someone stole my card," they're closer to "this didn't go the way it was supposed to."

That fork gets encoded as a reason code, a short alphanumeric string the network assigns that basically says "here's the category of complaint." Everything that happens next, what the merchant can submit to fight it, how long they have, whether Visa's rules or Mastercard's rules apply and how those differ, all of it flows from that one code.

## How much time everyone actually has

The cardholder side is the most generous. Visa gives you 120 days to file, but that clock doesn't always start at the transaction date. For a "never arrived" dispute, it starts at the date you expected delivery, not the date you paid, which can push the real deadline out to as far as 540 days after the original purchase. That's why "item never showed up" disputes can surface months after the order, the cardholder isn't being slow, the clock just started later than most people assume.

Once the merchant is notified, their clock is tighter, and it depends on the network. Visa gives merchants 30 days to respond. Mastercard gives 45, with one important exception, a Request for Information, or RFI, which is Mastercard's way of asking the merchant for documentation before deciding whether to actually push a full chargeback through. Merchants only get 18 days to answer an RFI, and critically, no money moves during it. An RFI is the network asking "can you back this up," a chargeback is the money actually leaving the merchant's account. Confuse the two and you'll either panic over paperwork or under-react to something that's about to actually cost you.

## What you'll actually notice

Depending on your issuer and the dispute type, you'll often see the money back in your account before anything is actually resolved, that's a provisional credit, not a final decision. Your issuer is fronting you the money while the dispute plays out in the background. If the merchant successfully fights it later, that credit can get reversed. I'll go deeper on who actually eats that loss, and why it's not always the merchant, in the next post.

## Why this is worth knowing

Most people think of a dispute as a two-party fight, you versus the merchant. It's not. You're not even the one talking to the merchant. You're talking to your bank, your bank is talking to a network, the network is relaying rules and messages to another bank, and that bank is the one telling the merchant what's going on. Every one of those hops has its own timeline, its own rules, and its own incentives, and that's before anyone has looked at whether your complaint even has merit.

Next up in this series: what happens after that first message lands, the actual chargeback, and why the loss doesn't always land on the merchant the way you'd expect.
