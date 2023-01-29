---
description: >-
  Lokinet is our powerful general-purpose onion router, using Oxen Service Nodes
  as routers for encrypted data
---

# 🌐 Lokinet

Lokinet is a cutting-edge onion router that uses the Oxen Service Node network to relay traffic. Lokinet’s power lies in its ability to route both TCP and UDP traffic via network-layer onion routing — in practical terms, this means that Lokinet can be used for secure, highly anonymised video streaming, voice and video calling, and other high-bandwidth use cases that are impossible using competing onion routers.

Of the tools in the Oxen ecosystem, Lokinet has the broadest range of possible applications for developers; if implemented correctly, it allows any new or existing web application to completely anonymize its incoming and outgoing network connections. Developers can also use Lokinet in their applications to access clearnet addresses using exit node functionality. You can read more about exit nodes [here](https://docs.oxen.io/products-built-on-oxen/lokinet/exit-nodes).

### Usage examples

#### Anonymous voice chat

Mark wants to create a realtime voice chat application which uses VoIP to allow groups of users to communicate with each other synchronously. He wants his application to respect the privacy of its users, and respect his privacy as the operator of the service, by not collecting any identifying information, like IP addresses. Mark can accomplish this by hosting his voice chat server on Lokinet.

#### Anonymous website access

Anna runs an online video streaming platform. Recently, her customers have complained that their ISPs are blocking them from accessing her website. Anna wants her customers to be able to access her website and bypass blocking by ISPs. Anna can accomplish this by setting up her website to be accessible on Lokinet, providing her customers with total anonymity.

### Developing with Lokinet: A quickstart guide

#### Server-side

As a developer, you are typically going to be hosting your application from a VPS or dedicated server. You’ll need to install the Lokinet package on your server in order to make your web application accessible via Lokinet.

_Video guides:_

[https://www.youtube.com/watch?v=hENA75XV7\_s](https://www.youtube.com/watch?v=hENA75XV7\_s)

[https://www.youtube.com/watch?v=FPPs2FFGOGI](https://www.youtube.com/watch?v=FPPs2FFGOGI)

#### Client-side

Using applications and services over Lokinet from the client side is typically very straightforward if your application supports hostnames (rather than relying only on IP addresses) and the user has Lokinet installed and running. If these requirements are met, your application or service should be able to work with no changes or updates.

Example: a Mumble (VoIP chat) server exists at the .loki address `probably.loki` (name purchased via ONS), port 64738. Using the stock Mumble [application](https://www.mumble.info/downloads/) with no changes, any user with Lokinet installed and running can access this server by simply entering `probably.loki` as the server address.

What’s happening behind the scenes? When a user installs Lokinet, they also install a DNS resolver into the host system. When a .loki TLD is entered into your application, Lokinet will resolve this .loki address, build an anonymised path to the service, and then return a local IP address to your application. Any packets sent to that local IP address will be wrapped and sent down the anonymised path to reach the server hosting the .loki address.

#### ONS (Oxen Name Service)

By default, Lokinet addresses are long pseudorandom strings which contain the public key of the Lokinet service, versioning information, and other important protocol information:

dw68y1xhptqbhcm5s8aaaip6dbopykagig5q5u1za4c7pzxto77y.loki

These addresses are difficult for users to remember, and hard for developers to communicate to users. If you want a human-readable name for your application, you can purchase an alphanumeric ONS name to be mapped to your service’s .loki address. This mapping is then stored in the Oxen blockchain. ONS names can be purchased inside the Oxen desktop wallet or CLI wallet. You can check out our guide on how to do this [here](../using-the-oxen-blockchain/using-oxen-name-system.md).

#### Lokinet library

Currently, users need to install and run the Lokinet client in order to access applications and services over Lokinet. Although Lokinet clients can be bundled with existing application installers, this increases installer size and can confuse users about what is being installed on their computer.

We are currently in the early stages of developing a library implementation of Lokinet which will be able to be imported into any existing application and called on when Lokinet connections need to be made. This will allow developers more granular control over Lokinet, and make it easier to seamlessly integrate Lokinet into applications without relying on users installing additional software packages.
