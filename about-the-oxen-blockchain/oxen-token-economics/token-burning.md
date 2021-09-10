---
description: >-
  Oxen has a robust token burning scheme designed to keep inflation under
  control
---

# 🔥 Token burning

A crucial consideration for any [tokenomics system](./) with an indefinite emissions tail is how to keep inflation under control. In short: How can we keep rewarding [service nodes ](../oxen-service-nodes.md)indefinitely without also ending up with so much $OXEN floating around that the token becomes worthless? 

$OXEN solves the inflation problem through a robust **token burning scheme**. This means that the effective net emissions are lower than they may seem, because most routine interactions with the Oxen network, including [Blink transactions](../blink-instant-transactions.md) and [ONS name purchases](../../using-the-oxen-blockchain/using-oxen-name-system.md), provably burn a certain amount of $OXEN. 

* Blink fees are 0.05 $OXEN per transaction output, plus a very small per-byte amount of $OXEN; all transactions have at least 2 outputs \(recipient\(s\) and change\), so the effective fee is around 0.101 $OXEN for a typical simple transaction
* Of that fee, 40% goes to the service node that constructs the block validating the transaction \(forming part of that service node’s reward\), and the remaining 60% \(i.e. around 0.06 $OXEN for a typical transaction\) is burned
* Basic ONS registrations burn 7 $OXEN \(for Session/Wallet namespace registrations, a registration lasts forever; for Lokinet, a basic registration lasts for 1 year\)
* You can also buy 2, 5, and 10-year Lokinet registrations for 2x, 4x, or 6x the basic fee, respectively

We periodically reevaluate and adjust fees and burn amounts to maintain reasonable levels relative to the price of $OXEN. For example, in the Valiant Vidar hardfork, the Blink fees and burn ratios were adjusted from approx. 0.2 $OXEN fee with 80% burned in a typical transaction to approx. 0.1 $OXEN fee with 60% burned.

You can view the total burned $OXEN in the [block explorer](http://oxen.observer/).  
  
  


