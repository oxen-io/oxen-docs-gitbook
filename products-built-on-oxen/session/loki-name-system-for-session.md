# 🔤 Oxen Name Service for Session

## Namespaces

The ONS namespace is broken up into two distinct sections. One section is responsible for all Lokinet names, referred to as `.loki` names, and the other section is responsible for Wallet and Session usernames.

## Names

Each ONS name can resolve to a Session public key, Wallet address or `.loki` address. Session and Wallet names are within the same namespace. When purchasing a Session record, the Wallet record is automatically added to your possession. Lokinet names are in their own namespaces. For example, when purchasing ‘KeeJef’ in the Session namespace, you can assign a Session public key. Additionally you can update the wallet record under a 2nd transaction.

So when a user looks up ‘KeeJef’ they are returned both a Account ID and Wallet address:

```
AccountID: 053b6b764388cd6c4d38ae0b3e7492a8ecf0076e270c013bb5693d973045f45254 
Wallet Address: LBMTVEK8WRiC9rmfoKEjyrZSRje6PdiTU6926LjMkGMUdzyApMvXUbH4LswHnLjMjMPLUbDKiL3RCRQe5XFiobWb8jQrApR
```

It is also possible to purchase ‘KeeJef.loki’ in the Lokinet namespace that is unique from the Session and Wallet namespace.

Depending on which context the name is being used, the application will automatically use the relevant mapping.

## Rules

Both namespaces (Session/Wallet and Lokinet) have restrictions on the characters allowed in a name. All names are case-insensitive.

For Session (or Wallet), the name has to:

* start with an alphanumeric character or underscore
* consist of alphanumeric, hyphens or underscores in between
* end with an alphanumeric character or underscore

Users may register names with special characters or emojis by using the equivalent Punycode representation. The name must be at least 1 character long, and at most, 64 characters long.

For Lokinet, the domain must:

* start with an alphanumeric character
* contain alphanumeric characters or hyphens

The character before the domain suffix `.loki` must be alphanumeric followed by the suffix `.loki`. Users may register names with special characters or emojis by using the equivalent Punycode representation. The domain name must be at most 253 characters long, including the `.loki` suffix.

## Time

By default all mappings in the Session/Wallet namespace will be preserved forever. This is important to ensure that wallet mappings don't change when a user wants to send money to a name. The `.loki` namespace will have re-registration periods to deter domain squatting and release names if the keys that registered them are lost.

## Ownership, transfer, updates and management

By default names are owned by the wallet address that purchased the name. However, names can also be purchased on behalf of another user. Up to 2 wallet addresses may be specified as the owners of a name. This means up to 2 wallets can update and or transfer ownership of the record.

Once a name it can be transferred to another user's Oxen wallet by specifying the address of that wallet and paying the standard transaction fee to transfer the ownership.

Updates to mappings can be made at any time by the owner, at the cost of the standard transaction fee to include the new mapping in the blockchain.

Initially, management of all owned names will be possible through the Oxen Desktop Wallet; however we aim to add functionality to register and manage names to Session clients directly in the future.

## Cost

Names in the Session/Wallet namespace cost 7 $OXEN to register. We will try to update this cost per hardfork to continue targeting the $5-10 USD range.

## Privacy

It is up to each user to choose which information they map publicly. If you don’t want to map your Account ID or Wallet address to your real world identity you might want to choose a different alias instead of something related to your name. Basic encryption is employed to mask publicity of data on the surface level, but please do not rely on this for critical privacy requirements.

All wallet sub-addresses generated via account new are supported as owners.

## Technical documentation

### Record encryption/decryption

ONS records stored in the blockchain undergo basic encryption and decryption to deter monitoring of the chain. This does not provide strong guarantees for privacy and should not be relied on for situations with critical privacy requirements.

At its core, a typical ONS record from the database looks like:

* `name_hashed`
* `encrypted_value`
* `register_height`
* `owner`
* `backup_owner`
* `txid`
* `prev_txid`
* `register_height`
* more fields (implementation details, see oxen\_name\_system.h)

Of which the `name_hashed` and `encrypted_value` fields use some form of encryption or decryption.

### `name_hashed`

* Human readable name is hashed with `blake2b` with the following parameters
  * Key Length: 0 bytes
  * Hash Length: 32 bytes
* Name hash converted to base64 for storage into the `sqlite3` database (this provides optimal lexicographic lookup as a key).

```
    hash32 = Blake2B(name, key=0)
    name_hash = Base64Encode(hash32)
```

### `encrypted_value`

* Generate the secret key for decryption/encryption using the unhashed name with `Argon2ID v1.3` and the following parameters as of Valiant Vidar v7.1.X
  * Iterations: 3
  * Memory: 268\_435\_456 bytes
  * Salt Length: 0 bytes
  * Key Length: 32 bytes
* Decrypt/encrypt the value represented in binary with `XSalsa20Poly1305` and the following parameters as of Valiant Vidar v7.1.X
  * Nonce: 0 bytes

```
    key32 = Argon2ID(iterations=3, memory=268435436, salt=0)
    encrypted_value = XSalsa20Poly1305Encrypt(value, key32, nonce=0)
    decrypted_value = XSalsa20Poly1305Decrypt(encrypted_value, key32, nonce=0)
```

### Owners

In ONS, an owner of a record has the ability to update information about the record by making a transaction on the Oxen blockchain. Owners are specified when buying a record and in ONS we support 2 types. A standard Ed25519 keypair and an Oxen Wallet address.

By default, the wallet is configured to assign itself as the owner of the mapping when purchased.

### Record updating

ONS records can be updated by getting the owner of the record to generate a signature that authorises the protocol to update fields in the record.

The following fields can be updated in a record

```
Value
Owner
Backup Owner
```

* Copy the fields to update into a buffer
* Copy the TXID into the buffer that last updated the record (which can be retrieved by querying the mapping).
* Hash the buffer with `blake2b` with the following parameters
  * Key Length: 0 bytes
  * Hash Length: 32 bytes
* Sign the hashed buffer
  * If the current owner (or backup owner) is a wallet address, it must be signed with the current owner's (or backup owner's) wallet secret spend key.
  * If the current owner (or backup owner) is a ed25519 key, it must be signed with the current owner's (or backup owner's) ed25519 secret key.

```
    // *If value is specified, copy the value to the buffer, otherwise skip.
    buffer[] = {*binary_value, *owner, *backup_owner, prev_txid}
    hash32 = Blake2B(buffer, key=0)

    if (monero)
        signature = generate_signature(hash32, spend_pkey, spend_skey)
    else
        signature = ed25519_signature(hash32, ed25519_skey)
```
