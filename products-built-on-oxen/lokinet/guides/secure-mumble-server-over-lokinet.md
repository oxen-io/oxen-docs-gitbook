# 📞 Run a secure Mumble server over Lokinet

Mumble is a fantastic open-source voice chat platform known for its reliability and ease of use.

And Lokinet is a cutting-edge onion routing network that offers unparalleled security and anonymity potential.

In this guide, we’re going to show you how to run a Mumble server over Lokinet, combining Mumble’s ease of use with Lokinet’s security and anonymity to create the ultimate secure voice chat service. With just 15 minutes of your time and $3 a month, you or your organisation can create one of the most secure voice chat platforms possible. 

A Mumble server running over Lokinet on a server you control gives you absolute certainty that your voice conversations, associated metadata, and other Mumble activity cannot be stored or recorded, because no computer ever knows who is talking to whom — not even the Mumble server itself. So long as you trust the device that you run the Mumble server on \(which you can, because it’s yours\), you can be certain that no one else on earth can eavesdrop on your conversation — or even know that you’re connected to the server at all.

If this is your first time using SSH and the Linux command line, don’t stress. We’ll walk you through every step! With that, let’s get to it. 

### Step 1: Rent a VPS

The first thing you’ll want to do is rent yourself a VPS \(Virtual Private Server\) to host your Mumble voice chat server. You _could_ run the Mumble server from your own computer instead, but if you want the server to stay up 24/7, _without_ having to leave your own PC on all the time, a VPS is the way to go. Mumble’s chat server has extremely low system requirements, so a VPS with any amount of storage and at least 512MB RAM will do the trick — you can find VPSs that meet these requirements for around US$3 a month. 

Try [https://www.hetzner.com/cloud](https://www.hetzner.com/cloud), or [https://evolution-host.com/vps-hosting.php](https://evolution-host.com/vps-hosting.php) if you want to pay in $OXEN! When ordering, select Ubuntu 20.04 or Debian 10 as the operating system.

### Step 2: Prepare your VPS

Once you have access to your new VPS, you’re almost ready to install Lokinet, but there’s a little bit of preparatory work to do first. Start by opening a command prompt on your local machine \(Terminal on macOS, any command prompt on Linux, or PowerShell on Windows 10\). SSH into _\(get remote access to\)_ your VPS with this command:

`ssh root@`**`[VPS IP address]`**

Replacing `[VPS IP address]` with the IP of your VPS.  It’ll prompt you for a password which will usually be provided to you by the VPS host. More advanced users can and should disable root password access and instead use SSH keys, but if that sounds hard, don’t worry about it for now. As you learn more about Linux, you’ll get more familiar with these best practices.

Once you’ve logged in, we’re ready to roll. First, we’ll update our package lists to make sure our VPS sees the most recent versions of all available packages. Type:

`sudo apt update`

You’ll see a bunch of package lists being downloaded. Once this command completes, run the following command to upgrade any outdated packages currently installed on the VPS:

`sudo apt upgrade`

We’ll also need to make sure the `curl` command is installed before we proceed. Run this command:

`which curl`

It should output the location of your installed curl command. If you get an error, install `curl`:

`sudo apt install curl`

Then run `which curl` again to make sure `curl` is installed. 

Success? Congrats, you’re ready to move on to the next step:  

### Step 3: Install Lokinet

To install Lokinet, we need to add the Lokinet repository. Run the following command to install the public key used by the Lokinet dev team to sign Lokinet binaries:

`sudo curl -so /etc/apt/trusted.gpg.d/oxen.gpg https://deb.oxen.io/pub.gpg`

Then run the following command to tell `apt` where to find the Lokinet packages:

`echo "deb https://deb.oxen.io $(lsb_release -sc) main" | sudo tee /etc/apt/sources.list.d/oxen.list`


Next, update your repository package lists again with:

`sudo apt update`

And now, install Lokinet:

`sudo apt install lokinet`

Congrats, Lokinet is now installed and running in the background. We’re nearly there.

### Step 4: Installing the Mumble server

Run this command:

`sudo apt install mumble-server`

That’s it. The Mumble server is now installed. On to Step 5:

### Step 5: Setting up a persistent keyfile

This step is a bit more involved. We need to set up Lokinet to always generate a keyfile in the same directory, so it will work consistently. Linux servers don’t have a graphical interface, but they do ship with some in-terminal text editors. We need to edit a file now, so start by opening your `lokinet.ini` file with this command:

`sudo nano /etc/loki/lokinet.ini`

Using the arrow keys, move the cursor down to the \[network\] section of the file. Remove the \# from before the “keyfile=” line, then add the following after the = symbol:

`/var/lib/lokinet/mumble.private`

Then hit Ctrl+X. Type “Y” \(for yes\) when asked if you want to save your changes, then press Enter to save and exit.

Now that you’ve exited `nano`, you’re back in the terminal. Restart Lokinet to generate a keyfile for Mumble:

`sudo systemctl restart lokinet`

### Step 6: Binding the Mumble server to Lokinet

Now we need to make sure your Mumble server is using Lokinet for all traffic. Start with this command to grab the IP address we need to bind Mumble to:  


`dig @127.3.2.1 +short localhost.loki`

This command will output 2 strings of text: a long string of random letters and numbers ending in .loki, and an IP address \(a number in the format xxx.xx\(x\).x.x\). 

Select and copy \(Ctrl+C on Windows or Linux; Cmd+C on macOS\) the IP address. Some SSH clients allow you to copy by highlighting the text and right-clicking on it as well.

Now, we need to point the Mumble server to that IP address. Use this command to open the configuration file for the Mumble server:

`nano /etc/mumble-server.ini` 

Using the arrow keys, navigate down to the line `“;host=”` under the section _Specific IP or hostname to bind to_. Delete the `;` from the start of the line, then paste the IP address we copied earlier after the `=` symbol. Hit Ctrl+X to exit. Type “Y” when asked if you want to save your changes, then press Enter to save and exit.

Back at the command line, restart the Mumble server to apply changes:

`systemctl restart mumble-server`

### Step 7: Done!

Congrats! A Mumble server is now up and running on your VPS, and all its traffic is being routed through Lokinet. All that’s left is to grab the Lokinet address of the Mumble server and give it to anyone you want to be able to connect. In case you missed it, run this command to find the Lokinet address of the Mumble server:

`dig @127.3.2.1 +short localhost.loki`

This is the same command we ran earlier, but this time, pay attention to the long string of characters ending in .loki \(be sure to include the .loki part!\). This is the Lokinet address of your secure, onion-routed Mumble server. 

Copy this address and provide it to anyone you want to be able to connect to the server — all they have to do is paste the address into the Address field of the Add Server dialog in the Mumble client, add a username and label to identify the server, hit OK, and connect!

Mumble can be [downloaded for free](https://www.mumble.info/) on all major platforms. Anyone that wants to access your secret Mumble server will also need to have Lokinet installed and running. To download and install Lokinet, just head to [https://lokinet.org/](https://lokinet.org/). Additional Lokinet guides can be found back at the [Lokinet guide hub](./) here on Oxen Docs.

And that’s it! Only 15 minutes and $3 later, you can now have completely surveillance-free conversations over the internet. We hope to integrate voice features into [Session](../../session/) to make it even easier to access secure voice channels with this level of privacy and security. 

In the meantime, though, this Mumble/Lokinet setup is perhaps the most secure voice channel option available. This unique combination of services is just one example of the power of the Oxen tech stack — stay tuned for more guides and articles about what Oxen’s tech can do. 

Have fun!

