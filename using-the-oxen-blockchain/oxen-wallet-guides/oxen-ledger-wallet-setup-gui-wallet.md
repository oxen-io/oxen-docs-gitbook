# Oxen Ledger Wallet setup: GUI Wallet

Using the Ledger hardware wallet with the Oxen GUI wallet is almost identical to the process with keys stored on your desktop. The two differences are:

1. There is a special process to create the wallet file.
2. When sending transactions the ledger device will prompt you to confirm the transaction.

**Connect the hardware wallet to the PC and navigate to the oxen app.**

The wallet needs to communicate with the Ledger and it must be on the Oxen dashboard page. This guide will demonstrate the process on a Ledger Nano S Plus. But both the Ledger Nano X and the Ledger Nano S will follow the same process.

![](https://lh3.googleusercontent.com/0MUtiAtky3qvrYycEAabDiowYpZ4ITg-XNlLxsQViTqSIzrkumoyKvWxTTG2WkXE4KysUNqN3HEKvH\_kNemvh3nIFVZwzPhuBA8BL0ddNsxxj\_hxorA4ThTXOEwxghWfyFRjiZV8HL9TzwkJZaJyTSR2wtE1k-1MOUBw17G1mDWhlbU9ngfwsXcJjw)

\
From the Ledger app homepage (app selection) navigate to the Oxen app and press both buttons to select.

As the Oxen wallet app is currently being reviewed by the Ledger team you will first be prompted with a screen that reads “Pending Ledger review”. This screen will display for a few seconds and then allow you to progress by pressing both buttons at the same time.

![](https://lh6.googleusercontent.com/QnPrjD2NxVEpnF\_0BN57RTFHLXnmrShwFV7ilCkAeXZggRcx1Zh\_IwiIHtYAaWqytUCYsGWFWqPMlQNrK7SnOt-mXTMlI6svrK-H949UlJDMIIhdKQ6Rr\_hxDcNUPQJhTDUE6D-1e2WTxXzAq-A8aZ1wHtFBGrlQMaGXrNxVnKVMXe11iPXOAfWlWw)

You should now arrive on the Oxen dashboard. The divide will display “OXEN wallet” and a truncated string of the Oxen address. On this Ledger you can see that the Oxen Address is “LCYwfQr..XHS”. This address should also match the address displayed on the GUI wallet.

![](https://lh6.googleusercontent.com/7Wtr9OBQJmKaPEb\_RPqV\_3pFHV6HD24D7b3PR95emzd2lZpkkMWy4DRxUeQ9wOYoMXN3HfuCYvG125C8PVGF\_3On91e480REQdQKAjtL2bpmtD4Rmkhj0ZYmq\_YHENQY1-tRGD3m5QVuoxS8CcF7OfZZ2LU6p31npxXjohy4LC0J0uf5ewPcyGxjYA)

The device will need to be on this screen to work with the Oxen GUI wallet. All communications to the device happen via this page on the Oxen wallet app. If you are on any other page on your Ledger device, the GUI wallet will display an error message.

**Setting up a hardware wallet file:**

When you open the GUI wallet you will see the regular wallet select screen. We will select “Create New Wallet” from the dropdown menu which appears when clicking the + icon in the top right of the window.

![](https://lh6.googleusercontent.com/DilYP4vLmUooU6AKLHKezSZbX3zfJc\_2P236APeQjKaCavkqp\_TLhJjPyShUkCi65FaVvxcmr-hehilUbq9FiRvN5tl8f5M4GI6AFmyBT5Y3mKULEjXQYZu2CSDODlA2KOpHMNXUWbLVx7vLWzNX3EEbrWMWFP3tUm49sppUH3F202DcF4xu8-1GzQ)

The 1.8.0 version of the Oxen GUI wallet now has a checkbox on this page to confirm that the wallet will be a hardware wallet. Fill in the form with the new wallet name, password, check the hardware wallet box and click “Create Wallet”

![](https://lh3.googleusercontent.com/WJ-E09KSHawBXqMmywU8T1b6rBh3yVoxRoMJ7wUo5Ab6kOvDHuHnYvWUM8XOy7kKPNtufDEo32ZHMpfx7c8OK6m57AFHfXoiRgzy18OfeppLtMyf-MpUg2-7ptCyxi5546HrHOHVLSaN8akVf7Yu\_jSE2-2OwYhbqpFSOqm-6GcwYjV4GdwEpUgIpg)

You will then receive a prompt from the Ledger to “Export View Key” in order to create the wallet file on your desktop. Navigate to the “Always Export'' page by clicking the right button once, then click both buttons to accept.

![](https://lh6.googleusercontent.com/zWw\_MXxK2DqFTGRXCbSTUVkOTwVyNp6r5RjqjH0MJVXNUlDPUX042uI\_FfZUBZZrn1KfPuPl\_12WelHGMULR-QCT1FVjqQfbwJahvt8KVU6\_NDMdux0R8pIj7bc5ta0x5OI5UWBQ3la3Rp9JtKDHYu4xxawT5qGa2W\_CJ6COKoRydXRMnnBJ9zYqsA)

![](https://lh5.googleusercontent.com/yzOtBOEO5XJDxwM23CPHtisAcW7pEiIywUdRuP0Rl2PtJcjUwmTeb92Te6dIhHhv3MqRQaexW80jvA3q94Sc47ip\_K09z7GuMKJFdMWrSRDE9u6s3ThxHqpqsWIyUQjYFAzolyuSrSSItrIR5JotmWUWepCr\_QkzkZaa18tk6QGuOLt4EVTdbzpn7Q)

Background on Exporting the View Key - Optional Technical Details on what just happened

One of the core functions of the wallet is to scan the blockchain which contains encrypted transactions. Each wallet has two keys that are used to:

1\) Decrypt the encrypted transactions from the blockchain (View Key)

2\) Craft transactions to spend funds in a transaction (Spend Key)

The wallet scans many transactions and processes a large amount of data to find the funds on the blockchain that belong to your wallet. This is an intensive computation that does take time on a regular computer. When using a hardware wallet the desktop does not have access to these keys, so we have two options to process the transaction:

1. Do the heavy computation on the small ledger wallet chip
2. Copy the View Key over to the desktop and let the bigger CPU handle this

There are drawbacks to both options. Doing the computation on the ledger is very slow. Exporting the view key is privacy reducing as it means if your desktop wallet is compromised it will be able to view your transactions (However it will not be able to make transactions).

Our recommendation is to export the view key and let the desktop process this calculation. The biggest benefit to having a hardware wallet is that a compromised computer will not be able to spend your funds so the tradeoff for processing the blockchain in a reasonable time is worth it in our opinion.

**Continuing with the wallet creation**

Once you have exported your view key (or not), the GUI wallet will display the “Wallet Created Page” which shows the address of your newly created hardware wallet. This address should match what is on your ledger device.\
\
![](https://lh6.googleusercontent.com/BQxpU7mmbe\_ZLZcyNuKDjT1mQEjUN23Q2qhMRxYDjeqaiYtY\_ctcEmm67owok6sfxBGC7pLS\_97OZENbmO4zmKY-LGZLUME45wBnHz1jOKpZ36SPNwjs8i5oihiA\_8QKc9zC9Z2CxGWDNK1j-m5ZpBpWvTJEJQtYrUANUpSuc5OZlVas07p9WG-DvA)\
At this point the wallet has been successfully created. Click “open wallet” and the GUI will navigate to the regular wallet dashboard page where you can spend/stake etc as usual.

![](https://lh5.googleusercontent.com/aJ6Zy34LMXM2ti5T4nMaV9TyX2g5f5Uq2HXHgaHJkloQePGrexEzobNhzIijhzbz1cSpPJnks4eeCu5e2dmIkbShYm6HinfZM\_QwGf3X\_6BlmbRVJlr4WFJGUAB27QMQR2bs7kMiTHWqatdU1TncgCTofjM61JlAz5qQhBXWvyW-Yk2SdQGmYVKEMA)

To access the hardware from now on, simply navigate through the wallet select page and you will see the new hardware wallet available under a heading called “Hardware wallets”. Clicking into this wallet will require the ledger device to be opened to the Oxen App main screen.

![](https://lh6.googleusercontent.com/YBU2745GmLrSPx6oTEe6xHC16dhFIaXYa7IL\_xL3wq\_6uenW6dbVka1po6XT3MoIDpqIp0nWwiOav7rNbCiHnj-BGQr6mmF\_oRynuYYrkU1zSiKEiPkE6t5V9hJGf5jUy7LyIwLRWUw3KaIB5BGkK63Vhh4Ty-iK9alEeKmGT6FRBDCOgJZ\_54ulYw)

#### Sending a transaction using the hardware wallet:

Navigate to the Send Transaction page on the wallet GUI

![](https://lh4.googleusercontent.com/A34d2Z86u1p6t7exkxq6YjDcMzRkTHIZ4qZvVTDVfs7kGp7h0q\_EVu0wCxihX\_OlCW1IWxFkCyeiE0iMhTSURR\_QF2DEUneni1wy80iG\_7ELcSuRwppgusVlRvS2JJquhgHL3OKST2mHoNPMsjvqjHZbGgYOi\_hptFo01JbBjIt25IV3MACX4uu6kQ)

Create a transaction by filling in the address and amount fields of this page and then clicking “Send”

![](https://lh3.googleusercontent.com/46czSXzuEtap4Bbqv6VmU8fljXI5NyzVvnmceE2A7458VRJG1HCtSOMNmQGFExMIdFyvcpx6ia3qZ3EP2uDG8jceXg2kZxDwu5D0zeuU5LESfVGhkTFikAkkFOj4-3U4NnCsO-1nvDt2TR9zEGq8LR4L25BiVbt8hyKYwjKHr4v3HiCdCjWaeVKUEQ)

The hardware wallet will then begin processing the transaction. At first you will see the “Processing TX” screen. This page may take some time as the ledger has to do some heavy calculations.

![](https://lh6.googleusercontent.com/qKkW3FtfD\_p9mcveaNkpGefEJRgnajlXzjBKwbU-3hFY1eNgRRozGpMnwmi2V\_VVAajDT1zcttPTrqVXybjiTIymlsmwwNeFic\_EUnn-w-eIUiMGWqXlA2a2rE-a0Qu5pxKqTld9M5J8Vv0U7KiBtrrWDS5GLJZImiHABbC7pKXAINzmcemO\_JpWGg)

After this point the Ledger will confirm the transaction fee. Click the right button once to navigate to the “Accept” page and click both buttons to progress.

![](https://lh4.googleusercontent.com/PlgTu31S6G9Fje7Gll6MR5-85S-PYkoQh5F0N\_qOE8GuiWCKGRsNbhubuN3-H0gc1HxvXOGtlBviewDt7tg0Zc8p7ra91JnAdCB48wnb-EcDQB0ln20PdcvDqk2hkNwmBRCjSiQfXnUcSN-QY5q14vaV5SbhHMYfcNrDs3x-Lnz2gJjz7hUIdtQ7Fg)

Next the wallet will confirm the amount and the recipient. Navigate right with the right button and the recipient should match what was entered into the GUI software page. Keep navigating right until the ledger displays the “Accept” page and click both buttons to progress

![](https://lh4.googleusercontent.com/ZPxhZcJsIVDSK3x0NauM0PUedcc-LlxhJ-ioH3GzauMxkZIJz7Dc17RZ49C0yidbUKTLLAfJkDbRpNqUhmfR05hIX0zfRTxxSxACiLhjH3nYpQdFyCENQX\_9YEtQtRWLYHj9NS1tmqb5sCl26JXIFIBPbkaBuLEZwUYgXDus9H56ZLG8\_rWMGyj2Dw)

![](https://lh6.googleusercontent.com/A3UFE8kxWd7HyUI\_svsQEqqbgqLGd98yrqwYL69GiJ-fwlu-AO4N-f6onzBt2OajZ-5FH6jCRqYiEwPoj0w-0uOX1LTwrhWlO9tAsWUJrFbKfsKj3S\_h3zedPmRpxCw277XuXJmqyTnFKT40b\_Mt5wVroPUnrOPjmuDu4Wwe6u-MFELzWhbnIrSIAQ)

After this point the GUI wallet will prompt you to confirm send. Click “Send” to broadcast the transaction to the network.

![](https://lh3.googleusercontent.com/\_ctj9g5qggPgKn3KQvZSg3FkmOfyu2Hk5ic3O-Q689DfLduIRkA-hFbwi3Z7nsJYnw6xGa9mp74C8tfL0O6G8KCywbxag3mwXArWvxUAvkxsCzIoyt6dZNN6JCsU94TraPTVgKPRdxiW1mYy88I\_AFxuJu8sfIbwKzKbRbld7LTjsiMptC5NzlL11w)

The ledger will do some final processing and you will be rewarded with a green “Transaction Sent” notification in the GUI wallet.\
