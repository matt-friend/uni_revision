# Security 101 Revision Notes

## Historical cryptography

### Kerchoff's principle
* Assume the enemy knows all about your system - it is not secret
* Security of the system relies solely on the secrecy of the keys

### Brute force
* If you have a number combination lock with n numbers, there are 10^n combinations for an attacker to try. As the number n increases, the combinations increase exponentially.
* Increasing the possible keys prevents the likelihood of a brute force attack
* For example a 10 bit binary key would be extremely easy to break but a 256 bit key would take an impossibly long time to crack

### Historical Ciphers
#### Caesar cipher 
* Shift of all letters by a certain number of positions
* Very easy to break, can use brute force as only 26 possible keys
* St Cyr slide or cipher disks used to manually encrypt messages

#### Monoalphabetic cipher
* Replace each letter of the alphabet with a different random letter
* Often uses a key word at the start of the alphabet then the rest of the alphabet sequentially
* Harder to brute force than Caesar cipher - 26! keys; use letter frequency and common words instead. Does not hide word length or repeated letters

#### Patterns
* Despite a monalphabetic cipher having 2^80 possible keys, still not secure as does not hide patterns. 
* A good cipher/ encryption should hide all kinds of patterns in the message such as letter frequency, repeating words and letters
* This includes word length and message length
* Length should be padded for extra security
* Therefore number of key combinations != security level

#### Vigenere cipher
* Use a repeating word (key) to encrypt the message
* Add letter value to message value to get ciphertext - modulo result with 26 to stay within alphabet
* Hides letter patterns
* Can remove space to hide word lengths
* Still not very secure
* Modern crypto uses modulo 256 (bytes) or modulo 2^32 or 2^128 (blocks)

### Bits of security
* If a system as n bits of security:
	* An "efficient function of n steps/time to operate it (e.g. n^2)
	* to break the system takes 2^n steps/time

### System security recommendations
* Modern standard is that systems have a security of at least 128 bits
* 80 bits of security is weak and less than that is terrible
* 192 bits is recommended
* 256 bits is considered secure

### Key lengths
* Key length does not equal security level
* Public key schemes (e.g RSA) have much longer keys than the security level (256 bit = 15424 key length)
* Symmetric encryption schemes usually have a security level equal to the key length

### Passwords
* A good password is at least 20 characters
* This is much shorter than a secure keylength which is why using passwords as encryption keys is a terrible idea

---

## Encoding and encryption
* Encoding != Encryption
* Encoding is the method of transforming data into a differemt format in order for it to be communicated.  
* These methods are publicly known
* There is therefore no secrecy, anyone can decode
* Encryption is the method of trasnforming data so that no-one can understand it except the intended recipients
* Requires a **key**
* A way to encode is a **code**, a way to encrypt is a **cipher**

### Types of encoding
* Email is encoded; done automatically by email program
* Morse code - different characters have different lengths
* Baudot code - each character has same size of 5 dots (blank or filled)
	* this is the same as saying Baudot characters are 5 bits
* ASCII - American Standard Code for Information Interchange
	* 7 bits per character, no accents (just english characters and punctuation etc)
	* Can be stored in a byte, with top bit 0
* Unicode- every character gets a number (**code point**)
	* Is a standard, not an encoding so can't be used to write directly
	* Pick from: 
		* UCS-2 (2 byte characters, basic subset of Unicode)
		* UCS-4 (each charcter is 4 bytes)
		* UTF-8/16 (variable length encoding)
		* UTF-7 (email-safe format, 7 bits per byte)

### Text and binary formats
* A file is in text format is it contains text in some type of text enoding e.g. ASCII, UTF-, UCS-
* A file is in binary firmat if it just contains random bytes; may not print correctly if opened.

### Text-safe encoding
* It is important to encode binary files before sending or viewing, as they are hard to view in raw form
* This is important for cryptogtographic data like keys, data, or signatures which need to be viewed and manipulated and are binary data.

### Base64 encoding
* Splits 3 bytes into 4 6-bit pieces
* Encodes the 4 blocks as characters
* Allows binary data to be viewed as text whilst only increasing the length of the encoded data by 4/3, rather than doubling like binary to hex
* Base64 has its own unique encoding - not same as ASCII
* Is what enables messages encrypted into binary data to be viewed as text in something like an email

---

## Cryptography Part 3 -  Secret Key Cryptography

### Block ciphers (and modes of operation)

#### Block Cipher
* e.g. AES - advanced encryption standard
* Uses a 16-32 byte key, encrypts a message to ciphertext of same length
* E(k,m) = c
* Once a key is picked, block cipher is random-looking function from blocks to blocks, like a monoalphabetic cipher
* Monoalphabetic is letters to letters, block cipher is blocks to blocks
* Not completely random as has to be decrypted - just needs to be *random-looking*
* However, does not hide patterns over blocks

#### ECB mode
* Method of using block cipher that invloves simply uses the encryption function to sequentially encrypt blocks from the message: **C<sub>i</sub> = E<sub>k</sub>(P<sub>i</sub>)**
* **Do not do this**
* example: 2013 Adobe data breach, passwords were encrypted in 7 letter blocks
	* 123456 is encryoted to half the length of 12345678
	* "password" has same encrypted start as "password1"

#### Modes of Operation
* Good modes include an extra random block at the start
* Called the *initialsation vector*
* Ensures that ciphertext is not the same if you encrypt the same message twice

##### Example: CBC - Cipher Block Chaining
* Initialisation vector used in encryption of first block, output is used in encryption of second block etc.
* **C<sub>1</sub> = E<sub>k</sub>(P<sub>1</sub> + IV)** 
* This hides patterns and repetition

##### Example: CTR mode - Counter mode
* Is a stream cipher, each plantext character/block is combined with a keystream character/block generated using the initialisation vector, stream position and key 
* Stream **S<sub>i</sub> = E<sub>k</sub>(IV,i)**
* Ciphertext **C<sub>i</sub> = P<sub>i</sub> + S<sub>i</sub>**

#### Confidentiality
* You do not learn anything about the contents of a message except its length

#### Encryption Scheme - Symmetric
* Message is encrypted with a secret key to get ciphertext
* Ciphertext is decrypted with that same key to get original message
* A **symmetric** encryption scheme contains two algoriithms with **D(k, E(k, m)) = m**
* Without k, c = E(k, m) should tell you nothing about m
* A **block cipher** encrypts blocks of a fixed length
* It needs to be used in a sensible mode of operatoiin in order to encrypt longer messages successfully.

### MACs

#### Integrity
* An "internet" attacker may be able to:
	* Send a message twice
	* Modify the message
	* Block the message
	* Redirect messages ...

#### Active and Passive attacks
* A **passive** attack only observes data
* An **active** attack affects the data e.g. modification oor deletion

#### MAC - Message Authentication Code
* Function M uses fixed length key and variable length meassage to create a fixed length tag
* This tag is computed by the original sender and attached to the end of the message they are sending
* The recipient of the message can then use the same MAC function along with the key to recompute the tag from the message
* If the tag is the same as the one received, the message is not modified; otherwise the tag shows it has been tampered with
* Without the key it is very hard to find M(k, m) for any m

#### Things that are not MACs
* Checksums (used in barcodes, QR codes etc)
* Hash functions (on their own)
* These do not provide integrity as they do not invlove the usage of a key so would be easy for the attacker to recompute
* MACs require a *key*
* On the Microsoft website, downloadable files display hashes of the orginal file that the user can compare against to check if the file they dowload has been corrupted or modified

#### CBC MAC
* This is a type of MAC that utilises CBC encryption
* The MAC is the last block of a CBC of the message where the IV is 0

#### HMAC
* This is a type of MAC that utilises hash functions
* Hash-MAC; two-step hash of message
* 2 sub-keys (inner and outer) are created using the original key
* The inner key is hashed with the original message
* The result of this hash is hashed with the outer key
* The result is the MAC tag

#### Confidentiality and Integroty
* Encryption provides confidentiality
* A MAC provides integrity
* Combining both schemes does not always result in having both confidentiality and integrity

#### How not to do it
* Do not use encrypt **and** MAC 
* MAC does not keep anything confidential - not secure
* Lose confidentiality!
* MAC **then** encrypt is also poor - MAC authenticates message
* Encrypt **then** MAC is best

#### Authenticated Encryption
* Provides both confidentiality and integrity
* Without k, should be hard to:
	* learn anything about m from c = E(k, m)
	* compute E(k, m) for any m

#### AED
* **Authenticated encryption with associated data** 
* Message is confidential
* Integrity for message and for associated data e.g. address
* Uses different modes of operation that do both at once

#### GCM
* GCM = Galois Counter Mode
* Uses CTR and Galois MAC
* Galois MAC authentication tag **t = E<sub>k</sub>(IV,0) + f(C<sub>n</sub> + f(C<sub>n-1</sub> + ... + f(C<sub>1</sub> + f(AD))))**
* Provides AEAD - one block cipher call per block + 1 final
* Standardised
* Encryption and Authentication can be computed independently or in parallel
* Very tricky to implement 

---

## Cryptography part 4 - Public key cryptography

### Key Management

#### Problem
* Needing to have secret key pairs with everyone creates a problem of key management
* Need to meet up with person to get key
* Need to keep track of and store all keys
* Between n people, O(n<sup>2</sup>) keys
* If you need to communicate with someone you have not met before (e.g. website), how do you do it?

### Public-key encryption

#### Padlock analogy
* You have a box with a padlock on it, and the key
* You open the padlock with the key, and send the box to the person you are talking to
* They place the message in the box and lock the padlock, send it back
* You can open the box with your key, which no-one has seen except you
* The person has sent you a locked (encrypted) message without seeing your key
* **Public-key encryption** uses a combination of public and secret keys
* The **public** key is like the padlock
	* used to encrypt messages
	* can be handed out to anyone
* The **secret** key is like the key
	* used to unlock (decrypt) messages
	* never handed out to anyone

#### One way encryption
* Cryptographic keys are long, random looking numbers
* For most systems, easy to compute public key from secret key
* Practically impossible to compute other way (secret from private)

#### Public key cryptography
* Every user has public key (pk) and secret key (sk)
* The public key is published in a directory but the secret key is private
* **pk = f(sk)** (usually)

#### Public key encryption
* The message needing to be sent is encrypted with the public key of the recipient
* The recipient can then use their secret key to decrypt the message
* There are three algorithms involved:
	* **K -> pk,sk**: A key generation algorithm K which gives you the public and secret keys needed  
	* **c = E(pk, m)**: An encryption algorithm using the public key and the message
	* **m = D(sk, c)**: A decryption algorithm using the ciphertext and the secret key
* So if **(pk, sk)** come from **K** then **D(sk, E(pk, m)) = m)**
* There is **no authenticity** with public key encryption - anyone with your key can send you a message

#### Hybrid encryption
* Public key encryption is very slow
* Symmetric encryption is much faster
* Most modern schemes use hybrid encryption:
	* The public key is used to generate a key for a symmetric encryption scheme.
	* The message is encrypted using this scheme
	* The symmetric key is encrypted under the public key encryption scheme, using the public key
	* Both encryptions are sent
	* Then the only the symmetric key needs to be decrypted via the public key encryption scheme, leading to a fast symmetric decryption of the actual message

### PGP
* Public-key encryption invented and patented in 1977
* Algorithm called RSA
* First free public-key encryption called PGP (Pretty Good Privacy) implemented in 1991
* Led to dispute between RSA and the US government, gave up in 1996

#### GPG
* Once PGP legal, went commercial
* GPG (GnuPrivacy Guard) took over as go-to encryption software, used widely (especially on linux)

### Key exchange
* Can two people publicly communicate and agree on a secret key without anyone learning it?
* Diffie, Hellman, 1976: yes

#### Magic Functions
* Need a function that is both:
	* one-way - easy to compute, impossible to invert
	* linear (commutative) - **f(a+b) = f(a) + f(b), f(c.a) = c .f(a)**

#### Diffie-Hellman key exchange
* Both parties have their secret keys, Party 1: a, Party 2: c
* Party 1 sends b = f(a)
* Party 2 sends d = f(c)
* The shared key is both k = a.d and k = c.b
* Derivation: k = a.d = a.f(c) = c.f(a) = c.b
* Anyone listening only has b and d; csnnot derive the key

#### Key derivation
* The shared key k is actaully a hash H(a.d)
* This helps with security 
* Necessary to get a random looking bitstring **k**
* Gives lots of keys for one exchange: **k<sub>i</sub> = H(i, a.d)**

---

## Authentication
* What does it mean for data to be secure?
	* Only those who are supposed to see the data can see it
	* Data is only used for it's intended purpose

### CIA
* Confidentiality
	* Restricted data
	* Only those meant to see it can see it
* Integrity
	* Data is only permitted to be modified by those with the correct clearance
* Availability
	* Data is available to be modified by those people who can

### Authentication
* How do you prove your identity and associated priveleges?
* **Identification** - who is this?
* **Authorisation** - are they allowed to do this?
* **Authentication** = Identification + Authorisation

### Methods of authentication
* Authentication is usually done in three ways, with something about a person that they:
	* Have (passport, ID)
	* Are (biometrics)
	* Know (password)
* Multi-factor authentication is becoming more and more common (e.g password + SMS, biometrics + SMS)

### IDs
* IDs contain much more information than is needed to verify if a person can purchase alcohol, drive etc
* BUT woul dbe very costly to use to track everything people bought

### Online/ Offline
* Online authentication
	* Person can check against their own database to verify
* Offline authentication
	* Hand out a credential
	* Can be stolen/ forged

### Passwords
* A good password should be:
	* Easy for a person to remember 
	* Hard for a computer to guess
* If a site is hacked, often does not matter how good the password is
* Email password is most important to protect as hacker can then use password reset feature on other websites

---

## Cryptography part 5 - Digital Signatures

### Digital Signatures
* For a message to be authenticated between two people, a MAC can be used
* This requires the sharing of a key
* *However*, this does not prove the identity of sender to other people
	* A MAC can be computed by the recipient
* For verifying interactions between multiple people, everyone needs a key for the MAC to work
	* This is not an ideal situation

#### Digital Signature Scheme
* Contains three algorithms:
	* Key pair generation algorithm **K -> (pk,sk)** produces a public a secret key pair
	* Signature algorithm that creates a signature based on the message and the secret key **s = S(m, sk)**
	* Verification algorithm that returns "yes" or "no" after taking in the signature, message and public key of the sender **v = V(pk, m, s)**
* If the verification algorithm returns "yes" then we know that either the message is from the person who owns the secret key, or that the secret key has been compromised
* So if (pk,sk) comes from K then **V(pk, m, S(sk, m)) = "yes"**

#### Digital signatures
* Usually have a fixed size - done by signing a hash of the message
* Is an extra piece of data (attached to end of message)
* Two different messages signed by the same person will have different signatures

#### Use by PGP and GPG
* Used by gnupg.org download page to enable users to verify new downloads of gnupg before installation
* PGP/GPG allow signing of emails as well as encryption
* Encryption only requires public key of the recipient
* Signing requires your own public key - need password to access it

#### What does a digital signature prove?
* It proves one of two things:
	1. the message is from the person who it says it is from, as they are the sole owner of that secret key which matches the public key used to sign 
	2. the secret key of the supposed sender has been compromised
* **Non-repudiation** - if you sign a message with your secret key, anyone with you signature can prove to a third party *with an authenticated copy of your public key* that you signed the message (or your key is compromised)

### PKI - public key infrastructure
* If someone gives you their public key, how can you trust that it belongs to that person?
* If you unsure of the true owner of the key then that is a security vulnerability
* Need a way to authenticate public keys

#### Key authentication
* Public-key cryptography solves the key distribution problem of secret-key cryptography but introduces a new problem of **key authentication**
* Sending a document encrypted under a key you just recieved by email is unsafe as you cannot be sure of the key owner
* A signature by an unauthenticated key proves nothing

#### Getting authenticated public keys
* Need a way of establishing a foundation of trust between key owners:
	* distribute in person
	* publish in a directory
	* chain of trust
	* web of trust 
* **Distributing in person** 
	* Slow
	* Impractical
	* BUT very secure
	* Horrible for actions like visiting a secure website
* **Publish keys in a directory**
	* Shifts trust from person to directory/ website
	* Improves ease of access; publish keys alongside contact info etc
	* If directory hacked, back to square one - need to trust people again

#### Web of trust
* When a key is distributed between two people who trust each other, the person receiving the key signs it to show that they trust it
* Multiple people can sign the same key to show that this person has many people that trust them
* Signing of a key is done between two people; a person can give their friend keys from other people but the friend will only sign the persons key, not anyone elses they are bein given
* If someone tries to place a key on your keyring under another persons name:
	* They cannot sign it as you, because signing requires your secret key which they have no access to
	* They can't include a key without a signature from someone you've already got a key from

* What if a person B creates a key under someone elses name C and signs it, then sends it to you. If you send a message to this third person C, B can read it if they intercept it. So how do you know if you can trust that the key under the name C can be trusted, considering it came from B and only has B's signature?

#### Web of trust (theory)
* One solution to the web of trust problem is to have third-party keys be signed by multiple people before accepting them
* PGP terminology:
	* **ultimate trust** - this is my own key
	* **full trust** - I trust this person to sign other peoples keys
	* **marginal trust** - I trust this persons key, but not for them to sign other's keys
* GPG accepts a key with one full or ultimately trsuted signature, or at least 3 marginally trusted signatures
* Additionally, chains of trust cannot be more than 5 steps long

#### Chain of Trust
* For updates, OS packages, package managers...
	* master public key is distributed with package manager
	* package manager developers sign keys of individual package developers
	* package developers sign packages with their own keys
* Package managers only install packages with a chain of trust from the developers master key
* Usually done via GPG
* A package manager is a piece of software that you install and run - if you use it, then it follows that you trust it
* Package developers have to ask the package manager developer to sign their keys
* Can this be done with websites and browsers? 

---
  
## Securing data at rest

####  Security
* How can we secure a laptop/ server?

* Confidentiality of data:	
	* when not in use
	* against anyone except authorised persons
* Integrity of data
	* detect if your file are modified when you are away
* Availability of data
	* if the CPU melts, can we recover the data?
* If confidentiality is needed for a laptop (in the case of it being stolen) then all data must be encrypted.

### Full disk encryption

#### Linux
* Many distros have full disk encryption as a standard setup option
* Uses LUKS, Linux Unified Key Setup
* Everything on the drive is encrypted except the boot data

#### Windows
* Bitlocker - need a password to unlock a drive
* Used to only be in Windows Ultimate, now available
* requires password AND key-on-chip (or backup)

#### Macintosh
* Filevault - password and key on disk
* Recovery key can be printed out/ stored on cloud

#### Full disk encryption
* Hard to get wrong; nothing user accesible left unencrypted
* easy to use; only user interaction is setup password
BUT
* performance penalty (slower)
* all data available while machine running

### File encryption

#### File-level encryption
* Problem: we need to have a decrypted file to work with
* Not much use storing encrypted + unencrypted files next to each other
	* **zero** security if disk stolen
* Need software that can work with encrypted files

* Office can encrypt individual files for added security
* Early versions had password protection but offered poor or no encryption

#### Encrypted filesytems
**Veracrypt**
* Used for encrypted volumes (files that work like extra drives) 
* One file holds an encrypted container that can be mounted as a drive or folder

##### Linux
* ecryptFS - encrypted home directory - standard option in Ubuntu
* encFS - mount/ unmount in arbitrary directory
* Veracrypt

#### Encrypted filesystems
PROS
* can link to particular user, or only unlock when needed
	* e.g. only unlock when being worked on
* faster than encrypting whole disk

CONS
* more config/ management (harder to work with)
* can make mistakes easily e.g. copy file out of folder

### University Policy
* If *confidential data* is stored on a device **or** accessed from a device, the **whole device** must be encrypted (i.e full disk encryption), regardless of ownership
* UoB confidential data should only be **stored** on UoB machines
* Accessing data through ssh ensures no storage on remote machine

#### Technical matters
* Don't need public key encryption for data at rest
* Symmetric encryption, usually AES
* Need specific mode of operation to allow for random access to encrypted drive

---

## Securing data in transit

### Taking data with you
* File-level encryption is useful for data in transit
	* simplest way to encrypt with a password is with an encrypted archive i.e with 7-Zip
* Containers can be used - encrypt the whole drive

### Data in transit

#### How do I decrypt once I have arrived?
* place a copy of the encryption software on the drive itself

#### Were there confidential files on the stick before?
* deleted files are easy to recover - quick format only removes the index
* full format *may not help* on flash drives - don't take with you if held confidential data previously
* If the data is worth more than a flash drive, you can afford a new one

#### Are you doing 256-bit AES with an 8-bit password?
* If so, may as well not encrypt at all
* One option - password + long random string you keep on your person
* **or** use public-key cryptography if the recipient understands it

* If you ever have previously travelled with confidential data on a laptop and you are travelling with no need for that data, use a *freshly reinstalled* laptop, avoid any problems

### Across the Internet

#### Remote Desktop
* Connect to a Uni computer wherever you are
* Prevents files from being stored on your remote device
* Can use Uni software
* BUT can be slow

#### The Cloud
* Store files in the cloud
* No local files on your person
* BUT **not** encrypted (usually)
* Do NOT use for confidential data
* Exception - Uni-provided Google Drive; separate section from public cloud

### Secure Tunnels

#### Tunnels
* Idea: create a "tunnel" from your computer to another (**end-to-end** security)
* **confidentiality** - no-one else can read the content
* **integrity** - no-one else can modify the content

#### Alternative: point-to-point security
* Bounce data through nodes
* Each travel path between nodes is secure
* Re-encrypted at each node
* PRO: can hide some of the address info along the way
* CON: each node can decrypt

### Man-in-the-middle attack
* A wants to communicate with B
* M intercepts the traffic between A and B
* M passes along the information so that A and B do not suspect anything
* M can be:
	* **passive**: just listens to data; observes; records
	* **active**: modifies, deletes or inserts data

### VPN
* Software that creates a secure tunnel between your computer's ISP/ WAP and a server in a different location
* No one on that path can read or modify the data
* Websites viewing you will see your location as the location of the VPN
* If the website you are visiting is https, a tunnel is created through the VPN; even the VPN cannot read your data

### ssh
* secure shell
* log in to a linux/unix machine from another
* ssh server setup is required on a computer you want to ssh to
* when it is installed, a host key pair is created
* use keys, disable passwords! - more secure

### Chat
* Whatsapp, messenger etc

#### Point-to-point encryption
* Messages go through the messaging app (server?)
* If app manages keys, or both parties have TLS connection to server, not secure
	* the app can (be ordered to) decrypt your messages

#### End-to-end encryption
* Much more secure
* No way for app to read your messages
	* You own the keys
	* app only provides channel for messages
* A communication channel is end-to-end is only the end points of the path can decrypt the data beiing communicated
* Whatsapp, iMessage, Telegram use e2e 

---

## The Internet and how it works

#### Web browsers
* What happens when you type a website address into the address bar?

### Internet Protocol (IP)

#### The Internet (Internet Protocol Layer)
* The internet is basically many devices connected to a network of routers
* Data is sent in chunks (packets) through the network to the destination
* Routers know where to send the packets because they have information headers containing the source and destination adresses, prefixing the data
* Routers direct packets to other routers closer to the destination (fastest path)

#### Security issues with paths
* Data packets are directed dynamically to their destination by routers
	* This means the routers decide in the moment where to send them
	* The path is usually dependent on network traffic and to circumvent broken links etc
* This can be a security issue:
	* Anyone one a path can interfere with data traffic
	* Routers can advertise their speed as faster than it actually is, encouraging more traffic that should'nt be there or trust that router

#### Addresses - IPv4
* Internet Protocol, version 4
* 32-bit addresses e.g. 201.32.4.187
* Gives ~2<sup>32</sup> addresses (~4.29 tr)
* Already run out
* Proposed solution: IPv6, 128-bit addresses

### IP addresses and identification
* Does an IP address identify an individual person/ computer?
* For a server: usually
	* main purpose 
	* BUT ISP or router on the way can serve you anything they want
* For a client: no
	* IP addresses are distributed dynamically and are a shared resource
* IP geofences (this service is not available in your country) can be navigated via use of VPNs 
* **IP addresses do not authenticate anything**

#### The Internet (Internet Protocol Layer)
* gives no guarantee of where packets are going

#### Security issues on the IP layer
* Threats:
	* packets and destinations can be redirected, spoofed, blocked
	* people can spy on traffic
* Who can do this?
	* anyone on the internet - if they manage to get onto a packets path
	* in particular, your ISP/ WAP

### Transmission Control Protocol

#### Network stack
* TCP header 	TCP payload	TCP trailer
	     	\|/           
		 v
* IP header	IP payload 	IP trailer
		\|/
		 v
* link header	link payload 	link trailer

#### The need for TCP
* IP *tries* to get packets from A to B
* Packets can get lost, suffer errors or take different paths and arrive in the wrong order
* TCP over IP gets you **reliable** and **stream-based** communication

#### TCP - sending
* example: string aaabbbccc
* TCP splits into packets: aaa bbb ccc
* packets are given an order/ index so they can be reassembled at the destination
* packets are sent off 

#### TCP - receiving
* TCP receives packets 
* Packets may be in incorrect order
* TCP labelled at origin so can be reassembled

#### TCP - control
* When packets reach their destination, an acknowledgement signal is sent back indicating that the end server/ device has has received it.
* If a packet is sent, and no acknowledgment signal returns specifying that packet has arrived is returned, then the packet gets resent
* Packet loss happens for a number of reasons including getting lost, errors or even interceptions

#### TCP - checksum
* To reduce errors in packet data (tampering, signal loss) checksums are appended to the packets.
* If the receiver of the packet calculates the checksum of the packet data and it is different to the sent checksum, it drops the packet, assuming it is corrupt
* In dropping the packet no ack signal is sent
* Therefore TCP protocol is followed by the sender: no ack signal was received, so packet must be resent.
* Only if the checksum is good will the receiver accept the packet

#### TCP
* TCP header contains:
	* sequence no.
	* checksum
	* source / destination ports
* IP header contains:
	* source address
	* dest address
* IP payload contains TCP header, payload and trailer

#### TCP - security issues
* TCP is a protocol system meant to protect against random errors and failures.
* It is not meant as a security system
* It would be easy for an active attacker to be able to send data packets with forged checksums and payloads if they wanted to - obviously after they have established a way to connect and interfere with the network.
* TCP **does not** provide CIA; it's not a security protocol

### Domain Name System

#### Domain name system
* Computers use IP addresses to find each other
137.222.0.38 -> www.bristol.ac.uk (v4)
2a00:1450:4009:80a::2004 -> www.google.com (v6)
* People find it hard to remember strings of random numbers - websites addresses are used instead
* The first thing a browser does when given a name address is convert it to the numbered IP address
* each section of a domain name separated by a '.' is in a tree hierarchy

#### DNS
* DNS has a directory for addresses and their IP addresses
* DNS can redirect the browser to a different page (e.g. a WAP login page)

#### Security issues with DNS
* DNS was designed for a US military network of trusted participants - there is no security built in
* DNS responses are not normally signed (no authentication)
	* exists DNSSEC as extension for this
* Your access point can spoof DNS responses (common for login redirect)
* DNS cache posioning - returning a fake DNS response pointing to your own server, for a different domain

---

## Internet Security

### ssh

#### Login to remote computer
* Many old ways, no security
	* telnet, rlogin, rsh etc

#### ssh - secure shell
* log into a UNIX/ linux machine from another
* scp - secure copy 
	* copy a file to/from a remote unix machine to your own
* windows has putty for ssh, winscp for copying

#### ssh tunneling
* protocols such as rsync or git can be tunnelled through ssh e.g using git clone for repo with ssh prefix
* cs department has snowy for students/ staff to ssh into 
* ssh mainly used for remote login/admin of web servers. 
* ssh has other uses such as port forwarding and proxying

#### ssh server setup
* The server computer that you want to ssh to needs to be running the ssh server 
* On server setup, a key pair (for the host) is generated
* ssh is very popular and so makes for a popular target for attackers; make sure you have completed secure setup.
* use keys, disable passwords!

#### ssh protocol
* The server signs the DH public parameter with its host key
* The ssh traffic is encrypted and MACed for confidentiality and integrity

#### ssh key authentication
* How do you know if the remote is the real host?
* Never connected before: display host key, ask user
* Connected before, known key: it's fine
* Connected before, changed key: angry warning
	* User must manually delete key from file
	* Often happens when server host reinstalls OS and has to change key

#### ssh client authentication
* ssh supports a number of protocols
	* most common are public key and password
* Public key more scecure - private key is never sent to server, unlike passwords

### Forward Secrecy and ephemeral key exchange

#### Forward secrecy
* If both parties have long-term key pairs, why not to DH exchange with those?
* Problem with long term keys:
	* secret key may get leaked
	* enables anyone to read past messages

#### Ephemeral key exchange
* Solution to long term key pairs: ephemeral key exchange
* Create a new,*temporary* key pair for each session and do DH swap with that.
* Delete eph key pairs as soon as exchange ends.

#### Forward secrecy
* Ann interactive session has forward secrecy if there is no chance of session contents being revealed through party compromise after it has ended.
* The usual way to do this is to create an ephemeral key pair
* The creation of new keys specifically for the session and deletion of the keys afterwards ensures this forward secrecy.

#### Ephemeral key exchange
* Why have long-term keys at all?
* These are for authentication; parties sign the ephemeral keys with their long-term keys
* Without siging the keys could be from anyone

### TLS - the successor to SSL

#### SSL/TLS timeline
* All versions of SSL are broken and banned
* TLS 1.0 is forbidden for credit card data as of July 16
* TLS 1.1 is allowed "with reservations"
* TLS 1.2 is current
* TLS 1.3 is current as of last summer 18

#### TLS layers
* Record layer
* Protocol layer

#### TLS record layer
* secure tunnel
* confidential, authentic
* Encrypted and MACed

#### TLS cipher suites
* Many variations; for TLS 1.2 using Open SSL 1.0.1f, 80 suites
* e.g ECDHE-RSA-AES256-GCM-SHA384
	* ECDHE - key exchange - elliptical curve diffie-hellman ephemeral
	* RSA - signature - public key encryption
	* AES256 - block cipher - Advanced Encryption Standard
	* GCM - mode - galois counter mode
	* SHA384 - hash - secure hash algorithm 384

#### TLS certificates
* Certificate Authority certificates are shipped witth your browser
* These are root CA certificates
* A chain of trust has to be established between the root and the site via an intermediate CA certificate

### Encrypted e-mail
* TLS protects identity of sender and receiver against eavesdroppers
	* PGP -encryoted e-mail cannot hide the to/from fields
* End-to-end encryption (PGP) is apropriate for those 'at risk'

#### PGP
* Biggest problem is usability
* Partial validity/ web of trust
* What if you lose device with key on it?
* Encryption/ decryption requires secure client-side code execution; need correct software like outlook. Harder in web browser

---

## Networks - routers, wireless, mobile

### Routers
* Connect multiple devices to the internet

#### Threats
* You visit a malicious website (Not routers fault)
* Someone on the internet tries to access your machine
* Someone tries to hack into your router
* (If wireless router) someone else tries to join your network without your permission

#### IP and NAT
* Computers communicate using IP addresses
BUT
* IP addresses are running out (poor addressing methods)
* You don't necessarily want every computer in the world to be able to communicate with yours.
* Routers can split the network into two parts
	* Computer/ device + router
	* Router + internet
* The router can then perform Network Address Translation (NAT).

#### Consequences of NAT
1. Ability to have several devices behind one NAT, which has only 1 external IP address
2. Behind the NAT, devices can initiate outbound connections, but the rest of the world cannot initiate connectoins to you - the router would not know which device to forward to.
	* You cannot, without extra set-up, host a server behind an NAT 
3. If your router is secure, you are protected from a lot of incoming attacks
	* The attacks cannot reach your computer.
	* This means an NAT does some of the work of a firewall

### Port forwarding

#### Ports
* On the TCP/ UDP layer, applications use **ports** to distinguish several applications running on the same machine that are sending and receiving data.
* E.g. http uses port 80 (or 443 if using TSL)
* To connect to another machine you need an IP address and a port number.

#### Port forwarding
* NAT is great until you want to be able to play video games online or chat with friends
* One solution is to connect via a server:
	* This is good for setting up a game (matchmaking) but uses up too much bandwith for actual play data.
	* Would rather use peer-to-peer
	* NAT prevents this
* Solution 2: port forwarding
	* Tell the NAT to always allow data through one port (hole in the NAT wall)
	* Choose to expose one or more TCP/UDP ports on your machine to the internet
	* Lets others connect to you
	* Make sure software for using these ports is up to date

#### Dynamic port forwarding
* NAT is very secure, prevents all peer-to-peer
* Port forwarding is less secure, but enables games/ chat
* **UPnP** (universal Plug n Play) or dynamic port forwarding allows ports to be opened only when a game or application is running. (i.e. only when it is needed)
* *However* UPnP is know for being unsecure; no authentication
	* Malware can look for it on your machine and use it for its own purposes.

#### NAT types
* Open (type 1)- No NAT, or port forwarding
* Moderate (type 2)- NAT, Dynamic port forwarding
* Strict (type 3)- NAT, no port forwarding
* If type addition between two people is more than 4, no chat available

#### Advice
* Keep PC/laptop behind NAT unless required.
* Static port forwarding is probably safer than UPnP
* As long as nothing on your machine is using the forwarded ports, risk is minimal

### Wi-fi
* When on wifi, threats can come from most places

#### Threats
* People on the internet try to access your machine
* People on your network try to access your machine
* When you connect to a malicious website (not routers fault)
* If the router itself is malicious

#### Malicious actors
* If the wifi traffic is not encrypted, other users on the network can see it.
* Lots of networks (esp. free ones) allow you access in return for tracking and using your data 
* TLS prevents anyone from seeing the contents of your traffic or from taking over sessions, but does NOT hide the websites you are visiting.
* For maximum security, use VPN

### Wi-fi encryption

#### Encryption
* **Unencrypted** wifi (even if need to register) = NO security against other users
* **Encrypted** wifi requires a password or key before you connect, not a landing page afterwards.
	* coffee shops can do this (password on menu etc)
	* BUT public access points (like The Cloud) cannot
* **WEP** (wired equivalence privacy):
	* same key for all devices (no protection against other users on network)
	* rubbish encryption
	* withdrawn officially 2004
* WEP is broken
* **WPA** (Wi-fi protected access)
	* replaces WEP
	* can use one password, or different user accounts
	* derives a different key for each device
	* also broken
* Current standard is WPA2, with WPA3 coming
* VPN still best security (if you trust it)

### Mobile

#### Security model
* "Same as landline"
* Government can see who you are talking to etc (everything)
* Can track location by seeing what base stations you are connecting to

#### Privacy
* Every phone has an IMEI (International Mobile Equipment Identifier)
* This lets networks detect and block stolen phones
* Every SIM card has an IMSI (International Mobile Subscriber Identity)
* For privacy reasons, base station assign a TMSI (temporary IMSI) when it connects
* In theory, ensures privacy as TMSI does not identify a user
	* In practice, base station can easily send signal requesting IMSI

#### IMSI catchers
* **IMSI catchers** or "stingrays" are mitm devices that simulate a base station
* They can request an IMSI just like a base station
* According to Sonwden, can be used to send a fake shutdown signal to phone in order to still be able to record

#### Baseband processor
* A **SIM** card has a microprocessor of its own
* A phone typically has a baseband (co)processor to manage network traffic. Has own RAM and operating system
	* Seems to be under complete control of network operator

#### IMSI catchers
* Enable interception and surveillance without a warrant
* Enable mass surveillance

#### Conclusion
* Mobile phones (inc smartphones) do not offer any meaningful security against network threats.
* Everyone can be "on the network" with build-your-own software-defined radios
* Techinical abilities to turn a phone into a spying device exist, but governemnt/ police right to do this is questionable

---

## Malware

#### Advice
Don't
	* click on links
	* open attachments in emails
	* download files
	* install programs
	* visit insecure websites
* (Within reason)

#### Good or bad files - what are safe?
* Many different file types can contain malware
* **PDF**
	* can contain both embedded JavaScript and malformed content that exploits the software showing it
	* Adobe is doing what they can to secure Acrobat reader
	* Try and keep software up to date, usually warns about malicious activity
* **Office docs**
	* Can contain macros
	* Huge problem around Windows 95
	* Window now has default secure setting, so macro problems have almost disappeared
* By default, Windows does not show fie extensions
* File icon comes from within file so executable can be made to look like text file

#### Conclusion
* Cannot trust anyonen or anything
* Files can be made to look like files they are not
* Many file types can contain malware
* Almost impossible to be 100% secure
* Its about lowering risk
* Acceptable risk level depends on nature of files and people involved

### Types of malware

#### Virus
* Malicious program that infects host files such as:
	*  executables (programs)
	*  office docs
	*  other parts of the computer (e.g  boot sector)
* example: Melissa (1999), word macro virus
	* Infects word docs
	* Uses scipting features to send itself to everyone in address book (assuming usage of outlook)
	* estimated 1 in 5 computers infected

#### Worm
* Malicious program that can spread itself on a network
* Can rely on certain programs to spread (e.g MS word) but does not infect them
* example: SQL slammer 2003
	* Bank of america ATM network shutdown within 15 mins
	* general internet slowdown
	* only affected unpatched machines

#### Ransomware
* software that encrypts a users files and demands payment in order to unlock them
* usually requests cryptocurrency or other currency that is hard to trace
* e.g. cryptolocker 2013-14
	* 3-4 million dollar gains
	* Just needs read/ write acces to files

#### Scams
* Presents a non-existent problem to user in order to extract money from them
* e.g. browser shows Windows error, provides phone number to call for help
* targeted at those people who are unfamiliar with computers

#### Adware
* software that injects or modifies adverts when you are browsing the web or playing a game
* advertising is a very common way to make money on the internet these days
* if a program manages to get a companies adverts on someones commputer they can earn money

#### VLC
* Original VLC was good media player, no malware installed
* Search up now, many downloadable results
* Most of them will not be original VLC, will contain malware

#### Superfish
* Adware hard to avoid if it comes pre-installed on your system
* Superfish installed by lenovo onto some devices
* Injected ads
* Circumventes certificate checking TLS protocol

#### Terminology
* Classification between malware is mostly academic
	* many attacks are combinatons of attack types
	* trojan, virus, worm, etc
* distinction between anti-malware and anti-virus mostly ploy; software that protects against only one type of attack is not much use

### What can we do about malware?

#### Rule \#1
* Back up your data
* Make at least 3 copies of everything important
* If you get infected, have the ability to reinstall and restore everything from the ground up

#### Rule \#2
* Keep your software up to date
* You will only be protected from modern attack methods if your software is as good as it can be
* Often it is unpatched and out of date machines that get attacked.

#### Rule \#3
* Constant vigilance
* Impossible to be 100% secure without distrusting everything you get sent (not much point)
* BUT if something doesnt seem right, carefully investigate

#### Recommendation
* Use adblocker (uBlock origin)
* Ads are a common infection vector, reducing exposure increases security
* Often adblocking more effective at preventing succesful attacks than anti-malware software.

---

## Phishing

#### Principle
* Go for the weakest point in a system when attacking
* This is often a person

### Examples

#### An e-mail
* May be an email telling the user to click on a link to complete an account-related action
* Email will pretend to be a known figure/ organisation of authority
* How to recognise:
	* sender address will not be from the organisation/ authority being imitated
	* clickable links will be to strange site address (hover to check)

#### Phishing
* In these email cases the links can be modified so that hovering to show the address also shows a fake address
* The landing pages will try to imitate the actual sign-in page of the true site
	* Often very hard to distinguish from real site

### Defences

#### Phishing
* **Phishing**: tricking people into handing over confidential info (usually username/ password)
* **Spear phishing**: Tailoring phishing attacks to target a particular person
* **Whaling**: Going after influential/ powerful members of a company such as CEO or other executives/ VIPs

#### Phishing sites
* Usually short-lived (few days until taken down)
* Can be profitable if attracts enough victims before taken down
* Often hosted:
	* on legitimate sites, by hacking into site
	* on self-hosting/ cloud providers
	* on domains bought with stolen cards

#### Safe browsing
* Google service integrated into Chrome and Firefox
* Checks url against downloaded blacklist (on own machine) - regularly updated

#### Not a good idea
* DO NOT assume you can type in your actual password and fake site will not know what to do with it
* Often phishing sites are **man-in-the-middle** attacks, will send data through to actual site to check if credentials are correct

#### A good idea?
* If a site looks 'dodgy' then avoid it
* BUT many phishing sites will look authentic
* Good ones will look exactly the same.

#### General advice
* If an emaiil has a link to a site where you have an account, go to site via browser
* If email is true, site will tell you if there is action to be taken
* HOWEVER many legitimate business use links in emails to reset passwords

* Never use a PIN number online - never needed for online transactions.
* Never give out password/ letters of password over the phone (unless service actually uses this)

### Psychology of phishing

#### Why does it work?
* People are generally taught to be kind and hekpful - this can be easy to exploit
* Employees often want to do their job well, and so will avoid going through correct security protocol to help someone
* People follow "common practice" not theory

#### Marketing, deception, pretexting
* **Marketing**: convincing people to do/ think things, spreading selective truths etc
* **Deception**: spreading of untrue information
* **Pretexting**: giving a reason to justify something when it is not the real reason.

#### Demographics
* Some groups of peeple ar emore likely to fall for scams than others
* Obviously those with less computing experience
* People who are 'agreeable' - more likely to do things for others
* Nigerian Prince scams - filter out people who are unlikely to complete whole scam process


