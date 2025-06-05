---
description: >-
  This page provides a more in-depth look at the technical side of Pulse, Oxen's
  PoS consensus mechanism
---

# 🤿 Pulse: Deep dive

{% hint style="warning" %}
**The Oxen Network has transitioned to the Session Network. More information** [**here**](https://oxen.io/blog/development-is-transitioning-to-session-token)**.**&#x20;
{% endhint %}

### Introduction

Pulse is [Oxen](../../)'s Proof of Stake (PoS) scheme. There are various reasons this scheme was implemented in favour of the more popular blockchain consensus mechanism, Proof of Work (Pow).

Pulse makes Oxen more secure, more sustainable, and more powerful.

### **Overview**

Every 120 seconds, an active [Oxen Service Node](../oxen-service-nodes.md) is chosen to create a new candidate block. This one true service node creates a candidate block and sends it to 11 randomly chosen _validators_.

Validators are service nodes tasked with independently assessing the candidate block and ensuring it does not contain any duplicate transactions (or any other malicious activitiy). If 7 of the 11 validators verify the block's legitimacy, it can be published to the network as a valid block.

This process is then repeated to create the next block.

### **Security**

Thanks to Pulse, it is both difficult and unprofitable to attempt an attack on the Oxen network. At the core of any blockchain's security is its consensus mechanism. There are a few critical attacks that blockchains must be secured against, which Proof of Stake consensus schemes excel at thwarting — making sure the value stored on the Oxen blockchain is safe.

#### Increasing the resource investment needed to attack Oxen

In a Proof of Work blockchain, double spending a token requires an attacker to secretly produce an alternate blockchain with more cumulative difficulty, reverse a previously spent transaction, and then create a new transaction with the same token(s).

Unless the attacker is using sophisticated attacks — such as selfish mining or cartels — this requires the miner to obtain 51% of the entire network's hash power.

Hash power is generally equivalent to computing power — more computing power, more hash power. This means the attacker only needs to acquire enough computing power relative to the size of the network to perform a 51% attack.

Performing the same attack against a PoS blockchain like Oxen's is a little different. Without considering more sophisticated attacks, the attacker would need to gain control of half of the nodes in the network to pull off a 51% attack.

Because Oxen is a finite resource, and its availability is reduced with each node that is created, conducting this attack would be many, many times _more_ expensive than the equivalent attack on a PoW blockchain.

**Increasing sunken cost of an attack on Oxen**

The resources required to attack a PoW blockchain retain their value after the attack — that is, the computing power purchased to complete the attack can be used again to attack another blockchain, used to complete other computing tasks, or sold.

However, the same assault on a PoS blockchain requires you to purchase the token associated with that _specific_ network. Assuming a successful attack would devalue the token, anyone hoping to attack a PoS blockchain will likely have trouble recouping the cost of their attack.

**Checkpointing**

Checkpointing provides additional 🛡 defence on top of Pulse. Every four blocks, the Oxen blockchain records a checkpoint — taking a snapshot of everything before it on the blockchain. Nothing transactions older than 2 checkpoints (also known as 8 blocks) can _ever_ be changed.

This means that even _if_ Pulse was somehow compromised, the Oxen blockchain would **still** be protected.

Checkpointing also provides additional protection against more sophisticated (and luck-dependent) attacks.

### Sustainability

As the blockchain space grows, we have to start seriously considering whether our current models are sustainable — higher sustainability, more scope for long-term growth.

Environmental and economic sustainability are becoming increasingly pertinent issues in the tech world, and this is especially true in blockchain.

#### **Power consumption**

PoW consensus, while serviceable for securing the blockchain, has an inescapable flaw — it is extremely inefficient, and this problem becomes worse as the network scales.

Bitcoin, which uses PoW, [consumes over 100 TWh](https://www.cbeci.org) per year, more than the consumption of entire countries like the Netherlands and the Phillipines — and that's just one blockchain! No one blockchain should have all that power.

PoS means a sustainable future for blockchain technology. Instead of relying on power-intensive computation, the blockchain is secured through the cooperation of [service nodes](../oxen-service-nodes.md), which is much more energy efficient — say goodbye to the wasteful ways of PoW.

### **Power**

Lots of major players in the blockchain world have thrown their weight behind PoS — most notably, Ethereum.

This is largely because PoS is more scaleable, more flexible, and easier to participate in. PoS is what will finally bring blockchain to the masses.

Of course, the nodes that maintain the Oxen blockchain do a lot more than just create and validate blocks — they form the backbone range of services and applications, like [Session](../../products-built-on-oxen/session/) and [Lokinet](../../products-built-on-oxen/lokinet/). This is just the beginning of what Oxen can do with its powerful network of service nodes.

### Implementation

This is the nitty-gritty of _exactly_ how Pulse works. If you're only interested in the _why_ of Pulse, all the information you need is above, but for the _how,_ read on.

**Proposal round**

A single leader is sequentially chosen from the Oxen Service Node list, as well as a group of 11 validators chosen deterministically using a random value included in the previous block.

The leader scans the transaction pool and collects valid transactions into a block.With this information, the leader creates a candidate block.

The leader is rewarded according to the service node coinbase formula, and receives all transaction fees included in the block.

The leader then signs this candidate block and sends it to all 11 validators.

Each validator also sends the candidate block to 3 other validators, ensuring all validators successfully receive it.

**Commit round**

Every validator now generates a 128 bit integer, and sends a hash of that integer to all other validators.

**Reveal round**

Validators will then reveal the pre-image of the hash — which in is the random integer they hashed during the commit round — to all other validators. All validators should now have a subset of the revealed values, which they can combine to produce a new random value.

Validators add this value to the candidate block, signing the resultant block.

This signature is then sent to all other validators.

**Block submission**

Each validator is then able to check the validity of each of the other validator's signatures, ensuring they are all signing both the same candidate block and the same combined random integer.

Once signatures have been validated, any validator in the quorum may submit a block to the network which includes at least 7 validator signatures.

#### Key properties of Pulse

* 120 second block time
* Cannot be significantly biased by any one member of the quorum
* Tolerates failure or malfeasance of up to \~45% of service nodes

### Fault tolerance

#### Failures

There is only ever one way Pulse can fail: a timeout.

**Timeouts and swap blocks**

A timeout occurs when a valid block is not produced after 120 seconds.

As soon as the _next_ leader realises there has been a timeout, they can immediately start building and distributing their own candidate block — containing a special “swap” tag.

If this block is signed by the validators, the swap tag will indicate that there was a quorum failure to both newly syncing nodes and nodes which have been offline.

**Demerits and de-registrations**

Validators in a quorum can submit intent to demerit messages at any stage for their leader or any peer validator(s) they feel haven't followed the protocol.

For example, if a leader never publishes a candidate block, any validator in the quorum may send an intent to demerit message to their other peers. If this message is signed by the majority of other validators it may be submitted to the network as a demerit point message.

This message is not a transaction, and simply propagates through the p2p layer.

#### Common failure cases

To get a better understanding of Pulse, it is worth discussing what happens in some common failure cases.

* The leader doesn't submit a candidate block
* Not enough validators participate in the commit or reveal rounds
* Validators sign two different blocks
* Validators are unable to submit a block due to differences in revealed values

**Leader doesn't submit a candidate block**

If a leader fails to construct and send a candidate block, the validators will be triggered to submit an intent to demerit message.

If this message is signed by a simple majority of the quorum, a demerit point is be applied to the leader and the next leader creates a swap block.

**Not enough validators participate in the commit or reveal rounds**

Although all Oxen Service Nodes are incentivised to cooperate in order to avoid demerits, some participants may either act maliciously or simply fail.

The protocol should be able to tolerate the failure of 6 of 11 validator nodes during the commit reveal stage.

As above, any validator that doesn't participate in a commit round, a reveal round, or reveals a different value from their commitment can be flagged by other validators with an intent to demerit message. If this message is signed by a majority of nodes, this message is submitted to the network as evidence of a node’s non-performance in a block creation quorum.

If more than 6 of 11 nodes don't participate in the commit or reveal round, the quorum will not be able to create a block — which means the next quorum can take over block creation once a timeout occurs.

**Validators are unable to submit a block due to differences in revealed values**

There may be a number of cases where not all of the service nodes correctly receive all of the committed or revealed values. This will result in differing final random integers being calculated by the validators.

As with other errors, the scheme will progress normally as long as the revealed values for 7 of the 11 validators match. Nodes with invalid revealed values can query the other validators in their quorum again to receive any values they missed during the two rounds.

If more than 7 nodes disagree on the final revealed value, then the scheme will fail after once a timeout occurs and the next leader produces a swap block.

**Validator signs two different blocks**

Any service node, including a validator or leader, may submit a deregistration transaction if they can produce signatures from any other service node that signs multiple blocks which compete for the same height or signs a competing chain once more than two checkpoints have been applied.

This means any node that signs blocks indiscriminately in block creation quorums will be immediately removed from the network with their funds locked.

### Traditional PoS attacks

#### Stake grind attacks

Stake grinding usually refers to a class of attacks where a validator or leader can iterate values to bias the selection of the next chosen validator. Since validators are chosen by using a collaboratively generated random integer in each block, it is possible to bias the results of the scheme. However, the only way to actually do this is for a validator to chooses to hide the value they shared during the commit round.

In the context of Oxen Service Node selection for the next quorum, this biasing is extremely weak, and the action of committing but failing to reveal a value leads to a demerit message being submitted for the validator, making this attack infeasible.

#### Long range attacks

Long range attacks target nodes that are syncing the chain, or that are offline for a period of time — attempting to convince them of an alternate chain state.

This is possible because old service nodes can use previously held service node keys to re-sign blocks that appear as alternate chains.

The Oxen blockchain already provides a solution for online nodes through service node checkpointing. This prevents service nodes from altering blocks that have received more than 2 checkpoints.

However, tackling the problem of nodes which go offline and miss more than two checkpoints in a row or nodes who are syncing for the first time is more difficult.

**Syncing nodes or partially offline nodes**

When a node has no previous state of the blockchain, or has been offline long enough to miss enough checkpoints that it is infeasible for it to validate the non alteration of blocks, it must use an external trusted source to correctly identify the main chain. This is usually referred to as weak subjectivity.

The easiest way to solve this issue is to allow syncing service nodes, and service nodes which have been offline to refer to the seed nodes and ascertain which is the true chain. To implement weak subjectivity for Pulse, seed nodes should provide a list of the latest checkpoints, so that when a client is syncing they can ensure they're syncing to the correct chain — and not being segmented onto a network with valid but altered signatures.

#### Nothing at stake

The ‘nothing at stake’ problem references a validator’s tendency to sign or authorise multiple competing blocks in a Proof of Stake system. They are incentivised to do this because they can’t be sure which chain will become the true chain in the long term, thereby they should sign competing blocks to ensure they don't miss rewards.

With Pulse, the ‘nothing at stake’ problem is prevented by allowing nodes in the network to submit evidence of double signed blocks. If this evidence can be validated, the offending node can be deregistered from the network, providing a punishment for nodes who do not effectively choose a single block to sign.

#### DDOS attacks

All service nodes in the Oxen network maintain a public IP address so they are reachable by other service nodes and [Lokinet](https://lokinet.org)/[Session](https://getsession.org) users. These IP addresses are held in a distributed list that anyone can access. This opens the service node network and especially block producing quorums to DDoS attacks. For example, an attacker could wait for a block to be successfully published and immediately identify the members of the next quorum who will produce a block. With this information, they could launch a DDoS attack aimed to interrupt the communication between validators, or between the leader and validators. If successful, they would prevent a new block from being produced and could continue this attack on successive quorums.

Although individual service nodes have incentives to prevent DDoS attacks (so they can earn rewards and avoid demerit points), there are some protections we can offer in the Oxen software suite which can be deployed in the case that the previous block has failed to be produced. The most trivial protection is that after a failure in block creation, service nodes in the next selected quorum should ignore any connections from IP addresses which do not belong to another service node. This limits the scope of the attacker’s DDoS to only service nodes — which are costly to operate and far less numerous than a botnet-like DDoS attack. service node operators could also reasonably rate limit other service nodes on a per key basis, choosing not to respond to service nodes who put undue strain on their own networks.

### Considerations

#### Safety of commit reveal schemes

Collaborative commit reveal schemes for generating random values are well studied, and limitations on such schemes are well known. The primary downside to using this type of scheme is that although committed values cannot be changed after they have been committed, a contributor to the scheme may bias the results by not revealing their committed value at all.

For example, say a block creation quorum produces a value between 0 and 10 each block, and Alice wants to run an online casino which uses this random integer to flip a coin, with bets being placed on heads or tails. Alice’s casino algorithm reads the random integer from the blockchain and if the random value is between 0-4 then the coin is tails, and if it's between 5-10 it’s heads.

A possible attack on this application would be for an Oxen Service Node operator, let’s call her “Mallory”, to wait to become a validator in a quorum, act honestly, and commit to a random value. Once she has committed to the scheme, she waits for the other validators to reveal their original integers. She can now calculate the shared random result for those validators that have revealed. Let’s suppose their combined final value will be 3. Using this information and whatever deterministic algorithm that is used to combine each validators contribution, Mallory can now decide whether or not she wants to reveal her result and add her contribution to this data.

If Mallory bets on tails and realises revealing her commitment would change the overall result to heads, then she should not reveal her commitment. If her commitment (when combined with the other commitments) still selects a number under 3, then she can reveal her committed value. Of course, the cost of not revealing a commitment is demerit points being applied to Mallory’s service node which will lead to deregistration in the event that Mallory has not revealed her commitments consistently. However, as long as Mallory’s winnings are more than the cost of being deregistered from the service node network, her dishonest actions are financially incentivised.

It should be clear from this example that service nodes can significantly bias the results of decisions which have few available outcomes, like a coin toss. So applications should not use the random integer we are creating for these type of use cases! The Oxen blockchain, however, does not use the source of randomness in this way. We use the randomness to choose service nodes in a service node list, where biasing the results has an insignificant effect as the possible outcomes are far more numerous. With Pulse, the randomness seeds an algorithm that chooses a combination of 11 nodes out of currently about 620 nodes. Mallory, in this case, would be able to choose between two (and only two) different random samples of 11 nodes from the selection of 620, but this single extra choice is highly unlikely to provide a notable benefit in terms of opportunistic selection.

#### Block times

Pulse targets a predictable block time of **120 seconds** — excluding the rare case where block creation fails. In this case, a block would be created in the subsequent 120 seconds. This means that in a majority of cases users won't be left wondering when the next block is coming like on PoW blockchains, since blocks are be predictability created every 120 seconds.

### Conclusion

Pulse is a robust yet simple scheme for block creation and transaction ordering in the Oxen network. Pulse does all this while increasing security and decreasing the need to use energy intensive PoW. Additionally, it's better aligned with existing incentive structures to reward those who do the most work to create order, and secure the network — service nodes.
