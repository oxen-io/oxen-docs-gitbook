---
description: Oxen Service Nodes provide the underlying support for the Oxen Network
---

# 🖥 Oxen Service Nodes

Oxen’s networking functionality, scalability, and decentralisation tech stack is powered by a set of incentivised nodes called Oxen Service Nodes. To operate a service node, an operator time-locks 15,000 $OXEN, and the service node begins providing a minimum level of bandwidth and storage to the network. In return for their services, service node operators receive a portion of the block reward from each block mined on the Oxen blockchain.

{% embed url="https://www.youtube.com/watch?v=d193VVqlj58" %}

The resulting network benefits from market-based resistance to Sybil attacks, addressing a range of problems with existing onion routers and privacy-centric services. This resistance is based on supply and demand interactions which help prevent single actors from possessing a large enough stake in Oxen to have a significant negative impact on the second-layer privacy services Oxen provides. [DASH](https://github.com/dashpay/dash/wiki/Whitepaper) first theorised that cryptoeconomics can provide a network with properties of Sybil attack resistance. In our case, as an attacker accumulates $OXEN, the circulating supply decreases, in turn applying demand-side pressure and driving the price of $OXEN up. This effect spirals, making it increasingly costly for additional $OXEN to be purchased and thus making an attack prohibitively expensive.

To maintain this protection, Oxen encourages active suppression of the circulating supply. In particular, the emissions curve and staking requirement have been designed to ensure enough circulating supply is locked (and reasonable rewards are provided to operators) to ensure Sybil attack resistance.

> Just looking for a guide on setting up a service node? [Click here](../using-the-oxen-blockchain/oxen-service-node-guides/setting-up-an-oxen-service-node.md). Interested in staking into a shared service node? Check out our [Staking Guide](../using-the-oxen-blockchain/oxen-service-node-guides/staking-to-shared-service-node.md).

### Service Node activities

An Oxen Service Node becomes active on the network when its owner stakes (locks up) the required amount of $OXEN, which submits a registration transaction. Once accepted by the network, the service node becomes eligible to receive block rewards. Multiple participants can stake to a single service node, combining their smaller stakes to meet the 15,000 $OXEN staking requirement. The block reward can be automatically distributed among the participants in a ratio proportional to each participant's staking contribution.

Once active on the network, Oxen Service Nodes are required to:

* Monitor other service nodes and vote on their performance
* Create new blocks on the Oxen blockchain (see [Pulse](pulse-pos-on-oxen/))
* Receive, temporarily store, and forward encrypted messages (see [Session](../products-built-on-oxen/session/))
* Participate in quorums to enable instant $OXEN transactions (see [Blink](blink-instant-transactions.md))
* Route end-user internet traffic (see [Lokinet](../products-built-on-oxen/lokinet/))

### Guides & Resources

| Resource Name                                                                                                                                    | Description                                                                          |
| ------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------ |
| [**Service Node setup**](../using-the-oxen-blockchain/oxen-service-node-guides/setting-up-an-oxen-service-node.md)                               | How to host and maintain an Oxen Service Node using a Debian package and CLI wallet. |
| [**Staking to a shared service node as a contributor**](../using-the-oxen-blockchain/oxen-service-node-guides/staking-to-shared-service-node.md) | How to stake to a shared Oxen Service Node as a contributor.                         |
| [**Oxen Dashboard**](https://lokidashboard.com)                                                                                                  | Statistics, data, maps, and other information on Oxen Service Nodes.                 |
| [**Service node RPC Calls**](../using-the-oxen-blockchain/advanced/service-node-rpc-calls.md)                                                    | List of Oxen Service Node JSON 2.0 RPC calls (advanced users only).                  |
| [**Active service node list**](https://lokiblocks.com/)                                                                                          | The Oxen Blockchain Explorer, with a list of current Service Node public keys.       |
