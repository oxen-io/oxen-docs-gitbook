# ⏺ Configuring SRV Records

It is possible to configure your SNApp so that others can get service information when looking it up. For example, you may want to tell the user you are hosting an xmpp server at a specific port, or even at a different .loki address, or perhaps even load-balance a service across multiple .loki addresses.

To start open the Lokinet config file, If you have built Lokinet from Deb packages, you can open your `lokinet.ini` file in the `nano` text editor with the following command:

```
sudo nano /etc/loki/lokinet.ini
```

Otherwise, if you have built Lokinet from source, your `lokinet.ini` file will be in the folder `~/.lokinet/lokinet.ini`, and can be opened in `nano` with the following command:

```
sudo nano ~/.lokinet/lokinet.ini
```

Under the section heading `[network]` add one or more entries with the following format:

```
srv=_service._protocol priority weight port [target]
```

An example:

```
srv=_xmpp._tcp 10 10 1234 mylokiaddress.loki
```

This would be an entry for the XMPP protocol, pointing to `mylokiaddress.loki` at port 1234.

Another example:

```
srv=_mumble._tcp 10 10 64738
```

This would be an entry for Mumble, pointing to the SNApp you are configuring at port 64738.

The target in this entry MUST be one of the following:

* empty, which means "just use the .loki for this SNApp"
* a single dot (`.`), which means "this SNApp does NOT have that service available"
* any valid name in the .loki TLD.

For more information on SRV records and what you can do with them, visit [the Wikipedia article](https://en.wikipedia.org/wiki/SRV\_record).
