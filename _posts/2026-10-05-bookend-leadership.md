---
layout: post
title: Bookend leadership
---

In an effort to avoid the "hey, can we talk about your PR for a minute", which then dovetails into a 10 person meeting on a project you thought was narrowly scoped, I'm sharing a framework I use to help my senior engineers manage their time.  I call it bookend leadership, the idea is fairly simple, I think as a senior you have the most impact at the beginning and end of a project.

<svg viewBox="0 0 480 205" width="100%" style="max-width:480px;display:block;margin:1.5rem auto;" font-size="16" role="img" aria-label="Effort over time drawn as a shelf: two tall bookends at design and release, with short books for the PRs of the build in between">
  <path d="M28,152 V30 M22,38 L28,30 L34,38" fill="none" stroke="#6272a4" stroke-width="1.5"/>
  <path d="M26,150 H460 M452,144 L460,150 L452,156" fill="none" stroke="#6272a4" stroke-width="1.5"/>
  <rect x="98" y="116" width="26" height="34" rx="2" fill="#44475a" stroke="#6272a4"/>
  <rect x="126" y="108" width="28" height="42" rx="2" fill="#44475a" stroke="#6272a4"/>
  <rect x="156" y="122" width="22" height="28" rx="2" fill="#44475a" stroke="#6272a4"/>
  <rect x="180" y="112" width="32" height="38" rx="2" fill="#44475a" stroke="#6272a4"/>
  <rect x="214" y="104" width="26" height="46" rx="2" fill="#44475a" stroke="#6272a4"/>
  <rect x="242" y="120" width="28" height="30" rx="2" fill="#44475a" stroke="#6272a4"/>
  <rect x="272" y="110" width="24" height="40" rx="2" fill="#44475a" stroke="#6272a4"/>
  <rect x="298" y="114" width="30" height="36" rx="2" fill="#44475a" stroke="#6272a4"/>
  <rect x="330" y="106" width="26" height="44" rx="2" fill="#44475a" stroke="#6272a4"/>
  <rect x="358" y="118" width="24" height="32" rx="2" fill="#44475a" stroke="#6272a4"/>
  <rect x="50" y="40" width="24" height="110" rx="2" fill="#ff79c6"/>
  <rect x="50" y="146" width="44" height="4" rx="1" fill="#ff79c6"/>
  <rect x="406" y="40" width="24" height="110" rx="2" fill="#ff79c6"/>
  <rect x="386" y="146" width="44" height="4" rx="1" fill="#ff79c6"/>
  <text x="62" y="95" text-anchor="middle" dominant-baseline="central" fill="#282a36" font-weight="bold" font-size="14" letter-spacing="1" transform="rotate(-90 62 95)">Design</text>
  <text x="418" y="95" text-anchor="middle" dominant-baseline="central" fill="#282a36" font-weight="bold" font-size="14" letter-spacing="1" transform="rotate(-90 418 95)">Release</text>
  <text x="240" y="174" text-anchor="middle" fill="#6272a4">build</text>
  <text x="240" y="198" text-anchor="middle" fill="#6272a4" font-size="13">time</text>
  <text x="14" y="95" text-anchor="middle" fill="#6272a4" font-size="13" transform="rotate(-90 14 95)">effort</text>
</svg>

In the beginning, being involved while planning and design (both product and engineering) are in review gives you a chance to really help set the direction, identify issues and ensure you understand the scope on day 1.  Later on, if you see a discrepancy in a PR, you can point back to the design document: "I thought we landed on doing X?".  If the code changed but the design doc didn't, this is a good opportunity to meet w/ the DRI and regain context (and confidence) in this new path.

In the middle, your job is much simpler. PR and code changes shouldn't surprise you.  Rarely will you need to block a code change or be the bottleneck on implementation because you've already agreed to the design.  Your job here is to make sure what's being implemented meets your bar and the agreed upon contracts.  It also prevents re-litigation on big ideas that can implode a project and people's time.  The reduced time and stress this produces is a welcome change because before this, senior engineers would scour PRs looking for the next big issue, where now they're looking for big discrepancies from plans.  It's a much different day to day, and it cuts down the FUD for everyone, not just the seniors, since the DRI isn't bracing for a surprise redesign in review.

In the end, when the feature is nearing completion and the team is holding bug bashes or release readiness reviews, this is also a great time for you to jump in.  Does this release meet your bar?  If it does, great, if it doesn't what would you want to see changed to get there?  This is both a mentorship opportunity and a confidence building exercise for everyone involved.

