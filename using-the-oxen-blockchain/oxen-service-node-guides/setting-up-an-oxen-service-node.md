# 🏎 Express service node setup guide

Thinking of running an Oxen Service Node? Great! The guide below will help you configure a device with the necessary service node software packages, and stake $OXEN to register the node on the Oxen network.

> Note: This guide assumes some familiarity with the  command line and running a server. For a more detailed walkthrough, check out our [full service node setup guide](full-service-node-setup-guide.md).

### Operating system requirements

One of:

* Debian 10 \("buster"\)
* Debian unstable \("sid"\)
* Ubuntu 18.04 \("bionic"\)
* Ubuntu 20.04 \("focal"\)
* Ubuntu 20.10 \("groovy"\)

> Note: There are strict uptime requirements for service nodes \(see [Service Node deregistration](service-node-deregistration.md)\). It is **not recommended** to run a service node on a personal device or any device which will not be constantly online. We recommend running your service node on a VPS with a reliable provider.

### Firewall Configuration

If you are using a firewall then ensure that the following ports are open/reachable

* Port 22020 \(storage server to storage server\)
* Port 22021 \(client to storage server\)
* Port 22022 \(blockchain syncing\)
* Port 22025 \(service node to service node\)
* Port 1090 \(UDP, not TCP, unlike all of the above; lokinet router data\)

### Ultra-express guide

You can configure a new Oxen Service Node by running the following 4 commands on the Linux server you want to become a service node \(these commands will work on Ubuntu; modifications may be necessary on other Linux distributions\):

```text
sudo curl -so /etc/apt/trusted.gpg.d/oxen.gpg https://deb.oxen.io/pub.gpg

echo "deb https://deb.oxen.io $(lsb_release -sc) main" | sudo tee /etc/apt/sources.list.d/oxen.list

sudo apt update

sudo apt install oxen-service-node
```

The services will run via systemd as `oxen-node.service`, `oxen-storage-server.service`, and `lokinet-router.service`.

Once the blockchain has synced to the server \(which usually takes a few hours\), your service node will be [ready to be staked](setting-up-an-oxen-service-node.md#staking-your-service-node). You can use the `oxend status` command to check blockchain sync progress.

### Express guide

#### Step 1: Initial repository setup

To add the Oxen repository, run the following commands.

> Note: You only need to follow this step the first time you want to set up the repository; after you've done it once, the repository will automatically update whenever you fetch new system updates.

This first command installs the public key used to sign the Oxen Service Node packages:

```text
sudo curl -so /etc/apt/trusted.gpg.d/oxen.gpg https://deb.oxen.io/pub.gpg
```

The second command tells `apt` where to find the packages. **Note:** Replace `<DISTRO>`with the appropriate value to match your operating system.

To find your `<DISTRO>` run the following command: `lsb_release -sc`

Alternatively, your `<DISTRO>` can be found by using the following list:

* sid      \(Debian testing/unstable\)
* buster   \(Debian 10\)
* bionic   \(Ubuntu 18.04\)
* focal     \(Ubuntu 20.04\)
* groovy    \(Ubuntu 20.10\)

```text
echo "deb https://deb.oxen.io <DISTRO> main" | sudo tee /etc/apt/sources.list.d/oxen.list
```

Then resync your package repositories with:

```text
sudo apt update
```

#### Step 2: Oxen Service Node configuration

To configure your service node, simply install the `oxen-service-node` package:

```text
sudo apt install oxen-service-node
```

This will detect your public IP \(or allow you to enter it yourself\) and automatically update the /etc/oxen/oxen.conf configuration file with the necessary additional settings to run a service node.

Congratulations! Your service node is now ready to be registered and staked.

### Staking your service node

#### Preparing your service node for registration

To prepare your service node for registration, run the following command:

```text
oxend prepare_registration
```

This will prompt you for some registration details, then output a registration command. Copy the output from this command in preparation for the next step.

> Note: You can safely run this command multiple times if you change your mind about some of the registration questions before you submit the registration.

#### Staking and registering your service node

To stake and register your service node, open the Oxen GUI wallet. Make sure your wallet has a balance of at least 15,000 $OXEN to meet the service node staking requirement \(less if you're configuring a [shared service node](full-service-node-setup-guide.md#5-2-setting-up-a-pooled-service-node)\). Navigate to the `Service Nodes`tab &gt; `Registration` section, and paste the output from the above command, then click **Register Service Node**. 

Done! Your staking transaction will now be submitted to the network, and after a short delay, your service node will be registered and begin contributing to the network \(and receiving rewards!\).

#### Checking registration status

You can easily check if your service node is registered on the network. First, connect to the VPS where the service node is running and run the following command to retrieve your service node's public key:

```text
oxend status
```

This will output a bunch of information about your service node, but there's one part we're interested in at this stage: The long string of random letters and numbers after the characters `SN:` . This string is your service node's public key, used to identify your service node on the list of registered and operational service nodes. Select and copy the public key \(do not copy any of the surrounding information\).

You can now jump onto [oxen.observer](https://oxen.observer/), open the full list of active service nodes, and use Cmd+F/Ctrl+F to check if your service node's public key appears in the list.

#### Upgrading

When a new release is available, upgrading is as simple as syncing your repositories:

```text
sudo apt update
```

Then installing any updates using:

```text
sudo apt upgrade
```

> Note that this will install both updated Oxen packages _and_ any available system updates \(this is generally a good thing!\)

During the upgrade, all running instances of `oxend` will be restarted to switch them over to the updated `oxend`.

If for some reason you want to install _only_ updated Oxen package upgrades but not other system packages, then instead of `sudo apt upgrade` you can use:

```text
sudo apt install oxen-storage-server oxend lokinet-router
```

Having trouble? Just [head to our Support section](../../support.md).

