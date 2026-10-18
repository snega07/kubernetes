SSL/TLS

user -> browser : data transfer

symmetric -> the same secret key is used for encryption and decryption.

- TLS handshake starts.
- Client sends supported algorithms and a key share.
- Server sends its supported choice, certificate, and key share.
- Both client and server use the exchanged key-share information plus their own private information to independently calculate the same shared secret.
- TLS derives symmetric traffic keys from that shared secret.
- Application data is encrypted using those symmetric keys.

Client                                  Server
  |                                       |
  |---- Supported algorithms + key share->|
  |                                       |
  |<--- Certificate + key share ----------|
  |                                       |
  |                                       |
  |  Both derive shared secret            |
  |                                       |
  |  🔑 Traffic keys derived              |
  |                                       |
  |==== Encrypted data ==================>|
  |<==== Encrypted data ==================|



Asymmetric -> encrypt data using private key. 

Why certificates?