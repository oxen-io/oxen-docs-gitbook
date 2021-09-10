# 📎 Attachments

Although [service nodes](../../about-the-oxen-blockchain/oxen-service-nodes.md) have the ability to store data on behalf of clients, this responsibility only extends so far. Requiring service nodes to store attachments, which can easily be orders of magnitude larger than messages \(and might need to be stored for longer periods of time\) would place an undue burden on the service node network.

With this in mind, a logical solution is for Session to interface with an untrusted centralised server that stores data obliviously. As long as the central server cannot know the contents of files, or who is storing and requesting the files, this system does not cause any metadata leakage.

This is achieved by first padding each attachment to fit within a fixed number of constant sizes between 0 and 10 megabytes, then encrypting the attachment with a random symmetric AES key. The sender then uploads the encrypted file using an onion request. In response, the file server provides a link to the piece of content, returned via the onion request path.

Once the sender obtains this link, they then send a message to the recipient via an existing pairwise session. This message contains a link to the content, a hash of the content, and the decryption key. The recipient then uses an onion request to pull the encrypted attachment from the centralised file server and decrypt it locally using the decryption key provided by the sender. The recipient also checks the hash against the attachment, ensuring the file has not been modified in transit.

By default, all Session clients use a Session file server run by the [Oxen Privacy Tech Foundation](https://loki.foundation/) for attachment sending and storage. Since attachments are not considered a core feature of Session, this design is in keeping with Session's design principles. The file server is fully open-source, with setup instructions provided so that users are able to set up their own file server. In an upcoming update users will be able to specify in the Session client which file server they want to use for attachment sending functionality. This is important both for providing users with choice and control, and ensuring the continued usefulness and functionality of Session if the OPTF were no longer able to maintain the default Session file server.

