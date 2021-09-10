# 🛑 Service Node deregistration

Deregistration rules are used to manage the network in a decentralised way by having Oxen Service Nodes police each other's performance, ensuring all operational service nodes are up to date and performing adequately to maintain a healthy network.

### Service node states

Each Oxen Service Node can be in one of four states: awaiting, active, decommissioned or deregistered.

| **State** | **Description** |
| :--- | :--- |
| **Awaiting** | Service node is awaiting the staking requirement being met. |
| **Active** | Service node is staked, performing tasks as required, and receiving rewards. |
| **Decommissioned** | Service node is not performing tasks as required or is not meeting uptime standards, and has been put into an inactive state where it does not receive rewards. |
| **Deregistered** | Service node has been inactive for too long, and has been deregistered. |

#### Decommission and credits

Active Oxen Service Nodes earn "credits" which are then used up during any periods where the service node goes offline \(or otherwise stops meeting network requirements\) to stop a deregistration from occuring. A new service node starts out with `INITIAL_CREDIT`, and then builds up `CREDIT_PER_DAY` for each day the service node remains active, up to a maximum of `DECOMMISSION_MAX_CREDIT`.

| **Variable** | **Value** |
| :--- | :--- |
| `INITIAL_CREDIT` | 60 blocks \(~2 hours\) of decommission time. |
| `CREDIT_PER_DAY` | 24 blocks \(~0.8 hours\) of decommission time. |
| `DECOMMISSION_MAX_CREDIT` | 1440 blocks \(~48 hours\) of decommission time. |
| `MINIMUM` | 60 blocks\(~2 hours\) of decommission time. |

**Example**:

If an Oxen Service Node stops sending uptime proofs, a quorum of service nodes will analyse whether the service node has built up enough credits \(at least `MINIMUM`\). If so, instead of submitting a deregistration, the quorum instead submits a decommission. This removes the service node from the list of active service nodes, both for rewards and for any active network duties.

If the service node comes back online \(i.e. starts sending the required performance proofs again\) before its credits run out, a quorum will reinstate the service node using a recommission transaction, which adds the service node back to the bottom of the service node reward list, and resets the node's accumulated credits to 0. If the service node does not come back online within the required number of blocks \(i.e. if the node does not come back online and begin performing as required before its credits are depleted\) then a quorum will send a permanent deregistration transaction to the network, locking that node's stake for 30 days.

### Testing quorums

A testing quorum is a random set of 10 Oxen Service Nodes that are required to test a portion of the network for uptime proofs and service node IP changes. At each block, a new quorum is formed, and this quorum is required to test 50 service nodes or 1% of the network.

If 7 of the 10 service nodes in a quorum \(a "supermajority"\) vote that a service node is malicious or not meeting the minimum requirements then they will create a `State_Change` transaction to decommission or deregister the service node, or drop it to the bottom of the rewards list.

#### Quorum Tasks

| **Task** | **Description** |
| :--- | :--- |
| **Uptime Proofs** | The quorum will check to see if a service node has provided uptime proofs within the last 2 hours.  If a Service Node is found to have not provided uptime proofs, but it has at least `MINIMUM` credits, it will be decommissioned. If it does not have at least `MINIMUM` credits, it will be directly deregistered. |
| **IP Changes** | The quorum checks to see if the SN has advertised more than one IP to the network in the last 24 hours.  If a service node's advertised IP has changed, it will be forced to the bottom of the service node reward list. |
| **Checkpointing** | The quorum checks that each service node within the quorum has provided a hash of a block for a specific block height \(refer to [Checkpointing]() for more information on this procedure\).  If a service node within the quorum does not provide a hash, but it has at least `MINIMUM` credits, it will be decommissioned. If it does not have at least `MINIMUM` credits, it will be directly deregistered. |

### State change transactions

A state change transaction changes the state of an Oxen Service Node. Typically, state change transactions are only created when a quorum comes to a consensus about a given service node's activities or lack thereof.

The only state change transaction that can be created by anyone is the `Register` transaction, which changes the state of a service node from `awaiting` to `active`.

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>State_Change_Transaction</b>
      </th>
      <th style="text-align:left"><b>Description</b>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>Register TX</code>
      </td>
      <td style="text-align:left">
        <p>A transaction (lock transfer) created from the <b>client</b> registering
          the service node.</p>
        <p>
          <br />State Change: <code>Awaiting -&gt; Active</code>
        </p>
        <p></p>
        <p>Note: This is <b>not</b> a quorum transaction.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>Decommission TX</code>
      </td>
      <td style="text-align:left">The service node is temporarily deregistered; it remains in the service
        node list, but is removed from the rewards list and from any network duties.
        <br
        />
        <br />State Change: <code>Active -&gt; Decommissioned</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>Recommission TX</code>
      </td>
      <td style="text-align:left">The service node is added back to the service node list and placed at
        the bottom of the rewards list.
        <br />
        <br />Stage Change: <code>Decommissioned -&gt; Active</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>IP Change TX</code>
      </td>
      <td style="text-align:left">The service node is put at the bottom of the rewards list as punishment
        for changing its IP.
        <br />
        <br />Stage Change: N/A (no change)</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>Deregister TX</code>
      </td>
      <td style="text-align:left">The service node is deregistered.
        <br />
        <br />State Change: <code>Active -&gt; Deregistered</code> / <code>Decommissioned -&gt; Deregistered</code>
      </td>
    </tr>
  </tbody>
</table>

