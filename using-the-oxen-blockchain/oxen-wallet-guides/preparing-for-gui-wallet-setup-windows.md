# Preparing for GUI Wallet setup (Windows)

When installing the Oxen Wallet, overzealous antivirus software can sometimes block the wallet or some of its components from functioning, or even delete them entirely. To avoid these issues on Windows, follow the below steps.

> Note: when you reach Step 6 (downloading the Oxen Wallet), ensure you are downloading the Wallet **directly from an official source:** either [here on the Oxen Docs](../../downloads.md), or on the [official Oxen Github releases page](https://github.com/oxen-io/oxen-electron-gui-wallet/releases/).

For the purposes of this example, our images show how to do this on Windows Defender

### Antivirus Steps

1. Create a folder called “Wallet” at the root of your C: drive (or another convenient location)
2. Open your antivirus program and go to “Manage Exceptions”, “Exceptions”, "Exclusions", or similar:

![](<../../.gitbook/assets/1 (1).png>)

![](../../.gitbook/assets/2.png)

3. Click “Add an exception”, "Add an exclusion", or similar:

![](../../.gitbook/assets/3.png)

4. Select your Wallet folder as the location for the exception:

![](../../.gitbook/assets/4.png)

5. Disable protection features for this folder:

![](../../.gitbook/assets/5.png)

6. Download the latest version of the Oxen Wallet **directly into the Wallet folder**:

![](../../.gitbook/assets/6.png)

7. Run the Oxen Wallet installer and select the Wallet folder as the install location:

![](<../../.gitbook/assets/7 (1).png>)

8. Check your antivirus program’s firewall and ensure all executables named “Oxen xxx” are permitted through the firewall:

![](../../.gitbook/assets/8.png)

Congratulations! You can now proceed to our [GUI Wallet setup guide](gui-wallet-setup.md) as normal.

**Note:** If you’re using Bitdefender specifically, and your wallet still does not function after following the above steps exactly, disable Advanced Threat Detection under Features
