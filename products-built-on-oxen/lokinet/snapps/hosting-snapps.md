# 🖥 Hosting SNApps

Depending on the nature of the application or service which you want to make accessible over Lokinet, you may want to only run a temporary SNApp, or a SNApp that continues to persistently use the same .loki pubkey.

A temporary SNApp is a service accessible over Lokinet that does not have a permanent address. This means that the address you supply to others will not work once the server hosting the SNApp is restarted.

If you want users to be able to access your application or service over an extended period of time, a temporary SNApp is **not** recommended.

If you only want to host a temporary SNApp, jump to [Step 2](hosting-snapps.md#step-2-finding-your-snapps-lokinet-address).

> _Note: This guide assumes you are running a standard Debian or Ubuntu Linux distribution on the machine on which you'll be hosting the SNApp and that you have Lokinet installed and running on this machine, if not please follow the guide_ [_here_](../guides/installing-on-linux-cli.md) _to get Lokinet running. This guide also assumes you are relatively familiar with using the command line._

### Step 1: Preparing your Lokinet address

Start by opening your `lokinet.ini` file and adding a path to where your SNApp key files will be stored.

If you have installed Lokinet from Deb packages, you can open your `lokinet.ini` file in the `nano` text editor with the following command:

```
sudo nano /etc/loki/lokinet.ini
```

Otherwise, if you have built Lokinet from source, your `lokinet.ini` file will be in the folder `~/.lokinet/lokinet.ini`, and can be opened in `nano` with the following command:

```
sudo nano ~/.lokinet/lokinet.ini
```

With `lokinet.ini` open in the text editor, scroll down to your `[network]` section and add the following line:

```
keyfile=/var/lib/lokinet/snappkey.private
```

Alternatively, you can set the filepath to wherever you want your SNApp private key to be stored.

Now, when you restart Lokinet, it will generate your `snappkey.private` file in the directory you have set.

```
sudo systemctl restart lokinet
```

### Step 2: Finding your SNApp's Lokinet address

You can find your SNApp's current address using a host lookup tool:

```
nslookup -type=cname localhost.loki 127.0.0.1
```

You can also use the `host` command (the .loki address to query is the same, but the resolver uses the address `127.3.2.1` as to not conflict with other resolvers you may have installed):

```
host -t cname localhost.loki 127.3.2.1
```

### Step 3: Install nginx

Install a proper web server:

```
sudo apt install nginx
```

### Step 4: Configure nginx

#### Setup a Lokinet exclusive SNApp

If you want your SNApp to be accessible only via Lokinet and not via your IP address or domain name then you will need to configure `nginx` to run only on the lokinet interface.

First we will need to set the Lokinet interface IP range, we can do this by accessing our Lokinet config file as we did in [Step 1](hosting-snapps.md#step-1-preparing-your-lokinet-address) and adding this line in the `[network]`section:&#x20;

```
ifaddr=10.67.0.1/16
```

this can go underneath your existing network config changes, after this change is made restart Lokinet using:&#x20;

```
sudo systemctl restart lokinet
```

Once Lokinet is restarted run the following command to open your nginx default configuration file, we are going to make a few changes here.

```
sudo nano /etc/nginx/sites-enabled/default
```

change any `listen` directives to use the lokinet IP `10.67.0.1` remove any IPV6 `listen` directives and replace the .loki address after `server_name` with the Lokinet address you discovered in [Step 2](hosting-snapps.md#step-2-finding-your-snapps-lokinet-address)

This should leave your default file in sites available looking something like this:

```
server {
        listen 10.67.0.1:80;

        root /var/www/html;

        # Add index.php to the list if you are using PHP
        index index.html index.htm index.nginx-debian.html;

        server_name 73qqjm7ju98g6obua8bprce1tjyyphnotknnijhpn8mypwumqs8o.loki;

        location / {
                # First attempt to serve request as file, then
                # as directory, then fall back to displaying a 404.
                try_files $uri $uri/ =404;
        }

}
```

save the changes and exit the file. Once this step is complete you can reload nginx with the following command:&#x20;

```
systemctl reload nginx
```

Proceed to [Step 6](hosting-snapps.md#step-6-all-done)

**Setup a clearnet and Lokinet accessible SNApp**

If you want your SNApp to be accessible via Lokinet and your clearnet IP/Domain name then fewer changes are required. First open `/etc/nginx/sites-enabled/default` using:

```
sudo nano /etc/nginx/sites-enabled/default
```

Then add your .loki address (which you discovered in [Step 2](hosting-snapps.md#step-2-finding-your-snapps-lokinet-address)) as a `server_name`, after the changes your nginx default file should look something like this:

```
server {
        listen 80 default_server;
        listen [::]:80 default_server;

        root /var/www/html;

        # Add index.php to the list if you are using PHP
        index index.html index.htm index.nginx-debian.html;

        server_name _ 73qqjm7ju98g6obua8bprce1tjyyphnotknnijhpn8mypwumqs8o.loki;

        location / {
                # First attempt to serve request as file, then
                # as directory, then fall back to displaying a 404.
                try_files $uri $uri/ =404;
        }
}
```

Proceed to [Step 6](hosting-snapps.md#step-6-all-done)

#### Nginx Tips:

TIP: By default, you can drop files into `/var/www/html` to serve them as a SNApp. Make sure they are accessible via the `www-data` user (or whichever user `nginx` runs as.

TIP: You can make `nginx` generate a directory listing of files by adding `autoindex on;` on a new line into the `location` block in the nginx config file.

### Step 5. Security (Optional)

Make sure no services bind to all interfaces.

Suggested [UFW](https://wiki.ubuntu.com/UncomplicatedFirewall) firewall settings when using nginx:

```
# change me to the range used by lokinet
LOKINET_RANGE=10.0.0.0/16

# drop all from lokinet
ufw deny from $LOKINET_RANGE

# allow tcp 80 from lokinet
ufw allow 80/tcp from $LOKINET_RANGE
```

### Step 6: All done!

Congratulations! Your SNApp should now be accessible over Lokinet. Use the Lokinet address you discovered in [Step 2](hosting-snapps.md#step-2-finding-your-snapps-lokinet-address), to access your SNApp
