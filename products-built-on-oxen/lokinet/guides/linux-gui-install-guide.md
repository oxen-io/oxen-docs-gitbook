# 🐧 Installing on Linux \(GUI\)

### Initial setup

First add the oxen.io apt repository to your system:

```text
sudo curl -so /etc/apt/trusted.gpg.d/oxen.gpg https://deb.oxen.io/pub.gpg
echo "deb https://deb.oxen.io $(lsb_release -sc) main" | sudo tee /etc/apt/sources.list.d/oxen.list
```

Update your apt package list:

```text
sudo apt update
```

Install the `lokinet-gui` package:

```text
sudo apt install lokinet-gui
```

This will also install and start a background service called `lokinet.service` that can be controlled with the lokinet-gui application.

### Toggling Lokinet off and on

Simply jump into the lokinet-gui client and click the large green power button.

### Using Lokinet

Head over to [Exit nodes](../exit-nodes.md) or [Accessing SNApps](../snapps/accessing-snapps.md) for an overview of the exciting things you can do with Lokinet up and running!

### GUI installation troubleshooting

See the troubleshooting guide [here](./linux-troubleshooting.md) if you have issues.

#### Linux Mint does not work with \(lsb-release\)

It has been reported that Linux Mint users may need to use the following command instead of the second command in [Initial setup](linux-gui-install-guide.md#initial-setup):

```text
echo "deb https://deb.oxen.io jammy main" | sudo tee /etc/apt/sources.list.d/oxen.list
```

