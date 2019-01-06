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

