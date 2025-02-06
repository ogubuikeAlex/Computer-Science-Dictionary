- There is an implied existence of security when we talk about a client server interaction but unfortunately with HTTP, a third party could intercept the underlying IP packets.

- What is Encryption?
This is when we turn information to a random illegible version of the same data. This data should be able to be decrypted.

What is Symmetric Encryption: One Key Lock
This is the type of encryption where we use a single key to encrypt and decrypt information. This is fast but also not very secure because once a person gets this key they can easily hijack the process.

What is a man-in-the-middle-attack?
An attack in which the attacker intercepts a line of communication that is thought to be private by its two communication parties. If a malicious actor intercepted and/or mutates an IP packet on its way from a client to a server, that would be a man in the middle attack. M.I.T.M attack are the primary threat that encryption and HTTPs aim to defend against.

What Is Asymmetric Encryption: Two Lock Key
This is the use of a pair of keys to secure a system aka public-key encryption. This usually involves using a private key and a public key to encrypt and decrypt data. The keys are generated using cryptographic algorithms and are mathematically connected such that data encrypted with the public key can only be decrypted with the private key. Crypto wallets use this kind of system.

AES
Stands for Advanced Encryption Standard. It is widely used encryption standard that has three symmetric key algorithm (AES-128, AES-192, AES-256). It is considered to be a golden standard for encryption.

What is the difference between HTTP and HTTPs
HTTP is the Hyper text transfer protocol and Https is Hyper text Transfer Protocol Secure
HTTPs is an extension of HTTP that runs on TLS or is encrypted using TLS. It is used for secure online communication.
TLS stands for Transport Layer Security. It is a security protocol built on top of TCP, to encrypt data communication between a client and a server.

What is a Certificate Authority?
A trusted entity that signs digital certificates - namely SSL certificates that are relied in HTTPs connections.

What is a TLS Handshake?
is a series of steps that allows two parties – typically a client and a server – to authenticate each other, agree on encryption standards, and establish a secure channel for transferring data. The two machines involved in using the TLS protocol must achieve to be able to transact successfully.

How Does The TLS Handshake Work?
- First a client will send a cHello (which is a random message) to the server
- The Server will repl;y with a cServer message. This cServer reply will also be accompanied by an SSL Certificate which will be encrypted using the private key from a third party Certification Authority. Within the SSL certificate will be information about the server especially its public key. 
- When the client gets the reply from the server, it will decrypt the ssl certificate using the CA's public key. Then it will retrieve the servers public key
- Using the servers public key, the client will create a paymaster secret. This is also basically a random string encrypted with the public key
- Then the client will send this to the server. At this point both the server and the client will have => cHello, sHello and the paymaster secret.
- Using these, they would both create session keys. Because they are using the same items to create the same session keys on both end, the session keys will be the same

What is an SSL Certificate? and What is a CA?
An SSL certificate and its domain/website 
An SSL certificate is a digital certificate that is issued by a CA to an organization, it verifies that a trusted third party has authenticated that organization's identity. and enables the organization to establish secure and encrypted connection with other systems.