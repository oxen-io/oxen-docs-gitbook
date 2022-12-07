# 🗺 Domain Names For Lokinet (ONS)

## Lokinet Addresses

By default Lokinet uses 52 character non human readable strings as addresses in the network, for example:&#x20;

[http://dw68y1xhptqbhcm5s8aaaip6dbopykagig5q5u1za4c7pzxto77y.loki](http://dw68y1xhptqbhcm5s8aaaip6dbopykagig5q5u1za4c7pzxto77y.loki)

These default addresses can be hard to remember and communicate to users.&#x20;

### ONS Records

ONS (Oxen Name Service) allows SNApp operators to map a human readable name to any default .loki address, as an example

[http://probably.loki](http://probably.loki) \
\
Which is an ONS record mapping the human readable name `probably` and the Lokinet address `dw68y1xhptqbhcm5s8aaaip6dbopykagig5q5u1za4c7pzxto77y`&#x20;

ONS records are registered as transactions the [Oxen blockchain](../../../about-the-oxen-blockchain/overview.md) and can be resolved by Lokinet clients, which with the Service Node network. They work in a similar manner to other blockchain naming services like [ENS](https://ens.domains/)

### Registering a .loki ONS Record

> _Registering a .loki ONS mapping requires the user to create an Oxen wallet and deposit at least 7 Oxen (plus transactions fees), visit the_ [_Oxen website_](https://oxen.io/buy-oxen) _to find out where Oxen can be bought_

You can register a ONS record by opening the "Oxen Name Service" tab in the [Oxen GUI wallet](../../../downloads.md)&#x20;

<figure><img src="../../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

Once the "Oxen Name Service" tab is open, select the drop down menu underneath "ONS Record Type", and choose how long you would like to register your Lokinet name for.&#x20;

<figure><img src="../../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

Add your preferred human readable name, and your existing .loki address, an Oxen address may also be specified if you want to delegate ownership to a wallet separate from the wallet currently in use. Backup owners can be specified and have the same rights as the owner.&#x20;

Once the relevant fields have been populated press "purchase" and confirm the transaction, transactions can take up to 20 minutes to confirm, once confirmed, you record will be registered and resolution between the human readable name and the default loki address should be complete.&#x20;
