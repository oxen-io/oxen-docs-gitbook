# 📨 Message routing

## Message routing

Session follows one of two distinct cases for message routing, depending on the availability of participating clients:

### Asynchronous (offline) routing

By default, or when either of the participating clients' statuses is determined as offline (see Synchronous Routing for how client status is determined), Session will use asynchronous routing. In asynchronous routing, the sender determines the recipient's swarm by obtaining the deterministic mapping between the recipient's long-term public key and the currently registered Oxen Service Nodes. This information is initially requested from a random service node by the sender and updated whenever the client gets an error message in the response that indicates a missing swarm.

Once this mapping is determined, the sender creates the message protobuf and packs the protobuf in an envelope with the information to be processed by service nodes: the long-term public key of the recipient, a timestamp, TTL ("time to live") and a nonce which proves the completion of the required proof of work. The sender then sends the envelope using an onion request to one or more random service nodes within the target swarm (in practice, each request is always sent to 3 service nodes to achieve a high degree of redundancy). These service nodes then propagate the message to the remaining nodes in the swarm, and each service node stores the message for the duration of its specified TTL.

![](../../.gitbook/assets/image.png)

> _Alice uses an onion request to communicate with three random service nodes in Bob’s swarm. Bob then uses an onion request to retrieve said message, by talking to three random service nodes in his swarm._

### Synchronous (online) routing

Session clients include their online status in the encrypted protobuf of any asynchronous message they send. Along with their online status, a sending client also lists an Oxen Service Node in their swarm to which they are listening via onion request.

When a Session client receives a message which signals the online status of another client, the receiver sends an onion request to the sender's specified listening node. The recipient also exposes their own listening node to the sender. If this process is successful, both sender and receiver will have knowledge of each others' online status and corresponding listening nodes. Messages may now be sent synchronously through onion requests to the conversing clients' respective listening nodes.

![](<../../.gitbook/assets/image (1).png>)

> Alice uses an onion request to send a message to Bob’s listening node. Bob receives this message using an onion request, then sends a message to Alice’s listening node.

Messages sent using this synchronous method do not contain proof of work, and listening nodes do not replicate or store messages. To ensure messages are not lost, receiving clients send acknowledgements after receipt of each message. If either device goes offline, this acknowledgement will not be received, and the client which is still online will fall back to using the above asynchronous method of message transmission.

#### Friend requests

Friend requests are sent the first time a client initiates communication with a new contact. Friend requests contain a short message with a written introduction, the sender's prekey bundle, and meta-information like the sender's display name and public key, which the recipient can use to respond. Friend requests are encrypted for the public key of the recipient using ECDH. When a friend request is received, the client can choose whether to accept it. Upon acceptance, the client can use the prekey bundle to begin a session and start sending messages asynchronously.
