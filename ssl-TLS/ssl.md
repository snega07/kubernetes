- symmetric encryption
- asymmetric encryption
- certificate
- how ssh works. How ssl works


How ssh works?

**Create an SSH key pair:** Generate a public key and a private key. Keep the private key secure.
Copy the public key to the server: Store it in the target Linux user's ~/.ssh/authorized_keys file. In AWS EC2, AWS typically installs the selected public key when the instance launches.
**Initiate SSH:** Your SSH client connects to the server and offers the public key corresponding to your private key.
**Check authorization:** The SSH server checks whether the public key is authorized for the requested user.
**Prove private-key ownership:** Your SSH client signs session-related authentication data using the private key and sends the digital signature to the server.
**Verify the signature:** The server uses the authorized public key to verify the signature. If verification succeeds, the server knows the client possesses the corresponding private key and grants access, subject to other authentication and account policies.

**Remember:** The private key never needs to be sent to the server. SSH uses the private key to sign and the public key to verify.

How SSl/TLS works?

The correct one-way TLS flow
1. Server sends its certificate. Your browser validates the certificate chain, trusted CA, expiration, and hostname using the SAN (and, where applicable, the CN).
2. Both sides exchange temporary public key shares. Each side generates its own temporary key pair. They exchange their public shares — not their private keys — and independently calculate the same shared secret using ECDHE.
3. Server proves it owns its private key. The server signs handshake data using the private key corresponding to its certificate. The browser verifies the signature using the certificate's public key. This proves possession of the private key and authenticates the handshake.
4. Both sides derive traffic keys. TLS uses the shared secret material and handshake-dependent inputs in its key-derivation process to derive matching client-to-server and server-to-client traffic keys.

Traffic key	      Browser has	Server has
Client-to-server	Encrypts	Decrypts
Server-to-client	Decrypts	Encrypts
For example, when you log in to a website:
1. Both sides derive matching traffic keys during the TLS handshake.
2. Your browser encrypts your credentials using its client-to-server key.
3. The server decrypts them using its matching client-to-server key.
4. The server encrypts its response using its server-to-client key.
5. Your browser decrypts the response using its matching server-to-client key.
Important: The keys aren't transmitted over the network. Both sides derive them independently from the TLS key schedule.

5. Encrypted communication begins. The browser encrypts your credentials using its client-to-server traffic key. The server decrypts them using its corresponding client-to-server traffic key. The response uses the server-to-client traffic key in the opposite direction.

In mTLS, the flow includes additional client-authentication steps:
1. Server sends its certificate; the client validates it.
2. Server requests a client certificate.
3. Client sends its certificate to the server.
4. Server validates the client certificate against its configured trust store and applicable policies.
5. Client proves it possesses the corresponding private key, typically by signing handshake data.
6. Both sides complete the handshake and derive traffic keys.


