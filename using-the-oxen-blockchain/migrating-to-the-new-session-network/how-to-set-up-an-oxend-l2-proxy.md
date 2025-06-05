# 🔀 How to set up an oxend L2 proxy

{% hint style="warning" %}
**The Oxen Network has transitioned to the Session Network. More information** [**here**](https://oxen.io/blog/development-is-transitioning-to-session-token)**.**&#x20;
{% endhint %}

With the 11.1.2 Oxen release, a new capability for oxend was been added to proxy requests to an L2 provider. This feature is aimed in particular at operators who intend to run multiple nodes on the new Session Network following the migration.

This feature works by having only 2-3 of your nodes configured with an L2 provider URL, and having all the others configured to talk to those 2-3 nodes to get Arbitrum updates, without having to use multiple or paid tier accounts for your set of nodes.

This feature works through the existing oxen "quorumnet" port, and so does not need to open additional ports, but it does require some one-time configuration that this guide will walk you through.

### Step 1: Choosing your proxies <a href="#step-1-choosing-your-proxies" id="step-1-choosing-your-proxies"></a>

It is suggested to choose least two nodes, on different servers, ideally in different data centers. The idea here is redundancy: if one of your proxy nodes has a problem, you don't want that one node to take down _all_ your other nodes relying on it. Configuring 2-3 different ones on different servers avoids this risk.

Note that it doesn't matter if the proxies are active service nodes or not: the main point is that they are running in service node mode so that they will have a reachable quorumnet port that the proxy-using nodes use to communicate with the proxy. In other words, even if the proxy gets deregistered or unlocks the network, it will still continue to function as a proxy.

On each of these proxies, you will need to edit the /etc/oxen/oxen.conf configuration file (or, if you are using a dedicated server with a multi-sn setup, the /etc/oxen/node-XX.conf file for whichever service node on the server will be the proxy), and add configuration lines to set up the L2 provider URLs.

There are a couple of config items you'll need to add to the `oxen.conf` file: one or more `l2-provider` lines specifying how the proxies themselves fetch Arbitrum data, and an `l2-proxy=FILENAME` option that specifies a file containing the pubkey of other nodes allowed to use your node as a proxy. You'll edit the file (e.g. with `nano /etc/oxen/oxen.conf`) and add some lines like this:

```
l2-provider=https://first.provider.url/abc
l2-provider=https://second.provider.url/xyz
l2-proxy=/etc/oxen/proxy.txt
```

It's also suggested to add the following line when setting things up the first time so that you can track the L2 proxy usage in the logs. Once everything is set up and working, you can come back and delete this line to reduce the amount of log verbosity:

```
log-level=l2_proxy=debug,l2_tracker=debug
```

Also make a note of the `service-node-public-ip=xxx` line, and, if present, the `quorumnet-port=xxx` lines. You'll need these values in Step 4. (If the quorumnet-port= line is missing, that's fine. This just means it is using the default port).

On the _second_ proxy node, you'll need to to reverse the order of the two l2-provider lines. That way the first proxy will use "first.provider.url" as its primary source of info, and the second will use "second.provider.url", and each one will use the other's primary source as a backup source. This is only one possible suggestion: you may want to have more backups, or use completely different providers on each proxy.

### Step 2: Whitelisting proxy-using nodes <a href="#step-2-whitelisting-proxy-using-nodes" id="step-2-whitelisting-proxy-using-nodes"></a>

The second step we need to do is to list the service node pubkeys of all of our nodes that are allowed to use the proxy in the /etc/oxen/proxy.txt. This is relatively straightforward (if a bit tedious, if you have a lot of nodes):

```
nano /etc/oxen/proxy.txt
```

Add the pubkeys of nodes to allow, one per line. You can use comments (starting with #) in here if you like. For example:

```
abc00a97b44e7a70202f8cb1f36637b25b9efc36ecab04a5e687290d1345e715 # my favourite
def647edf7706aa9d4e17ae871992dab6cd3654653960a5c1d18585c53d848a9
#9f022f08ecb801299cb43fafba721dfb7d9deb10a7734951f46b9c7c8ec1c274
1f216f4a8d132fcf7d74657dd82167a2037c5bbe40abefdf7a533133f0134601 # vps on SuperAwesomeISP!
```

This would allow access from `abc0...`, `def6...`, and `1f21...`

Two important notes here:

* The proxies will monitor this file for changes, so it is _not_ necessary to restart the oxend proxies if you add or remove node pubkeys to the file.
* If you have very old nodes (first installed in Oxen 7.x or earlier) then you may have a node with different "primary" and "ed25519" keys: for such a node with dual keys you want to use the "ed25519" pubkey, not the main service node public key. (For nodes installed since then, the primary and ed25519 pubkeys will be the same. If you are unsure, dual-key service nodes will have both a `key` and a `key_ed25519` file; unified key nodes have only the `key_ed25519` file. Alternatively, you can look up your service node on https://oxen.observer: if the Session Node Details lists separate `Session Node Public Key` and `Session Node Auxiliary Pubkey` values then you want to use the latter (Auxiliary) key.

### Step 3: Restart the proxy nodes <a href="#step-3-restart-the-proxy-nodes" id="step-3-restart-the-proxy-nodes"></a>

Now it's time to restart the proxy oxends with the new proxy-enabling configuration to set themselves up to allow proxy requests:

```
systemctl restart oxen-node
```

(If you are using a multi-sn config and have configured node "00" as the proxy, then you would use `oxen-node@00` instead of just `oxen-node`).

### Step 4: Configure the proxy-using nodes <a href="#step-4-configure-the-proxy-using-nodes" id="step-4-configure-the-proxy-using-nodes"></a>

All of the nodes using proxies now need to be configured to get their L2 data from the proxies. To do this, you edit the config file of the proxy-using node (e.g. `nano /etc/oxen/oxen.conf`) and add lines to the config file for each oxend proxy such as:

```
l2-oxend=10.7.8.9:22025/1f203f36faecd16d1c1c9514143d4a6715be1b16e160760f57942ab6da3e4ed5
l2-oxend=10.99.88.77:22025/1f509f05c49da0478818ee5772c6e15b457b0923ece38d034d9d4f601a161419
```

Also look for any existing `l2-provider=...` lines and either delete them or comment them out by adding a `#` at the beginning of the line: oxend does not support using L2 oxend proxies and direct L2 RPC providers at the same time.

You need to replace the IPs, ports and pubkeys listed here with those of your proxy nodes.

The whole line is logged by the proxy during startup, such as:

```
[2025-03-13 16:51:20] [+0.011s] [l2_proxy:info|l2_tracker_proxy.cpp:150] L2 proxy whitelist intialized with 3 pubkeys
[2025-03-13 16:51:20] [+0.011s] [global:info|cryptonote_core.cpp:763] Running as an L2 proxy reachable at:
	10.23.45.67:22501/9f301f21400a69a43286b8f5add7faec26b948de2862a745f71eab822ccd7b1c
```

But you don't have to get it that way: The IP and port you may have written down from Step 1; the pubkey here is the Ed25519 pubkey of the proxy. If you didn't write them down, the IP is simply the public IP of the proxy, and the port is the quorumnet port of the proxy: by default this is 22025 for mainnet nodes and 11025 for stagenet nodes, but if your proxy node config file specifies an alternative you will need to use that port value instead.

The pubkey here is the node's Ed25519 pubkey, which could be different from the primary pubkey if node has dual pubkeys; see the note about this in Step 2.

Once you have added the l2-oxend= line, restart the node with `systemctl restart oxen-node` (or `oxen-node@NN` for a multi-sn node).

If you check the logs during startup, you should see some messages such as:

```
[2025-03-13 20:05:54] [+2.072s] [l2_proxy:info|l2_tracker_proxy.cpp:441] Connected to remote oxend L2 proxy 12345678...cdef @ tcp://1.2.3.4:22025
[2025-03-13 20:05:54] [+2.073s] [l2_proxy:info|l2_tracker_proxy.cpp:490] Subscribed to L2 updates from 12345678...cdef @ tcp://1.2.3.4:22025
```

indicating that it has successfully subscribes to L2 updates from the proxy.

### All done! <a href="#all-done" id="all-done"></a>

If you want to add new nodes in the future that use your proxies, all you need to do is add their pubkeys into your /etc/oxen/proxy.txt files on the proxies, and add the `l2-oxend=...` lines into the proxy-using oxend configs and restart the proxy-using oxends. (The proxy nodes themselves do _not_ need to be restarted to pick up the proxy.txt changes).

### Advanced Configurations <a href="#advanced-configurations" id="advanced-configurations"></a>

#### Non-service node proxy <a href="#non-service-node-proxy" id="non-service-node-proxy"></a>

Proxies do not have to be running in service node mode at all, but you will have to make one additional config change to make the proxy accessible to other nodes if they are not (because non-service-nodes do not listen on the quorumnet port by default). The change is to add a line to the proxy's config of:

```
lmq-curve=tcp://0.0.0.0:12345
```

This will add a listener on port 12345 (you can change this to whatever you like), accessible on the machine's IP. Note that you don't need to replace 0.0.0.0 here: that special address means to listen on all available IPs on the machine. The proxy-using nodes then specify the IP of the machine (the actual IP, _not_ 0.0.0.0) and port 12345 in their config, along with the node's Ed25519 pubkey.

Nothing stops you from adding this line on a service node to add an additional listener, though there is no noticeable benefit of doing on a service node compared to simply using the required quorumnet port.

#### Local unix socket on the same machine <a href="#local-unix-socket-on-the-same-machine" id="local-unix-socket-on-the-same-machine"></a>

If your proxy-using node is on the same machine as the proxy node itself then you can use its local oxend socket rather than the quorumnet port. In this case you would configure the proxy-using nodes using:

```
l2-oxend=ipc://PATH_TO_OXEND_SOCK
```

For example:

```
l2-oxend=ipc:///var/lib/oxen/oxend.sock
```

When using such a local unix socket connection, you do _not_ append the proxy's pubkey to the l2-oxend= line, nor is it necessary to list the pubkeys of nodes accessing via the unix socket in the proxy.txt file (it won't hurt, but it isn't needed).

#### **Multi SN**&#x20;

For Multi SN users, please see this additional [guide](https://github.com/jagerman/loki-multi-sn?tab=readme-ov-file#using-oxend-as-an-l2-proxy).
