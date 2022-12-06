# Exporting a CSV of your wallet transaction history

The Oxen Wallet allows you to export a CSV of transactions that have occurred in the wallet. The process for both the GUI wallet and the CLI wallet are as follows:

### **GUI Wallet**

The Oxen GUI wallet as of v1.6.0 supports the exporting of your transactions. After logging into your wallet:

* Click on SETTINGS at the top right of the app
* Click on Export Transfers

![](https://lh5.googleusercontent.com/PJbDsGGe4DCJjVaMHQY28TEFLSs9Q7DtvF5ZoRkl4v23U2OYh6Vb8DjH1yfY8R3znCrOGUa3ijN94q1NCJD0DXOpl3FgvEFj3hT3njH99AvY9NNCpom8Imrzrm0lFk2XyWw86Bk1)

* The wallet will then prompt you for a folder to save the CSV to. This defaults to a CSV directory within the Oxen folder (Same directory your wallet keys are stored) but you can edit this to any location.

![](https://lh6.googleusercontent.com/woqk9Xh9QZp8r\_UfVFuTtkOJmFolFvxbMEbEpJoo3LxL6zfXwnMFmswLXLyS-vFNHJuqdMEUz0PaXyPk40NWU6y1U75dfMLFe0cBRs33Xkp9t46Xj7UydzRhHN0BVP57Yw7cwI4f)

* Click Export and you will be prompted for your wallet password
* You will then see a green notification at the bottom of the window if the export is successful. For example: “Transfers exported to /home/sean/Desktop/transfers.csv”

### CLI Wallet

Note you will either need to be running a local daemon (oxend) or have a remote node that the CLI wallet can communicate with.

Navigate to the directory that the oxen-wallet-cli executable is stored in:

```bash
sean@sean-MS-7A33:~/oxen-core/build$ cd bin/
sean@sean-MS-7A33:~/oxen-core/build/bin$ ls
oxen-blockchain-ancestry  oxen-blockchain-import              oxen-blockchain-usage      oxen-sn-keys
oxen-blockchain-depth     oxen-blockchain-mark-spent-outputs  oxend                      oxen-wallet-cli
oxen-blockchain-export    oxen-blockchain-stats               oxen-gen-trusted-multisig  oxen-wallet-rpc
```

Run the oxen-wallet-cli:

```bash
sean@sean-MS-7A33:~/loki-core/build/bin$ ./oxen-wallet-cli
```

If you are using a remote node instead of a local daemon you will need to add additional command line arguments here:

```bash
sean@sean-MS-7A33:~/loki-core/build/bin$ ./oxen-wallet-cli --daemon-address public.loki.foundation:22023
```

The wallet will ask you to enter the name of your wallet file. If you are coming from the GUI wallet these are stored in the following locations

1. Linux: \~/Oxen/wallets/MyWallet.keys
2. Windows: C:\Users\&lt;username>\Documents\Oxen\wallets
3. Mac: \~/Oxen/wallets/MyWallet.keys

These can alternatively be included as command line flags

```bash
sean@sean-MS-7A33:~/loki-core/build/bin$ ./oxen-wallet-cli --daemon-address public.loki.foundation:22023 --wallet-file ~/Oxen/wallets/MyWallet --password "password"
```

Once the wallet has synced you will be able to call \`export\_transfers\` which saves the CSV to the same location as where you ran the oxen-wallet-cli (Most likely the same location as the executable).

```bash
[wallet T6SjAL (has locked stakes)]: export_transfers all output=oxen.csv
```
