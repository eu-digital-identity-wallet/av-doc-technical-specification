# Age Verification Solution Technical Specification

## Overview

**Certification authorities** (CAs) that are independent, trusted and audited regularly already, are tasked with generating **birth-year certificates** and providing them to people on secure hardware token devices just like when purchasing extended validation (EV) code-signing certificates.

**Certification authorities** generate one certificate with a public and private keypair for each **birth-year value**, such as 1990, 1991, 1992, 1993 [...] and shall always import the exact same public & private keypair, and certificate, into each token for as long as the private key is not compromised yet.

A **birth-year certificate** only contains a **Common Name (CN)** value being the **birth-year**, without any identifying information about the person it was granted to, and never containing the full birthdate, only the year value.

In order to account for people being born at birthdates such as **December 31, 1999** to avoid them having a **1999** birth-year value when they were closer to **2000**, birth-years are always rounded to the next upper value whenever the **month of birth** is June (06) or higher.

Because there are 12 months in a year, the 6th month and higher automatically get rounded to the next upper year value.

## Delivery

Any person of 18-years or older age can request the delivery of such hardware token with a certificate containing their birth-year value in it by contacting a **Certification authority** such as GlobalSign, Certum, Certigna and so on.

They upload necessary identity documents online and provide a shipping address, then receive the physical token with the certificate inside by physical mail.

The person receives a link while the shipping is being prepared, where they can confirm, after the physical device has been delivered to them, that they received it; when they confirm delivery, that page shows them what the token PIN code is (and PUK code).

It will be shown once, the user must remember it, alongwith a PUK code shown only once too.
Afterwards this information is erased from the Certification authority's database.

**The user must be able to change the PIN code and PUK code at any time to their own desired values.**
The PIN code must support lowercase & uppercase alphabet, digits, spaces, special characters and support up to atleast 64 characters (unlike a SIM card PIN code).
The PUK code must support atleast up to 16 characters.
The user must be able to customize all aspects of the hardware device by being able to modify the PIN policies, such as allowed character sets, maximum PIN length, and maximum PIN retry count.

*The more technically literate users must be able to request an hardware device that uses challenge-response (Admin key) for PIN unblock with an Admin (SO) PIN if they wish to have that instead of a static PUK code, which will by default be 48-zeroes (3DES challenge-response mechanism).*

*Do not lock the hardware device to read-only mode, because if the birth-year certificate is revoked, the user will want to delete the revoked certificate and reuse the token for other personal purposes.*

## Scope

Any European-Union certificate authority cans deliver a birth-year certificate to **any** citizen of an European-Union country.

Polish certification authorities can deliver birth-year certificates to a German citizen for example.

## Certificate Transparency

In order to prove that the Certification authority didn't secretly generate a specifically unique birth-year certificate for a specific person, all birth-year certificates must be disclosed in a **Certificate Transparency** logserver similar to what Google Chrome uses for TLS certificates, and must be based on a Merkle-hash-tree system.

Any person cans then verify that their birth-year certificate has indeed already existed before they placed an online order for a birth-year certificate, when it was added to the **CT (Certificate Transparency)** log, and whether other certificates exist for that same birth-year value (Common Name, aka. CN value).

## Verification of Age

The verification of age solely verifies **age**, not **identity** and therefore knowing whether somebody is a minor potentially using an adult person's birth-year certificate copy is out of the scope of this specification.

**Collaborating services** aka. websites requiring age verification must generate a random challenge between **8 to 16 bytes** valid for **3 minutes**.

The user downloads and runs **any open-source program** that cans sign data using an hardware token such as using PKCS11, OpenSSL, OpenSC or whatever compatible method, then signs the challenge using their **birth-year certificate**.

This will prompt for the certificate PIN, the user enters their PIN (that they can change at any time via any PKCS11 or minidriver-based software), and a signature is generated containing only the **certificate itself** and the **signature for the provided challenge**.

This is base64-encoded and generated **fully offline** by the user, then pasted directly into the **collaborating service**'s signature verification input box.

The user clicks on **Validate** then if the challenge is validly signed, the service requiring age verification SHALL only store in its database an **boolean** attribute such as `is_adult` set to `true`, or `1`.

The collaborating service SHALL NEVER store the birth-year value itself.

## Anonymity

The **Certificate authorities** generate a bunch of private key + public key + certificates for each birth year from e.g. 1900 to (current year - lowest possible legal age value within EU countries).

The **Certification authorities** SHALL always use the exact same certificate and private key on as many hardware devices as possible, importing the same certificate into atleast **10000** hardware devices if not even more, as long as the key is not compromised, before using a different public & private keypair, certificate with the next batch of devices.

## No-logging

All purchased hardware tokens containing birth-year certificates are **non-refundable** and **non-returnable**, and therefore the Certification authority must never keep ANY trace of the purchase after the device has been confirmed as successfully delivered to the customer (person ordering a birth-year certificate online).

Certification authorities MUST allow purchasing the birth-year certificates using **both** credit cards and debit cards, and **MUST** accept ANY picture of IDcard (face & rear side) that is sufficiently legible, and contains authentic information such as the correct picture, card serial number that exists and matches identity, and logical and correct checksum of the bottom checksum lines.

The certification authority **MUST** never arbitrarily **discriminate** against a scanned document picture or a photo picture of an IDcard based on the fact that it got converted to **PNG, PDF**; whether its **EXIF metadata** has been stripped for privacy reasons; or require the use of a live Camera, application or doing an intrusive, invasive **KYC (Know Your Customer)** check.

## Private key compromise

When a private key is **proven** compromised, by actually finding the private key leaked online, which is extremely unlikely due to the hardened anti-tamper nature of hardware signature tokens, then the **certificate revocation** is done by having the **private key itself** published to a public **revocation log** similar to a certificate transparency log, but for **private keys**.

Then any **private key** published on that revocation log, that actually matches an existing birth-year certificate public key, is considered a valid revocation submission and gets added permanently to the **revocation log**.

We basically revoke private keys by proving that we actually have it and publishing it directly in software form into a revocation log, and the revocation log checks whether the private key is indeed matching atleast one public key in a **certificate transparency (CT) log**, to avoid DoS submissions with infinite amounts of private keys that are worthless and never used in any birth-year certificate.

*Revocation log entries consist of private key itself, public key & certificate to make lookups easier and quicker.*

## Renewal for revoked certificates

Due to anonymity requirements, the same private key must be used on atleast **10000** hardware devices before generating a different one for the next batch of devices.

Therefore a private key compromise will affect atleast **10000** device owners anywhere inside the European Union (EU).

For how the renewal works, it's simple:
The person who found the leaked private key of a birth-year certificate cannot themselves claim **when** it was compromised.

The **revocation date** for that private key, and therefore all birth-year certificates using it, only starts at the **time the private key was published on a revocation log**.

All revocation log submissions, when confirmed valid, must be accompanied by a TSA signature (time-stamping authority signature) in either RFC3161 or similar timestamp-response format, which certifies that the private key indeed existed in software form, and therefore got compromised, starting at a specific date only, and **not before**.

## What happens after a revocation

When a birth-year certificate is revoked, which affects atleast **10000** people (basically), **collaborating services** MUST NOT retroactively revoke the `is_adult` status of users who verified their age before the key was compromised.

**ANY** collaborating service found to be doing this will be accused of having secretly kept verification data that should strictly have been **securely erased** immediately after verification.

This 'retroactive revocation' should not even be possible, because collaborating services **MUST NOT** keep any historical data, traces, or copies of the information used for age verification, including the birth-year certificate used, the challenge bytes used, and the response signature.

The revocation is only valid and effective for **future** identity verification attempts, not the ones done before the private key compromise.

## What people can do after their certificate is revoked

People can simply delete the birth-year certificate from their hardware device, certificate which would no longer be useful anyway, and perhaps start using their token for other purposes such as importing their own certificates into it and reusing it for other personal purposes.

The hardware token which has its birth-year certificate revoked does not become useless and an expensive paperweight at all; it stays useful, under the control of the user themselves, and cans be reused (given a new life) for different purposes and perhaps even used for data encryption.

If the person does not need to complete any new age verification soon, they do not need to purchase a new birth-year certificate yet.

## Roots of Trust

Any certification authority trusted for Extended Validation (EV) code-signing certificate issuance cans apply to become issuer of trusted birth-year certificates.

Then their dedicated **root certificates** will be added to a reference pool of trusted age verification root certificats that **collaborating services**  MUST honor without discriminating against specific certification authorities for any reason, whether be it political, personal or subjective.

## Portability

While primarily thought of with computers in mind, mobile phones can still be supported.

They can be supported either via **USB On-The-Go (USB OTG)** or via **NFC**, by having the hardware device support both contact and contactless interfaces.

Most mobile phones, even very old ones should support atleast one of the two options.

NOTE: It **MUST** be forbidden for any application developer to include any form of **root detection**, **bootloader lock state detection**, **custom ROM check**, **Xposed check**, and any form of device integrity check in any application that wishes to be compliant with anonymous age verification.

You do not own the user's device and therefore have no right to know, and no business knowing, whether the user customized their mobile device.

Your open-source application (which must always be open-source) must also reasonably support the oldest possible operating system versions that it targets, such at **atleast** Android 5.0 Lollipop, Windows 7 SP1, macOS 10.12 Sierra and / or iOS 13.

Even if you cannot attain compatibility all the way down to these example operating system versions, you **must** do your best as much as possible to target support for the oldest possible versions that you can possibly and technically support.

# Bypassing the Age verification

Yes, it will still be possible that some minors might ask adults to sign their age verification challenge.
And this is **not** a problem that cans be addressed by age verification, because it's **age** verification, not **identity** verification.

Basically, age verification is nothing more than a legal checkbox that legally allows the **collaborating service** to disclaim liability for allowing underage people to access their website, as long as they complied with this anonymous age verification system.

**Being able to bypass the age verification system is a fundamental pillar of an anonymous age verification system.**

If you cannot bypass it, then that means you can exactly know **who** is verifying their age, and therefore, that means you **can deanonymize people** who verify their age online.

If your age verification system is anonymous, then you **MUST NOT** be able to know or figure out whether it's actually a child signing their age verification challenge with an adult's hardware device, or really an adult.

Any attempt to figure this out would intrude on their privacy and anonymity, and *anybody who would dare to trade their privacy for more security, will deserve neither and lose both.*

*Remember that parents are responsible for educating their children.*
Verifying your age has only one purpose; allow people to legally claim that they are adults online, anonymously, and without compromising their anonymity or privacy.

If children insist on never listening to their parents, then maybe the parents should check themselves and wonder whether they should request assistance from social services or CPS instead of ruining the Internet for everyone else.

**Age verification is like cryptography.**

Asking for unfoolable age verification is like asking for a cryptographic algorithm that cans protect people's data, but still be broken by governments incase a criminal uses that algorithm.

This cannot work, you cannot have both; it's either secure or it's not.

Same for age verification; it's either anonymous or it's not.
You can't 'temporarily deanonymize' people to know whether they are really adults, then promise them to delete their face data and personal real-life information.

# Forbidden collaborations

It's strictly forbidden, and illegal, to act as a **collaborating service** of make use of age verification in any way, direct or indirect, for any business, service, company and any Internet service provider in the below sectors / categories:

1. VPN services
2. Email providers
3. Carrier / Telecom providers
4. Operating systems
5. Firmwares
6. Physical products
7. Client-side software
8. Platforms where online payment is fundamental part of it (e.g. Amazon, AliExpress, eBay...)

More types of services and categories can be added at any time, and immediately and retroactively apply to all earlier versions of this specification.

Once a service type / category has been banned from using age verification, it cannot be unbanned from using it, therefore later versions of this specifications cans only ban more types of services & categories, not remove any from the list.
