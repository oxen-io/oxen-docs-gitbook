# 🌐 Lokinet

Lokinet is open-source software which uses the Oxen Service Node network to operate a low-latency onion routing protocol. This allows users to browse the web without the destination or origin of data packets being exposed via [Lokinet exit nodes](exit-nodes.md), as well as accessing internally-hosted services called [SNApps](snapps/). 

### Why Lokinet?

Lokinet is an open-source, fully decentralised overlay network which allows users to browse the internet privately and securely, as well as access services \(called SNApps\) hosted on Lokinet. Lokinet achieves this through use of a low-latency onion routing protocol \(LLARP\) designed as a hybrid between I2P and Tor, while providing additional benefits. 

When using Lokinet to browse the internet, encrypted data packets are routed through multiple service nodes. No single node ever knows the full path your data takes, so you can browse with true anonymity. Since these nodes are part of the Oxen network, they are also required \(and economically incentivised\) to provide high-quality service, unlike onion routers like Tor which rely on volunteers. 

Lokinet also provides the benefit of being protocol-agnostic — while competing onion routers operate on the transport layer and are only able to carry TCP traffic, Lokinet operates on the network layer and is therefore able to onion-route any IP-based protocol: TCP, UDP, ICMP, etc. In plainer terms, this means Lokinet can relay real-time voice and video calls, video streams, and other high-bandwidth content which simply can't be routed over other onion routers.

### Get Lokinet

To download Lokinet, head over to [the Lokinet website](https://lokinet.org/). 

