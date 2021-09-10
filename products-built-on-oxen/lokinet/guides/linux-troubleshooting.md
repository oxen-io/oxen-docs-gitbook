# 🐧❗️Linux troubleshooting

### Setting your DNS

If you're having issues with resolving .loki addresses, you need to edit your resolv.conf files and add your DNS resolver.

**Method 1**

Run the following command:

```text
apt install resolvconf
```

Then restart Lokinet:

```text
systemctl restart lokinet
```

**Method 2**

If Method 1 doesn't work, you'll need to add the Lokinet nameserver manually.

Run the following command:

```text
sudo nano /etc/resolvconf/resolv.conf.d/head
```

Add the following line at the bottom of this file:

```text
nameserver 127.3.2.1
```

Once that line is added, hold Ctrl and type X, then type Enter to confirm the file changes.

Next we need to update our /etc/resolv.conf file by running the command:

```text
sudo resolvconf -u
```

Then restart Lokinet: 

```text
systemctl restart lokinet
```

