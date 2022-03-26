# 🐧 Installing on Linux \(CLI\)

### Initial setup

#### 1. Computer Preparation

Begin by updating your package lists. The below command downloads package lists from your repositories and "updates" them to get information on the newest versions of packages and their dependencies. It will do this for all repositories and PPAs.

Run the following command:

```text
sudo apt-get update
```

You'll notice a bunch of package lists were downloaded. Once this is complete, run the below command to fetch new versions of any packages we currently have installed on the system:

```text
sudo apt-get upgrade
```

You'll be prompted to authorise the use of disk space. Type `y` and press Enter to authorise.

If you do not have `curl` installed on your computer, now is also a good time to install it, as we will use it later:

```text
sudo apt install curl
```

#### 2. Installation

You only need to do this step the first time you want to set up the Lokinet repository. After you've done it once, the repository will automatically update whenever you fetch new system updates.

This first command installs the public key used to sign official Lokinet binaries.

```text
sudo curl -so /etc/apt/trusted.gpg.d/oxen.gpg https://deb.oxen.io/pub.gpg
```

The next command tells `apt` where to find the packages:

```text
echo "deb https://deb.oxen.io $(lsb_release -sc) main" | sudo tee /etc/apt/sources.list.d/oxen.list
```

> Note: if you're running Linux Mint and get an error with this command, check out [Troubleshooting](installing-on-linux-cli.md#linux-mint-does-not-work-with-lsb-release).

Then resync your package repositories with:

```text
sudo apt update
```

Now install Lokinet:

```text
sudo apt install lokinet
```

Congratulations, Lokinet is now installed and running in the background.

### Starting and stopping Lokinet

To start Lokinet manually, run the following command:

```text
sudo systemctl start lokinet
```

To stop Lokinet manually, run the following command:

```text
sudo systemctl stop lokinet
```

### Updating Lokinet

To update Lokinet, run the following command:

```text
sudo apt update && sudo apt install lokinet && sudo lokinet-bootstrap && sudo systemctl
```

### Using Lokinet

Head over to [Exit nodes](../exit-nodes.md) or [Accessing SNApps](../snapps/accessing-snapps.md) for an overview of the exciting things you can do with Lokinet up and running!

### CLI installation troubleshooting

#### `Failed to decode boostrap RC`

If your bootstrap is not configured properly, run the following command:

```text
sudo lokinet-bootstrap
```

Then restart Lokinet:

```text
sudo systemctl restart lokinet
```

#### Linux Mint does not work with \(lsb-release\)

It has been reported that Linux Mint users may need to use the following command instead of the second command in [2. Installation](installing-on-linux-cli.md#2-installation):

```text
echo "deb https://deb.oxen.io focal main" | sudo tee /etc/apt/sources.list.d/oxen.list
```

