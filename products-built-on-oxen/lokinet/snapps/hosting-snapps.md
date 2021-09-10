# 🖥 Hosting SNApps

Depending on the nature of the application or service which you want to make accessible over Lokinet, you may want to only run a temporary SNApp, or a SNApp that continues to persistently use the same .loki pubkey. 

A temporary SNApp is a service accessible over Lokinet that does not have a permanent address. This means that the address you supply to others will not work once the server hosting the SNApp is restarted.

If you want users to be able to access your application or service over an extended period of time, a temporary SNApp is **not** recommended.

If you only want to host a temporary SNApp, jump to [Step 2](hosting-snapps.md#2-finding-your-snapps-lokinet-address).

> _Note: This guide assumes you are running a standard Debian or Ubuntu Linux distribution on the machine on which you'll be hosting the SNApp. This guide also assumes you are relatively familiar with using the command line._

### Step 1: Preparing your Lokinet address

Start by opening your `lokinet.ini` file and adding a path to where your SNApp key files will be stored.

If you have built Lokinet from Deb packages, you can open your `lokinet.ini` file in the `nano` text editor with the following command:

```text
sudo nano /etc/loki/lokinet.ini
```

Otherwise, if you have built Lokinet from source, your `lokinet.ini` file will be in the folder `~/.lokinet/lokinet.ini`, and can be opened in `nano` with the following command:

```text
sudo nano ~/.lokinet/lokinet.ini
```

With `lokinet.ini` open in the text editor, scroll down to your `[network]` section and add the following line:

```text
keyfile=/var/lib/lokinet/snappkey.private
```

**Replace** `<user>` with your computer username. Alternatively, you can set the filepath to wherever you want your SNApp private key to be stored.

Now, when you restart Lokinet, it will generate your `snappkey.private` file in the directory you have set.

```text
sudo systemctl restart lokinet
```

### Step 2: Finding your SNApp's Lokinet address

You can find your SNApp's current address using a host lookup tool:

```text
nslookup -type=cname localhost.loki 127.0.0.1
```

You can also use the `host` command \(the .loki address to query is the same, but the resolver uses the address `127.3.2.1` as to not conflict with other resolvers you may have installed\):

```text
host -t cname localhost.loki 127.3.2.1
```

### Step 3: Install nginx

Install a proper web server:

```text
sudo apt install nginx
```

Now configure `nginx` to run only on the lokinet interface \(in `/etc/nginx/sites-enabled/default` change any `listen` directives to use the lokinet IP, and remove any IPV6 `listen` directives\).

By default, you can drop files into `/var/www/html` to serve them as a SNApp. Make sure they are accessable via the `www-data` user \(or whichever user `nginx` runs as\).

TIP: You can make `nginx` generate a directory listing by adding `autoindex on;` into the `location` block in the nginx config file.

### 4. Security

Make sure no services bind to all interfaces.

Suggested firewall settings when using nginx:

```text
# change me to the range used by lokinet
LOKINET_RANGE=10.0.0.0/16

# drop all from lokinet
ufw deny from $LOKINET_RANGE

# allow tcp 80 from lokinet
ufw allow 80/tcp from $LOKINET_RANGE
```

### 5. Configuring SRV records

It is possible to configure your SNApp so that others can get service information when looking it up. For example, you may want to tell the user you are hosting an xmpp server at a specific port, or even at a different .loki address, or perhaps even load-balance a service across multiple .loki addresses.

To start, open Lokinet's .ini file for editing as in [Step 1](hosting-snapps.md#step-1-preparing-your-lokinet-address).

Under the section heading `[network]` add one or more entries with the following format:

```text
srv=_service._protocol priority weight port [target]
```

An example:

```text
srv=_xmpp._tcp 10 10 1234 mylokiaddress.loki
```

This would be an entry for the XMPP protocol, pointing to `mylokiaddress.loki` at port 1234.

Another example:

```text
srv=_mumble._tcp 10 10 64738
```

This would be an entry for Mumble, pointing to the SNApp you are configuring at port 64738.

The target in this entry MUST be one of the following:

* empty, which means "just use the .loki for this SNApp"
* a single dot \(`.`\), which means "this SNApp does NOT have that service available"
* any valid name in the .loki TLD.

For more information on SRV records and what you can do with them, visit [the Wikipedia article](https://en.wikipedia.org/wiki/SRV_record).

### All done!

Congratulations! Your SNApp should now be accessible over Lokinet. 

