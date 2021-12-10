# 🧑‍💻 SNApps

Lokinet SNApps allow users to interact with applications or services entirely within Lokinet, similar to Tor's hidden services. SNApps provide an even higher degree of anonymity than can be achieved when accessing externally hosted content through [exit nodes](../exit-nodes.md). SNApps allow for users to set up and host marketplaces, forums, whistle-blowing websites, social media sites, and other web-based applications on their own servers while maintaining full server- and client-side anonymity. SNApps greatly expand what's possible with Lokinet, and allow users to build meaningful communities entirely within Lokinet itself.

SNApp operators use the traditional server-client model, with the key difference being that [Oxen Service Nodes](../../../about-the-oxen-blockchain/oxen-service-nodes.md) act as intermediaries in a user's connection to the SNApp server. When a SNApp wishes to register on the network, it must update the DHT with its descriptor. This descriptor contains various introducers, which are specific Oxen Service Nodes that users can contact to form a path to the SNApp. When these paths are set up, users can connect to the SNApp without either party knowing where the other is located in the network.

### SNApp 📚 Guides

|                                         |                                                                       |
| --------------------------------------- | --------------------------------------------------------------------- |
| **Guide**                               | **Description**                                                       |
| [Accessing SNApps](accessing-snapps.md) | How to access SNApps.                                                 |
| [Hosting SNApps](hosting-snapps.md)     | Hosting your own SNApp (making a web service available over Lokinet). |
