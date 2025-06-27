# ANNEX B - Zero Knowledge Proofs for the Age Verification Solution

# B.1. Introduction 

Zero-knowledge proofs (ZKPs) are cryptographic techniques that enable one party (the 
User) to prove to another party (the Relying Party) that a statement is true without 
disclosing any additional information beyond the statement’s validity. This allows 
the User to demonstrate knowledge without revealing the knowledge itself.

For example, when the User needs to verify that they meet the required minimum age, 
the Age Verification App could utilise the Proof of Age attestation to generate a 
proof confirming that the User meets the required age threshold (e.g., 18 years) 
without disclosing their exact age or any other personal details. The Relying Party 
would receive and verify the proof. If valid, the Relying Party could be certain that 
the User meets the age requirement without gaining any further information. 
Additionally, a ZKP-based approach enhances privacy by ensuring unlinkability, making 
it computationally infeasible for the Relying Party to associate multiple proofs with 
the same individual. 

The Age Verification Solution will provide as an experimental feature the  ZKP scheme 
described in \[Fri2024\] and analyzed in the next sections.

## B.1.1. Audience Target audience/Usage

The intended audience for this document comprises Member States and designated 
organizations that seek to support ZKP in an Age Verification App and Relying 
Parties. 

## B.1.2. Terminology

This document uses the terms Attestation Providers (APs), Age Verification App 
Instances (AVIs), Relying Parties (RPs), and Proof of Age attestation as defined in [Operational, Security, Product, and Architecture Specifications](https://github.com/eu-digital-identity-wallet/av-doc-technical-specification-priv/blob/release/docs/architecture-and-technical-specifications.md).

## Β.2 Anonymous credentials from ECDSA 

Anonymous credentials based on ECDSA are presented in \[Fri2024\]. This approach 
builds upon the Ligero protocol \[Ame2017\].

### Β.2.1 Setup

This solution does not require a trusted setup phase. However, the arithmetic circuit 
used to perform the necessary cryptographic computations within the AVI must be 
carefully designed, implemented, and distributed. This circuit receives a secret 
input, referred to as the witness, as well as a public statement. The circuit 
performs a calculation and outputs true if certain conditions hold. An AVI can then 
generate a ZKP which proves that “the AVI knows a witness, which when provided as 
input to a certain circuit using the provided statement, the circuit outputs true”. A 
circuit for the Age Verification solution would have the following configuration:

**Witness**: A Proof of Age attestation

**Public parameters**: The public key of the AP, an attribute, a nonce

**Output**: The circuit outputs true if the following conditions hold:

* The Proof of Age attestation includes a signature that can be verified using the public key of the AP  
* The Proof of Age attestation includes the attribute with value set to True  
* The AVI can generate a signature of the nonce that can be verified using the public key included in the Proof of attestation  
* The Proof of Age attestation is within its validity period.

 

### Β.2.2 Issuance

An AP remains unaware of the use of this scheme; therefore, no changes are required 
to the existing issuance process.

 

### Β.2.3 Presentation

To generate a zero-knowledge proof for a Proof of Age attestation, the AVI first 
encodes the attestation as private inputs (i.e., witnesses) to a suitable arithmetic 
circuit that represents the desired statement. The circuit also defines any public 
inputs, such as the public key of the AP. The AVI then runs the zkSNARK prover 
algorithm over the circuit using the witness and public inputs, producing a succinct 
proof. This proof, which can be verified by any third party using the public inputs, 
is sent to the RP.

## Β.2.4 Current status

The authors of \[Fri2024\] have provided a private, beta implementation of the 
proposed solution in C++ (the repository can be found [here](https://github.com/google/longfellow-zk)). 
Additionally, the authors have submitted an individual draft to IETF (it can be found [here](https://datatracker.ietf.org/doc/draft-google-cfrg-libzk/)). 
The solution has not been peer-reviewed. Reportedly, Google has integrated this 
solution in their Wallet (announcement [here](https://blog.google/products/google-pay/google-wallet-age-identity-verifications/)) and it will be used by the online dating app [Bumble](https://bumble.com/)  

# Β.3. Other schemes

Five options were evaluated for the implementation of the ZKP to the age verification 
solution.

* BBS+ \[BBS2004\]  
* BBS+ with support for ECDSA proof of possession \[Her2021\] \[Cel2024\]  
* Pairing-free BBS+ \[Des2025\]\[Api2025\]  
* ECDSA Anonymous Credentials \[Fri2024\]  
* Crescent \[Paq2024\]

Additionally, we setup the following high-level requirements:

| ID | Requirement | Rational |
| :---- | :---- | :---- |
| Req01 | A ZKP scheme SHALL provide support for privacy-preserving proof of possession of a Proof of Age attestation by proving knowledge of a private key that corresponds to an ECDSA signature  | An AVI must be enabled to prove, in a privacy-preserving manner, that it possesses the correct private key. To utilize the cryptographic modules provided by mobile platforms, support for ECDSA signatures is necessary.  |
| Req02 | A ZKP scheme SHALL be peer-reviewed by the relevant scientific community and SHOULD rely solely on algorithms standardised by a standardisation organisation recognised by the Commission or in a standard recognised by the Commission. | Although standardisation is desirable and relevant efforts are currently underway, the availability of a suitable standard is not expected in the near term. |
| Req03 | A ZKP scheme SHALL introduce as minimum disruptions to existing infrastructure as possible.  | The Age Verification solution prioritizes a rapid time-to-market strategy by leveraging existing infrastructure wherever feasible. This approach minimizes integration complexity, reduces deployment overhead, and facilitates alignment with current systems and standards. As such, the use of ZKPs shall be as much compatible as possible with established identity frameworks and require minimal changes to existing architectures that may be used by APs and RPs |
| Req04 | A ZKP scheme MAY support the ability to prove that a Proof of Age attestation remains within its designated validity period | A fine-grained validity period in a Proof of Age attestation can serve as a tracking vector. A ZKP scheme may be used to conceal the validity period, but alternative mechanisms are recommended |

The following table shows which requirements are satisfied by each solution. 

|  | Req1 | Req2 | Req3 | Req4 |
| :---- | :---- | :---- | :---- | :---- |
| BBS+ | ❌ | ✅ | ❌ | ⚠️ |
| BBS+ with support for ECDSA proof of possession | ✅ | ✅ | ❌ | ⚠️ |
| Pairing-free BBS+ | ✅ | ✅ | ❌ | ⚠️ |
| ECDSA Anonymous Credentials | ✅ | ⚠️ | ✅ | ✅ |
| Crescent | ✅ | ⚠️ | ✅ | ✅ |

Among them, ECDSA Anonymous Credentials appears the most promising due to its compatibility with existing credential formats and issuance flows

# References

\[Ame2017\]	Scott Ames, Carmit Hazay, Yuval Ishai, Muthuramakrishnan Venkitasubramaniam, "Ligero: Lightweight Sublinear Arguments Without a Trusted Setup", in ACM CCS 2017

\[Api2025\]	Rutchathon Chairattana-Apirom, Franklin Harding, Anna Lysyanskaya, and Stefano Tessaro, "Server-Aided Anonymous Credentials," available at [https://eprint.iacr.org/2025/513](https://eprint.iacr.org/2025/513), 2025

\[Bar2016\]	Amira Barki, Solenn Brunet, Nicolas Desmoulins, and Jacques Traor´e, "Improved algebraic MACs and practical keyed-verification anonymous credentials," In Roberto Avanzi and Howard M. Heys, editors, SAC 2016, volume 10532 of LNCS, pages 360–380. Springer, Cham, August 2016

\[BBT2025\]	Trust Model : Securing digital identity with advanced cryptographic algorithms, available at [https://github.com/Orange-OpenSource/BBS-SHARP-doc-eudi-wallet](https://github.com/Orange-OpenSource/BBS-SHARP-doc-eudi-wallet) , 2025

\[Her2021\]	Armando Faz-Hernández, Watson Ladd, and Deepak Maram, “ZKAttest: Ring and Group Signatures for Existing ECDSA Keys,” available at [https://eprint.iacr.org/2021/1183](https://eprint.iacr.org/2021/1183)

\[Cel2024\]	Sofia Celi, Shai Levin, and Joe Rowell, "CDLS: proving knowledge of committed discrete logarithms with soundness," Progress in Cryptology – AFRICACRYPT 2024

\[Cha2020\]	M Chase, T Perrin, G Zaverucha "The Signal Private Group System and Anonymous Credentials Supporting Efficient Verifiable Encryption." In ACM CCS 2020

\[Des2025\]	Nicolas Desmoulins, Antoine Dumanois, Seyni Kane, and Jacques Traoré, “Making BBS Anonymous Credentials eIDAS 2.0 Compliant”, Cryptology ePrint Archive, Paper 2025/619, 2025, available at [https://eprint.iacr.org/2025/619](https://eprint.iacr.org/2025/619)

\[Fri2024\]	Matteo Frigo and abhi shelat, Anonymous credentials from ECDSA, Cryptology ePrint Archive, Paper 2024/2010, 2024, available at [https://eprint.iacr.org/2024/2010](https://eprint.iacr.org/2024/2010)

\[Gro2016\]	Jens Groth, “On the Size of Pairing-Based Non-Interactive Arguments”, in EUROCRYPT 2016

\[Kal2022\]	V Kalos, GC Polyzos, "Requirements and Secure Serialization for Selective Disclosure Verifiable Credentials", in IFIP SEC 2022

\[Loo2025\]	Tobias Looker, Vasilis Kalos, Andrew Whitehead and Mike Lodder, "The BBS Signature Scheme," available at [https://datatracker.ietf.org/doc/draft-irtf-cfrg-bbs-signatures/](https://datatracker.ietf.org/doc/draft-irtf-cfrg-bbs-signatures/), 2025

\[Orr2024\]	Michele Orrù, Stefano Tessaro, Greg Zaverucha, Chenzhi Zhu, "Oblivious issuance of proofs", In Annual International Cryptology Conference, 2024

\[Paq2024\]	Christian Paquin, Guru-Vamsi Policharla, and Greg Zaverucha, "Crescent: Stronger Privacy for Existing Credentials, Cryptology ePrint Archive, Paper 2024/2013, 2024, available at [https://eprint.iacr.org/2024/2013](https://eprint.iacr.org/2024/2013)

\[Tes2023\]	Tessaro, S. and C. Zhu, "Revisiting BBS Signatures", In EUROCRYPT, 2023

\[Woo2025\]	Anna P. Y. Woo, Alex Ozdemir, Chad Sharp, Thomas Pornin, and Paul Grubbs, "Efficient proofs of possession for legacy signature,". IEEE Security and Privacy, 2025

