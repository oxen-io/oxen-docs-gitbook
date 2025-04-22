---
description: >-
  Get answers to key questions regarding the upcoming migration to the new
  Session Network. This page will be updated continuously as additional
  questions are shared by the community.
---

# ❓ Migration FAQ

### What is the sequence of events for the migration?&#x20;

The transition from the Oxen Network to the Session Network happens in three phases: Lighthouse, Anchor, and Landing.&#x20;

During the Lighthouse phase, the contributors and community members have conducted work on the Session Network testnet and may have gained valuable experience by running a testnet node.

During the Anchor phase, final preparations are made for the transition to the Session Network. The Oxen Network undergoes the Anchor Hardfork, with mandatory upgrades to Oxen Core packages for all Service Nodes. Note that the Anchor release will introduce some [new requirements](connecting-to-an-arbitrum-one-rpc-endpoint.md) for nodes to prepare for the transition.

During the Landing phase, the transition to the Session Network is executed. The Oxen Network undergoes the Landing Hardfork, becoming the Session Network. Session Token (SESH) is generated and distributed to node operators and contributors.

### How does the automatic migration of nodes work?&#x20;

The automatic migration mechanics are designed to ensure the majority of nodes can migrate to the new Session Network. This is ensured in a few different ways.\
\
Firstly, staking requirements on the Session Network will be determined in such a way that operators who have been registered for the Service Node Bonus Program since the beginning of the program will have more than enough SESH to be able to re-stake their nodes. Similarly, operators and contributors to multicontributor nodes, where the operator and all contributors were registered for the Service Node Bonus Program from the beginning, will also have more than enough SESH for their node to be re-staked on the Session Network.&#x20;

Secondly, a system of automatic reallocation will ensure that any excess SESH goes to nodes that don’t meet the staking requirements of the Session Network after the conversion of staked Oxen and Service Node Bonus points, allowing as many of these nodes as possible to be re-staked. This is particularly important when it comes to ensuring multicontributor nodes are able to be re-staked.

Every wallet participating in the migration will effectively be assigned a “pool” of SESH converted from staked Oxen and Service Node Bonus points. SESH from this pool can be reallocated from one node to another. Suppose a multicontributor node cannot be re-staked, as the operator and/or one or more of the contributors does not have enough SESH for the node to be fully staked. In this case, the node will be disbanded, so that the SESH can be made available to other nodes that also require additional SESH to be re-staked, where there is a match between the amount of SESH in an operator or contributor’s “pool” and the amount of additional SESH required to fulfill their proportion of the node’s stake. Note that SESH cannot be allocated to nodes above the proportion of the operator or contributor’s original stake in the node.

In this way, SESH will be allocated efficiently across nodes to ensure that the maximum number of nodes can be re-staked on the new Session Network. However, in some cases, operators and contributors may have excess SESH with no nodes available to stake to.&#x20;

For example, an operator who signed up early to the Service Node Bonus program may have more SESH than is needed for the nodes they are running. As another example, a contributor to a node may have excess SESH if their node is disbanded for failing to meet staking requirements, and they weren’t staking to any other nodes to which the additionalSESH could have otherwise been allocated, or other nodes they were staking to did not require additional SESH to meet the staking requirements. In these instances, excess SESH will be claimable as staking rewards via the [Staking Portal](https://stake.getsession.org/). Operators and contributors can simply connect their Ethereum wallet to the [Staking Portal](https://stake.getsession.org/) and claim the SESH on the Arbitrum One network following the completion of the Landing Hardfork.

### What if my node deregisters after the Anchor Hardfork?

If your node deregisters after the Anchor Hardfork but before the Landing Hardfork, any points you have earned in the Service Node program will still be honored and the earned amount of SESH will be credited to your Ethereum address, available as a rewards, withdrawable any time after TGE, on the Arbitrum One network, via the [Staking Portal](https://stake.getsession.org/). However, your principal stake will not be automatically swapped for Session Tokens. Your Oxen balance in your Oxen wallet will unlock at the Landing hardfork and you will need to manually migrate these coins using the Oxen to SESH bridge at or after TGE.

### What are the staking requirements for Session Nodes?

The exactly full stake amount for Session Nodes is yet to be determined, but it is estimated to be between 20k-23k SESH tokens.&#x20;

### If I was operating a solo Service Node for the duration of the Service Node Bonus Program and I’m signed up for the program will I receive enough tokens to meet the new Session node staking requirement?&#x20;

Yes, the new Session staking requirement will be set such that a solo Service Node operator who has been running a Service Node for the entire Service Node Bonus Program and who is signed up will receive more than enough Session Tokens to be automatically staked to the Session network when the migration occurs.  Operators who have started new nodes during the Bonus Program may not have accumulated enough points to cover all the new nodes, in which case some nodes might get disbanded to provide enough SESH to stake the remaining nodes (see the previous question).

### If I was operating a multicontributor Service Node for the duration of the Service Node Bonus Program and my contributors and I are signed up will we receive enough tokens to meet the new Session node staking requirement?&#x20;

Yes, the new Session staking requirement will be set such that multicontributor Service Nodes which have been running for the entire Service Node Bonus Program and who have all contributors and the operator signed up will receive enough Session Tokens to be automatically re-staked to the Session network when the migration occurs.

It is possible, however, that either the operator or some contributors may not have enough SESH to cover the stakes of all of their nodes, particularly if new nodes were started or contributed to in the middle or towards the end of the Bonus program.  In such a case, Service Nodes stakes are prioritized with SESH contributions in order from oldest to most recent registration until a contributor runs out of allocated SESH tokens.

### What if I'm operating a multicontributor Service Node and I'm signed up to the Service Node Bonus Program, but some or all of the contributors to my Service Node are not signed up for the program

There's still time to get all of your contributors to sign up for the Service Node Bonus program [here](https://swap.oxen.io/), and you should encourage them to do so. However if any contributor is not signed up for the Service Node Bonus Program when the snapshot is taken (a few days after the Anchor Hardfork) then that contributor will not be automigrated to the Session Network. As an operator signed up to the Service Node Bonus program your staked Oxen and earned points will still be automatically converted and swapped into SESH, however your node will be disbanded. You can claim your SESH tokens on TGE from the [Staking Portal](https://stake.getsession.org/), by connecting the Ethereum wallet you used to register for the Service Node Bonus Program.&#x20;

### What if I'm a contributor to a Service Node and the Service Node operator has not signed up to the Service Node Bonus program?&#x20;

There's still time to get your node operator to sign up for the Service Node Bonus program [here](https://swap.oxen.io/), and you should encourage them to do so. However if they are not registered for the Service Node Bonus Program when the snapshot is taken (a few days after the Anchor Hardfork) then your staked Oxen and accumulated points will still be automatically swapped into SESH tokens and you will be able to claim this amount via the [Staking Portal](https://stake.getsession.org/) with the  Ethereum wallet you signed up with for the Service Node Bonus Program on the Arbitrum One network. You can then manually contribute to a new multicontributor node via the [Staking Portal](https://stake.getsession.org/).&#x20;

### Can I unstake and opt out of the hardfork, even if I am registered for the Service Node Bonus program?&#x20;

If you choose to submit an unlock and opt out of this hardfork, any points you have earned in the Service Node Bonus program will still be honored, and Session Tokens will be available for withdrawal to your Ethereum address at TGE via the [Staking Portal](https://stake.getsession.org/), on the Arbitrum One network. However, any unstaked OXEN balances will not be automatically swapped for Session Tokens, and you will need to manually swap these coins using the [Oxen to SESH bridge](https://token.getsession.org/oxen-coin-claims) at or after TGE.

### What happens if I don’t register for the Service Node Bonus program?&#x20;

If you are not registered for the Service Node Bonus program, you will not receive any bonus Session Tokens and any staked Oxen coins will not be automatically swapped and will become unlocked, unstaked coins in your Oxen wallet at TGE. You will need to manually swap these coins using the [bridge](https://token.getsession.org/oxen-coin-claims), as above.

