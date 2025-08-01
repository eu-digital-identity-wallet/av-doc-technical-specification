# Age Verification Solution Technical Specification

## Overview
The objective is to develop an EU-wide solution to age verification that reinforces the [Digital Services Act (DSA)](https://eur-lex.europa.eu/eli/reg/2022/2065) objective to ensure safe, secure, and trusted digital space—notably Article 28, which focuses on protecting minors—and the [Louvain-la-Neuve Declaration](https://bosa.belgium.be/sites/default/files/content/documents/LLN%20Declaration%20-%20Informal%20Telecom%20Council%20-%20v.12.04.2024.pdf ), which promotes a safer and more trustworthy online environment. The proposed solution is intended to bridge the gap until the [EU Digital Identity (EUDI) Wallets](https://ec.europa.eu/digital-building-blocks/sites/display/EUDIGITALIDENTITYWALLET/EU+Digital+Identity+Wallet+Home) become available by the end of 2026, enabling the incorporation of the age verification functionality in them.

Age verification plays a role across various scenarios, including access to online services, purchases of age-restricted products and claiming age-related benefits. Given the regulatory priorities and societal needs, this documentation focuses on age verification for accessing online services with restricted content or services.

However it must be kept in mind that only reasonable measures can be taken into use, as it is trivial to bypass age verification requirements in general, and if an annoying gate is put in front of the users they will simply search for "free VPN" and give their data to an even more dangerous hostile party.

The privacy, freedom, and security of EU citizens must not be compromised under any circumstances, and there must be no strong dependency on any of the following:

- EU citizens owning a mobile device, as the EU does not provide their citizens with one.
- Any devices being from authorized vendors.
- Any devices running an authorized OS, especially one from a large multinational corporation.
- EU citizens accepting the Terms of Service or Privacy Policy of 3rd party services, especially one from an entity outside the EU.
- Generally any dependency to companies and 3rd party services outside the immediate parties in the transaction, i.e. the user, and the service they are attempting to access.
- Reliable internet access.
- The parties requesting age verification to be vetted, approved, or pre-authorized in any way.

Due to the clearly recognizable danger of citizens being driven to the less safe methods of bypassing any such schemes to access content they wish to access, priority must be on minimizing the desire for anyone to do that.

It must also be recognized that the citizens of EU are constantly under threat of attempts to dismantle basic security measures such as secure encryption, so the privacy and security of transactions must not be dependent on methods that can be neutered by any passing of such laws.

There must be particular care placed on ensuring perfect forward secrecy, and that no identifiable information of who uses what service when must ever be possible to be stored even in case of the verification service being compromised by hostile actors. Similarly breaching the user's devices, or the services being accessed, must provide no means to identify which services have been used by who and when.

As per the guidelines and requirements set by the principles of the EU and the regulations in place, no dominant market position must be rewarded in any way. Citizens must always have the right to choose which devices they own and operate, and what operating system as well as other software runs on those devices, without being punished with any limitations to their freedom to use the services they wish to use.

If the goals set above are deemed not possible to be met, then the project must be marked as failed and a re-evaluation of the requirements must be demanded from the parties that presented them.

This repository contains the technical specifications for the Age Verification Solution, designed for extension, adoption and deployment by member states or other actors. 


## Contents

This repository contains:

- **[Operational, Security, Product, and Architecture Specifications](docs/architecture-and-technical-specifications.md)** that defines the the Age Verification Solution and the scope of the white label solution. It ensures that the solution meets functional, security, and scalability requirements in alignment with business and technical needs.

- **Annexes** include a list of annexes that provide additional information to the main document.
  - [Annex A - Age Verification Profile](docs/annexes/annex-A/annex-A-av-profile.md)
  - [Annex B - Zero Knowledge Proofs for the Age Verification Solution](docs/annexes/annex-B/annex-B-zkp.md)

## Contributing
Contributions are welcome to enhance and refine the specifications. If you would like to contribute to this repository, please follow the [contribution guidelines](CONTRIBUTING.md).

## Contact
For inquiries or collaboration opportunities, please reach out via GitHub Issues.

## Versioning
We use SemVer for versioning. For the versions available, see the [tags](https://github.com/eu-digital-identity-wallet/av-doc-technical-specification/tags) on this repository.

## License
See the LICENCE file for details.
