# ANNEX 2 - High-Level Requirements <!-- omit from toc -->

This Section presents high-level technical requirements addressed at interactions between the different roles within the technical framework.

References to the roles are provided using acronyms listed in the table below.

| **Role acronym** | **Role** |
|-----------|-----------|
| AVI	| Age Verification App Instance |
| AVAP | 	Age Verification App Provider |
| AP	| Attestation Provider |
| RP	| Relying Party (Proof of Age attestation verifier) |
| U	| User | 
| WB	| Web Browser (or web app)|

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED",  "MAY", and "OPTIONAL" in this Section are to be interpreted as described in RFC 2119.

The technical requirements will be further refined in the upcoming versions of the document and MAY break backwards compatibility with present choices. Key aims are ease of implementation, in short time frame maintaining compatibility to [[ARF 1.8](https://github.com/eu-digital-identity-wallet/eudi-doc-architecture-and-reference-framework/releases/tag/v1.8.0)].

## A.2.1	Provision of the Age Verification App Instance (AVI, AVAP)

The User should be able to download and install the age verification app from their App Store. Depending on the Age Verification App Instance, the user may need to undergo the initialisation process and provide information for user registration.

*High-level requirements*

- The Age Verification App SHOULD be a native application for a mobile operating system. The Age Verification App MAY be an application for any operating system (mobile or non-mobile).

- The Age Verification App made available as a mobile application SHOULD be published on the App Stores for Android and iOS operating systems and MAY be published on other App Stores (e.g. Huawei, Samsung).

- The Age Verification App is provided as a stand-alone app separate from the European Digital Identity Wallet, its Age Verification App Provider SHOULD strive for equivalence with Level of Assurance "Substantial" pursuant to Commission Implementing Regulation (EU) 2015/1502.

- The Age Verification App MAY include initialisation functionality that is required for the use of the app.

***Relevant technical specifications:* N/A**

## A.2.2	Proof of Age attestation issuance (AVI, AP)

Verifying the age of the User is a key element to build trust and foster adoption as it must be highly reliable while at the same time it must be easy for the User to prove to the Attestation Provider who equally should be able to verify it against authoritative sources.

Attestation Providers should be reliable registered actors. To enable swift deployment of the age verification solution it is proposed to rely on the existing register of eIDAS trust service providers instead of establishing a new register.

The User's age should be verified using a reliable issuing method.

Once the User's age is verified, the age verification app can request the issuance of the Proof of Age attestation. This can be initiated by the Age Verification App. To support the issuance of Proof of Age attestations that protect the privacy of the User, [Multiple Credential Issuance](https://openid.net/specs/openid-4-verifiable-credential-issuance-1_0.html#name-multiple-credential-issuanc) as per OpenID for Verifiable Credential Issuance standard should be supported.

*High-level requirements:*

- The Attestation Provider SHOULD be a qualified or non-qualified trust service provider (TSP) registered in the eIDAS dashboard as a provider of certificates for electronic signature in accordance with Article 22 of 2014/910.

- The Attestation Provider SHALL NOT issue a Proof of Age attestation before verifying the attestation subject's age at the Level of Assurance 'substantial' or 'high'.

- The Proof of Age Attestation Provider SHOULD enable issuance of Proof of Age attestations in compliance with [OpenID4VCI] or with [ISO/IEC CD TS 23220-3].

- The Proof of Age Attestation Provider SHOULD support [Multiple Credential Issuance](https://openid.net/specs/openid-4-verifiable-credential-issuance-1_0.html#name-multiple-credential-issuanc) as per OpenID for Verifiable Credential Issuance standard.

- The Age Verification App SHALL enable acquisition of Proof of Age attestations in compliance with \[OpenID4VCI \] and their storage.

- The Age Verification App SHALL provide a binding between a Secure Storage and the Proof of Age attestation.

- The Proof of Age attestations SHALL be compliant with mDoc in CBOR encoding [ISO/IEC TS 23220-2], [ISO/IEC 18013-5], except for the attribute schema that SHALL be compliant with the attribute schema defined in the section Data Architecture.

- The Proof of Age attestation SHALL use the namespace "eu.europa.ec.agev10n".

- A mobile driving licence compliant with \[ISO/IEC 18013-5\] MAY be used as a Proof of Age attestation provided that its attributes are compliant with the attribute schema defined in the section Data Architecture.

- A Photo ID compliant with \[ISO/IEC CD TS 23220-4\] (Annex C of the referenced standard) MAY be used as a Proof of Age attestation provided that its attributes are compliant with the attribute schema defined in the section Data Architecture.

*Relevant technical specifications:*

- [[OpenID4VCI](https://openid.net/specs/openid-4-verifiable-credential-issuance-1_0.html)]
- [[ISO/IEC 18013-5](https://www.iso.org/standard/69084.html)]
- [[ISO/IEC TS 23220-2](https://www.iso.org/standard/86782.html)]
- [[ISO/IEC CD TS 23220-3](https://www.iso.org/standard/86783.html)]
- [[ISO/IEC CD TS 23220-4](https://www.iso.org/standard/86785.html)]

## A.2.3	Request of Proof of Age attestation (WB, RP, AVA)

The user requests access to an online service (Relying Party) with age-restricted contents or services by navigating to its website using a Web Browser or by opening its mobile application. If the user has an account on the online service, and has performed age verification before, access can be granted without a new age verification. Depending on its policy or guidelines in force, the service may however repeat age verification regularly or in certain circumstances.

The online service requests Proof of Age from the user by redirection of the User's Web Browser (WB) to the Age Verification App (Same Device Flow) or by displaying a QR code that the User can scan using their mobile phone and open the Age Verification App (Cross Device Flow).

*Relevant technical specifications:*

- \[ISO/IEC 18013-7\] and standards referenced therein, such as (see
  next line)

- \[OpenID4VP\] (Sections 3.1 Same Device Flow and 3.2 Cross Device Flow)

## A.2.4 Proof of Age attestation presentation (AVI, RP)

The Age Verification Instance requests the User to authenticate themselves (e.g. using a PIN code defined by the user for the app, or biometric verification such as a fingerprint). The User checks the age verification request and confirms the Proof of Age response. The Age Verification Instance presents the Proof of Age to the Relying Party.

*High-level requirements:*

- The Age Verification Instance SHALL display the Proof of Age request to the user and obtain the user's confirmation prior to presenting the Proof of Age attestation.

- The Age Verification Instance SHALL authenticate its User in a reliable manner (e.g. using a password, PIN code, biometric verification, pattern) before presenting the Proof of Age attestation.

- The Age Verification Instance SHALL support the presentation of the Proof of Age attestation in compliance with the protocols defined in \[ISO/IEC 18013-7\].

*Relevant technical specifications:*

- ISO/IEC TS 18013-7, *Personal identification -- ISO-compliant driving licence -- Part 7: Mobile driving licence (mDL) add-on functions,* and standards referenced therein, such as (see next line)

- OpenID for Verifiable Presentations

## A.2.5 Privacy Preserving Mechanisms

As safeguarding citizens' rights to privacy and providing effective measures for data protection is a key objective behind the initiative to create a harmonised method for verifying that individuals meet the required minimum age for accessing services, several different mechanisms have been identified as critical to achieving this objective. Those mechanisms are described in further detail below. The solution SHOULD implement these mechanisms (salted hashes and zero-knowledge proofs).

## A.2.5.1 Salted hashes

The salted hash approach involves hashing data with a unique random value (salt) to prevent attackers from correlating or precomputing hashes, enhancing security and privacy. This approach enables selective disclosure of attributes but requires a batch issuance mechanism to mitigate potential privacy risks from linkability by releasing disposable attestations.

## A.2.5.2 Selective Disclosure

Selective disclosure is a straightforward way of implementing the principle of data minimisation. The mechanism is described in [ISO/IEC 18013-5] and can be used to disclose only the relevant attributes of the Proof of Age attestation. In access to online services, this would commonly be the age_over_18 attribute. However, in some cases where a different age limit is used, the Age Verification App would disclose the nearest TRUE-valued age_over_NN attribute equal to or larger than NN requested by the Relying Party. 

## A.2.5.2 Batch Issuance

Building on the selective disclosure mechanism, the age verification solution can be designed to enable the Proof of Age Attestation Provider to issue multiple age_over_18 attributes and possibly multiple age_over_NN attributes with the same NN. Attributes in the same batch would contain the same statement, but would be different by content, as they would be constructed using different cryptographic hashes. The Age Verification App can then present a new attribute from the same batch in every request by a Relying Party. When all the attributes have been used, the Age Verification App would need to request a new batch from the Attestation Provider in the form of a new Proof of Age attestation.

## A.2.6 Zero Knowledge Proof for Privacy Preservation

Zero-knowledge proofs (ZKPs) are cryptographic techniques that enable one party (the User) to prove to another party (the Relying Party) that a statement is true without disclosing any additional information beyond the statement’s validity. This allows the User to demonstrate knowledge without revealing the knowledge itself.

For example, when the User needs to verify that they meet the required minimum age, the Age Verification App could utilise the Proof of Age attestation to generate a proof confirming that the User meets the required age threshold (e.g., 18 years) without disclosing their exact age or any other personal details. The Relying Party would receive and verify the proof. If valid, the Relying Party could be certain that the User meets the age requirement without gaining any further information.

Additionally, a ZKP-based approach enhances privacy by ensuring unlinkability, making it computationally infeasible for the Relying Party to associate multiple proofs with the same individual.

Five options have been evaluated for the implementation of the ZKP to the age verification solution.

- BBS+/CL-Signatures/PS-MS
- BBS# 
- ECDSA Anonymous Credentials 
- Crescent
- ZKAttest 

Each option has been evaluated from several aspects:
- If there are implementations already available based on the solution
- If the solution has been peer reviewed
- If the solution does not rely on pairings
- If the solution supports secure element to prevent credential sharing
- If the solution requires changes to the issuer compared to the current
  requirements in EUDIW
- What is the time needed to generate proofs with the solution
- If the solution supports both sd-jwt and mdoc format

|  | **BBS+/CL-Signatures/PS-MS**| **BBS#​** | **EDSA Anonumous Credentials** |**Crescent**| **ZKAttest**|
|-----------|-----------|-----------|-----------|-----------|-----------|
|**Implementation available** | Yes | No (expected in short term) | No  (expected in short term) | Yes | Yes|
|**Peer reviewed**|Yes|No|No|No|Yes|
|**Does not rely on pairings**|No|Conditional|Yes|Yes|Yes|
|**Supports secure elements (prevents credential sharing)**|No|Yes|Yes|No (but it will be considered)|Yes|
|**Changes to issuer**|Many|Many|None|None|Medium|
|**Proof generation time**|~ms|~ms|<2 sec |<5 sec|<10 sec|
|**Supports both SD-JWT and mDoc**|Yes|Yes|No (only optimised mDoc|Yes|Conditional|


There is no ZKP solution available at the moment that would align with the ARF and be comprehensively tested. A more detailed assessment and the proposed solution are outlined in "Zero-Knowledge Proof product and architecture specification (D03.03)" that will be later published.

A contribution has been submitted by Google to ISO for adding support for zero-knowledge proofs (ZKP) to ISO/IEC 18013-5 (mDL, edition 2). Discussions on this proposal are ongoing in the working group.

In view of the goals of enabling privacy-preserving techniques in the implementation of the age verification app, the implementation of zero-knowledge proofs should be supported where possible.

High-level requirements:

- The age verification app SHOULD implement support of \[ISO/IEC 18013-5\] Edition 2 when it becomes available.

- The Proof of Age attestation issuer SHOULD implement support of \[ISO/IEC 18013-5\] Edition 2 when it becomes available.

- The online service SHOULD implement support of \[ISO/IEC 18013-5\] Edition 2 when it becomes available.

_Referencies for the ZKP solutions_

- _BBS+/CL-Signatures/PS-MS: Dan Boneh, Xavier Boyen, and Hovav Shacham, Short Group Signatures, Cryptology ePrint Archive, Paper 2004/174, 2004, available at https://eprint.iacr.org/2004/174; Stefano Tessaro, Chenzhi Zhu, Revisiting BBS Signatures, Cryptology ePrint Archive, Paper 2023/275, 2023, available at [https://eprint.iacr.org/2023/275](https://eprint.iacr.org/2023/275)_

- _BBS#: Orange, "The BBS# protocol", Technical Report, 2024_

- _ECDSA Anonymous credentials**: Matteo Frigo and Abhi Shelat, Anonymous credentials from ECDSA, Cryptology ePrint Archive, Paper 2024/2010, 2024, available at [https://eprint.iacr.org/2024/2010](https://eprint.iacr.org/2024/2010)_

- _Crescent: Christian Paquin, Guru-Vamsi Policharla, and Greg Zaverucha, Crescent: Stronger Privacy for Existing Credentials, Cryptology ePrint Archive, Paper 2024/2013, 2024, available at [https://eprint.iacr.org/2024/2013](https://eprint.iacr.org/2024/2013)_

- ZKAttest: Armando Faz-Hernández, Watson Ladd, and Deepak Maram, Ring and Group Signatures for Existing ECDSA Keys, Cryptology ePrint Archive, Paper 2021/1183, 2021, available at [https://eprint.iacr.org/2021/1183](https://eprint.iacr.org/2021/1183)_
