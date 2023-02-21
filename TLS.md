# Transport Layer Security #
A security protocol for the transport layer
- Provides encryption, authentification, and Integrity
- Was originally called SSL (secure sockets layer) before ranming in 1999

- **Encryption** - a process of encoding a message so that it can only be read by those with an authorized meands of decoding the message
- **Aunthetification** - a process to verify the identity of a particular party in the message exchange
- **Integrity** - A process to detect whether a message has been iterfered with or faked

## TLS Encryption ##
Uses a combination of symmetric and asymmetric cryptography
- Bulk of the message exchange is conducted via symmetric key encryption
- Initial symmetric key exchange is conducted using asymmetric key encyrption
- The process by which the initial secure connection is set up is called the **TLS Handshake**

- This makes sense as we need to encrypt both requests and responses, so primarily using symmetric encyrption would allow for this with the use of a single key
- However, to exchange this key securely, we need to use asymmetric encryption, in the from of the TLS handshake

### TLS Handshake ###
- A `ClientHello` message is sent after the TCP `ACK` (So immediately after connection is established, but before any data is sent to server)
  - Contains maximum version of TLS protocol that client supports as well as a list of Cipher suites the client is able to use
- After recieving `ClientHello`, server responds with `ServerHello` message
  - Sets TLS protocol version and Cipher suite, as well its certificate containing a public key and a `ServerHelloDone` marker
- After client gets message with `ClientHelloDone` marker, it inititates the key exchange process
  - Uses server's public key to send a keygen for the symmetric key it wants to use for future use of message encryption
  - In message, includes a `ChangeCipherSpec` flag to indicate to switch over to the symmetric key, andd a `Finished` flag to say the handshake is finished

**Note about cipher suite**
- It is a list of algorithms that can be used for each part of the encryption (key exchange, authentication, symmetric key encryption, messgae integrity)
