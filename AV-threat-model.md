# Threat Model — EU Age Verification Solution

This document describes the security and privacy threats considered in the design of the EU Age Verification (AV) solution, the mitigations applied to each threat, and the residual risk that remains together with the rationale for accepting it. The introductory description of each threat is written to be understandable without a technical background; the detailed assessment in each entry is more technical.

## Reference implementation and production deployment

The [technical specification](https://ageverification.dev/av-doc-technical-specification/docs/architecture-and-technical-specifications/) defines the required behaviour and security properties of the EU Age Verification Solution. The accompanying open-source white-label solution is a reference implementation that demonstrates one way to implement the specification, but it is not a production-ready service. Production implementers must complete the deployment-specific security, infrastructure, governance and operational controls identified in this threat model and the production hardening guide; the residual-risk ratings in this document assume that those actions have been completed.

## Contents

- [How the solution works](#how-the-solution-works)  
- [Scope of this threat model](#scope-of-this-threat-model)  
- [The forthcoming EU Age Verification Scheme](#the-forthcoming-eu-age-verification-scheme)  
- [How to read the threat entries](#how-to-read-the-threat-entries)  
- [Threat overview](#threat-overview)  
- [Threat details](#threat-details)  
- [Responsibilities of the production implementer](#responsibilities-of-the-production-implementer)  
- [Risks accepted by design](#risks-accepted-by-design)  
- [Out of scope](#out-of-scope)

## How the solution works

The Age Verification Solution lets a person prove **that they are over a certain age (for example, over 18\)**. No name, no date of birth, no photo, and no other personal information is shared with the online service performing the check.

Four parties are involved:

1. **The user** installs the **Age Verification app** on their phone.  
2. An **Attestation Provider** (the *issuer*) verifies the user's age once and provides the app with a batch of digitally signed *Proofs of Age*. Each proof states only "this person is over 18" (true/false).  
3. When an online service (the **Relying Party**, or *verifier*) needs an age check, the app presents one proof.   
4. The **EU Trusted List** tells verifiers which Attestation Providers are genuine, so a proof can be checked without contacting the issuer — the issuer never knows where the proof was used.

The solution can be used to verify whether a person is over a specified age threshold. For simplicity, age over 18 is used as an example in the threat model. 

## Scope of this threat model

In scope: the mobile app, the issuance of Proofs of Age and the presentation of proofs to online services.

The threats were consolidated from the project's threat analysis work, the current [technical specifications](https://ageverification.dev/av-doc-technical-specification/docs/architecture-and-technical-specifications/), and the [production hardening guide](https://ageverification.dev/Getting%20started/app_implementers_tasks/). 

Assumptions inherited from the specifications:

- The solution targets level of assurance **substantial**. Attacks that are only relevant at level *high* are out of scope.  
- The mobile operating system and the browser are treated as trusted components. A fully compromised device defeats any app running on it (see [Risks accepted by design](#risks-accepted-by-design)).  
- The Web PKI (the certificate system behind HTTPS) is assumed trustworthy; the threat model does not include malicious certificate authorities.  
- The solution is built on standardised protocols such as OID4VCI, OID4VP, the W3C Digital Credentials API and ISO/IEC 18013-7. The internal security of these protocols is covered by their own specifications and security analyses and is **out of scope** here; this document covers only how the solution profiles, combines and deploys them.
- The attestation is recommended to be valid for maximum of three (3) months and issues in batches of thirty (30) single-use attestations (Sections 3.4.1 and 3.4.3 of the specification; both are configurable under §6.5). A deployment that extends these parameters must re-assess every rating that depends on them.

## The forthcoming EU Age Verification Scheme

Commission Recommendation (EU) 2026/1035 of 29 April 2026 on a common framework for EU-wide age verification technologies establishes the EU Age Verification Scheme: the set of rules defining the trust model, governance and the requirements to be fulfilled by providers of proof of age attestations and of age verification solutions. It establishes a two-list trust architecture with the **EU trusted proof of age attestation providers list** and the **EU trusted solutions list** together with the **AVS Unit Attestation (AVSUA)**. The detailed content of the scheme is still under development.

Once in force, the scheme is expected to strengthen the mitigation of several threats in this document by providing a verifiable register of approved apps and solution providers and formal governance, accession and dispute-resolution arrangements for the trust lists. Individual threat entries refer to the scheme where it is expected to be material. Because the scheme is not yet defined, no residual-risk rating in this document depends on it.

## How to read the threat entries

Each threat has a status, shown in the overview table:

| Status | Meaning |
| :---- | :---- |
| **Mitigated** | The threat is addressed and no meaningful residual risk remains. |
| **Residual risk accepted** | Controls reduce the threat, but a known risk remains and has been consciously accepted. |
| **Accepted by design** | The condition results from an intentional design decision and does not require further mitigation within the scope of this solution. |

The **Implementer action required** Column indicates whether the residual-risk assessment depends on additional controls being implemented or operated by the organisation deploying the solution in production.

Within each threat entry, **Mitigations in the design** describes protections provided by the specification or reference implementation. **Required implementer actions** describes controls that must be completed by the production implementer.

Residual risk is rated None / Low / Medium. Where implementer action is required, the rating assumes that all applicable required actions have been completed correctly by the implementer. None of the threats has a residual risk rating above Medium.

## Threat overview

| ID | Threat | Status | Implementer action required | Residual risk |
| :------ | :---- | :---- | :---- | :---- |
| [T-01](#t-01-forging-or-altering-a-proof-of-age) | Forging or altering a Proof of Age | Mitigated | No | None |
| [T-02](#t-02-reusing-or-replaying-a-proof) | Reusing or replaying a proof | Mitigated | No | None |
| [T-03](#t-03-a-fake-issuer) | A fake issuer | Mitigated | Yes | None |
| [T-04](#t-04-issuer-signing-key-compromise) | Issuer signing-key compromise | Residual risk accepted | Yes | Medium |
| [T-05](#t-05-a-fake-or-cloned-app-obtains-proofs) | A fake or cloned app obtains proofs | Residual risk accepted | Yes | Low |
| [T-06](#t-06-a-fake-verifier-phishing) | A fake verifier (phishing) | Residual risk accepted | No | Low |
| [T-07](#t-07-copying-proofs-to-another-device) | Copying proofs to another device | Residual risk accepted | Yes | Medium |
| [T-08](#t-08-an-adult-passes-a-check-for-a-minor) | An adult passes a check for a minor | Residual risk accepted | Yes | Medium |
| [T-09](#t-09-a-minor-uses-an-adults-phone) | A minor uses an adult's phone | Residual risk accepted | Yes | Medium |
| [T-10](#t-10-hijacking-the-handover-between-browser-and-app) | Hijacking the handover between browser and app | Residual risk accepted | Yes | Low |
| [T-11](#t-11-a-tampered-app-or-compromised-phone) | A tampered app or compromised phone | Residual risk accepted | Yes | Medium |
| [T-12](#t-12-eavesdropping-on-network-traffic) | Eavesdropping on network traffic | Residual risk accepted | Yes | Low |
| [T-13](#t-13-tracking-users-across-services) | Tracking users across services | Residual risk accepted | Yes | Low |
| [T-14](#t-14-issuer-and-verifier-collude-to-identify-a-user) | Issuer and verifier collude to identify a user | Residual risk accepted | Yes | Low |
| [T-15](#t-15-cheating-during-enrolment) | Cheating during enrolment | Residual risk accepted | Yes | Medium |
| [T-16](#t-16-exposure-of-personal-data-during-enrolment) | Exposure of personal data during enrolment | Residual risk accepted | Yes | Low |
| [T-17](#t-17-trusted-list-compromise-or-outage) | Trusted List compromise or outage | Residual risk accepted | Yes | Low |
| [T-18](#t-18-weaknesses-in-the-zero-knowledge-proof-mechanism) | Weaknesses in the zero-knowledge proof mechanism | Residual risk accepted | Yes | Low |
| [T-19](#t-19-denial-of-service) | Denial of service | Residual risk accepted | Yes | Low |
| [T-20](#t-20-supply-chain-compromise) | Supply-chain compromise | Residual risk accepted | Yes | Medium |
| [T-21](#t-21-loss-of-the-device) | Loss of the device | Accepted by design | No | None |
| [T-22](#t-22-an-adult-deliberately-provisions-a-minors-phone-with-attestations) | An adult deliberately provisions a minor's phone with attestations | Residual risk accepted | Yes | Medium |
| [T-23](#t-23-reference-or-test-components-deployed-to-production-as-is) | Reference or test components deployed to production as-is | Residual risk accepted | Yes | Low |
| [T-24](#t-24-compromise-of-an-age-verification-app-providers-avsua-signing-key) | Compromise of an App Provider's AVSUA signing key | Residual risk accepted | Yes | Medium |

The distinction between design mitigations and implementer actions is fundamental to this threat model. Design mitigations are provided by the specification or the reference implementation. Required implementer actions are controls that must be added, configured or operated by the organisation deploying the solution in production.

Unless an entry explicitly states otherwise, its residual-risk rating assumes that all required implementer actions in that entry have been completed correctly.

## Threat details

### T-01: Forging or altering a Proof of Age

An attacker fabricates a proof of age from scratch, or takes a genuine one and changes its content.

| Aspect | Assessment |
| :---- | :---- |
| **Threat** | Fabrication or modification of an attestation: the age statement, the validity period, or any other element. |
| **Mitigations in the design** | Every Proof of Age is an ISO/IEC 18013-5 mDoc digitally signed by the Attestation Provider (ECDSA P-256, SHA-256). The verifier validates the signature against the issuer's certificate and the issuer's certificate against the EU Trusted List. Any modification breaks the signature. In the zero-knowledge path, the proof is additionally checked against an accepted, published circuit. |
| **Required implementer actions** | None. Production trust-anchor configuration is covered under [T-17](#t-17-trusted-list-compromise-or-outage). |
| **Residual risk** | None. |
| **Acceptance rationale** | Forging a proof would require breaking standard, well-studied cryptography, or stealing an issuer key (covered separately as [T-04](#t-04-issuer-signing-key-compromise)). |

### T-02: Reusing or replaying a proof

An attacker records a proof as it is presented and presents it again in another session or to another service.

| Aspect | Assessment |
| :---- | :---- |
| **Threat** | Replay of a captured or previously used presentation response. |
| **Mitigations in the design** | Each verification request contains a fresh, unpredictable nonce that the app must incorporate into its response, so a response from one session is invalid in any other. Plain (non-ZKP) proofs are single-use: the app deletes each proof after one presentation, which is why proofs are issued in batches. In the default presentation flow the response is encrypted specifically for the requesting verifier (HPKE, RFC 9180), so a response captured in transit cannot be read or redirected. ZKP presentations are bound to the individual session. |
| **Required implementer actions** | None. Nonce validation is part of the standard behaviour of the verification service. |
| **Residual risk** | None. |
| **Acceptance rationale** | Replay-style abuse involving a willing adult is a distinct threat, covered as [T-08](#t-08-an-adult-passes-a-check-for-a-minor). |

### T-03: A fake issuer

A fraudulent entity sets up a fake "Attestation Provider" to trick users into enrolling with it, or to trick verifiers into accepting its proofs.

| Aspect | Assessment |
| :---- | :---- |
| **Threat** | Impersonation of an Attestation Provider towards users (harvesting enrolment data) or towards verifiers (issuing proofs that appear legitimate). |
| **Mitigations in the design** | Only Attestation Providers registered on the EU Trusted List (an ETSI TS 119 612 signed list published by the European Commission) are accepted. Verifiers are required to validate every proof against this list, so proofs signed by an unregistered issuer are rejected. Issuers must operate under an ETSI EN 319 411-1 (NCP) certificate policy and are registered through a formal process via Member States. The forthcoming EU Age Verification Scheme will place this list under defined governance, accession and dispute-resolution arrangements. |
| **Required implementer actions** | Configure the production Trusted List as the trust-anchor source (never the sample/demo list) — a standing item in the production hardening guide. |
| **Residual risk** | None. |
| **Acceptance rationale** | A fake issuer's proofs are cryptographically worthless to compliant verifiers. The related risk of a fake *app* stealing users' ID data during a bogus enrolment is covered under [T-05](#t-05-a-fake-or-cloned-app-obtains-proofs). |

### T-04: Issuer signing-key compromise

Criminals steal the secret key an issuer uses to sign proofs, enabling them to produce unlimited genuine-looking proofs — including for minors.

| Aspect | Assessment |
| :---- | :---- |
| **Threat** | Compromise of an Attestation Provider signing key. This is the most serious single point of failure on the issuing side, amplified by a deliberate design choice: the system has no revocation mechanism for individual attestations. A stolen key can be abused until the issuer is removed from the Trusted List, and attestations already issued remain technically valid for their lifetime. |
| **Mitigations in the design** | The damage is bounded by design: attestations are valid for at most 3 months, users must re-verify their age at least every 3 months, and a compromised issuer can be removed from the Trusted List, after which verifiers reject its signatures. Issuers must meet ETSI certificate-policy requirements, which include protecting signing keys in certified hardware (HSM). |
| **Required implementer actions** | Issuer side: operate signing keys in an HSM with documented key ceremonies and access control; monitor for anomalous issuance; define an incident procedure for requesting Trusted-List removal. Implementers requiring faster recovery than the 3-month bound may additionally deploy a status list (Attestation Revocation) — permitted by the specification although not required. |
| **Residual risk** | Medium. |
| **Acceptance rationale** | In the worst case, forged proofs could circulate for up to 3 months in deployments without a status list. This is accepted because the attestation guards a comparatively low-stakes yes/no statement (not identity, money, or legal signatures), the validity window is short, and the trust-list removal path exists. Deployments protecting higher-stakes decisions are advised to add revocation. |

### T-05: A fake or cloned app obtains proofs

An attacker builds an app that imitates the Age Verification app to steal users' identity data during enrolment, or to obtain genuine proofs from a real issuer and misuse them.

| Aspect | Assessment |
| :---- | :---- |
| **Threat** | Two variants: (a) a look-alike app in the app stores that harvests the ID documents users scan into it; (b) a modified or fraudulent app that contacts a genuine issuer and requests real attestations. |
| **Mitigations in the design** | In variant (b), the issuer is the control point. Before issuing, an issuer shall verify that the requesting app is a legitimate instance of a compliant Age Verification App; that is, that the app's provider is on the Commission-maintained list of compliant Age Verification apps (to be formalised as the EU trusted solutions list) and that the app conforms to [the AV Profile](https://ageverification.dev/av-doc-technical-specification/docs/annexes/annex-A/annex-A-av-profile/). The AV Profile recommends following the issuance profile of the EUDI Architecture and Reference Framework, including the AVS Unit Attestation (AVSUA) or an internal mechanism achieving the assurance outcomes defined in the AV Profile, Section A.5. Compromise of the key used to sign AVSUAs is addressed separately as [T-24](#t-24-compromise-of-an-age-verification-app-providers-avsua-signing-key). The requirements an app must fulfil to be recognised as compliant are defined by the EU Age Verification Scheme. Together these close variant (b) for correctly configured issuers. Variant (a), a scam app that never contacts a real issuer, cannot be prevented by protocol design; it is countered by app-store policing and public communication, and the harm is limited to what the user enters into the scam app. |
| **Required implementer actions** | Meet the requirements the EU Age Verification Scheme sets for age verification solutions, and complete the [implementer checklist](https://ageverification.dev/Getting%20started/app_implementers_tasks/) in the production hardening guide before publication (platform integrity checking such as Play Integrity / App Attest is one example of the mechanisms covered); register the production app so that issuers can recognise it as compliant; monitor app stores for impersonating apps and request takedowns. |
| **Residual risk** | Low. |
| **Acceptance rationale** | Scam apps are an ecosystem-wide phenomenon affecting all categories of sensitive apps; the design ensures they cannot obtain genuine proofs from compliant issuers. |

### T-06: A fake verifier (phishing)

Any website can ask the app for an age proof as there is no register of verifiers. A fraudulent site can therefore request proofs it has no legitimate need for.

| Aspect | Assessment |
| :---- | :---- |
| **Threat** | The specification deliberately does not require verifiers to register (no trusted list of relying parties), to keep the system open and simple to adopt. Verifier identity therefore rests on ordinary website security (HTTPS/TLS). A hostile site can request a proof in the same way a legitimate one does. |
| **Mitigations in the design** | The decisive mitigation is data minimisation: the only information a hostile verifier can obtain is a single "over 18: yes/no" statement without any personal information or identifier usable for tracking (see [T-13](#t-13-tracking-users-across-services)). The app always shows the user which website is asking and what is requested, and requires the user's approval and authentication before anything is shared. A harvested response cannot be presented elsewhere ([T-02](#t-02-reusing-or-replaying-a-proof)). |
| **Required implementer actions** | None mandatory. |
| **Residual risk** | Low. |
| **Acceptance rationale** | A user can be tricked into proving their age to a site that did not need it. The consequence is the disclosure of one anonymous boolean, which was judged acceptable in exchange for an open ecosystem without a verifier-registration scheme. |

### T-07: Copying proofs to another device

Proofs are stored on the user's phone. Malware, a thief, or the phone's own adult owner extracts them and installs them on another device, for example, a minor's.

| Aspect | Assessment |
| :---- | :---- |
| **Threat** | The current specification does not require hardware-enforced device binding of attestations. Each attestation is bound to a device key, and the app relies on the phone's secure hardware (Secure Enclave, TEE, StrongBox) for key storage when available. An attacker with full control of a device, or a cooperating owner (see [T-08](#t-08-an-adult-passes-a-check-for-a-minor)), may be able to extract attestations and keys on devices without hardware protection or with broken protection (rooted/jailbroken). This limitation is a recorded project risk and a known consequence of the lightweight, on-device architecture. |
| **Mitigations in the design** | Hardware-backed key storage required whenever the device offers it; user authentication (pin code or biometrics) gating every presentation; 3-month validity and mandatory re-identification; issuance requires app/device legitimacy checks (T-05), so extracted material cannot be replenished through a fake app. |
| **Required implementer actions** | This is where implementer hardening matters most: enable runtime application self-protection (RASP) and root/jailbreak detection; prefer StrongBox/Secure Enclave-backed keys; consider the AVSUA Key Attestation, which attests the keys and the keystore holding them; apply the batch-protection guidance in the production hardening guide. These controls are explicitly [assigned to implementers](https://ageverification.dev/Getting%20started/app_implementers_tasks/). |
| **Residual risk** | Medium. |
| **Acceptance rationale** | On a hardened deployment, extraction requires defeating hardware security. On older devices without secure hardware some risk remains; excluding those devices entirely was judged worse for inclusion than accepting the risk, given the bounded validity and low stakes of the attestation. Device-bound attestations remain an open option for future specification versions. |

### T-08: An adult passes a check for a minor

A minor wants to enter an age-restricted site, and an adult completes the age check on their behalf, for example by scanning the QR code shown on the minor's screen.

| Aspect | Assessment |
| :---- | :---- |
| **Threat** | Variants include sharing a QR code with a remote adult, an adult relaying a live verification session to a minor's browser, and organised "verification-as-a-service" abuse. Collusion scenarios were rated among the highest-risk items in the underlying threat analysis. | 
| **Mitigations in the design** | The default presentation flow (W3C Digital Credentials API) includes a proximity check — it verifies that the phone presenting the proof and the browser being verified are near each other, which defeats remote QR-sharing in that flow. QR codes and sessions are short-lived and single-use. Every presentation requires the adult to authenticate (PIN/biometric) and deliberately approve, so verification cannot occur without the adult's active participation each time. Single-use proofs and issuance limits make wholesale proof-sharing operations impractical to scale. |
| **Required implementer actions** | Prefer and promote the DC-API flow with proximity checking; apply issuance velocity limits at the issuer (how many batches one person can obtain per period) to prevent redistribution at scale; consider presentation-time step-up policies. |
| **Residual risk** | Medium. |
| **Acceptance rationale** | A cooperating adult physically next to a minor can always pass a check for them — no remote-verification technology prevents physically co-located collusion, and heavier countermeasures (such as continuous face-scanning at every use) were rejected as disproportionate invasions of privacy. The design goal is to prevent scalable and remote bypass, which it achieves; the remaining risk is shared with every age-verification approach and is addressed by parental controls and education rather than by this protocol. |

### T-09: A minor uses an adult's phone

A child picks up an adult's unlocked phone and uses the already-installed app to pass an age check.

| Aspect | Assessment |
| :---- | :---- |
| **Threat** | Device sharing within households is common, and the phone's unlock code is often known to family members. |
| **Mitigations in the design** | The app shall authenticate its user (PIN or biometrics) before every presentation — unlocking the phone is not sufficient; a separate approval is always required. The specification recommends additional authentication factors where appropriate. Brute-forcing the app PIN is countered by increasing retry delays (verified in penetration testing). |
| **Required implementer actions** | Choose and document the authentication policy (for example biometrics-preferred with PIN fallback, retry limits, wipe thresholds), per the production hardening guide's user-presence-binding section. |
| **Residual risk** | Medium. |
| **Acceptance rationale** | If a child knows the adult's app PIN, or the child's fingerprint/face has been enrolled on the device, the technical control is defeated. No app can fully distinguish family members who share credentials. The risk is accepted as irreducible at the app level and addressed by user guidance and platform parental controls. |

### T-10: Hijacking the handover between browser and app

Age checks pass between a website and the app via links or QR codes. A malicious app or website inserts itself into that handover — receiving what was meant for the genuine app, or causing the user's app to answer an attacker's session.

| Aspect | Assessment |
| :---- | :---- |
| **Threat** | Two variants: (a) link-scheme hijack — the profile requires the app to register the custom URL scheme `av://`, and on both platforms any installed app may claim a custom scheme, so a malicious app could intercept the handover; (b) session relay — an attacker displays to a victim a QR code copied from the attacker's own session at a legitimate site, so that the victim's proof authenticates the attacker's session. |
| **Mitigations in the design** | Sessions are bound with fresh nonces and short lifetimes; the default cross-device flow checks device proximity, defeating remote relay; responses in the default flow are encrypted to the genuine verifier, so an interceptor learns nothing usable. The consent screen shows the requesting website, giving the user an opportunity to detect a mismatch. |
| **Required implementer actions** | Register verified links — Android App Links and iOS Universal Links, which cryptographically bind the link to the genuine app — as the primary invocation method, using `av://` only as fallback. This is a standing item in the [production hardening guide](https://ageverification.dev/Getting%20started/app_implementers_tasks/), together with using system browser components (Custom Tabs / ASWebAuthenticationSession) for authorisation redirects. |
| **Residual risk** | Low. |
| **Acceptance rationale** | With verified links, proximity checks and session binding in place, remaining exposure is limited to devices where the user has installed a malicious handler and also disregards the mismatching consent screen. |

### T-11: A tampered app or compromised phone

An attacker modifies the app itself (removing its protections) or runs it on a rooted or jailbroken phone where the operating system's guarantees no longer hold.

| Aspect | Assessment |
| :---- | :---- |
| **Threat** | A modified app could skip user authentication, fail to delete used proofs, leak the batch, or misrepresent itself to the issuer. On a rooted device, even an unmodified app can be inspected and instrumented (for example with hooking frameworks). The [architecture specification](https://ageverification.dev/av-doc-technical-specification/docs/architecture-and-technical-specifications/) explicitly states that these defences are not part of the blueprint scope (§6.1). |
| **Mitigations in the design** | Issuer-side app legitimacy verification (T-05) prevents tampered apps from obtaining attestations from compliant issuers, provided the AVSUA signing key is itself intact ([T-24](#t-24-compromise-of-an-age-verification-app-providers-avsua-signing-key)); keys in secure hardware cannot be extracted by software tampering alone; the open-source codebase allows public review, and the primary library ships with obfuscation. The forthcoming EU Age Verification Scheme is expected to set security requirements for solution providers, reducing the risk of insecurely maintained derivatives. |
| **Required implementer actions** | The production implementer provides the core mitigation for this threat. Per the production hardening guide: deploy RASP or an equivalent documented protection stack (anti-debugging, anti-hooking, repackaging detection, root/jailbreak detection, integrity self-checks); define a proportionate tamper-response policy (block, step-up, or report); enable screen-capture protection on sensitive screens; treat detections as risk signals verified server-side where applicable. |
| **Residual risk** | Medium. |
| **Acceptance rationale** | The design never relies on the client alone: what a tampered app can obtain is bounded by single-use proofs, short validity, issuance checks, and server-side policy. Accepted on that basis. |

### T-12: Eavesdropping on network traffic

An attacker intercepting internet traffic — a hostile network or a compromised proxy — attempts to read or alter proofs in transit.

| Aspect | Assessment |
| :---- | :---- |
| **Threat** | Interception or modification of issuance and presentation traffic. |
| **Mitigations in the design** | All traffic runs over TLS (HTTPS). In the default presentation flow (DC API), the proof response is additionally encrypted end-to-end to the requesting verifier using HPKE (RFC 9180\) — even a party able to terminate TLS would see only ciphertext. In the fallback flow (OID4VP `direct_post`), the response is protected by TLS only; the specification states this explicitly: "The threat model of the Age Verification Solution does not include malicious CAs. For this reason, `direct_post` response mode is used." PKCE protects the issuance flow; nonces prevent replay of anything intercepted. |
| **Required implementer actions** | Ship release builds with strict TLS validation (no user-installed CAs, no debug proxy overrides); utilise certificate-pinning and operate Certificate Transparency monitoring.|
| **Residual risk** | Low. |
| **Acceptance rationale** | The consciously accepted remainder is the fallback flow's reliance on the Web PKI: an attacker able to obtain fraudulent HTTPS certificates could read a proof response in the fallback flow. Such an attacker (in practice, a rogue certificate authority) is outside the declared threat model, the exposed data is a single anonymous boolean, and an intercepted response is not reusable ([T-02](#t-02-reusing-or-replaying-a-proof)). |

### T-13: Tracking users across services

The age check itself becomes a tracking tool — allowing websites, or third parties behind them, to recognise the same person across sites and visits. Preventing this is a core commitment of the solution.

| Aspect | Assessment |
| :---- | :---- |
| **Threat** | Digital signatures are unique byte strings: if the same signed proof were shown twice, two verifiers could match them. Threats in this family include colluding verifiers comparing received proofs, fingerprinting via validity timestamps, and reuse of the same device key across presentations. |
| **Mitigations in the design** | A layered scheme: (1) zero-knowledge proofs — the app shall use the ZKP mechanism as the preferred presentation method wherever the device supports it; a ZKP reveals no reusable byte pattern, making presentations cryptographically unlinkable even to colluding verifiers; (2) for the plain fallback, batch issuance and strict single-use — each of the \~30 proofs in a batch is a fresh signature shown at most once; (3) validity timestamps are normalised across a batch so they cannot serve as a fingerprint; (4) the attestation contains only the age statement — deliberately no user identifier, no serial number visible to verifiers, no other correlatable attribute; (5) verifiers are required to accept both ZKP and plain-fallback presentations, so devices that cannot produce a ZKP are not excluded. |
| **Required implementer actions** | Keep telemetry free of stable identifiers that could relink presentations (hardening guide, telemetry section). Verifiers must not add trackers or fingerprinting to age-check pages (a regulatory/deployment matter). |
| **Residual risk** | Low. |
| **Acceptance rationale** | Two bounded remainders: (a) devices that cannot run ZKP use the fallback, where linkability protection rests on single-use discipline — batch exhaustion or implementation faults could reintroduce short-horizon linkage; this shrinks as ZKP-capable platforms spread. (b) The issuer's identity is visible in the fallback presentation, typically revealing the user's Member State — see [Risks accepted by design](#risks-accepted-by-design). Neither remainder identifies the user. |

### T-14: Issuer and verifier collude to identify a user

The issuer knows who the user is (it verified their age). The issuer and a verifier — or a compelled issuer — combine their records to determine that a specific person visited a specific site.

| Aspect | Assessment |
| :---- | :---- |
| **Threat** | If an issuer retained copies (or hashes) of the exact proofs issued to each person, and a verifier shared received proofs with it, the issuer could match them in the plain-mDoc flow. This is a LINDDUN linkability threat rated high in the underlying analysis. |
| **Mitigations in the design** | ZKP presentations defeat this attack entirely — there is nothing on the verifier side to match against issuance records. The issuer is not required to store any permanent record of issued attestations, and the data-minimisation principle directs it not to. The verifier never contacts the issuer during verification, so no per-verification trace reaches the issuer. |
| **Required implementer actions** | Adopt and document an issuer retention policy: no per-attestation fingerprints; pseudonymous, time-limited issuance logs; cover this in the deployment's DPIA. Verifiers must not retain proof bytes after the verification decision. |
| **Residual risk** | Low. |
| **Acceptance rationale** | In the fallback flow, a deliberately non-compliant issuer that secretly retains fingerprints could link presentations reported to it. This requires active wrongdoing by a regulated, audited entity (a Trusted-List-registered provider subject to GDPR and national supervision) plus verifier cooperation — a legal and governance problem more than a technical one. Accepted on the strength of governance controls and the shrinking share of fallback presentations. |

### T-15: Cheating during enrolment

A minor obtains a genuine "over 18" proof by cheating at the start: using a borrowed, forged or stolen identity document, or defeating the face check with a photo, mask or deepfake.

| Aspect | Assessment |
| :---- | :---- |
| **Threat** | Enrolment happens either through an existing eID (which carries its own assurance) or by scanning a passport/ID card (reading the chip via NFC) combined with liveness check and a face match against the document photo. Attack vectors: forged or cloned documents, use of another person's document, and presentation attacks against the liveness check. |
| **Mitigations in the design** | Passport/ID chips are read using ICAO 9303 protocols whose signatures can be validated against national certificate authorities; the specification requires a biometric match of at least 98 % and liveness detection per ISO/IEC 30107\. The eID path inherits the notified eID scheme's level of assurance. Re-identification every 3 months limits the payoff of a single successful fraud. |
| **Required implementer actions** | The production implementer provides the core mitigation for this threat. This flow is deliberately not complete in the white-label toolbox. Per architecture spec §6.1, implementers shall validate document authenticity against national IACA certificates and/or the Schengen masterlist (the toolbox provides the interface, not the service). A liveness/biometric matching service is likewise not included; a deployment offering the passport-based flow needs to procure one and connect the issuing service to it. On-device passport-based enrolment of the blueprint does not reach level of assurance substantial — see [T-23](#t-23-reference-or-test-components-deployed-to-production-as-is). |
| **Residual risk** | Medium. |
| **Acceptance rationale** | Accepted given the bounded consequence (an "over 18" boolean, not an identity), the 3-month re-check, and continuing improvement of detection technology. Deployments should track the presentation-attack-detection certification level of their chosen biometric vendor. |

### T-16: Exposure of personal data during enrolment

Enrolment is the one moment real personal data exists in the system (a passport photo, a date of birth). That data leaks — from the app or from the issuer.

| Aspect | Assessment |
| :---- | :---- |
| **Threat** | After enrolment the app stores no personal data — only the signed yes/no attestations. The issuer sees identity data transiently to compute "over 18". The threat is retention beyond need, breach of issuer systems, or capture artefacts (images in caches, logs, crash reports) on the device. |
| **Mitigations in the design** | Data minimisation as a hard design rule: the attestation shall not include any attribute beyond the age statement(s); no date of birth is stored in the app; the issuer is not required to retain anything permanent. |
| **Required implementer actions** | Enforce retention limits at the issuer (for example, discard document images immediately after the attestation is issued; keep only pseudonymous, time-limited issuance logs); handle on-device captures in memory with deterministic cleanup on every exit path; conduct and publish a DPIA before go-live (a national-deployment obligation outside the toolbox's scope); provide the production privacy notice (the white-label app ships a placeholder only). |
| **Residual risk** | Low. |
| **Acceptance rationale** | A breach window exists only around the enrolment moment and only at parties that already lawfully process identity data. GDPR obligations apply to issuers independently of this specification. |

### T-17: Trusted List compromise or outage

The system's trust depends on an EU-published list of approved issuers. That list is tampered with, or becomes unavailable.

| Aspect | Assessment |
| :---- | :---- |
| **Threat** | Tampering: an attacker able to alter the list could insert a rogue issuer (see also [T-04](#t-04-issuer-signing-key-compromise)). Outage: verifiers that cannot refresh the list must choose between rejecting all users (service outage) and trusting a stale list. |
| **Mitigations in the design** | The list is digitally signed (ETSI TS 119 612\) and verifiers are required to validate the signature — hosting or CDN compromise therefore cannot silently alter it; only compromise of the Commission's signing key would, and that key is tightly held and ceremonially managed. The list changes slowly, so caching is effective against outages. The forthcoming EU Age Verification Scheme is expected to define the operational and dispute-resolution model for the trust lists. |
| **Required implementer actions** | Verifiers should cache the signed list and define a documented freshness/failure policy (for example, fail closed once the cache exceeds \~24 hours); pin the list-signing certificate; monitor list availability. |
| **Residual risk** | Low. |
| **Acceptance rationale** | Signing-key compromise at the Commission would be a severe but detectable event handled through the eIDAS governance framework; availability risk is operational and well controlled by caching. |

### T-18: Weaknesses in the zero-knowledge proof mechanism

The privacy of the solution relies on relatively new cryptography. The ZKP scheme contains a flaw, or a corrupted version of it is distributed.

| Aspect | Assessment |
| :---- | :---- |
| **Threat** | Four concerns: (a) soundness — a flawed circuit accepting a false "over 18"; (b) substitution — a malicious build shipping a backdoored circuit; (c) maturity and platform issues; (d) integrity or availability of the accepted-circuit set itself — the set of circuits accepted for the purposes of the profile, identified by their circuit hashes, is published and maintained by the scheme owner separately from the profile (AV Profile §A.8). Tampering with that set could smuggle in a malicious or flawed circuit; unavailability prevents Relying Parties from validating newly published circuits. |
| **Mitigations in the design** | The profile fixes a single audited scheme, `longfellow-libzk-v1` (Longfellow ECDSA anonymous credentials): peer-reviewed academic basis (IACR Communications in Cryptology, vol. 3, no. 1, 2026) and external security reviews by Trail of Bits (2025), ISRG (2025) and an academic panel (2025), with identified issues fixed in subsequent releases. A Relying Party shall verify the circuit hash against the set of accepted circuits *before* verifying the proof, and shall reject a presentation generated using a circuit that is not among them (§A.8) — verification therefore fails closed, which defeats circuit substitution. The accepted-circuit set changes rarely and is cacheable, limiting the impact of an outage. The plain mDoc fallback means a discovered ZKP flaw, or an unusable circuit set, can be handled by disabling the ZKP path without breaking age verification: Relying Parties shall accept the fallback presentation and shall not reject a presentation solely because it uses it (§4.4, §A.9). |
| **Required implementer actions** | Obtain the accepted-circuit set only from the authoritative channel published by the scheme owner and verify its authenticity; cache it with a documented freshness policy; keep the ZKP library updated. |
| **Residual risk** | Low. |
| **Acceptance rationale** | As with all deployed cryptography, future cryptanalysis cannot be excluded; the layered fallback and the ability to rotate accepted circuits provide the recovery path. For concern (d), tampering is bounded by fail-closed verification and an outage degrades privacy rather than function, because the plain mDoc fallback remains available and must be accepted. The governance, publication and signing arrangements for the accepted-circuit set are still under development; this rating is to be revisited once they are settled, and if the set acquires its own governance and trust anchor it may warrant a separate entry. |

### T-19: Denial of service

Attackers overload the issuer (preventing enrolment) or a verifier (preventing age checks), or drain a user's batch of proofs.

| Aspect | Assessment |
| :---- | :---- |
| **Threat** | Issuance endpoints, verification endpoints, and the per-user proof batch are finite resources. Signature and ZKP verification are computationally non-trivial, which an attacker can exploit. |
| **Mitigations in the design** | Sessions are inexpensive to reject early (nonce mismatch, malformed requests); batch issuance means users are unaffected by short issuer outages (they hold \~30 proofs); a hostile verifier repeatedly requesting presentations still needs the user's explicit approval each time, limiting batch-drain attacks to social engineering. |
| **Required implementer actions** | The production implementer provides the core mitigation for this threat. Standard service protection at issuer and verifier: rate limiting, anti-automation controls, capacity planning, early rejection of malformed requests; issuer-side per-user issuance velocity limits (also relevant to [T-08](#t-08-an-adult-passes-a-check-for-a-minor)). These are conventional operational duties assigned to whoever runs the services ("Implementers SHALL Secure, Protect, Deploy and Maintain" the issuing and verification services, spec §6.5). |
| **Residual risk** | Low. |
| **Acceptance rationale** | Denial of service degrades availability, not security or privacy — no false "over 18" result can be produced by it. |

### T-20: Supply-chain compromise

The solution is open source and will be built into many national apps. An attacker poisons the software supply chain — a dependency, a build server, an SDK — and a compromised app reaches users.

| Aspect | Assessment |
| :---- | :---- |
| **Threat** | Attack surface includes third-party libraries, build pipelines, app-store signing keys |
| **Mitigations in the design** | The codebase is public and reviewable; releases are tagged; the ZKP circuit is pinned by hash at verifiers ([T-18](#t-18-weaknesses-in-the-zero-knowledge-proof-mechanism)), so even a compromised app cannot substitute the proof mathematics; issuer-side app legitimacy checks mean a tampered fork cannot obtain attestations from compliant issuers without also compromising the implementer's app identity. The forthcoming EU trusted solutions list is expected to add scheme-level requirements on solution providers. |
| **Required implementer actions** | The production implementer provides the core mitigation for this threat. Per the hardening guide: controlled signing-key custody (HSM-backed, auditable); signed build artefacts tied to tagged revisions with dependency lockfiles and an SBOM; separation of upload/signing keys from CI logs; a force-update and kill-switch mechanism with signed configuration, so a compromised released version can be remotely deprecated; monitoring of distribution channels for tampered builds. |
| **Residual risk** | Medium. |
| **Acceptance rationale** | Supply-chain attacks are among the hardest threats for any software project; the mitigations make compromise detectable and recoverable (kill-switch, forced update, short attestation validity) rather than impossible. Accepted with the force-update capability as the essential safety net — which is why the hardening guide treats it as a baseline consideration. |

### T-21: Loss of the device

A user loses their phone, or it breaks. There is no backup of the proofs. Included here because the absence of backup is sometimes perceived as a flaw.

| Aspect | Assessment |
| :---- | :---- |
| **Threat** | Loss of access to issued proofs; potential exposure of proofs on the lost device. |
| **Mitigations in the design** | There is deliberately no cloud backup — a backup would create a copy of attestations outside the secure hardware, weakening the [T-07](#t-07-copying-proofs-to-another-device) protections, and would require account infrastructure at odds with the no-central-database design. The consequence is fully bounded: the user re-enrols on the new device. Proofs on the lost phone remain protected by the device unlock plus the app's own authentication, expire within 3 months at most, and contain no personal data even if extracted. |
| **Required implementer actions** | None. |
| **Residual risk** | None. |
| **Acceptance rationale** | An availability inconvenience for one user at a time, consciously chosen in exchange for stronger privacy and security.|

### T-22: An adult deliberately provisions a minor's phone with attestations

An adult enrols with their own identity on a minor's phone or passes the enrolment credentials to it, so that the minor holds a batch of valid "over 18" proofs and can pass age checks independently, without the adult being involved in each check.

| Aspect | Assessment |
| :---- | :---- |
| **Threat** | Unlike [T-08](#t-08-an-adult-passes-a-check-for-a-minor), the adult does not participate in each verification. Provisioning can happen in several ways: the adult completes enrolment on the minor's device in person; the adult shares their eID credentials with the minor temporarily and remotely, so that enrolment runs on the minor's device with the adult's identity — the eID path involves no face match on the enrolling device; or, in over-the-counter issuance via an OID4VCI credential offer with a pre-authorized code, the adult is identified at the counter but forwards the credential offer and transaction code to the minor's device. After provisioning, the minor controls the app, its PIN/biometrics and the full batch of proofs; the mandated user authentication at presentation does not mitigate this variant, because the "user" the app authenticates is the minor. Attestations are not biometrically bound to the enrolled person at presentation time, so once issued to a device they serve whoever holds it. An organised form of this is "attestation as a service": adults are paid to complete identity verification for issuance requests that in reality serve other people's devices. |
| **Mitigations in the design** | The design bounds this abuse in time and scale rather than preventing it outright. Only the passport path binds enrolment to the adult's physical presence (face match and liveness on the enrolling device); the eID path relies on the eID scheme's own protections against credential sharing, and the pre-authorized-code path on the transaction code being delivered to the identified person over a second channel. Issuer-side app verification (AVSUA, see [T-05](#t-05-a-fake-or-cloned-app-obtains-proofs)) constrains the organised variant: because issuance only succeeds into a genuine app instance on a real physical device, each paid enrolment provisions exactly one device and cannot be multiplied. This constraint depends on the integrity of the AVSUA signing key ([T-24](#t-24-compromise-of-an-age-verification-app-providers-avsua-signing-key)). In all paths, attestations are valid for at most 3 months and re-identification is required at least every 3 months, so provisioning must be deliberately repeated to keep a device supplied; a batch is single-use and finite (~30 proofs) and cannot be refreshed without re-enrolment; issuance velocity limits cap how many batches — and thus how many devices — one person can keep provisioned. Together these directly cap the economics of an attestation-as-a-service operation: one adult can serve only a few devices, each requiring renewed effort every 3 months. |
| **Required implementer actions** | Apply per-person issuance velocity limits at the issuer (see also [T-08](#t-08-an-adult-passes-a-check-for-a-minor)); implement anomaly detection at the issuer for repeated re-enrolments across many devices by the same person; verify app legitimacy (AVSUA or an internal mechanism of equivalent assurance of equivalent assurance) at every issuance, not only at first enrolment; prefer eID methods whose authentication is bound to the holder (for example a biometrically gated eID app) over shareable static codes where nationally available; for over-the-counter issuance, keep credential offers and transaction codes short-lived, single-use and rate-limited, use sufficient transaction-code entropy, and deliver the code over a channel tied to the identified person; user communication making clear that enrolling on another person's device — or sharing eID codes — to circumvent age limits defeats the purpose of the system and may have legal consequences under national law. |
| **Residual risk** | Medium. |
| **Acceptance rationale** | This is the provisioning form of the collusion risk accepted in [T-08](#t-08-an-adult-passes-a-check-for-a-minor): a willing adult can always give a minor access, whether per check, per batch, or by lending credentials. Closing it would require binding each presentation to the enrolled person's biometrics, which was rejected as a disproportionate invasion of privacy for a yes/no age check. The design instead limits the duration (3-month validity, mandatory re-identification) and the scale (single-use batches, per-device issuance, velocity limits) of the abuse — individual family-level collusion remains possible but does not scale into a service. The remaining risk is addressed by parental controls, education and national enforcement. |

### T-23: Reference or test components deployed to production as-is

The blueprint ships with demonstration infrastructure — a mock issuer, a test wallet-provider service, a sample trusted list and a test verifier — and with reference flows intended for evaluation. The risk is that a production deployment goes live still depending on some of these, or without completing the production hardening steps, because the blueprint reference is mistaken for a finished product.

| Aspect | Assessment |
| :---- | :---- |
| **Threat** | Examples of the same root cause: (a) a production app or verifier still points at test or mock endpoints, or trusts the sample trusted list — a sample trust anchor accepts test issuers, which bypasses the entire trust framework; (b) the reference on-device-only passport verification is shipped to production, although it does not reach the required level of assurance *substantial* and can be manipulated by the device holder (the issuer receives no independent evidence that the checks were performed); (c) the app is published without completing the implementer checklist, leaving production controls (runtime protection, integrity checks, production endpoints, signing) unconfigured. |
| **Mitigations in the design** | Demo and production are clearly separated in the documentation, and the mock services are identified as such. The architecture specification (§6) deliberately assigns the production duties to implementers, and the production hardening guide provides a pre-release security review checklist, including inspection of the release artefact to confirm that no mock endpoints or test trust anchors remain. The production Trusted List is signed, so a verifier correctly configured against it rejects test issuers. The forthcoming EU Age Verification Scheme and trusted solutions list add an external gate: a solution that has not met the scheme's requirements should not be recognised, and issuers shall not issue to unrecognised apps (T-05). |
| **Required implementer actions** | The production implementer provides the core mitigation for this threat. Complete the production security review checklist before store submission; verify on the release artefact that no demo or test endpoints and no sample trust anchors remain in production configuration; configure the production Trusted List as the only trust-anchor source; replace the on-device-only passport flow with an enrolment method that reaches at least level of assurance substantial (a notified eID, or passport onboarding with a server-side component for document and biometric verification); keep test and production environments, trust anchors and signing identities strictly separated. |
| **Residual risk** | Low. |
| **Acceptance rationale** | Every item in this entry is fully controllable by the implementing party, and the checklist makes the required steps explicit. What remains is process error, countered by release-artefact verification and, in future, by scheme-level conformance checks. The rating assumes the checklist is completed; a deployment that skips it is outside the assurances of this threat model. |

### T-24: Compromise of an Age Verification App Provider's AVSUA signing key

Before issuing proofs, an Attestation Provider checks that the requesting app instance is
genuine. That check rests on a signature made by the app's provider. An attacker who steals
the provider's signing key can make tampered apps, emulators or bots look genuine to every
Attestation Provider.

| Aspect | Assessment |
| :---- | :---- |
| **Threat** | Compromise of the key an Age Verification App Provider uses to sign AVS Unit Attestations (AVSUAs) — the certificate lodged with the Commission under specification §4.2. This is the app-side mirror of [T-04](#t-04-issuer-signing-key-compromise). The AVSUA is what an Attestation Provider relies on to distinguish a genuine, untampered instance from a repackaged app, an instrumented device or an emulator farm ([T-05](#t-05-a-fake-or-cloned-app-obtains-proofs), [T-11](#t-11-a-tampered-app-or-compromised-phone), [T-22](#t-22-an-adult-deliberately-provisions-a-minors-phone-with-attestations)). An attacker holding the key can mint AVIAs and Key Attestations that validate correctly, re-opening scalable issuance abuse. Where an Attestation Provider and an App Provider use an internal mechanism instead of the AVSUA (permitted by §4.3), the equivalent signing key or shared secret carries the same exposure. |
| **Mitigations in the design** | The abuse window is bounded and centrally recoverable. The AVIA has a time-to-live of under 24 hours (AV Profile §A.5), so sustained abuse requires continuous re-signing rather than a one-off theft. Validation is anchored in the certificate lodged with the Commission and published in the list of compliant Age Verification Apps (§4.2, §3.4.4), so replacing or withdrawing the certificate cuts the attacker off for all Attestation Providers at once. Attestation Providers validate against the *current* list at every issuance and refuse issuance where the solution or version has been withdrawn (§4.3). Individual app instances are revocable through the `client_status` mechanism (AV Profile §A.5). Proofs already obtained by fraudulent instances expire within the standard validity window (3 months) and, in the plain fallback, are single-use. Issuer-side issuance velocity limits bound the rate of abuse. |
| **Required implementer actions** | Operate AVSUA signing keys in an HSM with documented key ceremonies and access control; keep Key Attestation validity short, since §A.5 permits a KA lifetime longer than the AVIA's 24-hour bound and a long-lived KA weakens that bound; monitor AVSUA issuance for anomalies (volume, device-model distribution, integrity-signal patterns); maintain an incident procedure for lodging a replacement certificate with the Commission and withdrawing the compromised one; use `client_status` to revoke individual instances. Attestation Providers: validate AVSUA signatures against the current lodged-certificate list at every issuance, never against a cached copy of indefinite age. Where an internal mechanism is used instead of the AVSUA, apply equivalent key custody, monitoring and revocation controls. |
| **Residual risk** | Medium. |
| **Acceptance rationale** | Same shape as [T-04](#t-04-issuer-signing-key-compromise): a stolen signing key produces valid-looking evidence until the trust anchor is changed. Accepted on the same grounds — the exposure is bounded in time by the sub-24-hour AVIA TTL and by the 3-month proof validity, recovery is central and does not depend on every Attestation Provider acting individually, and the protected decision is a yes/no age statement rather than identity, money or a legal signature. The rating assumes the KA validity guidance above is followed; a deployment issuing long-lived Key Attestations should re-assess. |

## Responsibilities of the production implementer

The white-label solution is deliberately a reference implementation and deployment toolbox, not a finished production product. Conformance with the technical specification does not by itself complete the security, operational or governance work required for production deployment. The architecture specification, production hardening guide and threat entries assign the following mandatory deployment controls to the production implementer.

| # | Implementer duty | Protects against | Source |
| :---- | :---- | :---- | :---- |
| 1 | App hardening: RASP, code hardening, anti-tamper, root/jailbreak detection, screen-capture policy | [T-07](#t-07-copying-proofs-to-another-device), [T-11](#t-11-a-tampered-app-or-compromised-phone) | Spec §6.1; hardening guide Part 3 |
| 2 | App/device integrity: Play Integrity and App Attest production setup, server-side verdict verification, AVSUA verified at every issuance | [T-05](#t-05-a-fake-or-cloned-app-obtains-proofs), [T-07](#t-07-copying-proofs-to-another-device), [T-11](#t-11-a-tampered-app-or-compromised-phone), [T-22](#t-22-an-adult-deliberately-provisions-a-minors-phone-with-attestations) | Hardening guide Part 4 |
| 3 | Hardware-backed key policy (StrongBox/Secure Enclave preferred; documented fallback behaviour) and user-authentication policy (biometric/PIN, retry limits) | [T-07](#t-07-copying-proofs-to-another-device), [T-09](#t-09-a-minor-uses-an-adults-phone) | Hardening guide Part 4 |
| 4 | Replace all mock/demo endpoints; configure production issuer, wallet-provider server and the production AV Trusted List; keep test and production environments, trust anchors and signing identities strictly separated | [T-03](#t-03-a-fake-issuer), [T-17](#t-17-trusted-list-compromise-or-outage), [T-23](#t-23-reference-or-test-components-deployed-to-production-as-is) | Hardening guide Part 5 |
| 5 | Verified App Links / Universal Links for redirect handoffs; system browser components; strict TLS; documented certificate-pinning decision | [T-10](#t-10-hijacking-the-handover-between-browser-and-app),[T-12](#t-12-eavesdropping-on-network-traffic) | Hardening guide Part 5 |
| 6 | Force-update and kill-switch with signed configuration and key rotation | [T-20](#t-20-supply-chain-compromise), [T-04](#t-04-issuer-signing-key-compromise) | Hardening guide Part 6 |
| 7 | Privacy-preserving telemetry (no credential material, no stable linkable identifiers) | [T-13](#t-13-tracking-users-across-services) | Hardening guide Part 7 |
| 8 | Production distribution identity: application IDs, store accounts, controlled signing custody, signed artefacts and SBOM | [T-20](#t-20-supply-chain-compromise) | Hardening guide Part 2 |
| 9 | Document-authenticity validation against national IACA / Schengen masterlist (toolbox provides the interface only) | [T-15](#t-15-cheating-during-enrolment) | Spec §6.1 |
| 10 | Production liveness / biometric matching service; server-side component for passport-based onboarding to reach LoA substantial | [T-15](#t-15-cheating-during-enrolment), [T-23](#t-23-reference-or-test-components-deployed-to-production-as-is) | Spec §6.1; risk register |
| 11 | ETSI Trusted-List cryptographic validation in the verification service; list caching and fail-closed policy | [T-03](#t-03-a-fake-issuer), [T-17](#t-17-trusted-list-compromise-or-outage) | Spec §6.3 |
| 12 | Secure operation of issuing and verification services: rate limiting, monitoring, incident response, per-person issuance velocity limits | [T-19](#t-19-denial-of-service), [T-08](#t-08-an-adult-passes-a-check-for-a-minor), [T-04](#t-04-issuer-signing-key-compromise), [T-22](#t-22-an-adult-deliberately-provisions-a-minors-phone-with-attestations) | Spec §6.5 |
| 13 | Issuer data-retention policy, DPIA, production terms and privacy notice (placeholder only in the toolbox) | [T-14](#t-14-issuer-and-verifier-collude-to-identify-a-user), [T-16](#t-16-exposure-of-personal-data-during-enrolment) | Spec §6.1; national obligations |
| 14 | Issuance-flow protections: short-lived, single-use credential offers; transaction-code entropy and rate limiting; enrolment anomaly detection for repeated re-enrolments by the same person | [T-22](#t-22-an-adult-deliberately-provisions-a-minors-phone-with-attestations), [T-08](#t-08-an-adult-passes-a-check-for-a-minor) | AV Profile; threat analysis |
| 15 | AVSUA signing-key custody at the Age Verification App Provider: HSM-backed keys with documented ceremonies, short Key Attestation validity, issuance-anomaly monitoring, `client_status` revocation, and an incident procedure for replacing the certificate lodged with the Commission | [T-24](#t-24-compromise-of-an-age-verification-app-providers-avsua-signing-key), [T-05](#t-05-a-fake-or-cloned-app-obtains-proofs), [T-11](#t-11-a-tampered-app-or-compromised-phone) | Spec §4.2, §4.3; AV Profile §A.5 |

A production deployment that omits any applicable mandatory item above is outside the security assumptions of this threat model. In particular, the Mitigated, Low and Medium ratings assigned to threats with required implementer actions are valid only when those actions have been completed and verified in the production environment.

## Risks accepted by design

Beyond the per-threat residuals, four risks are accepted knowingly and are stated here so that adopters and users can make informed decisions:

1. **Physically co-located collusion ([T-08](#t-08-an-adult-passes-a-check-for-a-minor), [T-09](#t-09-a-minor-uses-an-adults-phone)).** An adult who is present and willing can always pass an age check for a minor. No remote age-verification technology closes this; the solution aims to prevent remote and scalable bypass, and achieves that.  
2. **Issuer identity reveals home jurisdiction ([T-13](#t-13-tracking-users-across-services)).** In the fallback presentation, the verifier learns which Attestation Provider signed the proof, which typically maps to a Member State. This is a deliberate trade-off for verifiability via the Trusted List. The ZKP path and possible future multi-issuer trust anchors reduce it.  
3. **Bounded, not instant, compromise recovery ([T-04](#t-04-issuer-signing-key-compromise)).** Without per-attestation revocation, recovery from an issuer key compromise is bounded by the 3-month validity window and Trusted-List removal. Judged acceptable for a low-stakes yes/no attestation; deployments with higher stakes should add status lists.  The same bounded-recovery shape applies to compromise of an App Provider's AVSUA signing key ([T-24](#t-24-compromise-of-an-age-verification-app-providers-avsua-signing-key)), where recovery runs through replacement of the certificate lodged with the Commission rather than through per-attestation revocation.
4. **State-level adversaries.** An adversary able to fully compromise a user's phone with advanced spyware defeats any app-based credential, including this one. The solution does not claim to defend against such adversaries.

## Out of scope

The following are outside this threat model, either because another party owns them or because the specification explicitly excludes them:

- The internal security of the standardised protocols the solution builds on — TLS, OID4VCI, OID4VP, the W3C Digital Credentials API, ISO/IEC 18013-5/-7 and HPKE. These protocols have their own specifications, threat analyses and security considerations; this document covers only how the solution profiles, combines and deploys them.  
- Level of assurance *high*; the solution targets level of assurance *substantial*.  
- Malicious certificate authorities (Web PKI compromise), per the specification's stated threat model.  
- Proximity (in-person) presentation per ISO/IEC 18013-5 offline flows, which the profile excludes.  
- Relying parties' compliance with their own legal obligations (for example DSA Article 28 duties beyond the age check itself).  
- The internal security of notified eID schemes and authentic sources.  
- General platform security of Android and iOS.

