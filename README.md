# Cryptography Dictionary

_Here lies a compiled (ongoing) dictionary of things related to Cryptography, feel free to contribute!_

[//]:"big-credit-to:Jean_Philippe_Aumasson"

#### 2013

The year of Edward Snowden’s leaks about the NSA’s classified activities, a turning point in cryptography. End-to-end encryption suddenly becomes an appealing topic.

#### 65537

The most common RSA public exponent; large enough to not be insecure, small enough to make exponentiation fast, and of a form that optimizes implementations’ speed (65537 = 2^16 + 1).

#### A5/0

One of the three encryption modes in early mobile telephony standards (GSM). A5/0 just means no encryption; therefore, the audio content from a mobile call would be received and transmitted in the clear between a mobile device and the nearest base station. It’s as secure as early TLS versions’ null cipher.

#### A5/1

The default GSM cipher in Western countries (prior to 3G and 4G technologies) that encrypts encoded audio mobile communications.
A stream cipher based on a curious mechanism involving three linear feedback shift registers irregularly clocked; so the update of a register depends on the values of certain bits in the two other registers. Sophisticated cryptanalytic attacks have broken A5/1. But in practice, the most effective attack is relatively simple: it’s a time-memory trade-off that exploits the short state of A5/1 (64 bits) and involves the precomputation of large rainbow tables. The A5/1 specification was initially confidential and unavailable to the public until it was reverse engineered in the late 1990s.

#### A5/2

The export version of A5/1, a euphemism meaning its technical requirements include something like must be easy to break by Western nations’ intelligence agencies. Designed to be insecure, A5/2 didn’t turn out to be outrageously insecure: after being reverse engineered around the same time as A5/1, academic researchers quickly found attacks on A5/2. But these attacks were more efficient on paper than in practice.

#### A5/3

At last, a real cipher in mobile phones. An upgrade to the do-it-yourself A5/1 that applies an algorithm already public and vetted, namely the block cipher KASUMI. KASUMI was used in 2G (along with A5/1), in 3G as A5/4 (along with SNOW 3G), and was no longer supported in 4G.
A See [KASUMI](https://en.wikipedia.org/wiki/KASUMI).

#### A5/4

A5/3 but with a key of 128 bits instead of 64 bits; A5/4 is secure, whereas A5/3 isn’t.

#### Adaptive attack

An attack in which the attacker’s actions depend on what they observe during the attack and the protocol’s execution. For example, in an adaptive chosen-plaintext attack, the attacker sends plaintext messages that depend on the responses to their previous queries. In a nonadaptive attack, the list of plaintexts for which the attacker queries the ciphertexts must be predetermined.

#### AEAD (authenticated encryption with associated data)

A type of _symmetric cipher_ that encrypts and authenticates data by producing a ciphertext as well as an authentication tag. The decryption step then only succeeds if the tag is valid, which proves that the ciphertext was created by someone who knows the key. To validate the tag, the receiving end generally computes it from the encrypted message and verifies that the computed value is identical to the one received.

The _AD_ in **AEAD** refers to associated data—also called additional or auxiliary— or data that isn’t encrypted but is covered by the authentication mechanism, because it’s taken as an input when computing the authentication tag.
AEAD can be realized in three ways:

- _With an established cipher and established MAC, such as AES in CBC mode as a cipher and HMAC-SHA-256 as a MAC. This is the traditional approach that is usually the least efficient. It’s also the most error prone because of the different ways to combine a cipher and a MAC (so-called encrypt-and-MAC, encrypt-then-MAC, and MAC-then-encrypt)._

- _With an established cipher and ad hoc MAC or mode, such as AES-GCM and ChaCha20-Poly1305, which are currently the most popular AEADs of this type. For example, TLS 1.3. AES-SIV also belongs in this category, although it’s a bit different (a MAC-and-encrypt rather than encrypt-then-MAC construction)._

- _With a custom 2-in-1 construction, such as **ACORN** and **AEGIS**, both ciphers selected by the **CAESAR** competition. In these algorithms the same internal state and logic encrypts and generates the authentication tag._

#### AES (Advanced Encryption Standard)

The ubiquitous block cipher standardized by [NIST](https://www.nist.gov/) in 2000. Designed by Belgian cryptographers [Joan Daemen](https://en.wikipedia.org/wiki/Joan_Daemen) and [Vincent Rijmen](https://en.wikipedia.org/wiki/Vincent_Rijmen), and winner of the AES competition, its use is universal today under its various modes of operation, such as **CBC**, **GCM**, and **SIV**.
_See [Rijndael](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)_

#### AES-CCM

AES in counter-with-CBC-MAC mode, which combines the CTR encryption mode with CBC-MAC authentication. AES-CCM is a NIST standard and is supported in TLS 1.3 and several other protocols, including Bluetooth Low Energy. But it’s much less popular than its sibling AES-GCM. The reason is that AES-CCM is generally slower and less convenient to use than AES-GCM. A research paper titled “A Critique of CCM” describes the limitations of the CCM mode. AES-CCM sometimes fits better than AES-GCM in embedded platforms because it only needs an AES algorithm and no additional logic (unlike GCM’s GMAC).

#### AES-GCM

AES in Galois counter mode, the most common authenticated encryption primitive at the time of writing. Also the primitive that ended the reign of HMAC authenticators. In GCM mode, a message is encrypted in CTR mode and the Galois MAC (GMAC, aka GHASH) generates an authentication tag from the ciphertext and associated data blocks. The carry-less multiplication instruction PCLMULQDQ was introduced in Intel CPUs in the 2010s to speed up GMAC computations.

##### _AES-GCM's Achilles' heel_

AES-GCM is secure except when called twice with the same nonce on distinct messages, which can leak plaintext data and reveal the authentication key GMAC uses. This unfortunate fragility hasn’t stopped AES-GCM from being used in countless protocols (including TLS, IPSec, and SSH) and from
being standardized by IEEE, ISO, and NIST. AES-GCM’s cousin AES-GCM-SIV eliminates the nonce-reuse problem but isn’t yet widely supported and is a bit slower.

#### AES-GCM-SIV

A variant of AES-GCM where the encryption nonce is determined from the tag computed by authenticating the plaintext (and any associated data). AES-GCM-SIV’s MAC, called POLYVAL, is slightly different from GCM’s GMAC: whereas AES-GCM is of the encrypt-then-MAC form, AES-GCM-SIV is a MAC-and-encrypt construction. The main benefit of AES-GCM-SIV compared to AES-GCM is that the former remains secure if a same nonce is reused—a property called misuse resistance.

#### AES-NI

Officially AES New Instructions but often called native instructions, which might be a better term. AES-NIs are CPU instructions that compute AES using hardware logic in the chip’s silicon as opposed to a combination of arithmetic operations using the chip’s ALU. When introduced by Intel in 2008, AES-NIs made AES software about 10 times faster, and as a by-product, immune to cache-timing attacks.

#### AIM (Advanced INFOSEC Machine)

A chipset designed by Motorola in the late 1990s that includes separate FPGAs for red and black operations. Pompously advertised as one of the most revolutionary advances in cryptography, ever. The NSA uses it to protect classified and sensitive national security information. The off-the-shelf AIM didn’t include classified (Suite A) algorithms, but users could program the FPGAs to support algorithms, such as ACCORDION or BATON.

#### AKA

In 3GPP standards parlance, the authenticated key agreement operation between users of a cellular network and the user’s home network, which might be different from the serving network. 

The AKA is very similar in 3G UMTS, 4G LTE, and 5G standards, and unlike many other key agreement protocols doesn’t use public key primitives; instead, it relies on a shared symmetric key and pseudorandom functions (except in
5G where public-key encryption is added).

The AKA looks like a straightforward protocol, taking a master key and
a sequence number to derive session keys (encryption, authentication, and anonymity keys) while ensuring mutual authentication, or more precisely mutual knowledge of the shared key. But despite its cryptographic boringness, AKA aims to achieve other, nontrivial goals specific to its unique environment. These goals include confidentiality of the user identity (IMSI), unlinkability of the user, authentication of the serving network (by the user and home network), and strong unreliability guarantees and resilience to unsafe pseudorandom generators.

#### AKS (Agrael-Kayal-Saxena)

The first deterministic primality test, as opposed to randomized ones. The 2002 research paper presenting the AKS algorithm, “PRIMES is
in P,” was the first proof that the problem of primality testing is in the
P complexity class, or the class of problems for which a nonrandomized polynomial-time algorithm exists.

_See [PRIMES](https://annals.math.princeton.edu/2004/160-2/p12)_

#### Algebraic cryptanalysis

A form of cryptanalysis where the target problem (typically key recovery, but also forgery, distinguishing, and so on) is modeled as a system of multivariate equations to which a solution is found by generic or ad hoc techniques. Algebraic cryptanalysis has been used to attack symmetric and asymmetric cryptosystems. An example target is stream ciphers based on feedback shift registers with low algebraic degree logic, giving rise to underlying equations exploitable by algebraic attacks.

_See [Grobner basis](https://en.wikipedia.org/wiki/Gr%C3%B6bner_basis)_

#### Alice

Bob’s partner in crime, but who never met Bob in person. According to their official biography in John Gordon’s 1984 speech: “Alice and Bob have tried to defraud insurance companies, they’ve played poker for high stakes by mail, and they’ve exchanged secret messages over tapped telephones. (. . .) Alice and Bob have very powerful enemies. One of their enemies is the Tax Authority. Another is the Secret Police. This is a pity, since their favorite topics of discussion are tax frauds and overthrowing the government.”

_See [Bob](https://en.wikipedia.org/wiki/Alice_and_Bob)_

#### All-or-nothing transform (AONT)

A reversible transformation where you need every bit of the output to determine any bit of the input. When an encryption scheme is an AONT, the decryption key is useless to determine the plaintext if you miss some bits of the ciphertext (unless the missing bits are so few that they can be brute-forced). The OAEP construction used for RSA encryption is an example of AONT. CBC or GCM encryption modes aren’t AONTs.

#### Anonymous signature

A signature that doesn’t reveal the identity (public key) of the signer and therefore needs some interaction with the signer to verify it. It implies invisibility.

_See [Invisible signature](https://support.notarius.com/en/help/kb/visible-vs-invisible-signature/)_

#### Applied Cryptography

The 1996 book by Bruce Schneier that has been the main reference in the field for many years; it introduced many students and engineers to cryptography. Famous for its opening paragraph: “There are two kinds of cryptography in this world: cryptography that will stop your kid sister from reading your files, and cryptography that will stop major governments from reading your files. This book is about the latter.”
Inevitably outdated 25 years after its publication, Applied Cryptography is still worth keeping on your shelf as long as you don’t blindly follow all
of its recommendations. It’s also much less outdated than Schneier’s two prior books, E‐Mail Security and Protect Your Macintosh.

The part of cryptography that emphasizes direct applications. In contrast, theoretical cryptography is less about engineering and more about fundamental understanding and analysis. The term applied is deceiving; both applied and theoretical cryptography can (and ought to?) be equally relevant to real applications.

#### ARC4

The original name of the RC4 stream cipher; also written as ARCFOUR. Before the reverse engineered RC4 was confirmed to be the actual RC4, it was prudently referred to as alleged RC4, which was shortened to ARC4.

#### Argon2

A password hashing function developed during the Password Hashing Competition. Also, a de facto standard for processing passwords or any low-entropy secret to derive cryptographic keys or store a verifier in a way that prevents efficient cracking using GPUs, FPGAs, dedicated hardware, precomputed tables, or side-channel attacks.
Unlike PBKDF2, Argon2 can enforce the use of a certain amount of memory in addition to a configurable number of iterations. Unlike bcrypt, this amount of memory can be an arbitrary value rather than fixed. Unlike scrypt and the two others, Argon2 offers a user-friendly interface to easily pick time and memory parameters. It’s also a simple design that only uses the hash function BLAKE2 internally rather than a combination of all the cryptography ever designed.

_See [bcrypt](https://dev.to/jyeett/what-is-bcrypt-and-why-2dd1), [scrypt](https://en.wikipedia.org/wiki/Scrypt), [PBKDF2](https://en.wikipedia.org/wiki/PBKDF2) (Password-Based Key Derivation Function 2)._

#### ARX (Add-Rotate-XOR)

An abbreviation that denotes cryptographic algorithms only doing integer additions, word bit shifts or rotations, and XORs (as opposed to, for example, algorithms using S-boxes). It was coined by cryptography and security researcher Ralf-Philipp Weinmann in 2009.

#### ASIACRYPT

Asia’s top academic cryptography conference, held every autumn in a different location in the Asia-Pacific region since 1990. The only IACR conference that includes IACR as a substring of its name. Researchers present peer-reviewed research papers with titles such as “StructurePreserving and Re-Randomizable RCCA-Secure Public Key Encryption and Its Applications” and “Cryptanalysis of GSM Encryption in 2G/3G Networks Without Rainbow Tables.”

_See [CHES](https://ches.iacr.org/), [CRYPTO](https://crypto.iacr.org/), [Eurocrypt](https://eurocrypt.iacr.org/), [FSE](https://fse.iacr.org/), [PKC](https://pkc.iacr.org/), [Real World Crypto](https://rwc.iacr.org/), [TCC](https://tcc.iacr.org/)._

#### Attack

In the context of cryptanalysis, the demonstration of a technique, described as an algorithm, that violates a security claim made by the designers of the primitive or protocol attacked.

#### Attribute-based encryption (ABE)

A generalization of identity-based encryption from one attribute (the identity) to more than one. It allows you to encrypt a message not to a given recipient, but to a set of attributes in such a way that only parties satisfying a valid combination of attributes can decrypt the message. ABE sounds powerful but hasn’t found many real applications. Allegedly, the reason is due to its relatively complex construction (using elliptic-curve pairings) and the need for a trusted third party (holding the master key needed to generate private keys).

_See [Identity based encryption](https://en.wikipedia.org/wiki/Identity-based_encryption)._

##### ABE for access control

ABE is sometimes pitched as suitable for fine-grained access control. For example, you can imagine an organization deploying ABE to encrypt sensitive documents by using attributes such as department, clearance level, and project to cryptographically enforce multilevel security and role-based access. For instance, a document might be encrypted by using a public key with the attributes {department=ENGINEERING, clearance=SECRET, project=LABRADOR}.

ABE could then guarantee that if you satisfy only two of these attributes (say, your department is ENGINEERING and your clearance is SECRET but you work on the project HUSKY), you won’t be able to decrypt the document. More interestingly, ABE guarantees that you won’t be able to decrypt the document by colluding with someone on the project LABRADOR in the ENGINEERING department if they only have CONFIDENTIAL clearance.

#### Axolotl

The original name of the Signal application’s end-to-end messaging protocol.

_See [Signal protocol](https://en.wikipedia.org/wiki/Signal_Protocol)_


