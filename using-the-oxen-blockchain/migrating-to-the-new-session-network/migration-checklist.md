---
description: >-
  Everything you need to do to prepare for the migration to the new Session
  Network.
---

# ✅ Migration Checklist

The migration from the Oxen Network to the new Session Network is nearly here! This is a convenient checklist for operators of active and registered Oxen Service Nodes who are migrating to the Session Network.&#x20;

### Node Preparation and Readiness&#x20;

* Confirm hardware and connectivity capacity. Session Nodes have largely similar requirements to Oxen Service Nodes, with some differences:
  * Ubuntu 22.04+ (latest LTS recommended) or Debian 11+ (latest stable recommended).&#x20;
    * The 11.2 and 11.3 releases will be the last to support Ubuntu 20.04, as their upstream update support is close to ending.  Future releases will support Ubuntu 22.04+.
  * 45GB or more of storage
    * Double check the amount of storage space on your server/s. Make sure you have at least 1GB additional space for future growth. Session Nodes have higher storage requirements compared to the previous minimum recommendation of 40GB for Oxen Service Nodes.&#x20;
  * 4-8GB of RAM (4GB absolute minimum)
  * 100Mb or faster connectivity
  * 1TB traffic per month or more
  * Redundant power with remote cycling ability, as found in most data centres
* Join relevant comms channels: Ensure you're in the [Session Token Community](https://discord.gg/sessiontoken) on Discord and following on X ([Oxen](https://x.com/oxen_io) | [Session Token](https://x.com/session_token)) to receive timely updates
* Register for the Service Node Bonus Program using your Oxen wallet address and Ethereum wallet address. This will automate the Session Token swapping process following the transition and ensure network continuity. Register [here](https://swap.oxen.io/).
* Make sure you can access the ETH wallet addresses you used to register for the Service Node Bonus and testnet programs. You can check [here](https://swap.oxen.io/).

Optional:&#x20;

* Set up a testnet node on the Session testnet to explore how the new web-based Staking Portal works, from registering to managing nodes. Open a ticket in [Discord](https://discord.gg/sessiontoken) to request access to test SESH tokens and test arbETH to stake to your testnet node, or post a message in the Oxen Service Node Operators community on Session.&#x20;

### Completing the Anchor Software Upgrade and Hardfork (22-29/04/2025)

* Set up an Arbitrum RPC Provider. All Service Nodes must maintain an active connection to an Arbitrum One RPC node to complete the Anchor software upgrade and hardfork. Learn how to fulfill this requirement [here](https://docs.oxen.io/oxen-docs/using-the-oxen-blockchain/oxen-service-node-guides/connecting-to-an-arbitrum-one-rpc-endpoint).
* Upgrade your Service Node to Oxen 11.2.0 (Anchor). Follow the upgrade instructions [here](https://oxen.io/blog/oxen-anchor-hardfork-11-1-0) and complete the upgrade as soon as possible during the one week upgrade period.
* Monitor your Service Node in case something goes wrong and it gets decommissioned. &#x20;
  * There is a 5-day grace period after the 29/04/25 hardfork takes effect during which network deregistrations won’t happen—in case anyone needs extra time to address an issue at the fork—but it is strongly recommended you address any problems as quickly as possible.
* Back up your Service Node keys, including the new \`key\_bls\`.  Oxen 11 adds a new BLS key, used for interactions with the smart contract, that needs to be restored along with the existing key\_ed25519 in case you need to move or restore your Service Node.
* Double check you have registered for the [Service Node Bonus program](https://swap.oxen.io/) with the correct Ethereum address. Snapshots will be taken prior to Landing Hardfork to calculate Session Token distribution.
  * If you want to change your registered Ethereum address, simply [register](https://swap.oxen.io/) again using your updated Ethereum address.

Optional:

* Set up an oxend L2 proxy to help reduce costs for running multiple nodes. For operators planning to run multiple nodes on the new Session Network, check out this [guide](https://docs.oxen.io/oxen-docs/using-the-oxen-blockchain/oxen-service-node-guides/how-to-set-up-an-oxend-l2-proxy).

### Landing Software Upgrade and Hardfork (06/05/2025-TBD)

* Fund your Ethereum address with some Ethereum on the Arbitrum One network. You will need a small amount to cover transaction fees which are required to claim rewards and register new nodes.
  * Note that SESH will be distributed on the Arbitrum One network, using the Ethereum address provided. To view your SESH at TGE, ensure you have selected the Arbitrum One network in your wallet.
* Upgrade your Service Node to Oxen 11.3.0 (Landing).
  * Find upgrade instructions via the Oxen blog. There will be a mandatory upgrade period following the release of Landing binaries. The Landing Hardfork and network migration will be executed at the end of the  upgrade period. **The date for the activation of the Landing Hardfork has not yet been determined**, but it will be within a few weeks of the Landing software upgrade release.
  * A grace period of 5 days will be implemented following the Landing Hardfork before nodes start to deregister
* Query your node’s status using `oxend print_sn_status` to ensure that it is running correctly.
* On TGE day, verify the Session Network [contract addresses](http://token.getsession.org/contract-addresses) and add Session Token to your wallet, using this [guide](https://docs.getsession.org/user-guides/for-beginners/how-to-view-sesh-in-your-wallet).
* Confirm the re-staking of Session Tokens using the [Staking Portal.](https://stake.getsession.org/)



