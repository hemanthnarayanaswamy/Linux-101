# SSH KEYS

An **SSH key file** is a cryptographic credential used in the **SSH (Secure Shell)** protocol to authenticate users or systems without relying on passwords. 

It comes in *pairs* — a private key (kept secret on the client) and a public key (shared with servers). Together, they enable secure, encrypted, and automated access to remote systems.

The ssh key-pair is generated on the **CLIENT SIDE**, where `private key` stays in clients system, while the `public key` should be shared with the server and be stored on the server.

![key](https://media.geeksforgeeks.org/wp-content/uploads/20260120121640390794/public_key.webp)

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

![ssh1](../../../resources/assets/ssh1.png)

### Key File Types:

1. **Identity Keys** – Private keys stored locally (e.g., `~/.ssh/id_rsa`).
2. **Authorized Keys** – Public keys stored on the server `~/.ssh/authorized_keys`.
3. **Host Keys** – Identify servers to prevent man-in-the-middle attacks.


Resources:
- https://www.howtouselinux.com/post/ssh-authorized_keys-file