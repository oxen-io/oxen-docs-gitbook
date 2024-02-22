# 🏎 Express service node setup guide

Thinking of running an Oxen Service Node? Awesome! The guide below will help you configure a device with the necessary Service Node software packages, and stake $OXEN to register the node on the Oxen network.

> Note: This guide assumes some familiarity with the command line and running a Linux server. For a more detailed walkthrough, check out our [full Service Node set-up guide](full-service-node-setup-guide.md).

### Operating system requirements

One of:

* Debian 12 ("bookworm")
* Debian 11 ("bullseye")
* Ubuntu 22.04 ("jammy")
* Ubuntu 20.04 ("focal")

Recommended: the latest Debian stable or Ubuntu LTS release (12 and 22.04, respectively, as of the time of writing).

> Note: There are strict uptime requirements for Service Nodes (see [Service Node deregistration](service-node-deregistration.md)). It is **strongly discouraged** to run a Service Node on a device that will not be continuously on-line. We recommend running your Service Node on a VPS with a reputable provider.

### Firewall Configuration

If you are using a firewall then you should ensure that the following ports (TCP unless otherwise
noted) are open and reachable:

* Port 22020 (storage server connectivity; requires *both* TCP and UDP)
* Port 22021 (client to storage server)
* Port 22022 (blockchain syncing)
* Port 22025 (Service Node to Service Node)
* Port 1090 (Lokinet router data; UDP only)

### Ultra-express guide

Configuring a new Oxen Service Node is as simple as running the following 4 commands on the Linux server you want to become a node (these commands will work on Debian and Ubuntu; modifications may be necessary for other Linux distributions):

```
sudo curl -so /etc/apt/trusted.gpg.d/oxen.gpg https://deb.oxen.io/pub.gpg

echo "deb https://deb.oxen.io $(lsb_release -sc) main" | sudo tee /etc/apt/sources.list.d/oxen.list

sudo apt update

sudo apt install oxen-service-node
```

The services will run via systemd as `oxen-node.service`, `oxen-storage-server.service`, and `lokinet-router.service`.

Once the blockchain has synced to the server (which can take several hours), your Service Node will be [ready to be staked](setting-up-an-oxen-service-node.md#staking-your-service-node). You can use the `oxend status` command to check the sync progress.

Alternatively, the blockchain can typically be downloaded in a fraction of the time required to sync it via the Service Node network, using the following command:

```
sudo oxend-download-lmdb https://public.loki.foundation/loki/data.mdb
```

### Express guide

#### Step 1: Initial repository set-up

To add the Oxen repository, run the following commands.

> Note: You only need to follow this step once, to set up the repository. The repository will subsequently be automatically updated whenever you fetch new system updates.

This first command installs the public key used to sign the Oxen Service Node packages:

```
sudo curl -so /etc/apt/trusted.gpg.d/oxen.gpg https://deb.oxen.io/pub.gpg
```

The second command tells `apt` where to find the packages. **Note:** Replace `<DISTRO>`with the appropriate value to match your operating system.

To find your `<DISTRO>` run the following command: `lsb_release -sc`

Alternatively, `<DISTRO>` can be found in the following list:

* sid (Debian testing/unstable)
* bullseye (Debian 11)
* buster (Debian 10)
* jammy (Ubuntu 22.04)
* focal (Ubuntu 20.04)
* bionic (Ubuntu 18.04)

```
echo "deb https://deb.oxen.io <DISTRO> main" | sudo tee /etc/apt/sources.list.d/oxen.list
```

Then resync your package repositories with:

```
sudo apt update
```

#### Step 2: Oxen Service Node configuration

To configure your Service Node, simply install the `oxen-service-node` package:

```
sudo apt install oxen-service-node
```

This will detect your public IP (or allow you to enter it yourself) and automatically update the `/etc/oxen/oxen.conf` configuration file with the necessary additional settings to run a Service Node.

Congratulations! Your Service Node is now ready to be registered and staked.

### Staking your Service Node

#### Preparing your Service Node for registration

To prepare your Service Node for registration, run the following command:

```
oxend prepare_registration
```

This will prompt you for some registration details, then output a registration command. Copy the output from this command in preparation for the next step.

> Note: You can safely run this command multiple times if you change your mind about some of the registration questions before you submit the registration.

#### Staking and registering your Service Node

To stake and register your Service Node, open the Oxen GUI wallet. Make sure your wallet has a balance of at least 15,000 $OXEN to meet the Service Node staking requirement (less if you're configuring a [shared Service Node](full-service-node-setup-guide.md#5-2-setting-up-a-pooled-service-node)). Navigate to the `Service Nodes`tab > `Registration` section, and paste the output from the above command, then click **Register Service Node**.

Done! Your staking transaction will now be submitted to the network. After a short delay, your Service Node will be registered and start contributing to the network (and receiving rewards!).

#### Checking registration status

You can easily check if your Service Node is registered on the network. First, connect to the VPS where the Service Node is running and run the following command to retrieve your Service Node's public key:

```
oxend status
```

This will output a bunch of information about your Service Node, but there's one part we're interested in at this stage: The long string of random letters and numbers after the characters `SN:` . This string is your Service Node's public key, used to identify your Service Node on the list of registered and operational Service Nodes. Select and copy the public key (do not copy any of the surrounding information).

You can now jump onto [oxen.observer](https://oxen.observer), open the full list of active Service Nodes, and use `Cmd+F`/`Ctrl+F` to check if your Service Node's public key appears in the list.

#### Monitoring

We highly recommend setting up monitoring for your Service Node. This is as simple as calling on the services of our Telegram or Discord bot. Contact `@OxenSNBot` on Telegram or `OxenSNBot#5812` on Discord and type `/start` or `$help` respectively to get started.

Another helpful tool is Konstantin Ullrich's [Oxen Service Node Operator app](https://play.google.com/store/apps/details?id=dev.konsti.oxen\_service\_node) for Android.

#### Upgrading

When a new release is available, upgrading is as simple as syncing your repositories:

```
sudo apt update
```

Then installing any updates using:

```
sudo apt upgrade
```

> Note that this will install both updated Oxen packages _and_ any available system updates (this is generally a good thing!)

During the upgrade, the running instance of `oxend` will be restarted to ensure that the updated `oxend` is now active.

If, for some reason, you want to install _only_ updated Oxen package upgrades, but not other system packages, then instead of `sudo apt upgrade` you can use:

```
sudo apt install oxen-storage-server oxend lokinet-router
```

### Back-ups

To show backup information for your Service Node's secret key (for future recovery/migration):

```
oxen-sn-keys show /var/lib/oxen/key_ed25519
```

For service nodes originally installed before 8.x there will be a /var/lib/oxen/key file that must
also be backed up (if this file does not exist then you do not need it):

```
oxen-sn-keys show /var/lib/oxen/key
```

Restore from SN secret key:

```
oxen-sn-keys restore /var/lib/oxen/key_ed25519
```

and, *only* when restoring from an older installation with an additional `.../key` file:

```
oxen-sn-keys restore-legacy /var/lib/oxen/key
```

### Support

Having trouble? Just [head to our Support section](../../support.md).
