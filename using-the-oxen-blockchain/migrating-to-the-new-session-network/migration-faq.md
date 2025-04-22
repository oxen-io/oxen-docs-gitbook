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

The automatic migration mechanics are designed to ensure the majority of nodes are migrated to the new Session Network. This is ensured in a few different ways.\
\
Firstly, staking requirements on the Session Network will be determined in such a way that operators who have been registered for the Service Node Bonus Program since the beginning of the program will have more than enough SESH to have their nodes automatically restaked. Similarly, operators and contributors to multicontributor nodes, where the operator and all contributors were registered for the Service Node Bonus Program from the beginning, will also have more than enough SESH for their node to be auto re-staked on the Session Network.

Secondly, a system of automatic reallocation will ensure that any excess SESH generated goes to nodes that don’t meet the staking requirements of the Session Network after the conversion of staked Oxen and Service Node Bonus points, allowing as many of these nodes as possible to be re-staked. This is particularly important when it comes to ensuring multicontributor nodes are able to be re-staked.

At a high level, the migration process works by assigning each participating wallet a “pool” of SESH tokens, based on the converted value of their staked Oxen and the points earned in the Service Node Bonus Program. The SESH in this pool is then reallocated to nodes the wallet was operating or contributing to, following a predefined algorithm. This algorithm ensures that tokens are distributed in a way that preserves as many existing node relationships as possible.

Let’s walk through a few example scenarios with some invented values to illustrate how the algorithm works.

**Scenario 1:**\
An operator is running 3 solo nodes on the Oxen network and is enrolled in the Service Node Bonus Program. On TGE, their combined converted stake and bonus points amount to 1,000 SESH, which becomes the balance of the “pool” of tokens used by the auto migration algorithm.

* Staking requirement: 200 SESH per Session Node
* The algorithm will allocate 200 SESH to each of the 3 nodes (starting from the oldest registered to the newest registered), consuming 600 SESH total.
* The remaining 400 SESH will be left over as excess, which the operator can claim from the [Staking Portal](https://stake.getsession.org/) and either re-stake or withdraw to their wallet.

The result is that the operator has 3 nodes which are automatically migrated and restaked on the Session network and has an amount of excess SESH which can be claimed from the [Staking Portal](https://stake.getsession.org/) and used to restake or stored in their wallet.

**Scenario 2:**\
Lets consider a more complex scenario, an operator stakes 2 solo nodes and contributes 25% of the stake to a third, multi-contributor node. At TGE, their pool contains 300 SESH (this smaller amount might be due to the fact that one of the solo nodes and the multi-contributor node were operating for only part of the duration of the Bonus Program later and thus earned fewer points).

* The algorithm allocates 200 SESH to one solo node.
* The second solo node is disbanded due to insufficient remaining stake, since it requires 200 SESH and only 100 SESH is available in the pool.
* Of the 100 SESH left, 50 SESH is used to provide 25% of the required stake to the multi-contributor node.
* The final 50 SESH is marked as excess and can be claimed through the [Staking Portal](https://stake.getsession.org/).

The result is that the operator goes from operating 2 nodes down to 1 node, preserves their stake in the multicontributor node and ends up with 50 SESH excess. They could claim this excess SESH from the [Staking Portal](https://stake.getsession.org/) and use it to stake a new multicontributor node using the same VPS/hardware as they previously used to run their second node or just withdraw the excess to their wallet.

Note: SESH is only allocated to nodes that the wallet previously operated or contributed to, and only up to the same percentage they were originally staking.

**Scenario 3:**\
Lastly, let’s consider a scenario focused on contributors. A contributor is staking 25% of the stake for 5 different multicontributor nodes. At TGE, their pool is calculated to be 100 SESH. Assume nodes 1 and 2 were operated by operators who did not register for the Service Node Bonus Program.

* Nodes 1 and 2 are disbanded because their operators did not register for the SN bonus program
* The contributor’s pool is reallocated to the remaining nodes (3, 4, and 5), starting from the oldest.
  * 50 SESH is used to cover 25% of the stake for node 3.
  * 50 SESH is used to do the same for node 4.
  * Node 5 is disbanded due to lack of available stake from the contributor.
  * Contributors and operators signed up to the SN bonus program from disbanded nodes (1, 2, and 5) will have their converted stakes and points returned to their respective pools. The algorithm will re-run on these new pool values to try to preserve as many nodes as possible.

The end result here is that this contributor’s SESH tokens are automatically reallocated to 2 multicontributor Session Nodes instead of the previous 5 Oxen Network nodes and that the contributor completes the migration with no excess SESH.

### What are the staking requirements for Session Nodes?

The exactly full stake amount for Session Nodes is yet to be determined, but it is estimated to be between 20k-23k SESH tokens.&#x20;

### What if my node deregisters after the Anchor Hardfork?

If your node deregisters after the Anchor Hardfork but before the Landing Hardfork, any points you have earned in the Service Node program will still be honored and the earned amount of SESH will be credited to your Ethereum address, available as a rewards, withdrawable any time after TGE, on the Arbitrum One network, via the [Staking Portal](https://stake.getsession.org/). However, your principal stake will not be automatically swapped for Session Tokens. Your Oxen balance in your Oxen wallet will unlock at the Landing hardfork and you will need to manually migrate these coins using the Oxen to SESH bridge at or after TGE.

### What if I end up with more SESH than is needed to restake my nodes?

If the amount of SESH you receive from the Service Node Bonus program and the automatic swap is more than is required to re-stake your nodes after migration, any additional leftover tokens will be claimable as staking rewards via the [Staking Portal](https://stake.getsession.org/).

### What if I don't end up with enough SESH for all my nodes to be restaked after the migration?

If the amount of SESH you receive from the Service Node Bonus program and the automatic swap is not sufficient for your nodes to be restaked following the migration, some of your nodes will be disbanded at TGE and the migrated SESH instead applied to fill the required stake for other nodes you have staked into. Any excess that does not get contributed into a Session Node stake will still be swapped automatically. You can then claim this excess SESH as staking rewards via the [Staking Portal](https://stake.getsession.org/).

### If I was operating a solo Service Node for the duration of the Service Node Bonus Program and I’m signed up for the program will I receive enough tokens to meet the new Session Node staking requirement?&#x20;

Yes, the new Session staking requirement will be set such that a solo Service Node operator who has been running a Service Node for the entire Service Node Bonus Program and who is signed up will receive more than enough Session Tokens to be automatically staked to the Session network when the migration occurs.  Operators who have started new nodes during the Bonus Program may not have accumulated enough points to cover all the new nodes, in which case some nodes might get disbanded to provide enough SESH to stake the remaining nodes (see the previous question).

### If I was operating a multicontributor Service Node for the duration of the Service Node Bonus Program and my contributors and I are signed up will we receive enough tokens to meet the new Session Node staking requirement?&#x20;

Yes, the new Session staking requirement will be set such that multicontributor Service Nodes which have been running for the entire Service Node Bonus Program and who have all contributors and the operator signed up will receive enough Session Tokens to be automatically re-staked to the Session Network when the migration occurs.

It is possible, however, that either the operator or some contributors may not have enough SESH to cover the stakes of all of their nodes, particularly if new nodes were started or contributed to in the middle or towards the end of the Bonus program.  In such a case, SESH is reallocated to be staked in other Session Nodes you are part of, as per the automigration algorithm.

### What if I'm operating a multi contributor Service Node and I'm signed up to the Service Node Bonus Program, but some or all of the contributors to my Service Node are not signed up for the program?

There's still time to get all of your contributors to sign up for the Service Node Bonus program [here](https://swap.oxen.io/), and you should encourage them to do so. However if any contributor is not signed up for the Service Node Bonus Program when the snapshot is taken (a few days after the Anchor Hardfork) then that contributor will not be automigrated to the Session Network. As an operator signed up to the Service Node Bonus program your staked Oxen and earned points will still be automatically converted and swapped into SESH, however your node will be disbanded. If you were operating or contributing to other nodes, your auto converted SESH from disbanded nodes will be used as per the auto migration algorithm to fill the stakes of any of the nodes you were involved in. You can claim any excess SESH tokens on TGE from the [Staking Portal](https://stake.getsession.org/), by connecting the Ethereum wallet you used to register for the Service Node Bonus Program. \


### What if I’m operating a multicontributor Service Node and I'm signed up to the Service Node Bonus Program but either I or some of the contributors to my Service Node do not meet the minimum staking requirement for the Session network

Service nodes where any contributor (including the operator) does not have sufficient SESH after the automatic swap, contributions as part of the conversion process will be disbanded at the hardfork, and all the contributions in the node will be credited to the contributors.

In the case where a contributor is part of multiple nodes, the SESH that you are credited is automatically used to fulfill the staking requirements of the other nodes they were staked to.

For example, a solo node operator staking since the start of the Service Node Bonus program would have earned enough Oxen to stake to a multicontributor node near the end of the program. However they likely wouldn’t have earned enough points from this multicontributor node alone to fill up a full slot in the multicontributor node during the migration. The catch here is that because of the design of the migration algorithm, excess SESH generated from the Solo node can be used to top up the SESH required to achieve staking of a full slot in the multi contributor node, ensuring both nodes are automigrated.\
\
Any leftover contributions that do not get staked will be credited as unstaked rewards that can be withdrawn to your registered Ethereum wallet via the [Staking Portal](https://stake.getsession.org/) once the network has successfully transitioned after TGE

### What if I'm a contributor to a Service Node and the Service Node operator has not signed up to the Service Node Bonus program?&#x20;

There's still time to get your node operator to sign up for the Service Node Bonus program [here](https://swap.oxen.io/), and you should encourage them to do so. However if they are not registered for the Service Node Bonus Program when the snapshot is taken (just before the Landing Hardfork upgrade release) then your staked Oxen and accumulated points will still be automatically swapped into SESH tokens and run through the migration algorithm. You will be able to claim any unstaked excess SESH amount via the [Staking Portal](https://stake.getsession.org/) with the  Ethereum wallet you signed up with for the Service Node Bonus Program on the Arbitrum One network.&#x20;

### Can I unstake and opt out of the hardfork, even if I am registered for the Service Node Bonus program?&#x20;

If you choose to submit an unlock and opt out of this hardfork, any points you have earned in the Service Node Bonus program will still be honored, and Session Tokens will be available for withdrawal to your Ethereum address at TGE via the [Staking Portal](https://stake.getsession.org/), on the Arbitrum One network. However, any unstaked OXEN balances will not be automatically swapped for Session Tokens, and you will need to manually swap these coins using the [Oxen to SESH bridge](https://token.getsession.org/oxen-coin-claims) at or after TGE.\


### What happens if I don’t register for the Service Node Bonus program?&#x20;

If you are not registered for the Service Node Bonus program, you will not receive any bonus Session Tokens and any staked Oxen coins will not be automatically swapped and will become unlocked, unstaked coins in your Oxen wallet at TGE. You will need to manually swap these coins using the [bridge](https://token.getsession.org/oxen-coin-claims), as above.

\


