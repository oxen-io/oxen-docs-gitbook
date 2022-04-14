# GUI Wallet Staking

Oxen allows users to stake their oxen by running service nodes which support the Oxen Network. To run a service node an operator is required to stake 15,000 oxen which they can either contribute the full amount or share the burden between multiple contributors to reduce the amount required individually.\
\
This guide does not explain how to set up a service node, instead it explains for non-technical users how they can contribute to a service node which is run by a separate operator, allowing them to share in the rewards earned by that service node.

The first step is to find out if there are any service nodes which are "Awaiting Contribution", this occurs when a service node requires additional contributors to make up the 15,000 oxen requirement.

If you navigate to [oxen.observer ](https://oxen.observer)you can see on the front page a section titled "Service Nodes Awaiting Contributions" (Highlighted in Orange Below).

![Service Nodes Awaiting Contributions](<../../.gitbook/assets/#1 Blockchain Explorer.png>)

We are going to stake to the service node highlighted in green (a321e...)

Throughout this guide we will be using the Oxen "testnet" for demonstration purposes. This differs slightly to our mainnet in that you only need to contribute 100 oxen to set up a service node. Aside from this, the process is identical.

#### About Service Node Staking

This section will explain some of the technical details contained in the above image and is optional, please feel free to skip to the next section which explains the GUI wallet usage if you please.\
\
In the above image there are 2 Service nodes that are awaiting contribution, the columns in the table are as follows:

* Public Key: Service nodes are identified by a unique public key, this will be used later in the staking process and for finding the node at later dates. So you will need to write this down
* Contributors: A maximum of 4 contributors are allowed to stake to a single service node. For the identified service node there is a single contributor so far (The operator who set up the node)
* Operator Fee: The operator is allowed to charge a "fee" which is used to cover their server expenses. In this case the operator is looking to charge a 10% fee for running the node. This fee is market driven and we see a range of fees depending on the token price, hosting fees etc. For a ballpark figure the range is usually between 5%-15%. The calculation for how the fee affects contributors is as follows:
  1. Each block that the service node creates will earn all contributors 16.5 oxen
  2. The operator will take their fee of 10% = 1.65 oxen
  3. The remaining balance 16.5 - 1.65 = 14.85 oxen will be distributed pro rata amongst all the contributors.
  4. In this guide we will be contributing 75% of the total staked amount. This means we will be receiving 11.1375 oxen per block the node creates.
  5. The operator who contributed 25% of the total staked amount will also receive 3.7125 oxen for their share. This is in addition to their fee so the operator will receive 5.3625 oxen in total.
* Contributed: This keeps track of the total oxen staked so far by contributors. When this amount reaches 15,000 oxen the node will be considered fully staked and becomes an active service node that can earn rewards.
* Open for Contribution: The remaining amount needed before the node is fully staked.
* Min Contribution: As there is a maximum of 4 contributors to a service node there is a minimum amount to contribute enforced on the stakers. This formula is: `Remaining Amount Open for Contribution / Slots left for stakers`
  * In this case there is only 1 contribution so far, 3 slots out of 4 remaining. 75 oxen open for contribution divided by 3 = 25 oxen
  * This usually results in a minimum of about 25% of the total requirement (25% of 15,000 = 3,750). However if a person contributes over the minimum amount it means that later contributors have a lower minimum. So infrequently we see a minimum required amount of a few hundred oxen.
* Expiry Date: Historically service nodes had an expiry date and operators were required to restake/restart the node. This has changed and now it is usual to run nodes infinitely with no expiry date.

#### Steps in the GUI wallet to stake to another Service Node

Open up the GUI wallet and click on "Service Nodes"

![Home page of the GUI wallet](<../../.gitbook/assets/#2 Home of GUI Wallet.png>)

When on the service nodes page, you will see the following:

![Service Nodes Page](<../../.gitbook/assets/#3 Staking Page.png>)

There is a fair amount going on in this page. The areas identified by the green numbers above are:

1. Buttons to navigate the service nodes page (You will want to stay on the "Staking" tab for now).&#x20;
   * My Stakes: Keeps track of nodes you have staked to and will be discussed below
   * Staking: Current page and what we are using to stake to another persons node
   * Registration: Used to set up a service node and is outside of the scope of this guide
2. Service Node Key: Here we enter in the the public key we identified from the oxen.observer page. We chose the key a321e... and have entered it in.
3. Amount: This needs to be above the minimum contribution and a maximum that will completely fill the service node. There are buttons on the right that conveniently calculate this for you
4. A list identical to the oxen.observer page is shown here. Clicking on the stake button on the right of the items will prefill the Service Node Key and Amount will be set to the minimum

We are deciding to fully stake the remaining amount to this service node, so have entered in 75 oxen (75+25 = 100 oxen required for testnet nodes, on mainnet this is 15,000).

When the fields are correct click on "Stake" (Highlighted with a green box above). You will be prompted with a confirmation.

![Confirm Stake to proceed](<../../.gitbook/assets/#4 Staking confirmation.png>)

After clicking Stake your wallet will send a transaction to the network. You should see a green notification that the staking transaction was sent successfully. This transaction will lock that amount so that you are unable to spend, but the funds still belong to you and are available to unlock (Which will be shown shortly). Navigating to the main page of the wallet will show the pending stake transaction.

![Pending Staking Transaction](<../../.gitbook/assets/#5 Transaction submitted.png>)

The pending transaction once confirmed on the blockchain will transform into a "Stake Transaction"

![Stake transactions only lock your oxen while earning rewards, you remain owner and can unstake at any time](<../../.gitbook/assets/#6 Successful Transaction.png>)

Once the transaction has been confirmed you will be able to check the oxen.observer website for the service node and it will confirm that the service node is active. You can find this page by searching the Service Node Public Key in the search box at the top of the page.

![Fully Staked Service node](<../../.gitbook/assets/#7 Observer page.png>)

Back in the wallet you will also be able to navigate to the "My Stakes" tab which will show the staked service node. Please note that this page is updated on wallet startup, so you will need to restart the wallet to see the changes after staking.

![My Stakes page showing active service nodes](<../../.gitbook/assets/#8 My Stakes.png>)

On this page you will see a complete list of staked nodes and how much you have contributed. On the right of each node you will see the option to unlock (Highlighted in red). When you wish to stop staking you will need to begin the unlock process, this takes 15 days on the mainnet so careful planning needs to be done to ensure you unlock early enough if you need access to the funds. Clicking unlock will prompt you with with a confirmation popup.

![Unlock warning](<../../.gitbook/assets/#9 unlock.png>)

Please note that when you contribute to a node other users also have a vested interest in the node. Unlocking will force the other users to also unlock and the operator will need to restart the node staking process which many operators find frustrating. Frequent unlocking is rude. \
\
When unlocking the node will continue earning rewards until the point it is unlocked (15 days). Funds will also remain locked during this time. Once the 15 days have elapsed the funds will be unlocked and available to spend and the node will cease to earn rewards.

![My Stakes confirming unlock](<../../.gitbook/assets/#11 Unlocking my stakes.png>)

The My Stakes page will also show the node as unlocking and when visiting the oxen.observer website the service node page will also show the node as unlocking.

![Service node scheduled to expire](<../../.gitbook/assets/#12 Oxen observer page.png>)

At this point you have all the skills that are necessary to stake to a service node and to unlock the stake when you wish to exit. Visit [https://oxen.observer/](https://oxen.observer) and start exploring the nodes that are awaiting contribution today!&#x20;
