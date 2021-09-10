# 🗣 Open group setup

Open groups are Session group chat servers that can host many thousands of chat participants. This guide explains how to set up a Session open group.

> Note: Open groups provide transit encryption, but open group messages are not encrypted while stored on the open group server, making **closed groups** \(which can be created within Session itself\) a better solution for high-security communications with groups of 100 or less people.

Open groups are based on an express REST API for serving persistent-history public chat rooms. Open groups are unlimited in size, as they are hosted by your own server. Please be aware these are **public** groups, and highly sensitive private information **should not be shared in this group format.**

Open group servers are run by 2 daemons: the platform server, providing an ADN standard REST API, and another daemon with Session-specific behaviors \(crypto-key registration and enhanced moderation functions\).

## Installation

**Note:** .debs for the Session Open Group server are currently only available for Ubuntu 20.04.  
For other operating systems, you can either [build from source](https://github.com/nielsandriesse/session-open-group-server/blob/main/BUILDING.md) or use [Docker](https://github.com/nielsandriesse/session-open-group-server/blob/main/DOCKER.md).

\*\*\*\*[**Video Guide**](https://www.youtube.com/watch?v=D83gKXn6iTI)\*\*\*\*

### 1. Pull in the Session open group server executable

Pull in the Session Open Group Server executable by running the following commands

```text
sudo curl -so /etc/apt/trusted.gpg.d/oxen.gpg https://deb.oxen.io/pub.gpg
echo "deb https://deb.oxen.io $(lsb_release -sc) main" | sudo tee /etc/apt/sources.list.d/oxen.list
sudo apt update
sudo apt install session-open-group-server
sudo chown _loki /var/lib/session-open-group-server -R
```

### 2. Add a room

Add a room of your choice with the following command:

```text
session-open-group-server --add-room {room_id} {room_name}
```

 `room_id` must be lowercase and consist of only letters, numbers and underscores.

For **example:** 

```text
session-open-group-server --add-room fish FishingAustralia
```

### 3. Print your server's URL

Print the URL users can use to join rooms on your open group server by running:

```text
session-open-group-server --print-url
```

This will output a result similar to:

```text
http://[host_name_or_ip]/[room_id]?public_key=2054fa3271f27ec9e55492c85d022f9582cb4aa2f457e4b885147fb913b9c131
```

You will need to replace `[host_name_or_ip]` with the IP address of your VPS or the domain mapping to your IP address, and `[room_id]` with the ID of one of the rooms you created earlier.

For **example**:

```text
http://116.203.217.101/fish?public_key=2054fa3271f27ec9e55492c85d022f9582cb4aa2f457e4b885147fb913b9c131
```

This URL can then be used to join the group inside the Session app.

### 4. Make yourself a moderator

Make yourself a moderator using the following command:

```text
session-open-group-server --add-moderator {your_session_id} {room_id}
```

For **example**:

```text
session-open-group-server --add-moderator 05d871fc80ca007eed9b2f4df72853e2a2d5465a92fcb1889fb5c84aa2833b3b40 fish
```

### 5. Add an image for your new room \(Optional\)

* Add your room on Session desktop using the URL printed earlier
* Use Session desktop to upload a picture for your room

Or

* Upload a JPG to your VPS
* Put it in `/var/lib/session-open-group-server/files`
* Rename it to `{room_id}` \(no file extension\)

### Customization

The default options the Session open group server runs with should be fine in most cases, but if you like you can run on a custom port or host, specify the path to the X25519 key pair you want to use, etc. To do this, simply add [the right arguments](https://github.com/nielsandriesse/session-open-group-server/blob/main/BUILDING.md#step-3-run-it) to the `ExecStart` line in your systemd service file \(normally located under `/etc/systemd/system`\) and restart your service using:

```text
systemctl restart session-open-group-server.service
```

### Getting help

If something in this guide doesn't make sense, or if you’re running into issues that you can’t identify on your own, the first place to go would be the Oxen [Telegram Group](https://t.me/Oxen_Community). Alternatively, you can find help on our other communication channels: [Twitter](https://twitter.com/Oxen_io), or [Reddit](https://reddit.com/oxen_io).

### Reporting bugs

If you've sought help through our communication channels but have not arrived at a solution for your issue, we recommend opening an issue ticket on the [Session public server](https://github.com/oxen-io/session-open-group-server-legacy/issues) GitHub repository.

