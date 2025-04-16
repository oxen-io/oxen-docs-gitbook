---
description: >-
  The Anchor hardfork introduces a new requirement for all Service Nodes to
  maintain an active connection to an Arbitrum One RPC node.
---

# 🔗 Connecting to an Arbitrum One RPC Endpoint

As part of the upcoming migration to the new Session Network, the Anchor software upgrade and hardfork implements the ability for Service Nodes to witness the Arbitrum One blockchain. \
\
All Service Nodes will be required to maintain an active and up-to-date Arbitrum One RPC endpoint to complete the upgrade. This endpoint will be crucial to allow Service Nodes to witness the state of Arbitrum smart contracts, which in the future will contain the list of active Session Nodes.\
\
Operators who have participated in the Session Testnet may already be familiar with this requirement using the Arbitrum Sepolia network.\
\
**There are two main options for fulfilling this requirement:**

1. Public RPC providers
2. Running your own Arbitrum node

**Public RPC providers**

Public RPC providers handle the complexity and overhead of running an Arbitrum One node. These providers offer a public endpoint for querying. Some examples of public RPC providers that support Arbitrum One are:

* Infura [https://www.infura.io/](https://www.infura.io/)&#x20;
* Alchemy [https://www.alchemy.com/](https://www.alchemy.com/)
* GetBlock [https://getblock.io/](https://getblock.io/)
* QuickNode [https://www.quicknode.com/](https://www.quicknode.com/)&#x20;
* A full more comprehensive list of official and third party RPC providers for Arbitrum can also be found here [https://docs.arbitrum.io/build-decentralized-apps/reference/node-providers](https://docs.arbitrum.io/build-decentralized-apps/reference/node-providers)&#x20;

You can sign up for a free account with most of these providers, which typically offer free usage up to a certain limit. In most cases, their free tiers will support up to 2 or 3 Session Nodes without exceeding usage limits. If you need to support more nodes, you can either register for multiple providers and use different endpoints for each set of nodes, or consider upgrading to a paid plan for higher usage.

Note: If you are operating several nodes (More than 2-3 nodes per public provider) you might want to consider deploying a caching server, such as[ json-rpc-cache-proxy](https://github.com/sourcapital/json-rpc-cache-proxy). This setup allows you to significantly reduce the number of calls made to your RPC provider by caching common queries, improving performance and reducing costs.

**Running your own Arbitrum Node**

If you are staking multiple nodes or prefer not to rely on a public RPC provider, you may want to run your own Arbitrum One node. But be aware that the hardware requirements for running an Arbitrum One node are very demanding.

A full guide on how to run an Arbitrum One node using Nitro can be found here&#x20;

[https://docs.arbitrum.io/run-arbitrum-node/run-full-node](https://docs.arbitrum.io/run-arbitrum-node/run-full-node)&#x20;

**Upgrading**

Once you have your Arbitrum node or public provider setup, you will receive a URL or local address that will look something like this, depending on your provider or setup:

https://arb-mainnet.g.alchemy.com/v2/32bfi3gb298fbb32byfb32bf

Your node can be upgraded using the following simple commands in your CLI:

&#x20;**Syncing your repositories:**&#x20;

&#x20;sudo apt update

**Then installing updates using:**

&#x20;sudo apt upgrade

**Please note:** this will install both updated Oxen packages and any available system updates (this is a good thing!).

During the Anchor upgrade process, you will be prompted to enter your Arbitrum RPC address. Enter the URL or local address of your Arbitrum node. You also have the option to specify backup RPC nodes, by adding multiple URLs, separated by a comma. These will be queried if your primary endpoint is unavailable. Adding backup RPC nodes is optional and not required to complete the upgrade.
