# 🔡 Using Oxen Name System (ONS)

{% hint style="warning" %}
**The Oxen Network has transitioned to the Session Network. More information** [**here**](https://oxen.io/blog/development-is-transitioning-to-session-token)**.** \
\
<mark style="color:$danger;">**It is no longer possible to register new ONS mappings. It is possible to update ONS mappings if you still hold OXEN.**</mark> \
\ <mark style="color:$danger;">**Once the new Session Name Service (SNS) launches, it will be possible to create new usernames mapped to Account IDs on Session. ONS mappings will also be migrated to the new SNS.**</mark>&#x20;
{% endhint %}

\
The following steps can be used to register and update ONS mappings in the CLI wallet. Note that you can access a detailed description of each command within the app, using “help \<command\_name>”.

### Purchasing an Oxen Name System record

Purchasing an ONS record in the CLI wallet uses the `ons_buy_mapping` command. All arguments for the `ons_buy_mapping` command are optional except for the `<name>` and `<value>` that the name maps to.

`ons_buy_mapping [index=<N1>[,<N2>,…]] [<priority>] [owner=<value>] [backup_owner=<value>] <name> <value>`

**Example:** Buy an ONS record that maps ‘KeeJef’ to a Session ID: 053 … 254. The wallet buying is the owner of the record:

```
ons_buy_mapping KeeJef 053b6b764388cd6c4d38ae0b3e7492a8ecf0076e270c013bb5693d973045f45254
```

**Example:** Buy an ONS record that maps ‘KeeJef’ to a Session ID: 053 … 254. The wallet buying the mapping is the owner of the record, but specifies a backup owner who will also be authorised to update the record:

```
ons_buy_mapping backup_owner=T6U7AaNgN2cARpR7CNHGChGMjsmjq5ffh4hLa4DjUUXtKS3bPy2rKTX614RxmpPPX6KjZzqUSSpAEcoghASTXqvP1qMsJzWch KeeJef 053b6b764388cd6c4d38ae0b3e7492a8ecf0076e270c013bb5693d973045f45254
```

**Example:** Buy an ONS record that maps ‘KeeJef’ to a Session ID: 053…254. You buy on behalf of another wallet T6UD8...ppir and specify another backup wallet:

```
ons_buy_mapping owner=T6UD8TM1t7mUYmMCHXQ67Kg5jXBwoVxNqGpXctnsLXtGBEFnhq37RQAA8jgqgD9U6QbeNGqAkkVXucXQ5txE6Mrk2aRwpppir backup_owner=T6U7AaNgN2cARpR7CNHGChGMjsmjq5ffh4hLa4DjUUXtKS3bPy2rKTX614RxmpPPX6KjZzqUSSpAEcoghASTXqvP1qMsJzWch KeeJef 053b6b764388cd6c4d38ae0b3e7492a8ecf0076e270c013bb5693d973045f45254
```

### Updating an Oxen Name System record: Wallet executing update is owner of record

Updating an ONS record in the CLI wallet uses the `ons_update_mapping` command. All arguments for the `ons_update_mapping` command are optional except the name of the record to update, and at least one field of the record to update (owner, backup\_owner, or value). The \[signature] argument is for deferring updates to the record, and is explained in detail in the next section.

`ons_update_mapping [owner=<value>] [backup_owner=<value>] [value=<ons_value>] [signature=<hex_signature>] <name>`

**Example:** Updating the owner of the record (essentially transferring ownership — after changing ownership, you will no longer be authorised to update the record):

```
ons_update_mapping owner=T6UC1nSy2289uX8R2jS3ci7y6eNnVdvhSQRoZtckPzmrQgJ3CyUhUtxgxuedusx9TCKVhZZBCuwFkKoJ3joXStWh1QozRsXXo KeeJef
```

_Note: At this time you can only update Session ID mappings, you cannot yet add a wallet address mapping to your name — this will require additional changes which will be implemented at a later date._

**Example:** Update all fields of the mapping.

```
ons_update_mapping owner=T6UD8TM1t7mUYmMCHXQ67Kg5jXBwoVxNqGpXctnsLXtGBEFnhq37RQAA8jgqgD9U6QbeNGqAkkVXucXQ5txE6Mrk2aRwpppir backup_owner=T6TEJJRfvhMZbJpRuchJtmQAjuyCUAyYy2yVcc9ySxTHXWgwQkupjUJUQsyCoyYfRGReAY3pgaYxUHwoKEkWNh5o2qe5Btt3x value=0596d2fdc1407490e1bb7cbca3f3674606d3ef9b1d01cf46199ee5c8932d83f40a KeeJef
```

### Updating an Oxen Name System Record: Wallet executing is not owner of record)

In this scenario, you have an ONS record that you wish to update, and you’re able to coordinate with the wallet owning the record. The wallet that owns the record can execute this command to generate a signature.

`ons_make_update_mapping_signature [owner=<value>] [backup_owner=<value>] [value=<ons_value>] <name>`

**Example**: User transfers ownership of a record to another person T6TEJ…t3x with the value 058c…c08. The original owner generates a signature.

```
ons_make_update_mapping_signature owner=T6TEJJRfvhMZbJpRuchJtmQAjuyCUAyYy2yVcc9ySxTHXWgwQkupjUJUQsyCoyYfRGReAY3pgaYxUHwoKEkWNh5o2qe5Btt3x value=058c72182ecf25172999414f098f91c07bcf12eae7e0c810659f533e81dc865c08 KeeJef
```

**The generated signature:**

```
3ede65e0a78eca500549dde612d02d8aeb3f3dd0f3accd767a2f013cab6e3d486582506fbeb7edb1bda209b333fe7f125fd29f6add6c72b20af320de3537788885fee8b6d76f14b4ad253db2f70a518054bb6f512465e1b6cc154c551d3d59b5bf528eef5a678dbee48e2da74a2803c47295acd6967ea5545f6213456a0f5ead
```

On the wallet to execute, the arguments must match the arguments specified in the `ons_make_update_mapping_signature` with the added signature argument:

```
ons_update_mapping owner=T6TEJJRfvhMZbJpRuchJtmQAjuyCUAyYy2yVcc9ySxTHXWgwQkupjUJUQsyCoyYfRGReAY3pgaYxUHwoKEkWNh5o2qe5Btt3x value=058c72182ecf25172999414f098f91c07bcf12eae7e0c810659f533e81dc865c08 signature=3ede65e0a78eca500549dde612d02d8aeb3f3dd0f3accd767a2f013cab6e3d486582506fbeb7edb1bda209b333fe7f125fd29f6add6c72b20af320de3537788885fee8b6d76f14b4ad253db2f70a518054bb6f512465e1b6cc154c551d3d59b5bf528eef5a678dbee48e2da74a2803c47295acd6967ea5545f6213456a0f5ead KeeJef
```
