---
description: >-
  Session is an anonymous encrypted messenger that uses onion routing and public
  key-based cryptography to minimise metadata leakage.
---

# 🔒 Session

Session offers fully anonymous account creation, with absolutely no identifying information required. Users can communicate in fully end-to-end-encrypted one-to-one chats or small group chats with full end-to-end encryption \(‘closed groups’\).

Session can be used as a basis for applications or services that rely on real-time notifications or communications, with the crucial advantage that developers of these services can onboard users without requiring them to provide phone numbers or reveal their IP addresses or any other identifying information.

### Usage examples 

#### Service notifications

Mary wants to build a notification service which lets subscribers know when there are new religious services running on her university campus. She wants users to be able to anonymously subscribe for updates, so their religious affiliation is not linked to their identity. Mary can build this service using the [Node Session client](https://github.com/hesiod-project/node-session-client) or a [Session open group](https://github.com/oxen-io/session-open-group-server).

#### Anonymous 2FA code delivery

Vijay wants to create a 2FA service which sends one-time authentication codes to users. However, he doesn’t want to pay for text messages sent to each device which registers. He also wants to create a service which respects user privacy, and doesn’t like the idea of collecting his users’ phone numbers. Vijay can allow users to register for his service with their Session ID instead of a phone number, enabling fully anonymous 2FA code delivery.

### Developing with Session: A quickstart guide

#### Session IDs

Session IDs are Session’s user account identifiers. A Session ID looks like this: 

`05067a2e9896678aa966ed3ece82d12f28344d4e669db98f6f2183411a8c98797c`

Practically speaking, Session IDs are encoded 🗝 public keys which allow messages to be encrypted for the recipient. You will require a user’s Session ID to register the user to your Session-based application or service, or send and receive messages with them. 

#### Node Session client

Basic programmatic interaction with the Session client is possible using a community-developed project called [`node-session-client`](https://github.com/hesiod-project/node-session-client/blob/master/sample.js). Using this client, you can send messages to Session IDs, receive and parse messages sent to your Session ID, send messages to an open group, and receive and parse messages sent to an open group. 

