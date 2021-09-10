# Restoring an Oxen CLI Wallet from keys

If you have a Mnemonic Seed Phrase, restoring your wallet from it would probably be a good option. Otherwise, you can restore your wallet from private keys. You will need to have the following keys to proceed:

* Wallet public address
* Spend Key
* View Key

### Step 1: Download and unzip CLI wallet

* [Download](../../downloads.md) the latest release of wallet CLI software for your desired operating system
* Unzip  `oxen-[operating-system]-[platform]-[version].zip` file

### Step 2: Run wallet in restore mode

* Open a [Command Prompt](https://en.wikipedia.org/wiki/Cmd.exe) \(Windows\) or [Terminal](https://en.wikipedia.org/wiki/Terminal_emulator) \(Linux / OSX\) and navigate to the wallet folder
* Run wallet with `--generate-from-keys` argument:

`./oxen-wallet-cli --generate-from-keys [New Wallet Name]`

Where `[New Wallet Name]` is a new wallet name. You can enter any name here, use something memorable and meaningful.

### Step 3: Enter wallet address, view and spend keys

On the next step, specify all 3 wallet keys, one by one:

* Enter **Standard address** and press \[Enter\]
* Enter **Secret spend key** and press \[Enter\]
* Enter **Secret view key** and press \[Enter\]

### Step 4: Enter wallet password

* You will be prompted for a password. Enter a new password that follows the [Password Policy](https://en.wikipedia.org/wiki/Password_policy) and press \[Enter\].
* Confirm password and press \[Enter\].

### Step 5: Specify a blockchain height

If you know the block height at which wallet was created or a first transaction was made, you can enter it here. Specifying a blockchain height will help to scan the wallet faster.

If you don't know a specific blockchain height, press \[Enter\] for scanning from block height 0.

### Step 6: Wait for the refresh process to finish

For refresh process to start, you need to have your daemon running. Another option would be to use a remote node. For that, use the following command, replacing  and  with the host and port number of the remote node you are connecting to:

```text
./oxen-wallet-cli --daemon-address <host>:<port>
```

Once refresh is done, you can use your full functioning restored wallet. Your public wallet address will remain the same.

