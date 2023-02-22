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
  - Sets TLS protocol version and Cipher suite, as well its **certificate containing a public key** and a `ServerHelloDone` marker
- After client gets message with `ClientHelloDone` marker, it inititates the key exchange process
  - Uses server's public key to send a keygen for the symmetric key it wants to use for future use of message encryption
  - In message, includes a `ChangeCipherSpec` flag to indicate to switch over to the symmetric key, andd a `Finished` flag to say the handshake is finished

**Note about cipher suite**
- It is a list of algorithms that can be used for each part of the encryption (key exchange, authentication, symmetric key encryption, messgae integrity)

## TLS Authentification ##
We need a way to make sure that our encrypted connection is with a party that is who they are claiming to be
- During the TSL handshake, as part of its `ServerHello` message, the server provides its **certificate**
- As well as providing the public key, the certificate provides a means of identification for the party providing it (identifies the owner of certificate)
  - However, certificates are publically available, so the person using it may not actually be thw **owner** of it
  - To combat this, an algorithm is used to send an encrypted message to the client to verify if the server is in possession of the correct private key for the certificate
  - If the client can confirm the server has access to the correct private key for the public key included in the certificate, the owner ie legit

## Certificate authorities and the chain of trust ##
CA (certificate authorities) are trustworthy sources of digital certificates
- They verify the party requesting the cert is who they sey they are (they own the domain they claim to own)
- They digitally sign certificates issued to signify that those certificates are trustworthy

## TLS encapsulation ##
TLS can be thought of as a protocol operating between HTTP and TCP
- When transporting application data, TLS encapsulates the data in the same way as other protocl units
- PDU from application is encpaulsated in data payload of TLS (called a record)
- Header fields include content type, TLS version, bit length, message authentication code, and padding

### MAC (Message authentication code)
This is different from MAC address
- Similar concept to the checksum we've seen in other PDUs, but different implementation and intention
- Meant to add a layer of security by providing a means of checking that message has not been altered in transit
  - This is differnt from other protocols like TCP, IP, or Ethernet, which check to see if data was lost or corrupted
    - Instead of using a checksum, it uses a hashing algorithm to capture a small chunk of the data (digest(, and place it in the MAC header field
    - The reciever uses the same hashing algorithm (communicated during the TLS handshake cipher suite negotition) to generate a digest once it decrypts the message
    - Reciever compares generated digest with the sent digest to see if they are the same
