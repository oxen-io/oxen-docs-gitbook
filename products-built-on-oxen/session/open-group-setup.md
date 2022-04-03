# 🗣 Session Open Group Server Setup

Session open groups servers (SOGS) are group chat servers that can host thousands of chat participants. This guide explains how to set up a Session open group server.

> Note: Session open group servers provide transit encryption, but open group messages are not encrypted while stored on the server; **closed groups** (which can be created within Session itself) are a better solution for high-security communications with groups of 100 or less people.

Open groups are hosted using [PySOGS](https://github.com/oxen-io/session-pysogs). Written in Python, PySOGS is the reference implementation of a Session open group server. PySOGS is used to run the official Session open groups, and is the officially supported open group server.&#x20;

Below you can find a video guide walking you through the process of setting up a Session open group server, as well as a written guide.

{% embed url="https://youtu.be/1br1D5mg-PI" %}

## Installation

**Note:** .debs for the Session Open Group server are currently available for Ubuntu 20.04 and newer, and for Debian 10 and newer. For other operating systems, you can [build from source](https://github.com/oxen-io/session-pysogs/blob/dev/install-uwsgi.md)&#x20;

### 1. Find a suitable server to run SOGS&#x20;

Typically, the simplest and cheapest way to host a server is by leasing a Virtual Private Server (VPS). There are hundreds of VPS providers to choose from, some popular options are: Vultr, Hetzner, DigitalOcean, Linode, Amazon Lightsail, etc.

You can run a SOGS from home, but consider most consumer internet connections have poor upstream bandwidth, typically don't provide a static IP address, and transient power and network outages are a relatively common. These factors can affect the stability of your SOGS and the ability for users to chat in your group.

Resource requirements are highly dependent on how many users you plan to support in your open groups and how frequent usage is, however, a good starting point would be:&#x20;

**1 Virtual core, 512 MB of RAM, 20GB HDD space**

Once you have signed up for a provider you should receive a static IP address, you will need to SSH into your server, you can find instructions on how to do this online.&#x20;

### 2. Add Oxen apt repository&#x20;

Add the Oxen apt repository by running the following commands

```
sudo curl -so /etc/apt/trusted.gpg.d/oxen.gpg https://deb.oxen.io/pub.gpg
echo "deb https://deb.oxen.io $(lsb_release -sc) main" | sudo tee /etc/apt/sources.list.d/oxen.list
sudo apt update
```

### 3. Install &#x20;

You have a choice to install either the sogs-standalone package or the sogs-proxied package.&#x20;

#### sogs-standalone

This is the simple SOGS package for most setups. It installs a SOGS that listens on a public IP/port for HTTP connections. It does not support HTTPS connections (but since all messages to/from SOGS are separately encrypted, HTTPS is not necessary nor recommended).

#### sogs-proxied

This package provides a more advanced SOGS configuration where SOGS itself will listen on an internal port and expects to have requests proxied to it from an ngnix or apache2 front-end server that listens on the public IP/port. The package will install basic site configuration files for either nginx or apache2, but extra configuration may be necessary.

This package is required if you want your SOGS to be reached over HTTPS: the HTTPS handling is configured on the front-end server (i.e. in nginx or apache) using a tool such as `certbot`. (This package does not auto-configure such HTTPS certificates, but there are many online help pages on setting up such HTTPS support for a front-end web server).

If you don't know what any of this means then stick with the `sogs-standalone` package.

```
sudo apt install sogs-standalone
```

OR

```
sudo apt install sogs-proxied
```

If installing the standalone version you will see this prompt, asking you to enter a URL or IP address, if you have not setup DNS to point towards the IP address of your VPS then you should enter your VPS Public IP address, which can usually be found on your VPS provider's website. &#x20;

![](<../../.gitbook/assets/image (2).png>)

### 4. Add a room

Once finished with installation, you will want to add a room, to add a room run:

```
sogs --add-room TOKEN --name "NAME" --description "DESCRIPTION"
```

Replace `TOKEN` with the address to use in the room URL (which must consist of letters, numbers, underscores, or dashes), replace `NAME` with the room name to display in Session and optionally replace `DESCRIPTION` with a short description of the topic of the room.

```
sogs --add-room fish --name "Fishing" --description "Australian fisheries chat"
```

As an example, setting up a room for discussion of Australian fisheries&#x20;

### 5. Make yourself an administrator

Make yourself a global administrator for all rooms hosted on your SOGS by running the following:

```
sogs --rooms + --add-moderators SESSIONID --admin --visible
```

Replace `SESSIONID` with the Session ID you want to be an administrator

For **example**:

```
sogs --rooms + --add-moderators 05d871fc80ca007eed9b2f4df72853e2a2d5465a92fcb1889fb5c84aa2833b3b40 --admin --visible
```

### 6. Join your SOGS

Once setup, you should be able to navigate to your VPS's IP address in a web browser, for example [http://116.203.70.33/](http://116.203.70.33)&#x20;

Here you should see a list of your rooms, clicking on a room will display a QR code and the link required to join the room, this link can be copied and pasted into a Session client to join a group.

![](<../../.gitbook/assets/image (3).png>)

### Getting help

If something in this guide doesn't make sense, or if you’re running into issues that you can’t identify on your own, the first place to go would be the [Session open group](http://116.203.70.33/session?public\_key=a03c383cf63c3c4efe67acc52112a6dd734b3a946b9545f488aaa93da7991238) inside Session. Alternatively, you can find help on our other communication channels: [Twitter](https://twitter.com/Oxen\_io), or [Reddit](https://reddit.com/oxen\_io).

### Reporting bugs

If you've sought help through our communication channels but have not arrived at a solution for your issue, we recommend opening an issue ticket on the [Session open group server](https://github.com/oxen-io/session-pysogs) GitHub repository.
