# 🐧 Linux troubleshooting

### Setting your DNS

If you're having issues with resolving .loki addresses, you need to edit your resolv.conf files and add your DNS resolver.

**Method 1**

Install `systemd-resolved` and let that manage dns.

```
apt install systemd-resolved
```

**Method 2**

Install `resolvconf` and let that manage dns.

```
apt install resolvconf
```

Then restart `lokinet.service` with systemd.


If resolvconf by itself doesn't work, you'll need to add the Lokinet nameserver manually to resolvconf.


```
sudo nano /etc/resolvconf/resolv.conf.d/head
```

Add the following line at the bottom of this file:

```
nameserver 127.3.2.1
```

Once that line is added, hold Ctrl and type X, then type Enter to confirm the file changes.

Next we need to update our /etc/resolv.conf file by running the command:

```
sudo resolvconf -u
```

Then restart `lokinet.service` with systemd.

### Lokinet becomes "stuck" trying to connect to the network.

As of `0.9.11` there is an issue where sometimes lokinet will get into a bad state and refuse to connect to the network.

This case is not related network censors blocking lokinet, it's a bug. see https://github.com/oxen-io/lokinet/issues/2116

If this happens, makes ure you back up any persisting private keys for .loki and....

* stop the lokinet service 
* remove `/var/lib/lokinet/profiles.dat`
* start the lokinet service

This will remove the client's inferred network state that it slowly generates over time, thus will start retrying nodes it thought were dead before.
