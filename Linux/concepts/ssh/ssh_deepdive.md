# How Does SSH Work ?

SSH consists of three distinct layers:

1. **The transport layer** establishes safe and secure communication between a client and a server during and after authentication. It oversees data encryption, decryption, and integrity protection. Furthermore, it helps speed up data exchange by providing data compression and caching.
2. **The authentication layer** communicates the supported authentication methods to the client. It also conducts the entire user authentication process.
3. **The connection layer** manages the communication between the machines after the authentication succeeds. It handles the opening and closing of communication channels and allows multiple channels for multiple sessions.

After running the command `ssh [username]@[server_ip_or_hostname]` the server receives a request.

When the server receives the requests, a session _encryption negotiation begins_.

- The server sends the client a set of supported encryption protocols. The server uses the _public key_ as the authentication method.
- The client compares the protocols to its own set. If there are matching protocols, the machines agree to use one to establish the connection. The client compares _the server’s public key to the stored private key stored in its system_ on the first connection attempt. If the keys match, the client and the server agree to use symmetric encryption to communicate during the SSH session.
- After the server is verified, both the parties negotiate a session key using a version of something called the **Diffie-Hellman algorithm**. This algorithm is designed in such a way that both the parties contribute equally in generation of session key. The generated session key is shared symmetric key i.e. the same key is used for encryption and decryption.

Once the [sercret key](A-secret-key-is-a-confidential-piece-of-used-in-cryptography-to-secure-data.-It-acts-as-a-digital-code-that-enables-encryption-and-decryption,-ensuring-that-only-authorized-parties-can-access-protected-information.) is calculated. The server then attempts to authenticate the user who request access.

![ssh](https://assets.bytebytego.com/diagrams/0224-how-does-ssh-work.png)

### User Authentication

The two most common SSH user authentication methods used are

1. Passwords
2. SSH keys.

Password authentication involves using a username and password to log in, while ssh key authentication uses a pair of cryptographic keys – a public key that is stored on the server and a private key that remains on the client.

The clients safely send encrypted passwords to the server. The passwords are a risky authentication method. Asymmetrically encrypted SSH public-private key pairs are a better option. Once the client decrypts the message, the server grants the client access to the system.

---

## SSH Keys

SSH keys allow users to authenticate securely with remote servers. They replace the need for passwords by using keys. SSH keys always come in _key pairs_, consisting of:

- `Public key` - Everyone can see it, no need to protect it (for encryption function).
- `Private key` - Stays in the computer, must be protected (for decryption function).

### Techniques Used in SSH

SSH uses three data encryption types during the communication between the machines. These are:

###### 1. Symmetric encryption

Same secret key is used for both encrypting and decrypting data. In SSH, this technique is mainly used after the connection is established.

- Requires a single shared secret key between sender and receiver.
- Very fast and efficient for encrypting large amounts of data.
- Common algorithms include _AES_ and _DES_

Whenever the client and the server negotiate which algorithm to use for an SSH session, they always choose the first algorithm on the client’s list that the server supports.

###### 2. Asymmetric encryption

Data is asymmetrically encrypted when machines use two different but mathematically related keys, public and private, to perform the encryption. The client machine that participated in setting up the encryption can decrypt the information using the private key. SSH uses temporal asymmetric keys to exchange symmetric keys, such as during the user authentication process.

SSH uses this technique to authenticate users and securely exchange keys without exposing sensitive information.

- Public key is shared openly, while the private key remains secret.
- Used mainly for user authentication and key exchange.
- Common algorithms include `RSA` and `DSA`.

![as](https://media.geeksforgeeks.org/wp-content/uploads/20260120123028571995/asymmetric_encryption.webp)

###### Hashing

SSH uses hashing to validate if the data packets come from the source they appear to come from. Hashing algorithms used to produce hashes in SSH are `Message Authentication Code` (MAC) and `Hashed Message Authentication Code` (HMAC).

It is a cryptographic process that converts input data of any size into a fixed-length value called a `hash`.

- Produces a unique hash value for given input data.
- Even a small change in data results in a completely different hash.
- Helps detect tampering during communication.

The receiving machine knows the algorithm used to create the hash and can apply it to the data. The purpose is to see if the calculated hash value will be the same. If the obtained hash value differs from the sender’s hash, the data got corrupted during the transfer.

![hash](https://media.geeksforgeeks.org/wp-content/uploads/20260120121858720879/combination_logic_circuits.webp)

### Components of SSH Keys

The ssh key-pair is generated on the **CLIENT SIDE**, where `private key` stays in clients system, while the public should be shared with the server and be stored on the server.

###### 1. Public Key

A public key is the part of an SSH key pair stored on the _server_ to authorize a user’s access. It works with the private key to verify the client’s identity, enabling secure, password less authentication.

- Stored in the server’s `~/.ssh/authorized_keys` file.
- Cannot decrypt data or log in by itself, ensuring security even if exposed.
- Used during authentication to validate the client without transmitting sensitive information.

###### 2. Private Key

A private key is kept securely on the client device and is used to prove the user’s identity to the server. It must remain secret, as possession of the private key allows access to servers authorized for that key.

- Generates cryptographic signatures during authentication.
- Never transmitted over the network to maintain security.
- Provides the “unlocking” capability for the server’s public key “lock.”

###### Key Pair Relationship

The public and private keys are mathematically linked, forming a pair that enables secure authentication. The private key signs authentication challenges, which the server verifies with the public key to allow access.

- Ensures the private key never leaves the client device.
- Enables password less login while maintaining strong security.
- Forms the basis for asymmetric encryption used in SSH.

### Types of Keys

1. **User Authentication Keys** These are used to verify the identity of a user when connecting to a remote server using SSH.
2. **Host Keys** These keys are used by SSH servers to prove their identity to connecting clients. They help clients confirm that they are communicating with the correct server and not an impersonator.
3. **Session Keys** Session keys are temporary symmetric keys generated during an SSH connection to encrypt data exchanged between the client and server. They are created after authentication is completed.

## SSH Key Authentication Working

1. **Client starts the connection**: The user’s computer (client) tries to connect to a remote server using the SSH protocol.

2. **Server checks authentication method:** The server checks whether SSH key–based authentication is enabled and allowed for login.

3. **Client sends its public key**: The client sends its public key to the server. This key does not contain any secret information and is safe to share.

4. **Server checks authorized keys**: The server looks inside its `authorized_keys` file to see if the received public key is already registered.

- If the key is not found, access is denied.
- If the key is found, the process continues.

5. **Server creates a challenge**: To confirm the client’s identity, the server creates a random piece of data called a _challenge_ and sends it to the client.

6. **Client signs the challenge**: The client uses its private key to sign the challenge

- The private key never leaves the client’s machine.
- Only the correct private key can create a valid signature.

7. **Client sends signed response**: The signed challenge is sent back to the server as proof that the client owns the private key.

8. **Server verifies the signature**: The server uses the stored public key to verify the signature

- If the signature matches, the client is authenticated.
- If it doesn’t match, access is denied.

9. **Secure access is granted**: Once verified, the server allows the client to log in securely without asking for a password.

## How To Generate SSH Key with [ssh-keygen](../../commands/ssh/ssh_keys_agent_config.md) in Linux ?

[ssh-keygen](../../commands/ssh/ssh_keys_agent_config.md) is the utility used to generate, manage, and convert authentication keys for SSH. ssh-keygen comes installed with SSH in most of the operating systems.

SSH protocol supports several public key types for authentication keys. The key type and key size both matter for security.

When generating a key, we need to decide three things:

1. **KEY ALGORITHM**:
   For the key algorithm, we need to take into account its compatibility.
2. **KEY SIZE**:
   For the key size, we need to select a bit length of at least 2048 when using RSA and 256 when using ECDSA; these are the smallest key sizes allowed for SSL certificates.
3. **PASSPHRASE**:
   For the passphrase, we need to decide whether we want to use one. If used, the private key will be encrypted using the specified encryption method, and it will be impossible to use without the passphrase.

`ssh-keygen` is able to generate a key using one of the following different digital signature algorithms.

- _rsa_ – Widely supported, but less secure at shorter key lengths.
- _dsa_ – Deprecated; avoid using.
- _ecdsa_ – Smaller and faster than RSA; based on elliptic curve cryptography.
- _ed25519_ – Modern, fast, and secure; recommended.
- _ecdsa-sk / ed25519-sk_ – Secure key variants for use with FIDO/U2F hardware tokens.

Based on the difference of each SSH key type, we recommend the following ways to generate SSH key file.

```bash
ssh-keygen -t rsa -b 4096
ssh-keygen -t dsa
ssh-keygen -t ecdsa -b 521
ssh-keygen -t ed25519
```

![ssh](https://freedium-mirror.cfd/img/medium/700/1*o1ahfyTI8xQWb8HNL9CQ3g.png)

1. Kicking Things Off: Connection Initialization
   The SSH journey begins when the client reaches out to the server. This happens over port 22, the default port for SSH. As soon as the connection request hits, the server responds by sending over its SSH protocol version along with a menu of supported cryptographic algorithms. These algorithms cover encryption, MAC (Message Authentication Code), compression, and key exchange — essentially laying out the tools that will keep your session secure.

Client's Move: Initiates the TCP connection on port 22.
Server's Response: Sends back the protocol version and supported algorithms. 2. Finding Common Ground: Algorithm Negotiation
Once the server's capabilities are on the table, the client picks its preferred algorithms. This step ensures both sides are on the same page when it comes to encryption methods. Why does this matter? Because it sets the foundation for everything that follows — how your data will be encrypted, authenticated, and protected from prying eyes.

Client's Role: Chooses its preferred algorithms for encryption, MAC, compression, and key exchange.
Server's Role: Confirms the selection, signaling that the negotiation is complete. 3. Securing the Connection: Key Exchange Phase
Now that the groundwork is laid, it's time to securely exchange keys. The client and server agree on a method for this, usually opting for a tried-and-true approach like Diffie-Hellman. The client might send its public key or, if needed, request the server's public key. Then, the magic happens: the client generates a session key, encrypts it with the server's public key, and sends it over. The server decrypts it using its private key, and voilà — a secure, symmetric encryption for the session is established.

Client's Action: Sends supported key exchange methods, possibly sending or requesting public keys.
Server's Action: Agrees on the method, potentially sends its public key.
Result: A secure session key is established, ready to protect your data. 4. Proving Identity: Authentication Phase
At this point, the client has to prove it's legitimate. SSH offers several ways to authenticate, each with its own level of security.

Password Authentication: The client sends an encrypted password, which the server checks. If it matches, access is granted.
Public Key Authentication: A more secure method where the client sends its public key. The server responds with a challenge, which the client signs using its private key. The server then verifies this signature.
Multi-Factor Authentication: For those who want extra security, SSH can require additional steps like an OTP (One-Time Password) or hardware token.
Client's Role: Initiates the authentication process, choosing the method.
Server's Role: Verifies the credentials and grants access if they're correct. 5. Setting the Stage: Session Setup
With authentication out of the way, it's time to configure the session. The client can request various options, like setting environment variables or defining the terminal type. It might also ask for SSH agent forwarding, which allows the client to use local SSH keys on the remote server without transferring the keys themselves. If needed, the client can also request port forwarding or X11 forwarding, which are handy for tunneling services through the SSH connection.

Client's Requests: Specifies session options like terminal type, agent forwarding, or port/X11 forwarding.
Server's Response: Acknowledges and sets up the session according to the requests. 6. Getting Down to Business: Secure Communication Phase
Now that the session is fully established and authenticated, it's time to get some work done. Whether you're executing commands, transferring files, or managing server configurations, everything you do from here on is securely encrypted. This phase can involve a wide range of operations, from simple shell commands to complex file transfers using SCP (Secure Copy) or SFTP (SSH File Transfer Protocol).

Client's Actions: Sends encrypted commands and data.
Server's Actions: Processes the commands and sends back encrypted responses.
Ongoing Communication: The secure exchange continues as long as the session is active. 7. Closing Time: Session Termination
All good things must come to an end, and so does your SSH session. When you're done, you can log out, or the session might time out if left idle. The server will then close the connection and clean up any resources that were being used.

Client's Move: Sends a logout command or lets the session time out.
Server's Move: Closes the connection and frees up resources.
