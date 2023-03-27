# running lokinet client on a soho router

the nicest way to run lokinet is on a soho router, it will make the defaults on the local network it serves use lokinet dns and even tunnel all traffic over lokinet in a way that it cannot leak.

## requirements

hardware with:

* 1 wired interface 
* 1 wireless interface
* ability to run debian bookworm or later

### network topology

the network toplogy of this box will be the following:

* the upstream network going wherever it goes to get internet on `eth0`
* LAN range is 10.200.0.1/24 on `wifi0`
* Lokinet range is 10.100.0.1/16 on `lokinet0`

note the interface names for `eth0` and `wifi0` will very likely be different, these are placeholder values for the purposes of this tutorial, please replace them with the appropriate values on your system.

### system component layout

the layout of the system has the following components:

* dnsmasq handles dhcp and forwards dns from LAN to lokinet and unbound.
* lokinet handles DNS for lokinet.
* unbound provides encrypted dns tunnel for ICANN.
* hostapd handles wifi access point.
* ifdown handles managing netowrk interfaces.

## setup

the setup have a few steps:

* installing the base system and base packages
* installing and configuring `lokinet`
* configuring the system to be a wifi ap with `hostapd`
* configuring the dns with `unbound`
* configuring the DHCP server with `dnsmasq`
* configuring the network interface management with `ifdown`


### install base system

first install debian bookworm onto your system. you will NEED the install medium including the extra firmware [here](https://cdimage.debian.org/cdimage/unofficial/non-free/cd-including-firmware/).
when installing you will want a minimal install with an SSH server and basic cli utils.

after completing install and rebooting the system into a fresh install, update the system:

`# apt update && apt full-upgrade`

install ssh onto the box:

`# tasksel install ssh-server`

it is highly recommended (but optional) to remove any non required packages that may have been installed.

`# tasksel remove desktop`

now, install the required packages from debian repo for our wifi setup:

`# apt install firmware-linux fwupd hostapd wireless-tools iw wpasupplicant dnsmasq unbound ntpd iptables ifdown`

finally you will want to enable ip forwarding in a persistent way:

`# echo 'net.ipv4.ip_forward=1' > /etc/sysctl.d/99-ip-forward.conf`

on system start up that rule will be applied, but you should apply them immediately with:

`# sysctl -p`


### lokinet

we now need to install and configure lokinet.

#### install lokinet

we add the apt repo for lokinet. follow instructions [here](https://deb.oxen.io/) to add the repo, dont install any packages from those instrctions. once you add the repos, update apt with:

`# apt update`

now install the lokinet package this way:

`# apt install lokinet --no-install-recommends`

#### set up lokinet configs

the easiest way to configure lokinet is to use config override files and not touch `lokineti.ini` at all.
that directory we try to load from if it exists is located at `/var/lib/lokinet/conf.d`. by default as of `0.9.11` tag it does not get autocreated.

so first, we create the config override dir:

`# mkdir -p /var/lib/lokinet/conf.d`

the first config override we want contains the network settings for lokinet for things like dns, ip range and interface name. this example will make lokinet only resolve `.loki` and `.snode`

put in config overrides into `/var/lib/lokinet/conf.d/wifi.ini`

file contents:

```ini
[network]
ifname=lokinet0
ifaddr=10.100.0.1/16

[dns]
bind=127.0.42.1:5533
# when this option is emtpy we only resolve .loki and .snode and will SERVFAIL any other queries.
upstream=

```

if you want to use an exit node make a new file at `/var/lib/lokinet/conf.d/exit-node.ini`

file contents:

```ini
[network]
exit-node=exit.loki
exit-node=exit2.loki
```

you can swap out exit.loki and exit2.loki with any other exit you know of, these are provided as functionging placeholder/default values for toy usage, the OPTF runs both of these exits at the moment.

you can add as many entries as you want and it will stripe traffic accross them per remote ip address.
(1.1.1.1 and 9.9.9.9 and 5.4.3.2.1 will use each select a random entry in your list of exits provided)

then edit the systemd unit to add a post up command we'll need for the routing table:

`# systemctl edit lokinet`

this will enter an editor. add the following snippet affer the the comment block where it tells you in the editor:

```ini
[Service]
PostExecStart=+/sbin/iptables -A POSTROUTING -j MASQUERADE -o lokinet0
```

then save and quit that editor (in nano it's control + o then enter then control + x).

### hostapd

this daemon manages the wireless access point.

#### configuration

in this example we set the ssid of the wifi network to be `lokinet` with the very strong password of `lokipass` (for real networks use a better password).
you MUST also set your country code, for the usa it's `US` for australia it is `AU` and for germany it is `DE`.

see the debian wiki page on [hostapd](https://wiki.debian.org/hostap) for more help on configuring.

config file at `/etc/hostapd/hostapd.conf`:

```ini
interface=wifi0
hw_mode=g
channel=0
ieee80211d=1
ieee80211n=1
ieee80211ac=1         
wmm_enabled=1
auth_algs=1
wpa=2
wpa_key_mgmt=WPA-PSK
rsn_pairwise=CCMP

ssid=lokinet
wpa_passphrase=lokipass

;; country_code=[insert your country code here]

```

#### caveats

many settings of hostapd may need to change depending on your hardware and region.
you will likely need to consult the main documentation for hostapd if you have more troubles.

### unbound

this daemon does dns for ICANN (the normal internet) via an encrypted DNS tunnel.

#### configuration

in this configuration example we use [adguard's DNS over TLS](https://adguard-dns.io/en/public-dns.html) as our dns provider. we cache results locally, and force ipv4 only.


this file goes to `/etc/unbound/unbound.conf.d/wifi.conf`

```conf
server:
    port: 5533
    interface: 127.0.42.2
    access-control: 127.0.0.0/16 allow
    do-ip6: no
    do-ip4: yes
    tls-system-cert: yes
    tls-upstream: yes
    rrset-roundrobin: yes
    qname-minimisation-strict: yes
    hide-identity: yes
    hide-version: yes
    aggressive-nsec: yes
    cache-max-ttl: 14400
    cache-min-ttl: 1200
    rrset-cache-size: 256m
    msg-cache-size: 128m
    
forward-zone:
    name: "."
    forward-tls-upstream: yes
    forward-addr: 94.140.15.15@853#dns.adguard-dns.com
    
```

### dnsmasq

this daemon handles dhcp on the wifi network.

#### configuration

add to the end of `/etc/dnsmasq.conf` the following line:

```conf
conf-dir=/etc/dnsmasq.d/,*.conf
```

then add the main configuration we'll put it at `/etc/dnsmasq.d/wifi.conf`

```conf

# dont load /etc/resolv.conf
no-resolv

# dns provider for lokinet
server=/loki/127.0.42.1#5533
server=/snode/127.0.42.1#5533
server=/100.10.in-addr.arpa/127.0.42.1#5533

# icann dns provider via local unbound daemon
server=/./127.0.42.2#5533

# use the interface called wifi0 for all our dns and dhcp
interface=wifi0

# dont use these interfaces, specifically adding eth0 so we dont take over the upstream network.
except-interface=lo,lokinet0,eth0

# we will use addresses from 10.200.0.10 to 10.200.0.220 for the wifi network's local ips.
# they will last 12 hours each, and will change unless the client is online on the wifi network.
dhcp-range=10.200.0.10,10.200.0.220,12h

# make all clients on the LAN use the wifi ap as the default DNS server.
# this will make all clients able to resolve .loki and .snode and also have locally cached dns for icann domains.
dhcp-option=option:dns-server,10.200.0.1

# make all client on the LAN use an NTP server on the wifi AP as the default too.
# this helps with making timesync faster with machines on the LAN.
dhcp-option=option:ntp-server,10.200.0.1

# assume the role of the only DHCP server on the LAN
dhcp-authoritative
dhcp-rapid-commit
log-dhcp

# dont cache ANY dns
no-negcache
cache-size=0

```

### ifupdown

we want to have ifdown set up the wifi ap so we give it a config file. to do so, add config file at `/etc/network/interfaces.d/wifi`

```ini
auto wifi0

iface wifi0 inet static
  address 10.200.0.1
  netmask 255.255.255.0
  dns-nameservers 10.200.0.1
  hostapd /etc/hostapd/hostapd.conf
```

then add a file for your upstream network in `/etc/network/interfaces.d/upstream`.
this exmaple will use dhcp in your upstream network to figure out how to get to the internet.

```ini
auto eth0

iface eth0 inet dhcp
```


#### caveats

your interface names will be different.

### reboot

now reboot the wifi box with:

`# systemctl reboot`

when it comes back up you can join devices to the wifi network called `lokinet` using the password `lokipass`.

#### caveats

lokinet `0.9.11` has a wire protocol handshake state machine race condition, this is planned on being fixed by `0.10.1`. the work arround is as follows:

stop lokinet:

`# systemctl stop lokinet`

remove the nodedb and profiles used by lokinet:

`# rm -rf /var/lib/lokinet/nodedb/ /var/lib/lokinet/profiles.dat`

then start up lokinet again:

`# systemctl start lokinet`

