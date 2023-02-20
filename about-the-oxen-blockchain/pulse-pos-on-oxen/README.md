---
description: Pulse is Oxen's Proof of Stake consensus mechanism
---

# 💓 Pulse: PoS on Oxen

Fundamentally, every blockchain — including Oxen — is exactly what it says in the name: a chain of blocks. One 'block' in the Oxen blockchain is made up of a set of $OXEN transactions which have been deemed valid by the network. But when the network consists of thousands of computers all over the world, how does it come to consensus about the state of the blockchain?

That's where Pulse comes in.

Pulse is Oxen's implementation of the 'Proof of Stake' consensus mechanism, and it forms the foundation of the Oxen Service Node network. Let's dive into how it works and why we use it.

### Proof of Stake: How does it work?

In our Proof of Stake (PoS) consensus system, active Oxen Service Nodes are randomly selected to create and publish new blocks of transactions to the blockchain approximately every 2 minutes, with each node being rewarded for doing so.

Once a service node has been selected to create a block, a set of other nodes (the _validators_) are chosen to validate the new block — checking to ensure it doesn't contain duplicate transactions or any other potentially malicious activity. Once these validator nodes have verified the block’s legitimacy, the block is published to the blockchain.

{% embed url="https://www.youtube.com/watch?v=WSDh8OCQvQ4" %}

### Security benefits of Proof of Stake

In a PoS-based blockchain, an attacker would need to control more than 50% of the total supply of staked or stakeable cryptocurrency tokens in order to carry out a 51% attack (allowing them to publish fake or invalid transactions, re-order the blockchain, etc.).

Acquiring this much of the total supply can be extremely expensive — and in some PoS-based blockchains, it be nearly (or completely) impossible. When crypto tokens are staked into nodes, they are locked for a pre-determined amount of time. This means they're removed from the marketplace and can't be bought. If more than 50% of the total cryptocurrency supply for a given blockchain network is staked, a 51% attack becomes extremely unlikely, because for an external attacker (i.e. someone not _already_ invested in the network), buying up or otherwise being able to control enough tokens to control more than half the nodes on the network becomes almost impossible. And there's a second layer of security here, too: if someone does buy up such a huge amount of tokens that they could gain control of the network, they would then be disincentivised from wanting to attack the network simply because attacking the network would harm the value of their tokens — once they're invested, they're simply better off acting in good faith, so their tokens gain value rather than losing value.
