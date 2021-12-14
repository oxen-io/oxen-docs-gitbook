# CLI Wallet setup (macOS)

This guide covers installing and configuring the Oxen CLI (Command Line Interface) Wallet on macOS.

> If you are new to console or terminal commands and/or would prefer a wallet with buttons and Oxen branding, we suggest you download the [GUI Wallet](../../downloads.md).

Before following the guide below, you'll need to download the CLI Wallet for macOS. This guide assumes you are running the latest version of macOS and have Xcode installed.

### Step 1: Opening oxen-wallet-cli and oxend.

To use the `oxen-wallet-cli` we must first have the daemon, `oxend`, up and running. The `oxend` is your node which the `oxen-wallet-cli` broadcasts through. Without the node running the `oxen-wallet-cli` will not be able to operate.

Open up the `oxend` file in the folder you extracted the release from. In most cases you will have the following security dialog box pop up.

If the dialog box opens up you will need to allow the `oxend` to open by moving your cursor to the top-left of your screen, clicking the apple logo, and then clicking on system preferences.

Once in System Preferences, click on the `Security & Privacy` button, check in this new screen that the `oxend` file was blocked, if it was you should be confident to click “Open Anyway”.

Keep clicking `open` and `allow` to give the software permission to run.

Once the software has permission a terminal window should pop up and say the following text:

```
**********************************************************************

The daemon will start synchronizing with the network. This may take a long time to complete.

You can set the level of process detailization through "set_log <level|categories>" command,

where <level> is between 0 (no details) and 4 (very verbose), or custom category based levels (eg, *:WARNING).

Use the "help" command to see the list of available commands.

Use "help <command>" to see a command's documentation.

**********************************************************************
```

Let the daemon run until the node is completely synced, you will know the node is synced once the terminal outputs the following text:

```
**********************************************************************

You are now synchronized with the network. You may now start oxen-wallet-cli.

Use the "help" command to see the list of available commands.

**********************************************************************
```

Now the daemon is synced we can run the `oxen-wallet-cli` file.

### Step 2: Setting up your oxen-wallet-cli account.

If this is your first time opening the `oxen-wallet-cli` it will request for you to specify a wallet name. For the purposes of this user guide we will use the example name `MyWallet`

```
Specify wallet file name (e.g., MyWallet). If the wallet doesn't exist, it will be created.

Wallet file name (or Ctrl-C to quit): MyWallet
```

Because this is the first time we have used the name `MyWallet` the following text will appear in our terminal. Type in `Y` or `Yes` to confirm your wallet name.

```
No wallet found with that name. Confirm creation of new wallet named: MyWallet

(Y/Yes/N/No): Yes
```

The `oxen-wallet-cli` has now generated us a wallet called `MyWallet` and is now prompting us for a password for our generated wallet.

> Please note:
>
> * When typing the password, the characters will not appear. It will seem as if you are typing and no text is appearing however the terminal is logging every character your clicking including if it is capitalised or lowercase.
> * Write down your wallet name and password on a piece of paper as this information will be required every time we want to enter our wallet.
> * Use a password with uppercase letters, lowercase letters, numbers, symbols and make the password at least 9 characters long.

```
Generating new wallet...

Enter a new password for the wallet:

Confirm password:
```

Now once we have chosen our password for the wallet we must choose our language. For the purposes of this user guide I suggest you use English by typing `1` and clicking enter.

```
List of available languages for your wallet's seed:

If your display freezes, exit blind with ^C, then run again with --use-english-language-names

0 : Deutsch

1 : English

2 : Español

3 : Français

4 : Italiano

5 : Nederlands

6 : Português

7 : русский язык

8 : 日本語

9 : 简体中文 (中国)

10 : Esperanto

11 : Lojban

Enter the number corresponding to the language of your choice: 1
```

The `oxen-wallet-cli` will generate and spit out several lines of text. Some of the information that was outputted will only ever show once, therefore it is very important to do this next section properly otherwise we may lose access to our account, thus losing access to our funds.

Let’s take a close look at each section of the newly generated wallet:

The text after `Generated new wallet` shows your public address. This address can be shared and will be used to receive $OXEN to your wallet. All Oxen public addresses start with an `L....` and are followed with a string of characters. The public address shown will be your primary address however multiple public addresses can be generated from this primary address.

You do not need to write down the public address, the command `address` will re-display it whenever required.

```
Generated new wallet: LAXk6eS3Ng98QxDTdC47eNdfCXttJycKraXxfsw9cMVngGUqP3kiSE6cwXoApU6gjzSXVX1ASAPAi1MSXA935XUs1MWEcv9
```

The View key address is not to be shared unless you want to show the transactions received to the public address connected to this wallet. You do not need to write down the view key as it can be re-displayed with the command `viewkey`.

```
View key: 97d3c27e20818e5e23a6548458b50d4f128a2709c55eb7f9518d0e957a5d2e0d
```

The next few lines of text show how to navigate the `oxen-wallet-client`.

```
This user guide will look into more detail the commands that can be used within the loki-wallet-client further in the guide.

Your wallet has been generated!

To start synchronizing with the daemon, use the "refresh" command.

Use the "help" command to see the list of available commands.

Use "help <command>" to see a command's documentation.

Always use the "exit" command when closing loki-wallet-cli to save

your current session's state. Otherwise, you might need to synchronize

your wallet again (your wallet keys are NOT at risk in any case).
```

The next section with the random 25 words is your mnemonic seed. The seed is used to easily back-up and restore your wallet without needing any other information. At this stage, grab a pen and paper and write down your 25 words in order (having these words out of order will not restore your wallet) and store the piece of paper in a safe and secure place. If your words are stored in a text file on your computer or stored online, you increase your risk of someone else getting control of your account.

```
NOTE: the following 25 words can be used to recover access to your wallet. Write them down and store them somewhere safe and secure. Please do not store them in your email or on file storage services outside of your immediate control.

ponies innocent oyster whale autumn knapsack jostle elapse

inroads joining doorway ticket drying obnoxious algebra tutor

biplane sack alpine zinger huge duets refer rigid inroads
```

The last of the outputs are the account balance, because your wallet does not have any $OXEN in it currently the balance is showing 0.

Once we receive a transaction of $OXEN into our wallet the balance will appear as soon as the transaction is confirmed in one block (usually less than 2 minutes). Once the transaction has been confirmed over 10 blocks the balance will show in unlocked balance.

The unlocked balance is the $OXEN available to be spent/sent to other addresses.

```
Starting refresh...

Refresh done, blocks received: 0

Untagged accounts:

Account Balance Unlocked balance Label

* 0 LAXk6e 0.000000000 0.000000000 Primary account

----------------------------------------------------------------------------------

Total 0.000000000 0.000000000

Currently selected account: [0] Primary account

Tag: (No tag assigned)

Balance: 0.000000000, unlocked balance: 0.000000000

Background refresh thread started”
```
