# 1. Introduction

## 1.1 Purpose of this document

The purpose of this document is to provide a comprehensive specification that combines a Software Requirements
Specification (SRS) and an Architecture Description (AD) for the age verification software system for online platforms
to protect minors. The document defines both the functional and non-functional requirements, as well as the system
architecture, with the objective of harmonizing the development of age verification solutions within the EU. This
approach aims to deliver a cross-border solution that minimizes development effort and duplication among Member States,
while fostering synergies.

The structure of this document is based on the relevant IEEE standards, specifically, IEEE 830-1998 for Software
Requirements Specifications and ISO/IEC/IEEE 42010:2022 for Architecture Descriptions, but has been adapted to meet the
specific needs and context of this project.

This document serves as a formal agreement between stakeholders and the development team, ensuring a clear and shared
understanding of the intended system behavior, constraints, and architectural decisions. Furthermore, it provides Member
States and other interested parties with a foundation for implementing systems that are compliant with this
specification.

This document is intended for developers, testers, project managers, and other stakeholders involved in the design,
implementation and validation of software systems for DSA-compliant age verifications.

## 1.2 Scope of this development

This specification defines the structural and functional aspects of the age verification software system for online
platforms. It details the key components, interfaces, and interactions within the age verification ecosystem, providing
a technical foundation to ensure interoperability, security, privacy and compliance with applicable regulatory and
industry requirements.

The scope includes:

- The end-to-end process for issuing, presenting, and verifying proofs of age for access to online services, including
  age-restricted content, products, or services.

- The operational, security, product, and architectural requirements necessary for a harmonized and scalable solution
  that
  can be adopted and extended by Member States or other actors.

- The definition of system boundaries, including the roles of age verification providers, Relying Parties, and users, as
  well as the interactions between these entities.

- The technical requirements for device compatibility, focusing on enabling users to present proof of age across
  commonly
  used devices such as mobile phones, tablets, laptops, and desktop computers.

- The specification of interoperability mechanisms to support integration with existing and future digital identity
  solutions, including the planned incorporation into EU Digital Identity Wallets.

This specification does not cover the internal implementation details of third-party Relying Parties, the development of
alternative age verification methods beyond those explicitly described, or the detailed integration of operational
systems required for an enrollment, as this is subject to country-specific requirements. It serves as a reference for
the harmonized
implementation and certification of age verification solutions across the EU.

This specification builds upon the Architecture and Reference Framework (ARF) for the European Digital Identity (EUDI)
Wallets, adopting the same foundational technical standards and design principles to ensure interoperability, security,
and privacy within the EU digital identity ecosystem. However, it defines only a subset of the requirements and
functionalities compared to a full EUDI Wallet implementation, focusing specifically on the needs of age verification
solutions. The objective is to provide a compatible architecture that facilitates seamless integration and
future-proofing: by aligning with the ARF, this specification enables a clear migration path, so that age verification
services can, in the future, be supported directly by EUDI Wallets as the ecosystem evolves.

## 1.3 Definitions, Acronyms and Abbreviations

For the purposes of the present document, the terms and definitions given in EU Digital Identity Architecture and
Reference Framework Specification and the following apply. A term defined in the present document takes precedence over
the definition of the same term, if any, in EU Digital Identity Architecture and Reference Framework Specification.

### 1.3.1 Definitions

For the purposes of the present document, the following terms and definitions apply:

### 1.3.2 Acronyms and Abbreviations

For the purposes of the present document, the following abbreviations and acronyms apply:

| **Term**           | **Definition**                                               |
|--------------------|--------------------------------------------------------------|
| AP	                | Attestation Provider                                         |
| AV app             | Age Verification application                                 |
| AVI	               | Age Verification App Instance                                |
| AVAP               | 	Age Verification App Provider                               |
| DG CNECT           | Directorate General Network, Content and Technology          |
| eIDAS              | Electronic Identification, Authentication and Trust Services |
| EUDI               | European Digital Identity                                    |
| EUDIW /EUDI Wallet | European Digital Identity Wallet                             |
| LoA                | Level of Assurance                                           |
| U	                 | User                                                         | 
| RP	                | Relying Party (Proof of Age attestation verifier)            |
| T-Scy              | Scytáles & T-Systems Age consortium                          |
| WB	                | Web Browser (or web app)                                     |
| ZKP                | Zero Knowledge Proof                                         |

## 1.4 Conventions

The terms "Shall," "Shall not," "May," "Need not," "Should," and "Should not" in this document are to be interpreted as
defined in RFC 2119 (Key words for use in RFCs to Indicate Requirement Levels). Their meanings are as follows:

- **Shall:** Indicates a mandatory requirement; compliance is required.

- **Shall not:** Indicates a mandatory prohibition; compliance is required.

- **Should:** Indicates a recommended requirement; there may exist valid reasons to ignore it in particular
  circumstances, but
  the full implications must be understood and carefully weighed before choosing a different course.

- **Should not:** Indicates a recommended prohibition; there may exist valid reasons in particular circumstances when
  the
  particular behavior is acceptable or even useful, but the full implications should be understood and the case
  carefully
  weighed before implementing any behavior described with this label.

- **May:** Indicates an optional requirement; implementation is permitted but not required.

- **Need not:** Indicates that there is no requirement to implement or perform the described behavior; it is optional
  not to
  do so.

These keywords are used throughout this document to clarify the level of obligation associated with each requirement.

## 1.5 References

### 1.5.1 Normative References

References are classified as either specific (identified by date of publication and/or version number) or
non-specific. For specific references, only the cited version is applicable. For non-specific references, the most
recent version of the referenced document (including any amendments) shall apply.

The following referenced documents are essential for the application of this document.

- [EU Digital Identity Architecture and Reference Framework][arf]

[arf]: https://github.com/eu-digital-identity-wallet/eudi-doc-architecture-and-reference-framework
<!---
NF, Add more background about the issue, motivation, etc, Mentions mdoc with respect to ZKP, digital credentials API, security considerations
-->
### 1.5.2 Informative References
<!---
NF, are you sure that all these are informative references? even [ISO/IEC 18013-5],
[OpenID4VCI] is informative, not all issuers have to implement vci
-->

References are classified as either specific (identified by date of publication and/or version number) or
non-specific. For specific references, only the cited version is applicable. For non-specific references, the most
recent version of the referenced document (including any amendments) shall apply.

The documents listed below are not required for the implementation of this document; however, they provide additional
guidance and context on specific subject areas.

| **Item Reference**      | **Standard name/details**                                                                                                                                                                                                                                                  |
|-------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [OpenID4VCI]            | [OpenID for Verifiable Credential Issuance](https://openid.net/specs/openid-4-verifiable-credential-issuance-1_0-ID2.html)                                                                                                                                                 |
| [OpenID4VP]             | [OpenID for Verifiable Presentations](https://openid.net/specs/openid-4-verifiable-presentations-1_0-24.html)                                                                                                                                                              |
| [HAIP]                  | [OpenID4VC High Assurance Interoperability Profile with SD-JWT VC](https://openid.net/specs/openid4vc-high-assurance-interoperability-profile-1_0.html)                                                                                                                    |
| [OIDC]                  | [Final: OpenID Connect Core 1.0 incorporating errata set 2](https://openid.net/specs/openid-connect-core-1_0.html)                                                                                                                                                         |
| [ISO/IEC 18013-5]       | [ISO/IEC 18013-5:2021: Personal identification — ISO-compliant driving licence. Part 5: Mobile driving licence (mDL) application. Edition 1, 2021.](https://www.iso.org/standard/69084.html)                                                                               |
| [ISO/IEC 18013-7]       | [ISO/IEC TS 18013-7:2024: Personal identification — ISO-compliant driving licence. Part 7: Mobile driving licence (mDL) add-on functions. Edition 1, 2024.](https://www.iso.org/standard/82772.html)                                                                       |
| [ISO/IEC 23220-2]       | [ISO/IEC TS 23220-2:2024: Cards and security devices for personal identification — Building blocks for identity management via mobile devices. Part 2: Data objects and encoding rules for generic eID systems. Edition 1, 2024.](https://www.iso.org/standard/86782.html) |
| [ISO/IEC CD TS 23220-3] | ISO/IEC CD TS 23220-3: Cards and security devices for personal identification — Building blocks for identity management via mobile devices. Part 3: Protocols and services for issuing phase                                                                               |
| [ISO/IEC CD TS 23220-4] | [OISO/IEC CD TS 23220-4: Cards and security devices for personal identification — Building blocks for identity management via mobile devices. Part 4: Protocols and services for operational phase.](https://www.iso.org/standard/86785.html)                              |
| [IETF RFC 2119]         | [Key words for use in RFCs to Indicate Requirement Levels](https://datatracker.ietf.org/doc/html/rfc2119)                                                                                                                                                                  |
| [IEEE 830-1998]         | [Recommended Practice for Software Requirements Specifications](https://standards.ieee.org/ieee/830/1222/)                                                                                                                                                                 |
| [ISO 42010-2022]        | [Software, systems and enterprise — Architecture description](https://www.iso.org/standard/74393.html)                                                                                                                                                                     |

## 1.6 Overview

TODO: describe document structure

# 2. Overall Description of the Age Verification Solution

The primary objective is to develop an EU-wide solution to age verification that reinforces
the [Digital Services Act (DSA)](https://eur-lex.europa.eu/eli/reg/2022/2065  ) objective to ensure safe, secure, and
trusted digital space, notably Article 28, which focuses on protecting minors and
the [Louvain-la-Neuve Declaration](https://bosa.belgium.be/sites/default/files/content/documents/LLN%20Declaration%20-%20Informal%20Telecom%20Council%20-%20v.12.04.2024.pdf ),
which promotes a safer and more trustworthy online environment. The proposed solution is intended to bridge the gap
until the EU Digital Identity (EUDI) Wallets become available by the end of 2026, enabling the incorporation of the age
verification functionality in them.

As stated in the Louvain-la-Neuve Declaration, "the presence of online harmful content and systemic risks continues to
be a major concern as the use of social media, in particular by children, increasingly plays a formative role in their
daily lives". Creating a safer, more secure, and trusted digital environment therefore has to be at the heart of
Europe’s digital strategy, with a firm commitment to support, empower and respect children online and protect them from
harmful content and systemic risks. The Declaration also calls on the European Commission to bring together the tools
available under the Digital Services Act (DSA) and the European Digital Identity Wallet, to provide comprehensive
safeguards for the users of digital services in the Union, in particular the most vulnerable groups, for example
children.

As stated in the study "[Mapping age assurance typologies and requirements](https://data.europa.eu/doi/10.2759/455338)",
age verification solutions provide a high degree of certainty in determining the age of a user. Through this design
document we are defining a Europe-wide effective and convenient method to age-gate access to specific online services.

Age verification plays a crucial role across various scenarios, including:

- Access to online services: Verifying age for online services offering age-restricted content or services, such as
  adult media, gambling, or other content deemed unsafe for minors.

- Purchases of age-restricted products: Ensuring legal compliance when purchasing products like alcohol or tobacco, both
  online and in physical stores.

- Age-related benefits: Validating eligibility for discounts or services offered to specific age groups, such as young
  or elderly individuals.

Given the regulatory priorities and societal needs, this document focuses on age verification for accessing online
services with restricted content or services. However the proposed solution defined in this document is not exclusive to
this use case but can be used broader in the scenarios mentioned above.

The age verification solution will serve as a foundational component of the European Union’s efforts to create a safer
digital environment. This document will contribute to the development of a future Age Verification Rulebook, which will
be included in the Electronic Attestation Catalogue under the governance of the European Digital Identity Regulation.

By establishing a secure, privacy-preserving, and interoperable framework for age verification, this initiative will
empower users, online services, and regulators to uphold digital safety and inclusivity while ensuring compliance with
Union law.

## 2.1 Perspective of the Age Verification Solution
<!---
NF, This exists in the original version as well, still I don't understand what
"defines a device-based solution" means. Device-based means mobile 
-->
The present document defines a device-based solution that enables secure and efficient age verification directly at the
device level. Once age-related attestations are issued to the device, they can be presented and verified by online
services in a simple, secure, and privacy-preserving manner. This capability is supported by the following elements:

- **Open Technical Specifications:**
  A comprehensive suite of open technical specifications has been developed to support the implementation of the
  end-to-end solution. These specifications cover the entire lifecycle, from the issuance of age attestations to their
  verification by online services. Their development involved close collaboration with all relevant
  stakeholders-including
  Member States, industry representatives, and technical experts-to ensure alignment with regulatory requirements,
  technological advancements, and user needs. This inclusive approach promotes interoperability and facilitates broad
  adoption of the solution.

- **Integration with the European Digital Identity Regulation:**
<!---
NF, Do we leverage "the existing eIDAS infrastructure"? Yes
-->
  The solution leverages the existing eIDAS infrastructure, including eIDAS nodes and the trust framework for trusted
  services, to ensure a high level of security and reliability. By aligning with the technical architecture of the EU
  Digital Identity Wallet [ARF](https://eu-digital-identity-wallet.github.io/eudi-doc-architecture-and-reference-framework/), the
  solution delivers secure, reusable, and interoperable proofs of age. This approach ensures compliance with EU
  standards and supports trusted, seamless cross-border functionality.

- **Reference Implementation:**
  A reference implementation, including a white-label Age Verification Application, is provided to facilitate the
  widespread deployment and adoption of age verification solutions.

By ensuring compliance with the standards outlined in the EU Digital Identity Architecture and Reference Framework (
ARF), this solution establishes a foundation for long-term sustainability and interoperability. The Age Verification
Profile, developed within the context of open standards, is designed to be forward-compatible with future EUDI Wallets.
This compatibility is achieved because the reference implementation for Age Verification builds directly upon the EU's
technical specifications and reference implementation for digital identity systems, ensuring architectural alignment.

The modular architecture enables two strategic pathways for Member States:

- **Privacy-Centric Operation:** The Age Verification Application can be deployed as a standalone solution, operating
  independently to minimize data exchange and address specific privacy concerns of users.

- **Seamless Evolution to Full EUDI Wallets:** The solution's design allows for incremental upgrades, where the Age
  Verification Application can later be expanded into a full EUDI Wallet by integrating additional functionalities (
  e.g., qualifications, attestations) as needed.

This dual approach ensures flexibility while maintaining technical coherence with the broader EU digital identity
ecosystem, future-proofing investments and simplifying eventual transitions.

## 2.2 Functions of the Age Verification Solution

The primary function of the solution is the issuance of Proof of Age Attestations that can be used across the European
Union. These attestations enable individuals to demonstrate that they meet a specific age threshold, currently focused
on "over 18", to access age-restricted online services and content. The solution is designed to be extensible, allowing
support for other age groups (e.g., "over 14" or "over 65") provided that appropriate enrollment methods are available.

### 2.2.1 Flexible Enrollment Methods
<!---
NF, Is this section correct? These are methods for an AP to obtain information about the age of a user. It is correct
-->
Users have the flexibility to choose from multiple enrollment methods to obtain their Proof of Age Attestation.
Supported methods include, but are not limited to:

- Scanning or reading an electronic identity card (eID)

- Scanning or reading a passport

- Using existing identifications provided by 3rd party apps

- Using an enrollment token in an on-site scenario

- Other secure identity verification processes as defined by Member States or authorized issuers

These enrollment methods ensure that the attestation is based on reliable and verifiable identity documents. Where
necessary, enrollment can also be conducted on-site to accommodate users who may not have access to electronic
identification means.
<!---
NF, Is this correct? Shouldn't a solution support all methods? It is correct
-->
Member States and solution providers retain the autonomy to determine which enrollment options they offer within the age
verification solution. This flexibility acknowledges the diversity of national identification systems, regulatory
environments, and user needs across the European Union.

### 2.2.2 Presentation of Proof of Age Attestation

The solution enables users to present their Proof of Age Attestation to Relying Parties, primarily for online use cases.
The system is optimized for secure and privacy-preserving online presentation, allowing users to prove their eligibility
without disclosing unnecessary personal information. In addition to online scenarios, the solution in principle can also
be adapted to support offline or proximity-based presentations, where attestations can be verified locally.
<!---
NF, Do we need this? Remove it. Write in fields that it is for future, proximity based uses
-->
For proximity scenarios, the Proof of Age Attestation may optionally include a user photo. This enables the user to
demonstrate possession of the device and further strengthens the binding between the attestation and the legitimate
holder.

### 2.2.3 Revocation and Re-Issuance
<!---
NF, We do not support revocation and re-issuance. To remove? Yes. Or write why do not have revocation
-->

The solution provides mechanisms for users to revoke and re-issue Proof of Age Attestations as needed. This ensures that
attestations remain valid and up-to-date and allows users to maintain control over their attestations in the event of
loss, theft, or other changes in status.

### 2.2.4 Verifiable across Europe

A Trusted List is maintained as part of the solution, allowing Relying Parties and users to validate which entities are
legally authorized to issue Proof of Age Attestations. This enhances trust and interoperability across Member States by
ensuring that only recognized authorities participate in the issuance process.

### 2.2.5 Note about the extensibility of the solution (for distributors):

While the current implementation focuses on the "over 18" age threshold, the technical architecture supports extension
to other age groups as required by regulatory or business needs. The same applies also to additional enrollment
options. The only prerequisite is the availability of a suitable enrollment process for the target age group, which can
be also adapted or conducted on-site if necessary.

## 2.3 User Characteristics

The primary users of the system are citizens of EU Member States who wish to demonstrate that they have reached the age
of majority. Secondary users of the solution include immigrants, tourists within the EU, and business travelers.

## 2.4 User Journey

To enable online age verification, the user is required to install an AV app on their mobile
device. This AV app  allows the user to securely and reliably present proof of age without disclosing
any unnecessary personal information. For instance, the user can demonstrate that they meet a minimum age
requirement-such as being at least 18 years old-without revealing their exact age, name, or other sensitive details.
This approach ensures both privacy and strict adherence to applicable data protection standards.

The following illustration and accompanying description outline a scenario-based sequence of the steps a user undertakes
to achieve this objective using the proposed age verification solution. This user journey spans multiple channels and
points in time, highlighting the seamless interaction between the user, the AV app, and the online
service.

<!---
NF, Is the picture correct? Specifically step 2
-->
![Figure 1](./docs/media/Figure_1_user_journey.png)
*Figure 1: User Journey of an Proof of Age attestation user*

The age verification process consists of two main phases: Activation and Use. The steps are illustrated in Figure 1 and
detailed below:

### 2.4.1 Activation of the App

**1. App installation and registration**

- The user downloads and installs the AV app on their mobile device.

- If required by the app, notably if it supports multiple user profiles, the user creates a local profile in the app to
  facilitate subsequent verification processes.

**2. Requesting a Proof of Age attestation**

- The user requests a Proof of Age from a designated AP, which will issue it after
  verifying the user’s identity at a level of assurance equivalent to "substantial" or "high" pursuant to Commission
  Implementing Regulation (EU) 2015/1502 through the following methods:

    - Notified or national eID schemes

    - Leveraging existing databases: Identity verification is conducted through recognised and well-established
      processes already in use for personal identification under national or Union law, such as National identity
      providers covering the level of assurance.

    - Know Your Customer (KYC) procedures employed by banks or the identity verification methods used for issuing SIM
      cards by Mobile Network Operators.

    - Document-Based Verification: Confirming the user’s age using official identification documents such as electronic
      ID cards, passports, or other government-issued credentials. The link between the document and the user should be
      verified.

**3. Issuance of Proof of Age attestations**

- Once the user’s age is verified, the AV app receives a digital Proof of Age attestation.

- This attestation is securely stored within the AV app and can be used to prove the user’s age to online service or
  physical retailers.

<!---
NF, I thinks this creates confusion and should be removed. High-level description of scenarios, consistent with the rest of the text
-->
- Decoupled Issuance Based on Pre-Authorized Codes : The issuance process for a Proof of Age can be also decoupled from
  the identity verification process. This can be achieved through pre-authorised secure codes provided to the user after
  successful identity verification or generated on demand by third-party applications, such as banking apps. These codes
  can then be used to retrieve a valid Proof of Age attestation, enabling a streamlined, flexible, and user-friendly
  issuance process while maintaining high standards of security and compliance.

### 2.4.2 Usage of the Age Verification solution

**4. User requests access to age-restricted services**

- After activation, the user can use the AV app to present their Proof of Age attestation to access age-restricted content
  or services (e.g., gambling, adult media, alcohol purchases).

- The user initiates access by requesting a service or content on the target service.

**5. Service requests Proof of Age attestation**

- The service replies by requesting a Proof of Age, typically through one of the following mechanisms:

    - Same-Device App Integration: The user's browser or a native app opens the AV app directly (if on the
      same device).

    - Cross-Device QR Code: The service displays a QR code that the user scans with their mobile device to open the age
      verification app.

<!---
NF, Do we need this here?
-->
- In addition the possibility for Cross-Device Shareable Links will be analysed, considering possible risks associated
  with their use. Such a link could be sent by the user to their mobile device, for example using a messaging service.

**6. Confirmation and presentation**

- The AV app receives the Proof of Age request and presents it to the user. The user reviews the request
  details, verifies the information, and confirms the transaction to proceed.

- The AV app securely transmits the Proof of Age attestation to the service.

**7. Verification and access**

- The service verifies the Proof of Age by:

    - Checking the attestation's validity and digital signature.

    - Ensuring the attestation's details meet the required criteria (e.g., age threshold).

- If the verification is successful, the service grants the user access to the requested service or product.

**8. Verification options for subsequent interactions**

- On-Demand Verification (8a): Services may request the Proof of Age each time the user attempts to access restricted
  content or services, without storing it in any session or account, ensuring privacy and minimising data retention.

- Stored Verification(8b): Services may optionally store information derived from the Proof of Age in the user's
  account, allowing the user to bypass repeated verification for future visits or purchases, streamlining the user
  experience. In this case, authentication methods such as WebAuthN should be utilised to ensure secure access while
  enabling the user to choose a pseudonym, preserving privacy. Risks in case of the device sharing should be
  considered.

## 2.5 Constraints

To enable rapid and reliable deployment of the age verification solution, the solution adheres to the following
approach:

- **Reliance on existing standards:** Where possible, it prioritizes the use of available standards and specifications
  to expedite development and integration.
  The [ARF](https://eu-digital-identity-wallet.github.io/eudi-doc-architecture-and-reference-framework/) serves as the
  primary pool of standards.

- **Collaboration with established APs:** The solution engages institutions capable of mass-issuing Proof of Age
  attestations, such as banks, mobile network operators, and governmental population registers.

- **Involvement of key stakeholders:** The solution partners with actors skilled in app development and eGovernment
  solutions, including mobile operating system providers, wallet solution providers, and operators of citizen-facing
  applications.

To ensure a robust and reliable solution, the framework is driven by the following design principles:

- **Data protection:**
<!---
NF, Do we do that? what are attribute statements? transaction? domain-specific identifiers?
-->
    - **Data minimisation:** Only the necessary user attributes or attribute statements for a specific transaction are
      released, in accordance with the principle of data minimisation. This is achieved by implementing technologies
      that inherently restrict data exposure and safeguard user privacy. Domain-specific identifiers, or pseudonyms, are
      used to enable users to avoid relying on the same unique identifier when interacting with online services.
<!---
NF, Do we do that? May be saying something like "initially we will use multiple attestations
then ZKP"
-->
    - **Unlinkability:** Cryptographic or protocol-level unlinkability is implemented to ensure that only the
      identifiers required for specific linkable transactions are disclosed. Protocols that enable default linkability
      across transactions are avoided, as they may violate data minimisation principles and compromise privacy.
      Profiling and tracking of individuals is prevented. The solution seeks to attain as many types of unlinkability (
      with respect to attestation providers, Relying Parties, or both) as possible, while maintaining technological
      practicability.

<!---
NF, Our solution does not do something about that. 
-->
    - **Storage limitation:** Data is retained by all involved parties only for as long as necessary for the purposes of
      age verification.

    - **Transparency:** Users receive clear information about how their personal data is used.

<!---
NF, Re-write to something "use available solutions for protecting..."
-->
- **Data security:** User data and credentials are securely stored to ensure confidentiality, authenticity, integrity,
  and availability. This protection safeguards data against unauthorized access, destruction, misuse, alteration,
  disclosure or loss.
<!---
It is suggested...
-->
<!---
NF, Re-write to something "will use..." for example.... safenet so that no rooted etc. 
-->
- **Cloning protection:** Cloning protection prevents the unauthorized duplication of credentials and user data. This
  measure ensures that illicit reproduction of credentials does not grant unauthorized parties privileges they would not
  otherwise possess.

<!---
NF, The protocols are using TLS, the protocols themselves do not do that
-->
- **Eavesdropping protection:** The protocols implemented within the age verification solution safeguard communications,
  preventing unauthorized interception and exposure of personal data.

- **Interoperability:** The solution ensures seamless integration across diverse device operating systems, wallet
  applications, and online services.

<!---
NF, Are they? May be "When possible"
-->
- **User binding:** User attributes and credentials are securely bound to the legitimate user to whom they are issued.
  This ensures the fundamental functionality of any government-issued document: accurately associating attributes with
  the individual to whom they apply.

- **User centricity:** The system is user-centric, allowing users to maintain control over their attributes and provide
  informed consent for their use, whether the attributes are managed locally or remotely by an identity or attribute
  provider.

- **User experience:** The solution provides a simple and intuitive process for users to verify their age and share
  attestations when required.

- **Compliance:** The solution aligns with data protection regulations, including GDPR, particularly the principles of
  data protection by design and by default, and adheres to the European Digital Identity Regulation.

- **Equity:** The solution is designed to be accessible and inclusive, ensuring that users with varying levels of
  technological access or proficiency can participate without barriers. Remedies, such as alternative attestation
  issuance procedures or human intervention, are available for cases where individuals cannot use the mainstream
  procedure, for example due to a lack of necessary identity documents or electronic identification means.

## 2.6 Assumptions and Dependencies

This specification operates under the following foundational assumptions:

### 2.6.1 Legal Compliance in Member States

It is assumed that EU Member States have established appropriate legal frameworks to authorize the collection and
processing of data required for issuing Proof of Age Attestations. This includes compliance with GDPR, the Digital
Services Act (DSA) , and national laws governing identity verification and age-restricted services. The enrollment
methods (e.g., eID, passport scans) rely on these legal bases to ensure lawful data handling.

### 2.6.2 Deployment Responsibility

Member States or third-party entities are responsible for deploying the age verification solution in their
jurisdictions. The solution is provided as a white-label, open-source implementation, enabling customization to meet
national legal, technical, and security requirements. No operational responsibility is retained by the project
consortium.

### 2.6.3 Open-Source Implementation

The project exclusively provides an open-source reference implementation via public repositories (e.g., published on
GitHub). Member States and third parties are responsible for hosting, maintaining, and adapting the solution to their
infrastructure.

### 2.6.4 Alignment with EU Digital Identity Architecture

The solution assumes ongoing development of the EU Digital Identity Wallet’s Architecture and Reference
Framework (ARF). By adhering to the ARF’s evolving specifications, the age verification solution ensures
interoperability with future EU Digital Identity Wallets and leverages synergies in trust frameworks, cryptographic
standards, and governance models.

### 2.6.5 National Adaptations

Member States are responsible for modifying the solution to align with national legal requirements (e.g., age
thresholds, data retention policies) and security standards (e.g., certification of issuers, encryption protocols). The
specification provides modular components to facilitate these adaptations without compromising interoperability.

## 2.7 Key Differences in between the Age Verification application and the Age Verification functionality in the EUDI Wallet

The Age Verification solution is intended to bridge the gap until the EUDI Wallets become available by the end of 2026,
enabling the incorporation of the age verification functionality in them. This section describes the key differences
between the Age Verification application and the age verification functionality in the EUDI Wallet.

The trust framework of EUDI Wallet requires that the Wallet Solution is certified, and the Wallet Solution provider is
registered. Also, the Relying Parties are registered, and a wallet unit needs to be able to authenticate the Relying
Party. In the Age Verification application there is no certification or registration need for the Age Verification App providers or
the Relying Parties.

<!---
NF, Is this correct? It should be high
-->
Onboarding of users to the European Digital Identity Wallet will be facilitated by relying on identity proofing process
comparable to LoA high.

![Figure 8](./docs/media/Figure_8_comparison_with_eudiw.png)
*Figure 8: Differences in trust frameworks depending on the way of making the age verification functionality available.*

The AV app is a stand-alone application that provides only the minimum functions. In case of authentication to the
national eIDAS node, only the age verification attestation is stored. In this application no other ID data is stored
than the ones described in section [data architecture].

![Figure 9](./docs/media/Figure_9_auth_national_eidas.png)

*Figure 9: Authenticating
to a national eIDAS node to receive the age verification attestation.
Additional issuing options are available and analytically presented in
the next sections.*

# 3. Specific Requirements for an Age verification Solution

## 3.1 Age Verification App Requirements
<!---
NF, TODO update using the conventions set in section 1.4
-->
This section defines requirements, that applies to Age Verification Apps:

- The Age Verification App shall be functional on both Android and iOS operating systems and available on
  the Play Store and App Store, respectively.
- The Age Verification App shall verify that an an Attestation Provider is included on the age verification trust
  list and is therefore authorized.
- An Age Verification App shall not store any data other than the proof of age attestation.
- An Age Verification App shall implement the protocols specified in Annex 4
<!---
NF, Do we need the following here?
-->
- The app must always maintain at least 10 proof of age attestation batches in reserve.
<!---
NF, Check the following. It has been re-written
-->
- The Age Verification app shall rely when possible on the device's native cryptographic hardware
  capabilities, such as the Secure Enclave on iOS, or the Trusted Execution Environment (TEE) and Strongbox on Android.
<!---
NF, What does this mean? Is this correct?
-->
- The Age Verification App is provided as a stand-alone app, its Age Verification App Provider SHOULD strive for
  equivalence with Level of Assurance "Substantial" pursuant to Commission Implementing Regulation (EU) 2015/1502.

## 3.2 Proof of age Attestation Requirements

This section lists the requirements to be met by proof of age attestations:

- The Proof of age Attestation shall only contain the attributes included in the data model.
<!---
NF, Is the following correct?
-->
- The proof of age attestation shall be signed in a manner that enables the generation of a zero-knowledge proof;
  otherwise, batch issuing must be utilized.
- The proof of age attestation shall expire after three months and be reissued.
- The Proof of Age attestations shall be compliant with mDoc in CBOR encoding [ISO/IEC TS 23220-2], [ISO/IEC 18013-5],
  except for the attribute schema that shall be compliant with the attribute schema defined in the section Data
  Model.
- The Proof of Age attestation shall use the namespace "eu.europa.ec.agev10n".
- A Photo ID compliant with \[ISO/IEC CD TS 23220-4\] (Annex C of the referenced standard) may be used as a Proof of Age
  attestation provided that its attributes are compliant with the attribute schema defined in the section Data
  Architecture.

## 3.3 Proof of age Attestation Provider Requirements

This section lists the requirements to be met by Attestation Providers:
<!---
NF, Is the following correct?
-->
- The Attestation Provider shall register with the Federation Authority to obtain proper authorization for issuing proof of age
  attestations.
<!---
NF, Is the following correct?
-->
- The Attestation Provider should be a qualified or non-qualified trust service provider (TSP) registered in the
  eIDAS dashboard as a provider of certificates for electronic signature in accordance with Article 22 of 2014/910.

- The Attestation Provider SHALL NOT issue a Proof of Age attestation before verifying the attestation subject's age at
  the Level of Assurance 'substantial' or 'high'.
<!---
NF, The following is impossible. The provider authenticates the user. I suggest to remove
-->
- An Attestation Provider shall not be able to link two issuance transactions to the same age verification app or user.
- The Attestation Provider shall provide support for the protocols defined in Annex 4.

## 3.4 Relying Party (Verifier) Requirements

This section lists the requirements to be met by Relying Parties:

- The Relying Party shall validate the authenticity and the integrity of a proof of age attestation.
- The Relying Party shall validate that the Attestation Provider is authorized to issue proof of age attestations.
<!---
NF, Is the following correct?
-->
- The Relying Party shall evaluate the value of the age_over_18 claim.
<!---
NF, Is the following correct?
-->
- A Relying Party shall not be able to link two presentation transactions to the same user.
- Two Relying Parties shall not be able to link two proof of age attestation presentation transactions to the same user
  by sharing the received presentations.

# 4. Architecture Model

The age verification solution defined in this document is guided by several key
objectives. First and foremost, the solution is intended to support access to online services, which is identified as a
regulatory priority. By focusing on this specific use case, the deployment of the age verification solution can be
accelerated, ensuring that it meets urgent regulatory needs while maintaining the potential for future expansion.

A further goal is to enable rapid and wide-scale deployment. Design decisions are made to support swift development,
integration, and rollout of the solution, making it broadly accessible across the European Union. To ensure inclusion,
the solution is designed for compatibility with all common devices used by Europeans to access online services,
including mobile phones, tablets, laptops, and desktop computers. The primary delivery channel will be a white-label
mobile application.


The solution relies on a device-based proof of age model, leveraging widely available mobile devices such as smartphones
and tablets to store age attestations. This approach supports the goal of rapid deployment and broad accessibility.
Alternative mechanisms for storing and presenting proof of age may be considered for future versions of the solution.

It is also recognized that devices may be shared among multiple users, for example, when a child has access to a
parent’s mobile phone. Such scenarios introduce risks related to privacy, security, misuse, data loss, and limited
parental controls. While the solution may not be able to address all these risks comprehensively, mitigation strategies
are being considered. These include implementing user authentication at the application level, such as requiring an
additional PIN, and encouraging Age Verification App Providers to incorporate further authentication factors as needed.
Although these mitigation measures are not described in detail in this architecture overview, they may be elaborated in
subsequent technical specifications.

Certain elements are currently considered out of scope. Specifically, the use of the solution for purposes other than
access to online services-such as for purchasing age-restricted products, obtaining age-related discounts, or other
applications - is not a current priority. However, where feasible, design choices that allow for broader applicability
of
age verification may be considered. 
<!---
NF, write it better
-->
Additionally, the storage of proof of age attestations by remote services is
excluded from the initial scope, as this would add complexity and potentially hinder rapid deployment. The possibility
of utilizing remote services for attestation storage may be revisited in future iterations, but, for now, technical
standards that necessitate such reliance should be avoided.

In summary, the defined technical architecture design aims to balance regulatory compliance and practical deployment
considerations, while remaining adaptable for future enhancements and broader use cases.

## 4.1 Functional Architecture

The following functional architecture illustrates the different entities within an age verification system and their
relationships to each other. The subsequent section provides a detailed definition of each reference point.

All entities shown in blue are provided by the toolbox, at minimum as mock services. Entities marked in green represent
existing governmental entities. Entities marked in yellow are proprietary. A solution implementation does not
necessarily need to include all components.
<!---
NF, Check figure numbering
-->

![Figure 2](./docs/media/Figure_2_functional_architecture.png)
*Figure 2: Functional Architecture*

## 4.2 Actors and Entities in an Age Verification Solution
<!---
NF, This section is very confusing. Why there are "Attestation Provider" and "Attestation Issuer",  "Age Verification Verifier" and
"Relying Party". I propose to minimize it. 
-->
An age verification solution involves several key actors and entities, each with distinct roles and responsibilities to
ensure secure, interoperable, and privacy-preserving verification of age attributes across digital services.

- **Attestation Provider (Actor)**: An attestation provider operates the Age Attestation Issuer which in turn uses and
  Authentic Source or a 3rd Party Data Source.
- **Authentic Source (Entity)**: A legally recognized and authoritative source from which the issuer extracts the
  necessary attributes to generate the proof of age attestation. Examples include government population registers,
  passport databases or other official registries.
- **Age Attestation Issuer (Entity)**: An authorized entity responsible for generating and issuing proof of age
  attestations (credentials) to users. The issuer validates the user’s age using authentic and trusted information
  sources, such as national identity registers, passports, or other official documents, or trusted 3rd party data
  sources before creating a digitally signed attestation that certifies the user meets the required age threshold.
- **Age Verification App (Entity)**: The Age Verification App is used by individual seeking to access age-restricted
  content or services. The user requests, receives, and manages their proof of age attestation with this app and
  presents the attestation using this app when required to Relying Parties.
- **Age Verification App Provider (Actor)**: Provides the EU-compliant Age Verification App.
- **3rd Party Data Sources (Entity)**: A trusted private data source is a non-governmental, commercially operated entity
  that maintains accurate and verifiable user information, which can be leveraged for age verification purposes (e.g.
  bank, mobile network operators, utility provider).
- **Age Verification Verifier (Entity)**: The verifier requests and validates the proof of
  age attestation presented by the Age Verification App (User), ensuring it is authentic and meets the necessary
  criteria. The verifier connects to other internal systems to implement a business process.
- **Internal Systems (Entity)**: Legacy Systems of a Relying Party.
- **Relying Party (Actor)**: The digital service, platform, or content provider that requires confirmation
  of a user’s age before granting access to age-restricted resources. The Relying Party operates a verifier and other
  internal systems.
- **Trust Provider (Actor)**: Operator of an entity or mechanism responsible for maintaining a trusted list (whitelist)
  of all legally authorized attestation issuers. This ensures that verifiers and users can validate the legitimacy and
  authorization of entities participating in the ecosystem.

### 4.2.1 Interaction Overview
<!---
NF, Update based on the correct roles
-->

- The user initiates an age verification process by enrolling with a credential issuer, which collects the necessary
  evidence from authentic sources or trusted 3rd party privater data sources.

- The attestation issuer generates a proof of age attestation and issues it to the user.

- The user presents the attestation to a verifier of a Relying Party (content provider) when attempting to access
  age-restricted services.

- The verifier checks the validity of the attestation, referencing the trusted list to confirm the issuer’s
  authorization.

<!---
NF, What is a reference point? Probably it means "interfaces", "interactions"
-->
## 4.3 Reference Points

A reference point consists of one or more interfaces of any kind. The following reference points are supported by the
Age Verification Solution:

### 4.3.1 Authentic Source to Issuer Communication
<!---
NF, Update based on the correct roles
-->
Communication flows between an Authentic Source (e.g., government register, trusted private entity) and an Age
Attestation Issuer cross the Rsrc-n reference point.

#### Interface Characteristics

- **Proprietary Design:** The Rsrc-n interface is proprietary, allowing implementers to customize integration based on
  organizational or national requirements.
- **Integration Methods:**
    - HTTP-based APIs: For modern systems, RESTful APIs over HTTPS are supported, ensuring secure data transmission.
    - Legacy System Adapters: A library or middleware component is provided to interface with legacy systems.
- **Complexity:** Typically, a separate issuer must be provided for each authentic or trusted third-party data source,
  as each issuer is required to implement a distinct business process tailored to the respective data source.

### 4.3.2 Issuer to Age Verification App Communication

Communication flows between an Age Attestation Issuer and the Age Verification Application cross the Riss-n reference
point.

#### Protocol Implementation

- OpenID4VCI Compliance: The Riss-n interface implements the OpenID for Verifiable Credential Issuance (OpenID4VCI)
  protocol, ensuring standardized, interoperable credential exchange across issuers and applications.
- Multi-Issuer Support: The verification application can interface with any issuer supporting OpenID4VCI, enabling users
  to obtain attestations from diverse authorized providers (e.g., national eID schemes, banks, or mobile operators).

#### Compliance
<!---
NF, Is the following correct?
-->
This implementation aligns with the EU Digital Identity Wallet's Architecture and Reference Framework (ARF) , ensuring
cross-border interoperability while allowing Member States to maintain control over authorized issuers through
national trusted lists.

### 4.3.3 Age Verification App to Verifier Communication

Communication flows between the Age Verification Application and a Verifier (Relying Party) cross the Rp reference
point.

#### Protocol Implementation

- OpenID4VP Compliance: The Rp interface implements the OpenID for Verifiable Presentations (OpenID4VP) protocol,
  ensuring standardized and interoperable presentation of verifiable credentials.
- Multi-Relying Party Support: The verification application can interact with multiple Relying Parties (verifiers),
  presenting any number of proof of age attestations as required. All such interactions are conducted exclusively via
  the Rp interface.

#### Key Features

- Interoperability: By adopting OpenID4VP, the solution guarantees compatibility with a wide range of verifiers and
  Relying Parties across different domains and jurisdictions.
- Flexible Presentation: Users can present one or more proof of age attestations to various verifiers, supporting
  diverse use cases and regulatory requirements.

### 4.3.4 Age Attestation Provider to Trust Provider Communication

Communication flows between the Age Attestation Provider and the Trust Provider cross the Rtap reference
point.

To distinguish authorized Age Attestation Providers from unauthorized issuers, all providers MUST be registered with the
EU and included in a centrally maintained trusted list. This registration process ensures compliance with EU regulatory,
technical, and security standards.

<!---
NF, Is the registration interface proprietary?  Shouldn't be provided by the commission?
-->
The registration interface follows a proprietary design.

### 4.3.5 Relying Party to Trust Provider or App to Trust Provider Communication

Communication flows between the Relying Party and the Trust Provider cross the Rtap reference
point. Also communication flows between the Age verification App and the Trust Provider cross the Rtap reference
point.

This interface uses HTTP GET Method to retrieve the trusted list.

## 4.4 Supported Enrollment Configurations

In the following two sub-sections, attestation issuance approaches are introduced grouped in:

- Methods with existing Identification, where only Authentication is needed by the citizen
- Methods without existing Identification, where the citizen needs to provide proof of identification and binding to it.

At a high level, the issuing options are illustrated in the figure below. At least one of the options must be
implemented.

![Figure 3](./docs/media/Figure_3_enrollment_options.png)
*Figure 3: Enrollment options with and without existing identification.*
The above architecture diagram is a functional diagram. For examples of physical mappings, see next clause.

### 4.4.1 Enrollment Methods with existing Identification

Enrollment leveraging existing identification mechanisms requires integration with the eIDAS 1.0 infrastructure,
specifically the notified electronic identification (eID) schemes recognized under the eIDAS Regulation.

#### Key Components

1. Notified eID Schemes

   Member States’ nationally recognized eID schemes (e.g., Germany’s eID, Italy’s SPID) serve as the foundational
   identity source. These schemes are pre-approved under eIDAS 1.0 and provide authentication at Substantial or High
   Levels of Assurance (LoA).

2. eIDAS Nodes

   Cross-border authentication is facilitated via eIDAS nodes, which act as gateways between national eID schemes and
   Relying Parties (e.g., service providers). These nodes ensure secure, standardized communication and data exchange.

<!---
NF,Authentication Workflow is not a component
-->
3. Authentication Workflow
<!---
NF, "returns the eIDAS Minimum Data Set to the **issuing party**". Is that correct? Does it mean to the app?
-->
    - The user initiates enrollment by selecting their national eID scheme.
    - The Age Verification App redirects the authentication request to the relevant eIDAS node.
    - The national eID scheme authenticates the user and returns the eIDAS Minimum Data Set (e.g., name, date of birth,
      unique
      identifier) to the issuing party.

#### Technical Standards
<!---
NF, What does it mean here with service providers?
-->
- SAML 2.0: Used for authentication requests and responses between eIDAS nodes and service providers.
- eIDAS Cryptographic Requirements: Compliance with TS 119 461 for identity proofing and cryptographic protocols.

#### Compliance

- Data Minimization: Only the eIDAS Minimum Data Set is transmitted, avoiding unnecessary personal data exposure.
- GDPR Alignment: Enrollment processes adhere to GDPR principles, including user consent and purpose limitation.

The age verification solution comprises two core components to support this:
<!---
NF, Update roles
-->
1. Age Verification Unit: Integrated into the user-facing Age Verification Application to perform age verification
   checks.
2. Age Attestation Issuing Service: A backend service responsible for generating and managing Proof of Age attestations.

#### Component Interactions
<!---
NF, Update roles and put references to annex 4
-->
1. Age Verification Unit
   The Age Verification Unit shall connect to the Attestation Issuing Service via secure, standards-based protocols to:

    - Request age verification attestations
    - Validate attestation authenticity
    - Enforce age-based access policies

2. National Identification Scheme Integration
   The age verification application SHALL implement functionality to verify users’ ages using the national
   identification scheme. This requires:
    - OpenID4VCI Issuing Service: A national implementation compliant with OpenID for Verifiable Credential Issuance (
      OID4VCI)
      draft 15.
    - Attestation Formatting Module: Converts national ID data into interoperable mdoc-based attestation with
      age_over_18
      claims, adhering to ISO/IEC 18013-5 and EUDI Wallet ARF standards.

#### Interconnection Requirements

| Interconnection Type       | Protocol             | Configuration Reference                                                                                                                                                               |
|----------------------------|----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Notified eId (eIDAS 1.0)   | eIDAS Node Connector | [Annex 3.1, PID Rulebook, ARF](https://github.com/eu-digital-identity-wallet/eudi-doc-architecture-and-reference-framework/blob/main/docs/annexes/annex-3/annex-3.01-pid-rulebook.md) |
| National Identity Provider | OIDC/SAML 2.0        | [Annex 3.2, mDL Rulebook, ARF](https://github.com/eu-digital-identity-wallet/eudi-doc-architecture-and-reference-framework/blob/main/docs/annexes/annex-3/annex-3.02-mDL-rulebook.md) 

Implementation Requirements:

- eIDAS 1.0: Support for LoA "Substantial" or higher as per eIDAS Regulation.
- OIDC/SAML: Compliance with OpenID Connect Core 1.0 and SAML 2.0 WebSSO profiles.

The Age Verification white label solution includes options for interconnection to (a) Notified eID (eIDAS 1.0 connector
configuration), and (b) National iDP (OIDC configuration and SAML configuration) that implementors shall configure.

<!---
NF, Do we need this section? 
-->
#### *Relevant technical specifications:*

- Technical specifications on
  the [eIDAS eID Profile](https://ec.europa.eu/digital-building-blocks/sites/display/DIGITAL/eIDAS+eID+Profile).
- The eIDAS
  node [technical documentation](https://ec.europa.eu/digital-building-blocks/sites/display/DIGITAL/eIDAS+eID+Profile )
  is publicly available as is
  the [open-source code for the application](https://ec.europa.eu/digital-building-blocks/sites/display/DIGITAL/eIDAS-Node+version+2.8 ).
  A web service designed to include the *age_over_nn* attribute has been developed and is available on the EUDI Wallet
  GitHub space (see [issuer](https://github.com/eu-digital-identity-wallet/eudi-srv-pid-issuer)
  and [web issuer](https://github.com/eu-digital-identity-wallet/eudi-srv-web-issuing-eudiw-py)).
- Specifications of national electronic identification schemes published by their providers.

### 4.4.2 Enrollment Methods without existing Identification
<!---
NF, Update roles
-->
In the approaches below, the age verification unit shall connect to the attestation issuing service, but it may not need
to connect directly to an existing identity provider service online or to use it fully.

#### Identity Cards and Passports (ICAO 9303)

The Age Verification Application shall interface with physical identity documents compliant with ICAO Doc 9303
specifications for Machine Readable Travel Documents (MRTDs), including:

- National ID cards (TD1/TD2 size per ICAO Doc 9303 Parts 5/6)
- Machine-readable passports (TD3 size per ICAO Doc 9303 Part 4)
- Other ICAO-aligned credentials with integrated circuit chips (contactless ICs)

##### Technical Requirements:

1. Document Reading

    - Support optical character recognition (OCR) of machine-readable zones (MRZ) with error correction per ICAO 9303-3
    - Implement contactless NFC communication for eMRTDs using PACE v2 protocol with Card Access Number (CAN)
      authentication
    - Validate document authenticity through:
        - MRZ checksum verification
        - Cryptographic validation of chip data signatures
        - Cross-referencing with issuing authority validation services

2. Biometric Verification Submodule
    - Perform ISO/IEC 30107-compliant liveness detection to prevent spoofing (masks, photos, deepfakes)
    - Conduct facial recognition matching between:
        - Live capture (selfie/video)
        - Document portrait (optical/NFC chip)
        - Threshold: >=98% similarity score with bias mitigation
3. Age Attestation Mechanism
    - Extract birthdate from MRZ (positions 7-14 in TD3 docs)
    - Generate tamper-evident attestation payload containing the age_over_18 claim.
    - Enforce cryptographic binding between attestation and biometric session data

##### Implementation Considerations:

- Integrate with national identity schemes (e.g., German eID function via AusweisApp)
- The white label app will provide extension interfaces for biometrics and passport reader capabilities. It will also
  build an extension module for identity and passport reading based on an open-source module which implements the
  interfaces.

![Figure 4](./docs/media/Figure_4_sw_interfaces.png)
*Figure 4: Enrollment options with and without existing identification.*

##### *Relevant technical specifications:*

- [ICAO Doc 9303 series (Machine Readable Travel Documents)](https://www.icao.int/publications/Documents/9303_p3_cons_en.pdf)
  and standards referenced therein, such as (see next line)
- ISO/IEC 14443 Identification cards -- Contactless integrated circuit cards -- Proximity cards (ISO/IEC 14443-1,
  ISO/IEC 14443-2, ISO/IEC 14443-3, ISO/IEC 14443-4)
- [Technical Guideline TR-03110-1 Advanced Security Mechanisms for Machine Readable Travel Documents](https://www.bsi.bund.de/SharedDocs/Downloads/EN/BSI/Publications/TechGuidelines/TR03110/BSI_TR-03110_Part-1_V2-1.pdf?__blob=publicationFile&v=1)

#### 3rd Party activation (out of band)
<!---
NF, Update roles
-->
The Age Verification Application may implement an issuance workflow that separates the identity verification process
from the subsequent generation and delivery of the age verification attestation. This can be achieved, for example, by
utilizing a pre-authorized code flow in OpenID4VCI.

With this architecture, identity verification is conducted independently and may take place in various trusted
environments, such as a bank, notary office, citizen service center or equivalent front office location. Upon
successful completion of the identity verification step, the user is issued an authentication token, such as a QR
code, along with a secondary authentication factor. This second factor is derived from information provided during the
front office interaction and may include a PIN code, SMS or email-based one-time password (OTP) or a
similar mechanism.

After completing this procedure, the user is able to use the provided code and second-factor credentials to retrieve
their age verification proof within the application.

##### *Relevant technical specifications:*

- OpenID for Verifiable Credential Issuance (
  Section [3.5.](https://openid.net/specs/openid-4-verifiable-credential-issuance-1_0.html#section-3.5) [Pre-Authorized Code Flow](https://openid.net/specs/openid-4-verifiable-credential-issuance-1_0.html#name-pre-authorized-code-flow))

<!---
NF, Is there any difference with the previous? Probably merge
-->
#### Link to pre-installed apps with age information

The Age Verification Application may support an issuance workflow analogous to the "third-party activation" flow
described previously, leveraging external applications or websites, such as those operated by banks or mobile network
operators, that already conduct identity verification using established processes.

In this model, the third-party application is responsible for generating a pre-authorized code upon successful identity
verification. This code can be incorporated into the user journey through various integration methods, including API
calls for app-to-app communication or QR codes for cross-device activation.

##### *Relevant technical specifications:*

- OpenID for Verifiable Credential Issuance (
  Section [3.5.](https://openid.net/specs/openid-4-verifiable-credential-issuance-1_0.html#section-3.5) [Pre-Authorized Code Flow](https://openid.net/specs/openid-4-verifiable-credential-issuance-1_0.html#name-pre-authorized-code-flow))

## 4.5 Data Model Definition for Proof of Age Attestation

The data model for the Proof of Age attestation is defined as a profile of the attribute schema specified in ISO/IEC
18013-5 (Mobile Driving Licence [mDL] Application) and ISO/IEC 23220-2 (Generalized Personal Identification Attributes).

### 4.5.1 Attribute Handling

The data model implements the rules for handling age_over_NN attributes as defined in ISO/IEC 18013-5 and ISO/IEC
23220-2, in line with the principle of data minimization. Application logic in the wallet or app holding the attestation
must support these rules, in addition to the generic logic for managing electronic attestations.

### 4.5.2 Attribute Set

The attribute set for Proof of Age attestations consists of:

- **Mandatory Attribute:**
    - *age_over_18:* This attribute is present in all Proof of Age attestations and indicates that the
      holder is above a specified age threshold. The value is a boolean value.
    - *issue_date:* Date (and if possible time) when the Proof of Age attestation was issued. The value is a tdate or
      full-date value.
    - *expiry_date:* Date (and if possible time) when the Proof of age attestation will expire. The value is a tdate or
      full-date value.
    - *issuing_authority:* Name of the administrative authority that issued the Proof of Age attestation. The value is a
      tstr value.
    - *issuing_country:* Alpha-2 country code, as specified in ISO 3166-1, of the country or territory of the provider
      of the Proof of Age attestation. The value is a tstr value.

- **Optional Attributes:**
    - *issuing_jurisdiction:* Country subdivision code of the jurisdiction that issued the Proof of Age attestation, as
      specified in ISO 3166-2:2020, Clause 8. The first part of the code shall be the same as the value for the issuing
      country. The value is a tstr value.
    - *portrait:* A reproduction of the Proof of Age attestation user's portrait. The value is a bstr value.

#### Note on Age Attribute Scope

While the technical architecture and data model (based on ISO/IEC 18013-5 and ISO/IEC 23220-2) inherently support the
age_over_NN attribute schema for arbitrary age thresholds (e.g., age_over_14, age_over_65), this specification
explicitly focuses on age_over_18 to address the most common regulatory requirement for age-restricted online services
in the EU.

Extending the solution to additional age thresholds would require:

1. Validation of enrollment methods for the target age group.
2. Updates to national trusted lists to authorize issuers for the new thresholds.
3. Alignment with jurisdictional legal frameworks governing the specific age-based restrictions.

Future iterations of this specification MAY incorporate additional age thresholds as standardized use cases emerge.

## 4.6 Procedures

### 4.6.1 Issuing of Proof of Age batches

Since Proof of Age Attestations are designed for single use, the system must support the issuance of attestations in
batches. It is recommended that each batch consist of thirty (30) attestations.

### 4.6.2 Re-Issuance of Proof of Age Attestations

The re-issuance process varies depending on the method of initial attestation issuance:

- **eID-Based Issuance:**
  When attestations are issued based on an existing electronic identity (eID), it is recommended that re-issuance be
  performed through re-identification of the user. In practice, this means the user must re-authenticate at least once
  every three (3) months, coinciding with the expiration of the attestations, or sooner if all attestations in the batch
  are used before the expiration date.

- **Passport and Identity Card-Based Issuance:**
  For the re-issuance mechanism for attestations issued via passport or identity card, it is recommended that
  re-issuance be
  performed through re-identification of the user. In practice, this means the user must re-authenticate at least once
  every three (3) months, coinciding with the expiration of the attestations, or sooner if all attestations in the batch
  are used before the expiration date.

- **Other Issuance Methods:**
  Re-issuance procedures for other methods of attestation issuance require a re-identification as well.

### 4.6.3 Attestation revocation and validity period

Attestation revocation is not required for age verification purposes. Implementing revocation mechanisms would
significantly increase the complexity for both Proof of Age Attestation providers and Relying Parties.

It is recommended that the Proof of Age Attestation be designed as a single-use credential and remain valid for a
maximum period of three (3) months from the date of issuance. If a revocation mechanism is required, a status list may
be utilized as an effective solution for managing the revocation status of attestations.

Furthermore, it should be noted that Relying Parties are not necessarily required to request a new attestation for each
<!---
NF, Fix chapter
-->
individual transaction, as outlined in Chapter 2.3.2, Section "8. Verification options for subsequent interactions."

### 4.6.4 Validation of Trust

The Digital Services Act (DSA) does not prescribe mandatory trust frameworks for age verification ecosystems. To
minimize operational complexity, the solution prioritizes leveraging existing trust infrastructures over
creating new age verification-specific frameworks. Specifically, the solution aligns with the trust framework
established under the eIDAS Regulation (EU) No 910/2014.

The trust framework for the Proof of Age attestation is based on trusted lists developed and operated pursuant to
Article 22 of the eIDAS Regulation. The trusted lists are available on
the [eIDAS Dashboard](https://eidas.ec.europa.eu/efda/home). Proof of Age Attestation Providers should be either
qualified or non-qualified trust service providers and they should be published in a trusted list that is made available
to the eIDAS Dashboard. Their publication should follow the requirements laid out in Implementing Regulation 2015/1505,
and the Trust Anchor defined therein (Service Digital Identifier) should be used by Relying Parties to validate the
attestation.

![Figure 5](./docs/media/Figure_5_trusted-list.png)
*Figure 5: Enrollment options with and without existing identification.*

The registration of Relying Parties that request age verification, or the registration of Age Verification App Providers
is not required. 
<!---
NF, "Proof of Age Attestation Providers may however set specific conditions as to which apps they can issue
such attestations to" How? Apps are public clients
-->
Proof of Age Attestation Providers may however set specific conditions as to which apps they can issue
such attestations to, and Age Verification App Providers may set similar conditions regarding Relying Parties.

Considering the need for reliability and trust in the age verification solution, a method comparable at least to LoA
Substantial should be required as laid out in Implementing Regulation 2015/1502.

#### Trusted List solution for the Proof of Age Attestation Providers (PAAP)
<!---
NF, Fix roles and acronyms
-->
It must be decided whether the Proof of Age Attestation Provider (PAAP) requires its own dedicated trust anchor CA.
Both implementation options are presented below. The final implementation approach will be defined in a later version of
this document.

**Option 1 – PAAP with trust anchor CA**

- The PAAP (or an entity acting on its behalf) manages the PKI infrastructure – including software, hardware, and HSM –,
  establishes a trust anchor CA, generates a key pair, and issues a certificate for signing or sealing the Proof of Age
  Attestation.
- The Trusted List Provider/Registrar adds the PAAP’s information and corresponding trust anchor to the Trusted List.
- For each PAAP, the Trusted List shall include information about the PAAP, together with its associated trust anchor.

![Figure 6](./docs/media/Figure_6_trusted-list-1.png)
*Figure 6: Trusted List proposed approach*

**Option 2 – PAAP without trust anchor CA**

- The PAAP (or an entity acting on its behalf) generates a key pair for signing or sealing the Proof of Age Attestation,
  along with a PAAP certificate request.
- The Trusted List Provider/Registrar operates the AV trust anchor PKI and issues the PAAP certificate for signing or
  sealing the Proof of Age Attestation (based on the public key contained in the PAAP’s certificate request).
  Information about the PAAP is made publicly available via the eIDAS Dashboard, with a URL to this information included
  in the PAAP certificate.
- The Trusted List shall include only one AV trust anchor.

![Figure 7](./docs/media/Figure_7_trusted-list-2.png)
*Figure 7: Trusted List proposed approach*

Comparison table of the options is shown below.

| Option                                  | Advantages​                                                                                                                                                                                                                                                  | Disadvantages​                                                                                                                                                                                                                                                                                                                               |
|-----------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Option 1 – PAAP with trust anchor CA​   | The PAAP may either operate its own trust anchor CA or choose a third-party trust anchor CA.                                                                                                                                                                 | The Trusted List is expected to be large, as it will include both the trust anchor and corresponding information for each PAAP.​ <br><br>The validity period of the Trusted List will be short, due to the frequent addition/removal of PAAPs.​ <br><br>Consequently, Relying Parties will need to update the Trusted List on a daily basis. |
| Option 2 – PAAP without trust anchor CA | The Trusted List is expected to be very small in size. ​<br><br>The validity period of the Trusted List may be relatively long (e.g., one year), allowing Relying Parties to cache the Trusted List locally and download a new version only upon expiration. | The Trusted List Provider or Registrar is responsible for operating the AV trust anchor PKI. <br><br> Information about the PAAP is not included in the Trusted List; instead, Relying Parties must follow the URL provided in the PAAP certificate to access the relevant PAAP information.                                                 |

# 5. Detailed Technical Architecture
<!---
NF, Fix acronyms. This is the same as section 4. Merge????
-->
The proposed high-level architecture of the age verification solution is presented in Figure 11 (Same Device) and Figure
12 (Cross Device). The Same Device flow means that the User presents their Proof of Age attestation to a Relying Party
interacting with the User (through the web browser or an app) on the same device that the device the Age Verification
App Instance (Age Verification App) resides on.

![Figure 11](./docs/media/Figure_11_solution_same_device.png)
*Figure 11:Age verification solution components, interfaces and protocols – Same Device*

The Cross Device flow means that the User presents their Proof of Age
attestation to a Relying Party interacting with the User on a different device that the device the Age Verification App
resides on. In this case the devices should be in proximity to each other to provide protection against certain attacks.

![Figure 12](./docs/media/Figure_12_solution_cross_device.png)
*Figure 12:Age verification solution components, interfaces and protocols – Cross Device*

## 5.1 Components of an Age Verification Unit

The following have been identified as the core components of an Age
Verification Unit depicted in figure 11 and figure 12:

- User device (UD): A User Device comprises the hardware, operating system, and software environment required to host
  and execute the Wallet Instance. The minimum hardware and software requirements for the User device will be determined
  by the Member States.

- Age Verification Instance (AVI): The app or application installed on a User device, which is an instance of an Age
  Verification Application and belongs to and is controlled by a User. This component implements the core business logic
  and interfaces. It directly interacts with the Secure Storage (see bullets hereafter) to securely manage cryptographic
  assets and execute cryptographic functions, ensuring adequate assurance for authentication.

- Secure Storage (SD): Through the Secure Cryptographic Interface (SCI) it enables the Age Verification Instance to
  communicate with the Secure Storage (SD) on the User device. This interface is specifically designed for managing
  cryptographic assets and executing cryptographic functions. A Wallet Secure Cryptographic Device (WSCD) according
  to [ARF](https://eu-digital-identity-wallet.github.io/eudi-doc-architecture-and-reference-framework/) may play the
  role of an SD. The white label age verification app will by default use on device secure storage (local native
  WSCD [ARF](https://eu-digital-identity-wallet.github.io/eudi-doc-architecture-and-reference-framework/) for ease of
  Distribution and to drive adoption.

## 5.2 Interfaces and protocols of an Age Verification Unit

The interfaces and their respective protocols illustrated in figure 11 and figure 12 are as follows.

a)  The Age Verification App Provider interface (support, and maintenance interface) is used by the Age Verification
Instance to communicate with the Age Verification App Provider. The interface is also used to provide support to the
user and collect metrics for accounting purposes if the Provider chooses to implement such a service in-app.

b)  The User Interface (UI) is the point of interaction and communication between the User and the Age Verification App.

c)  The Attestation Issuance Interface (AII) is based for example on the OpenID4VCI protocol and is used by the Age
Verification App to request the Proof of Age attestation to be stored in Age Verification Unit.

d)  The Web Browser interface is the user interface offered by the Web Browser to the User.

e)  See g)

f)  See g)

g)  The Presentation Interface (PI) empowers Relying Parties to securely request and receive the Proof of Age
attestation from the Age Verification Instance, accommodating both remote and proximity interactions. In a remote flow (
E, F), when a Relying Party requests the Proof of Age attestation provide access to an online service, the process
initiates through either a web browser or a mobile app. For remote presentation flows, the Age Verification Instance
implements ISO/IEC 18013-7 that relies on OpenId4VP. The technical specification W3C-DigCred being currently drafted by
W3C with participation of mobile operating system and web browser manufacturers may also be part of the technical
solution in future versions once it is by default enabled in both operating systems (iOS and Android), to provide a
harmonised API for web browsers and mobile apps relying on the Proof of Age attestation. In contrast, for the proximity
presentation flow (G), it adheres to the ISO/IEC 18013-5 standard.

h) The Secure Cryptographic Interface (SCI) enables the Age Verification Instance to communicate with the Secure
Storage (SD) on the User device. This interface is specifically designed for managing cryptographic assets and executing
cryptographic functions.

## 5.3 Issuer

The issuer SHALL provide the pre-authorized code flow and authorization code flow based on the user’s enrollment
selection (see OpenId4VCI, section 3.4 and 3.4):

- Pre-Authorized Code Flow: Used if the user has already undergone a compliant enrollment process.
- Authorization Code Flow: Used for unidentified users

The issuer SHALL implement a deferred credential endpoint (/deferred_credential) to handle delays caused by external
validations (e.g. registry lookups, manual approvals). If issuance exceeds 5 seconds, the issuer MUST return an HTTP
202 (Accepted) status with a transaction_id and pollable status endpoint (see penId4VCI, section 9).

Due to the current unavailability of ARF-compliant zero-knowledge proofs (ZKPs), the issuer:

- MUST issue proof of age attestations in batches (min. 30 attestations per batch).

The issuer SHALL publish attestation branding details in the /.well-known/openid-credential-issuer metadata:

```json
"display": [  
  {  
    "name": "EU Age Attestation",  
    "logo": {  
    "url": "https://issuer.example/age_attestation_logo.png",  
    "alt_text": "Government-verified age attestation"  
    }  
  }  
]
```

## 5.4 Verifier

When presenting a proof of age attestation, the system SHALL support either:

- Same Device Flow (as defined in Section 3.1 of OpenID4VP), or
- Cross Device Flow (as defined in Section 3.2 of OpenID4VP).

For a detailed description of these workflows, refer to the OpenID4VP specification (see section 3.1 and 3.2).

#### Request Mechanism:

A verifier requesting a proof of age attestation MUST utilize DCQL (Data Claim Query Language), as specified in Section
6 of OpenID4VP, to formulate the query.

#### Post-Validation Checks:

Upon receiving the attestation, the verifier SHALL verify:

- Integrity: The cryptographic integrity of the attestation.
- Authenticity: The authenticity of the attestation.
- Issuer Authorization: The issuer’s authorization status by checking against a pre-defined trust list (EU Age
  Verification Trust Framework).
- DCQL response: Evaluation of the returned value of the DCQL query.

## 5.5 Attestation Format

The ISO mdoc attestation format is used with

- issuerAuth as issuer data authentication, a COSE_Sign1 signature over the MobileSecurityObject (see ISO 23220-4
  7.1.3.4.2.1); containing
    - the Wallets long-lived key in deviceKeyInfo
    - signed hashes in the valueDigests
- deviceSignature as mdoc authentication method, a COSE_Sign1 MAC over the deviceAuthentication data (see ISO 18013-5
  9.1.3.5)
  containing the proof of age attestation data

## 5.6 Cryptographic algorithms

This section defines the cryptographic algorithms that MUST be used and are based on the recommendation of
the [SOG-IS](https://bmi.usercontent.opencode.de/eudi-wallet/eidas-2.0-architekturkonzept/04-privacy-and-security/#cryptographic-algorithms).

- Issuer Signatures, Device Signed:
    - ECDSA using brainpoolP512r1 and SHA-512
    - ECDSA using brainpoolP384r1 and SHA-384
    - ECDSA using brainpoolP256r1 and SHA-256
    - CDSA using secp256r1 and SHA-256
- Key binding/presentation and Verifier Signatures:
    - ECDSA using brainpoolP384r1 and SHA-384
    - ECDSA using secp256r1 and SHA-256
- MAC:
    - ECDH with secp256r1 + hmac-sha2
- Encryption:
    - AES-256-GCM
- Hashing, PKCE (code challenge method):
    - SHA-256
<!---
NF, Move to annex
-->
# 6. Age Verification Profile

This chapter defines a set of requirements for the existing specifications to enable interoperability among Attestation
Providers (APs), Age Verification App Instances (AVIs) and Relying Parties (RPs) where a substantial level of security
and privacy is required. This document is an interoperability profile that can be used by implementations in various
contexts, be it a certain industry or a certain regulatory environment.

This profile is part of the EU Age Verification technical specifications. It builds on existing specifications commonly
used for credential issuance and verification and defines a set of features to be implemented in the context of age
verification

This document outlines a profile for the age verification target architecture, serving as a conceptual framework rather
than a comprehensive specification of all features and requirements. At the current stage of development, not all
functionalities described herein have been implemented in the Age Verification Solution Toolbox. Furthermore, certain
features are also absent in the present development state of the EUDI Wallet libraries. These functionalities will be
developed and integrated incrementally over the course of the project.

The profile uses OpenID for Verifiable Credential Issuance [OID4VCI] and OpenID for Verifiable Presentations [OID4VP] as
the base protocols for issuance and presentation of Credentials, respectively. The attestation format used is ISO
mDoc [ISO18013-5].

A full list of the open standards used in this profile can be found in Overview of the Open Standards Requirements.

## 6.1 Target audience

The intended audience for this document comprises Member States and designated organizations that seek to implement an
age verification solution. This profile, the referenced technical standards and accompanying guidelines are designed to
support governmental authorities as well as commissioned organizations in the development, integration, and deployment
of compliant age verification systems within their respective jurisdictions or operational domains. This approach
ensures that both public sector bodies and their authorized private sector partners are equipped to realize and maintain
robust, privacy-preserving age verification in alignment with European regulatory requirements, age verification digital
services requirements, and digital identity initiatives.

## 6.2 Scope

The primary focus of this profile is on online services, with the intention that age verification should be conducted
digitally in online environments. The solution is specifically designed to enable users to prove their age when
accessing online content, products, or services that are subject to age restrictions. This approach addresses the
increasing need for robust online age verification mechanisms, ensuring that age-restricted content is accessed only by
individuals who meet the required legal age, and supporting compliance with relevant regulatory requirements for online
platforms.

The following aspects are in scope of this interoperability profile:

- Profile of OpenID4VCI to issue ISO mDoc Proof of Age attestations
- Profile of OpenID4VP to present ISO mDoc Proof of Age attestation
- Crypto Suites

Assumptions made are the following:

- The APs and RPs cannot pre-discover Age Verification App’s capability
- The AP is talking to the AVI supporting the features defined in this profile (via AVI invocation mechanism)
- Support for mDoc only as the Proof of Age attestation format
- No personal data, especially no information from personal identification documents such as national ID card, is stored
  within an AVI. Only the Proof of Age attestation, specifically indicating "older than 18", is utilized for age
  verification purposes

## 6.3 Out of Scope

The following items are out of scope for the current version of this profile, but might be added in future versions:

- Support for Level of Assurance (LoA) "high".
- Proof of Age attestation re-issuance (using refresh tokens) and revocation.
- RP registration and trusted lists of RPs.
- AVI attestations and trusted lists of Age Verification App providers.
- Device bound Proof of Age attestations.
- Proof of Age attestation presentation using proximity.
- Profile of OpenID4VCI to issue ISO mDoc [ISO.18013-5] is defined in ISO 23220-3.
- ISO mDoc specific requirements for OpenID for Verifiable Presentations over W3C Digital Credentials API

## 6.4 Standards Requirements

This specification enables interoperable implementations of the following flows:

- Issuance of ISO mDoc Proof of Age attestation using OpenID4VCI
- Presentation of ISO mDoc Proof of Age attestation using OpenID4VP
  Implementations of this specification do not have to implement all of the flows listed above.
  A parameter that is listed as optional to be implemented in a specification that is being profiled (i.e., OpenID4VCI,
  OpenID4VP and ISO mDoc) remains optional unless it is stated otherwise in this specification.

### 6.4.1 OpenID for Verifiable Credential Issuance

* Both AVI and AP MUST support and use [RFC7636] with S256 as the code challenge method
* Both AVI and AP MUST support and use authorization code flow

#### 6.4.1.1 Credential Offer

- As a way to invoke the AVI, at least a custom URL scheme avsp:// MUST be supported.
- The Grant Type `authorization_code` MUST be used as defined in Section 4.1.1 in [OID4VCI]
- For Grant Type `authorization_code`, the Issuer MUST include the scope parameter with value
  `proof_of_age`. The Wallet MUST use the same value in the scope Authorization parameter

#### 6.4.1.2 Authorization Endpoint

- The AVI MUST use the scope parameter with value `proof_of_age`
- The AVI MUST provide the `code_challenge` as per [RFC7636]

#### 6.4.1.3 Token Endpoint

- The AVI MUST provide the `code_verifier` as per [RFC7636]

#### 6.4.1.4 Credential Endpoint

- The AVI MUST use the `proofs` parameter providing an array of proofs of type JWT.

#### 6.4.1.5 AP Metadata

TBD

### 6.4.2 OpenID for Verifiable Presentations profile Requirements

- As a way to invoke the Age Verification App, at least a custom URL scheme avsp:// MUST be supported.
- Response type MUST be `vp_token`
- `response_mode` MUST be `direct_post`
- RP MUST send the request by value (i.e., support for JAR is not required)
- The client identifier scheme MUST be `redirect_uri` followed by the `response_uri`
- A request MUST specify the nonce parameter
- The DCQL query and response as defined in Section 6 of [OID4VP] MUST be used

### 6.4.3 OpenID for Verifiable Presentations over W3C Digital Credentials API

- AVI MUST support the W3C Digital Credentials API using the unsigned requests as defined in Annex A.3.1 of [OID4VP]
- Wallet Invocation is done via the W3C Digital Credentials API or an equivalent platform API. Any other mechanism,
  including Custom URL schemes, MUST NOT be used
- Response mode MUST be `dc_api`
- The Credential Format Identifier MUST be `mso_mdoc`

## 6.5 Crypto Suites

All entities MUST support P-256 (secp256r1) as a key type with ES256 JWT algorithm for signing and signature validation
whenever this profile requires to do so.
SHA256 MUST be supported by all the entities as the hash algorithm to generate and validate the digests in the MDOC VC.
Note: When using this profile with other cryptosuites, it is recommended to be explicit about which entity is required
to support which curve for signing and/or signature validation

## 6.6 Security Considerations

The security considerations in [OID4VCI] and [OID4VP] apply when compatible with Level of Assurance substantial only.

## 6.7 Comparison with other profiles

This section is non-normative

Since this age verification profile aims at achieving equivalence to Level of Assurance (LoA) substantial there is more
flexibility to use faster go to market approaches using the protocols that are mentioned in the EUDI ARF but adopt
configuration and profile parameters that will make implementers work easier and simpler without compromising the scope
and the business needs of the Age Verification solution.

### 6.7.1 Comparison with [HAIP]

#### OpenID for Verifiable Credential Issuance

*Credential offer*

No difference

*Authorization Endpoint*

- HAIP uses PAR [RFC9126]  to authenticate
  a Wallet Instance (referred to as AVI in the Age Verification solution) through an
  attestation verified via a trust chain. This mechanism relies on a predefined trust
  list of recognized solution providers. However, the Age Verification solution does
  not incorporate such a trust list. Using a self-signed
  certificate does not offer any value. For this reason the Age Verification
  Solution does not use PAR

*Token Endpoint*

- HAIP uses Client authentication using JWT Attestations. As already discussed
  this form of client authentication offers value only when combined with trust lists.
  For this reason the Age Verification solution does not use this approach.

*Credential Endpoint*

No difference

#### OpenID for Verifiable Presentations

Most differences compared to HAIP arise from the Age Verification solution’s reliance
on TLS and the Web PKI for authenticating RPs and establishing secure communication
channels. Similarly, when the Digital Credentials API is used, both the operating
system and the browser are considered trusted components. As a result, an additional
layer of authentication and encryption is deemed unnecessary. In more detail:

- HAIP requires encrypted responses (using `direct_post.jwt`) The Age Verification solution
  relies on the TLS for protecting the confidentiality of the `vp_token`, hence it
  uses `direct_post`.
- HAIP uses JAR [RFC 9101]  to protect the
  integrity of the authorization request. However, its effectiveness depends on the
  existence of a trust list of RPs. In the absence of such a list—as is the case with
  the Age Verification solution—an attacker could obtain a valid certificate and
  substitute a legitimate JAR with a malicious one, thereby undermining its intended
  security benefits. For this reason the Age Verification solution does not use JAR
- HAIP `x509_san_dns` and `verifier_attestation` as a client authentication
  scheme. As in the case of JAR: the effectiveness of these schemes depends on the
  existence of a trust list of RPs. For this reason the Age Verification solution
  uses the simpler `redirect_uri` scheme. An alternative could be the use of `x509_san_dns`
  together with the Web PKI, however, any malicious entity can obtain a valid Web PKI
  certificate.

#### OpenID for Verifiable Presentations over W3C Digital Credentials API

- HAIP requires support for signed requests to enable RP authentication through mechanisms beyond the Web PKI. This is
  not a requirement for the Age Verification solution; therefore, unsigned requests are used instead.
- HAIP requires encrypted responses, using the response mode `dc_api.jwt`. The Age Verification solution relies on the
  security of TLS and assumes that the operating system and the browser are trusted. For this reason unencrypted
  responses are used.

## 6.8 Examples

Non-normative examples to be added

# 7. Features of the White-Label Toolbox

This chapter provides an overview on the features of the Age Verification White Label Solution. The white label
solution is a toolbox and offers Age Verification App Providers, Proof of Age Attestation Providers and Proof of age
verifiers (Relying Parties) the opportunity to decide which services and software components they want to use and which
functionalities they may want to supplement with commercial offers.

The white label solution will be implemented based on the open source EUDI Wallet Reference implementation libraries.

![Figure 10](./docs/media/Figure_10_solution.png)
*Figure 10: Solution Overview*

## 7.1	Definitions, Acronyms, and Abbreviations

A list of the abbreviations and acronyms used in this document.

| **Acronym** | **Definition**                                                                                                                                                                                                                                                                                                |
  |-------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Implementer | An implementer is an individual who is responsible for                 translating software designs, specifications, or concepts into functional, operational systems. Presumably this is a contractor of the Age Verification App Provider, Proof of      Age Attestation Provider or Proof of age verifier. 

## 7.2 White Label Application
<!---
NF, Needs update. I guess statements like "The credential-offer flow in
the \[OpenID4VCI\] is not within the scope of the white label solution" are not correct
-->
The White label App shall include key functionality that is highlighted in the following section.

The white label app shall include an enrollment and issuing functionality to support \[OpenID4VCI\] Authorization Code
flow as the main method utilizing eID based authentication using delegation to an eIDAS node or a trusted identity
provider. The credential-offer flow in
the \[OpenID4VCI\] is not within the scope of the white label solution.

Additionally, the white label app shall include functionality to scan the ICAO9303 Machine Readable Zone (MRZ) utilizing
existing open source library capable of scanning or capturing an image of the data page, enabling extraction of the MRZ,
and following utilizing existing open source library to read using NFC and with Basic Access Control ICAO9303 passport
and identity data. Following the app shall be possible to transmit the data to the age verification issuing server. An
interface to obtain travel document results from an external module should replace the aforementioned process,
subsequently forwarding data to the issuing server for digital signature confirmation as age verification.

Implementers\' additional steps may include:

1. Service to check the authenticity of an ICAO9303 ID/Passport is not included. The implementor shall be responsible
   for validating a National eID's and ePassport's authenticity against a national IACA and/or Schengen masterlist. An
   interface to check an issuing authority public certificate will be available in the white label app for this purpose.

2. A liveness and biometrics matching service is not included. Member States may opt to include these features; however,
   the issuing service should connect to it.

Furthermore, the white label app shall include functionality to allow the use of a QR code and/or token following an
out-of-band identification (e.g. over the counter or through an existing app that has already identified the user with a
sufficient level of assurance). The QR code and/or token shall be based on the Pre-Authorized Code Flow of OpenID4VCI.

The white label application features also include the use of obfuscation and code protection of the primary library of
the app in binary format.

Implementers\' additional steps may include:

1. The app configuration and UI shall be further protected using code hardening and runtime application self-protection
   or similar security measures to protect the total application against malicious attacks.

2. The app attestation checks are not included in scope (including anti-root measures, etc.), with implementation
   responsibility resting with the implementers.

The white label app shall include localization and branding capabilities by enhancing the UI per the official languages
of at least three Member States.

The white label app does not include app back-end services, which are considered out of scope. All the data is stored in
the device, not in cloud.

The terms and conditions and the privacy notice shall be included by the Implementers, and the white label app shall
provide a placeholder to facilitate this.

The white label app includes key features for remote presentation using \[OpenID4VP\].

The white label app does not include proximity capabilities (ISO 18013-5).

## 7.3 Age Verification Issuing Service

The age verification issuing service included in the white label solution shall support mDoc and follow \[OpenID4VCI\]
authentication code flow. The issuing service shall support batch issuing for the age over nn attestation.

The claims of the proof of age attestation are statically calculated from the enrollment document. No zero-knowledge
proof is used in the first version. Consequently, when an individual reaches the age of nn, they must obtain a new proof
of age attestation.

The age verification issuing service shall include connectors for eIDAS node with SAML and OpenID Connect integration
points for identity providers. Furthermore, an extra module shall be included in the issuing service that shall receive
passport/id data from the white label app to produce the age over nn attestation. The issuing service will also include
capabilities for Pre-Authorized Code Flow using QR code/token.

Automatic attestation re-issuing is not in scope for the white label app deployment. A refresh token could theoretically
be used to re-issue the proof of age attestation. However, since the claims do not contain the date of birth, the user
would need to repeat the enrollment process to
recalculate the claims. For this reason, refresh tokens are only practical when used in combination with zero-knowledge
proofs. Additionally, revocation is not included in the first version of the issuing service.

Implementers\' additional steps shall include:

The PKI IACA and Document Signer (ISO18013-5 annex B provisions) are not
included in the scope, and the Implementer must implement them.

## 7.4 Trusted list

Issuers' trusted lists (ETSI) shall be deployed and managed by the
Member States.

The white label solution includes a sample trusted list and a validation
service. The white label application and the Relying Party will validate
the trusted list using the validation service.

## 7.5 Age Verification Service

In the toolbox, an age verification service that supports \[OpenID4VP\]
using mDoc age over nn attestation shall be available. The service shall
include an mDoc trust manager to verify the MSO validity and
authenticity. In order to check the authenticity based on ETSI Trusted
lists validation shall be provided by the implementers.

## 7.6 Overview of implementers\' actions for deployment of age verification
<!---
NF, Fix the roles
-->
Depending on the implementers' roles as an issuer, Relying Party or white label app provider, one or more of the
following actions shall be performed:

1. Integrate the issuing service to the national and/or other trusted eID services including eIDAS nodes.

2. The Implementers shall Secure, Protect, Deploy and Maintain the services from the Toolbox for

   a. Issuing Service

   b. Verification Service

3. Finalize app configuration (i.e. translations), compile, secure and publish the Apps from the toolbox.

All configurable features of the white label solution, such as attestation validity time and batch sizes follow the
recommendations defined in the Architecture and technical Specifications document.

Additional operational, security, performance and integration measures will be provided in the next version of this
document.

# 8. FAQ

**According to the technical document, Attestation Providers need to be included in the trust framework of the
solution. To what extent have other stakeholders already been involved in view of the fact that the age attestation
is to be carried out by third parties? Which third parties could be potential attestation provider in Germany and
which of them have already been involved in the development of the app?**

The Age Verification Solution contract includes four options for enrollment. One of these options is the use of
third-party apps. This approach ensures that companies which already perform a KYC (Know Your Customer) process and
have an existing app or service can integrate it seamlessly.

For example, a bank that already holds verified identity information could confirm a user’s identity without
requiring them to undergo a separate verification process. In a German context, a bank such as Sparkasse could issue a
Proof of Age attestation based on their existing customer data. This service would function similarly to how the
KulturPass app is used.

We are pleased to share that Sparkasse is interested in providing such a service, which would facilitate a smooth and
trusted age verification process leveraging their established identity verification capabilities.

**Can existing trust service providers qualify as attestation providers? Do they need to fulfil additional
requirements or is the qualification as trust service provider enough to be used for the age verification app? Or
does the age verification app require less than what is necessary for trust service providers?**

We would like to kindly point out that this topic is already addressed in section 4.5 of the documentation. As stated
there, a separate registration is required in any case. Initially, this registration process is expected to be
carried out manually. Section 4.5 says:

Proof of Age Attestation Providers should be either qualified or non-qualified trust service providers and they
should be published in a trusted list that is made available to the eIDAS Dashboard. Their publication should follow
the requirements laid out in Implementing Regulation 2015/1505, and the Trust Anchor defined therein (Service Digital
Identifier) should be used by Relying Parties to validate the attestation.

The registration of Relying Parties that request age verification, or the registration of Age Verification App
Providers is not required. Proof of Age Attestation Providers may however set specific conditions as to which apps
they can issue such attestations to, and Age Verification App Providers may set similar conditions regarding Relying
Parties.

Considering the need for reliability and trust in the age verification solution, a method comparable at least to LoA
Substantial should be required as laid out in Implementing Regulation 2015/1502.

**Could you please describe and depict the overall architecture of the whole age verification process the app
implements? In particular, a general overview of the minimum necessary institutions/actors/components and the
respective information flow between those institutions/actors/components (e.g. in form of a schematic representation,
flow-charts, information sources/destinations/flows etc.) would be useful.**

In principle, it is mandatory to support at least one enrollment option. For example, this could involve reading data
from the national ID card and providing an issuer that, based on the enrollment data, issues a Proof of Age
attestation.

The specific data collected during enrollment may vary depending on the option selected. Preferably, the issuer
should be operated either by the Member State or by a designated contractor. Furthermore, the issuer must be able to
fulfill the "level of assurance high" requirement-at least for the data flow up to the issuer-since not all necessary
information (such as "over 18") can be read directly from the ID card. The exact requirements depend on the chosen
enrollment method.

It is important to note that the issuer is not permitted to store the personal data. Instead, the issuer should
calculate whether the user is over 18 based on the date of birth and then issue a Proof of Age attestation that
contains only a true/false value for the age verification, without including the actual date of birth or other
personal data.

With a fully implemented EUDI Wallet, where the national ID card has been stored and zero-knowledge proofs are
utilized, it is no longer necessary to pre-calculate the proof of age attestation. Instead, the user can generate the
proof of age dynamically and securely within the wallet whenever required, without revealing their actual date of
birth or other personal information.

The user must install an age verification app on their mobile device. This app can either be published by the Member
State or by an entity that has implemented the EU Age Verification Profile and is recognized by the EU as an Age
Verification App Provider under the Digital Services Act (DSA).

The app itself does not store any ID card data or similar personal information. Instead, it only retains whether the
user is over 18, who issued the credential, and the issuance date. Optionally, it is possible to store a photo, for
example from the passport, but this is not mandatory. Storing the photo would likely require adjustments to
legislation in order to permit reading and saving the photo during the enrollment process.

The Relying Party requires a dedicated software component to receive and verify the user's proof of age attestation.
This component is responsible for ensuring that the attestation is authentic and has not been tampered with by
verifying the digital signature and the integrity of the data. Additionally, it must confirm the actual value of the
age verification-such as whether the user is over 18 years old-as stated in the attestation. An essential part of the
verification process is also to check whether the issuer of the attestation is listed on the official EU Trusted List
for Age Verification Providers, as maintained under the eIDAS Regulation.

To achieve this, the software component must interact with the trust framework based on the trusted lists developed
and operated according to Article 22 of the eIDAS Regulation. These lists are accessible through the eIDAS Dashboard
and provide the necessary trust anchors for validation. The Relying Party’s software should use these trusted lists
to automatically validate both the status and the authorization of the issuer.

Open technical specifications and reference implementations, including a white-label validation service and toolbox,
are being developed to support Relying Parties in integrating these verification capabilities. The verification
process itself can be performed using standard interfaces of OpenID4VP, such as the Presentation Interface, which
enables the secure receipt and validation of the proof of age attestation from the user's age verification app.
Importantly, the Relying Party does not need to register or be certified to use the age verification solution, but
must comply with the technical and trust framework requirements. The attestation only contains the necessary
information-such as a simple true or false value for the age threshold-thereby ensuring privacy and data minimization
throughout the process.

**What adaptions need to be made to the white label app by the MS before publishing it in an app store?**

A Member State can fundamentally decide which enrollment options it wants to offer. For example, if a Member State does
not wish to support reading data from the passport, it can simply remove that option.

Additionally, a Member State can customize the branding or provide further functionality within the framework of
national implementation projects. It is also expected that copyright, terms and conditions, and the privacy policy will
be adapted to national requirements.

In some cases, a Member State might even choose to develop the entire app from scratch, using only the specifications as
a basis.

After selecting the technical features and making local adaptations, the app must be published by the Member State
itself or by a company on its behalf. For this purpose, the publisher must be registered and the app must be certified
in order to be released in the app store (certified by apple or google for the store).

**Will the white label app be available in the German language (or other European languages)?**

The beta version to be released next week will only include English. However, the app is already prepared for
localization. All UI component texts are stored in a resource file, making it easy to add new languages in the future.
This will also be described in the documentation in upcoming versions.

For the release planned in June, we expect to provide support for the official languages of the EU Member States that
receive the in-depth support. Additionally, we are happy to offer German language support for users in Germany.

**Will the white label app support the usage (and technical interfaces) of already existing IDs in Germany? If yes,
which existing IDs? What needs to be done to connect the white label app with the interfaces of existing IDs?**

With the upcoming beta version, we will already demonstrate the option to use a national IDP or national eID, as
implemented in the EUDI Wallet project.

We also plan to support the use of passports for enrollment in the future; however, this option will only become
available later in the year. In addition, there will be the possibility to use third-party apps and tokens, should a
Member State wish to offer these options.

Beyond these options, a Member State may also introduce its own enrollment methods by implementing the relevant
interfaces independently.

**Which interfaces and data formats will the white label app support and what steps need to be done (technically and
organizational) that (i) attestation providers can receive and send their attestations from/to the app and (ii)
online-platforms will be able to accept the white label app via/within their service?**

Within this project, we are delivering a toolbox consisting of various specifications and services. Each Memberstate can
then select which services they wish to use.

In addition to the wallet, we will also provide open source reference implementations for both the issuer and verifier.
These components can be used to receive and process a proof of age attestation.

If a Relying Party prefers to implement this functionality directly, they can use the reference implementation as
documentation and make use of the underlying libraries or start its own implementation based on the specifications.
For the upcoming release of the specification, we plan to propose OpenID4VCI, including batch issuing, for credential
issuance, OpenID4VP for the presentation process, and mdoc as the data format. This approach ensures interoperability
and flexibility for national implementations, while supporting secure and standardized age verification processes across
the EU.

We would also be happy to discuss and propose additional national requirements to help ensure the successful adoption of
age verification solutions. Your input and collaboration are welcome, as they are essential for creating solutions that
meet the diverse needs of all Member States and improves our Age Verification Profile.

**f. Will the white label app initially/automatically follow the app store policies, i.e. Google Play Developer Program
Policies, Google Developer Distribution Agreements and Apple App Store Review Guidelines (especially provisions with
regard to design/interface/user-friendliness/platform conventions? Who will be accountable for those policies with
regard to the white label app, the commission or each MS?**

The Age Verification project will only provide an open source software project. Each memberstate is responsible for
publishing the app. Of course, we strive to comply with the policies of the relevant platforms and to ensure this
through user testing as well.


