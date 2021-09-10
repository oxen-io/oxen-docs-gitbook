# 🐧 Installing on Linux \(GUI\)

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
curl -s https://deb.imaginary.stream/public.gpg | sudo apt-key add -
```

The next command tells `apt` where to find the packages:

```text
echo "deb https://deb.imaginary.stream $(lsb_release -sc) main" | sudo tee /etc/apt/sources.list.d/imaginary.stream.list
```

Then resync your package repositories with:

```text
sudo apt update
```

Now install Lokinet:

```text
sudo apt install lokinet-gui
```

Congratulations, Lokinet is now installed and running in the background.

To access the GUI client all you need to do is open the `lokinet-gui` application from your preferred application launcher.

### Toggling Lokinet off and on

Simply jump into the lokinet-gui client and click the large green power button.

### Using Lokinet

Head over to [Exit nodes](../exit-nodes.md) or [Accessing SNApps](../snapps/accessing-snapps.md) for an overview of the exciting things you can do with Lokinet up and running!

