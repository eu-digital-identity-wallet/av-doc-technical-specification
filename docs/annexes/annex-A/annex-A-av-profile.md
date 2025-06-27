# ANNEX A - Age Verification Profile
(normative)

## A.1. Introduction

This document defines a set of requirements for the existing specifications to 
enable interoperability among Attestation Providers (APs), Age Verification App Instances (AVIs) 
and Relying Parties (RPs) where a substantial level of security and privacy is required. 
This document is an interoperability profile that can be used by implementations 
in various contexts, be it a certain industry or a certain regulatory environment. 

This profile is part of the EU Age Verification technical specifications. It builds
 on existing specifications commonly used for credential issuance and verification 
 and defines a set of features to be implemented in the context of age verification 

This document outlines a profile for the age verification target architecture, 
serving as a conceptual framework rather than a comprehensive specification of all 
features and requirements. At the current stage of development, not all functionalities 
described herein have been implemented in the Age Verification Solution Toolbox. 
Furthermore, certain features are also absent in the present development state of 
the EUDI Wallet libraries. These functionalities will be developed and integrated 
incrementally over the course of the project. 

The profile uses OpenID for Verifiable Credential Issuance [OID4VCI] for issuance
of Proof of Age attestations, and OpenID for Verifiable Presentations [OID4VP] 
and W3C Digital Credentials API [W3C Digital Credentials API] for presentation 
of Proof of Age attestations, respectively. The attestation format used is ISO mDoc [ISO18013-5]. 

A full list of the open standards used in this profile can be found in Overview of 
the Open Standards Requirements. 

 
### Audience Target audience/Usage

The intended audience for this document comprises Member States and designated 
organizations that seek to implement an age verification solution. This profile, 
the referenced technical standards and accompanying guidelines are designed to 
support governmental authorities as well as commissioned organizations in the 
development, integration, and deployment of compliant age verification systems 
within their respective jurisdictions or operational domains. This approach ensures 
that both public sector bodies and their authorized private sector partners are 
equipped to realize and maintain robust, privacy-preserving age verification in 
alignment with European regulatory requirements, age verification digital services 
requirements, and digital identity initiatives. 

## A.2. Terminology
This specification uses the terms 
Attestation Providers (APs), Age Verification App Instances (AVIs), Relying Parties (RPs), and Proof of Age attestation 
as defined in [Operational, Security, Product, and Architecture Specifications](../../../docs/architecture-and-technical-specifications.md).


## A.3. Scope
The primary focus of this document is on online services, with the intention that
 age verification should be conducted digitally in online environments. The solution 
 is specifically designed to enable users to prove their age when accessing online 
 content, products, or services that are subject to age restrictions. This approach 
 addresses the increasing need for robust online age verification mechanisms, 
 ensuring that age-restricted content is accessed only by individuals who meet 
 the required legal age, and supporting compliance with relevant regulatory 
 requirements for online platforms.
The following aspects are in scope of this interoperability profile:
- Profile of OpenID4VCI to issue ISO mDoc Proof of Age attestations
- Profile of OpenID4VP to present ISO mDoc Proof of Age attestation
- Crypto Suites
- Data model examples 

Assumptions made are the following:
- The APs and RPs cannot pre-discover Age Verification App’s capability
- The AP is talking to the AVI supporting the features defined in this profile (via AVI invocation mechanism)
- Support for mDoc only as the Proof of Age attestation format
- No personal data, especially no information from personal identification documents 
such as national ID card, is stored within an AVI. Only the Proof of Age attestation, 
specifically indicating "older than 18", is utilized for age verification purposes

### Out of Scope
The following items are out of scope for the current version of this document, but might be added in future versions:
- Support for Level of Assurance (LoA) "high".
- Proof of Age attestation re-issuance (using refresh tokens) and revocation. 
- RP registration and trusted lists of RPs.
- AVI attestations and trusted lists of Age Verification App providers.
- Device bound Proof of Age attestations.
- Proof of Age attestation presentation using proximity.
- Profile of OpenID4VCI to issue ISO mDoc [ISO.18013-5] is defined in ISO 23220-3.
- ISO mDoc specific requirements for OpenID for Verifiable Presentations over W3C Digital Credentials API 

### Standards Requirements
This specification enables interoperable implementations of the following flows:
- Issuance of ISO mDoc Proof of Age attestation using OpenID4VCI
- Presentation of ISO mDoc Proof of Age attestation using OpenID4VP
Implementations of this specification do not have to implement all of the flows listed above.
A parameter that is listed as optional to be implemented in a specification that is 
being profiled (i.e., OpenID4VCI, OpenID4VP and ISO mDoc) remains optional unless it is stated otherwise in this specification.

## A.4 OpenID for Verifiable Credential Issuance
* Both AVI and AP MUST support and use [RFC7636] with S256 as the code challenge method
* Both AVI and AP MUST support and use authorization code flow


### Credential Offer
- As a way to invoke the AVI, at least a custom URL scheme av:// MUST be supported. 
- The Grant Type `authorization_code` MUST be used as defined in Section 4.1.1 in [OID4VCI]
- The Grant Type `pre-authorized_code` MUST be used as defined in Section 4.1.1 in [OID4VCI]
- For Grant Types `authorization_code` and  `pre-authorized_code`the Issuer MUST 
use the `credential_configuration_ids` parameter which MUST be an array with a single element with value
`proof_of_age`. The AVI MUST use the same value in the scope Authorization parameter when 
grant type `authorization_code` is used and in the scope Token Request parameter when 
grant type `pre-authorized_code` is used. 

### Authorization Endpoint
- The AVI MUST use the scope parameter with value `proof_of_age`
- The AVI MUST provide the `code_challenge` as per [RFC7636]


### Token Endpoint
- The AVI MUST provide the `code_verifier` as per [RFC7636] when the Grant Type `authorization_code`
is used. 


### Credential Endpoint
- The AVI MUST use the `proofs` parameter providing an array of proofs of type JWT.


### AP Metadata
TBD

## A.5. OpenID for Verifiable Presentations profile Requirements
- As a way to invoke the Age Verification App, at least a custom URL scheme av:// MUST be supported.
- Response type MUST be `vp_token`
- `response_mode` MUST be `direct_post`
- RP MUST send the request by value (i.e., support for JAR is not required)
- The client identifier scheme MUST be `redirect_uri` followed by the `response_uri`
- A request MUST specify the nonce parameter
- The DCQL query and response as defined in Section 6 of [OID4VP] MUST be used


## A.6 W3C Digital Credentials API
**Note** The Age Verification Profile will prioritize adopting ISO/IEC TS 18013-7, Annex C; accordingly, this section will be revised.



## A.7. Crypto Suites
All entities MUST support P-256 (secp256r1) as a key type with ES256 JWT algorithm for signing and signature validation whenever this profile requires to do so.
SHA256 MUST be supported by all the entities as the hash algorithm to generate and validate the digests in the MDOC VC.
Note: When using this profile with other cryptosuites, it is recommended to be explicit about which entity is required to support which curve for signing and/or signature validation

## A.8. Zero-Knowledge Proofs
- AVI SHOULD support the generation of Zero-Knowledge Proofs using the solution 
detailed in:  *"Matteo Frigo and abhi shelat, Anonymous credentials from ECDSA, 
Cryptology ePrint Archive, Paper 2024/2010, 2024, available at [https://eprint.iacr.org/2024/2010](https://eprint.iacr.org/2024/2010)"*. Such a proof SHALL demonstrate that:
  - The Proof of Age attestation includes a signature that can be verified using the public key of the AP  
  - The Proof of Age attestation includes the requested attribute and its value is true
  - The AVI can generate a signature of the nonce that is verifiable using the 
  public key included in the Proof of attestation  
  - The Proof of Age attestation is within its validity period

- RP SHOULD be able to verify these Zero-Knowledge Proofs.


## A.9. Security Considerations
The security considerations in [OID4VCI] and [OID4VP] apply when compatible with Level of Assurance substantial only.

## A.10. Comparison with other profiles
This section is non-normative 

Since this age verification profile aims at achieving equivalence to Level of Assurance (LoA) substantial there is more flexibility to use faster go to market approaches using the protocols that are mentioned in the EUDI ARF but adopt configuration and profile parameters that will make implementers work easier and simpler without compromising the scope and the business needs of the Age Verification solution.
### Comparison with [HAIP]
#### OpenID for Verifiable Credential Issuance
*Credential offer*

- HAIP does not support the `pre-authorized_code` grant type.

*Authorization Endpoint*
- HAIP uses PAR [RFC9126]  to authenticate 
a Wallet Instance (referred to as AVI in the Age Verification solution) through an 
attestation verified via a trust chain. This mechanism relies on a predefined trust 
list of recognized solution providers. However, the Age Verification solution does 
not incorporate such a trust list. Using a self-signed
certificate does not offer any value. For this reason, the Age Verification 
Solution does not use PAR

*Token Endpoint*
- HAIP uses Client authentication using JWT Attestations. As already discussed,
this form of client authentication offers value only when combined with trust lists. 
For this reason, the Age Verification solution does not use this approach.

*Credential Endpoint*

No difference 

#### OpenID for Verifiable Presentations
Most differences compared to HAIP arise from the Age Verification solution’s reliance 
on TLS and the Web PKI for authenticating RPs and establishing secure communication 
channels. Similarly, when the Digital Credentials API is used, both the operating 
system and the browser are considered trusted components. As a result, an additional 
layer of authentication and encryption is deemed unnecessary. In more detail:

- HAIP requires encrypted responses (using `direct_post.jwt`). The threat model 
of the Age Verification solution  does not include malicious CAs. For this reason,   
direct_post is used. 
- HAIP uses JAR [RFC 9101]  to protect the 
integrity of the authorization request. However, its effectiveness depends on the 
existence of a trust list of RPs. In the absence of such a list—as is the case with 
the Age Verification solution—an attacker could obtain a valid certificate and 
substitute a legitimate JAR with a malicious one, thereby undermining its intended 
security benefits. For this reason, the Age Verification solution does not use JAR
- HAIP `x509_san_dns` and `verifier_attestation` as a client authentication
scheme. As in the case of JAR: the effectiveness of these schemes depends on the
existence of a trust list of RPs. For this reason, the Age Verification solution
uses the simpler `redirect_uri` scheme.  An alternative could be the use of `x509_san_dns`
together with the Web PKI, however, any malicious entity can obtain a valid Web PKI
certificate. 


#### W3C Digital Credentials API
- The Age Verification solution will use ISO/IEC TS 18013-7, Annex C. 



## A.11 Examples
The following section includes non-normative examples
### Proof of Age attestation
A Proof of Age attestation in ISO mDoc. 
```
{
  "issuerAuth": [
    <<{
      1: -7
    }>>,
    {
      33: h'308202BD30820263A00302010202144E4B041C8C22BA2C7AE96A64CD2475B27162FB2A300A06082A8648CE3D04030230693126302406035504030C1D41676520566572696669636174696F6E2049737375657220434120303131323030060355040A0C2941676520566572696669636174696F6E205265666572656E636520496D706C656D656E746174696F6E310B3009060355040613024156301E170D3235303430383233343034365A170D3236303730323233343034355A30653122302006035504030C1941676520566572696669636174696F6E204453202D2030303131323030060355040A0C2941676520566572696669636174696F6E205265666572656E636520496D706C656D656E746174696F6E310B30090603550406130241563059301306072A8648CE3D020106082A8648CE3D03010703420004BAE260A91CF520652293DB654C0F19F5CC7FAE9388F73D025B3252D035B6BCB96C799414FB510EF79C609284B379D1B85EB812240CCBC525318DB72016EBDB00A381EC3081E9301F0603551D23041830168014B13258C9ED461B5B1DCE3855E09F6FE89C90B0BA30160603551D250101FF040C300A06082B8102020000010230440603551D1F043D303B3039A037A035863368747470733A2F2F6973737565722E616765766572696669636174696F6E2E6465762F706B692F41565F43415F30312E63726C301D0603551D0E0416041420DEDF57FB318B998A762E464494B021CE76D310300E0603551D0F0101FF04040302078030390603551D12043230300603551D1204293027822568747470733A2F2F636F6D6D697373696F6E2E6575726F70612E65752F696E6465785F656E300A06082A8648CE3D040302034800304502200860F6AAEAA39FE479EC697E1EADBB0B473CB0E75C7B9E489EDD20BB5DD1AFBF022100A90A05B2FA2EAAB6A523FC6BE44F92EE7C21725A741D13629021FAD6AD14D3AC'
    },
    <<24(<<{
      "docType": "eu.europa.ec.av.1",
      "version": "1.0",
      "validityInfo": {
        "signed": 0("2025-06-20T08:45:29Z"),
        "validFrom": 0("2025-06-20T08:45:29Z"),
        "validUntil": 0("2025-09-18T00:00:00Z")
      },
      "valueDigests": {
        "eu.europa.ec.av.1": {
          0: h'2263E4515E0F1ABAD07FB1185A623944B5BD667A3CF189381E4C197133AEF99B'
        }
      },
      "deviceKeyInfo": {
        "deviceKey": {
          1: 2,
          -1: 1,
          -2: h'A0521B8EC99F9B9F52E716FC17CB93762935B9BFA6EE87F27F24682D5DA987F4',
          -3: h'BBA07F498D0F421E9753C94CA5A6B3B224EBE68BCE4DE8581040E8A63214E1B7'
        }
      },
      "digestAlgorithm": "SHA-256"
    }>>)>>,
    h'DEE5E0AB7E5A7F29CCEFE7E07C74B2389FE3B0078A35716D8313B60602476E3E4CEA73F844351F90FEBFFF7FAA3EA1A8D68AFB2676E507F6D0AFBFD5A94D6E8B'
  ],
  "nameSpaces": {
    "eu.europa.ec.av.1": [
      24(<<{
        "random": h'11D5B8F7AB303D50960E488B0C20B645876697812A67A1A007609FA95B9BCC17',
        "digestID": 0,
        "elementValue": true,
        "elementIdentifier": "age_over_18"
      }>>)
    ]
  }
}
```
Proof of Age attestation attributes:
```
{
  "age_over_18":true,
}
```

### Credential offer
#### With Grant Type `pre-authorized_code`
A credential offer generated by an AP

```
{
  "credential_issuer": "https://credential-issuer.example.com",
  "credential_configuration_ids": [
    "proof_of_age",
  ],
  "grants": {
    "urn:ietf:params:oauth:grant-type:pre-authorized_code": {
      "pre-authorized_code": "oaKazRN8I0IbtZ0C7JuMn5",
      "tx_code": {
        "length": 4,
        "input_mode": "numeric",
        "description": "Please provide the one-time code that was sent via e-mail"
      }
    }
  }
}
```

The same credential offer in a form that can be included in a QR code or a link used
to invoke an AVI

```
av://?credential_offer=%7B%22credential_issuer%22%3A%22https%3A%2F%2Fcredential-issuer.example.com%22%2C%22credential_configuration_ids%22%3A%5B%22proof_of_age%22%5D%2C%22grants%22%3A%7B%22urn%3Aietf%3Aparams%3Aoauth%3Agrant-type%3Apre-authorized_code%22%3A%7B%22pre-authorized_code%22%3A%22oaKazRN8I0IbtZ0C7JuMn5%22%2C%22tx_code%22%3A%7B%22length%22%3A4%2C%22input_mode%22%3A%22numeric%22%2C%22description%22%3A%22Please%20provide%20the%20one-time%20code%20that%20was%20sent%20via%20e-mail%22%7D%7D%7D%7D
```


#### With Grant Type `authorization_code`
```
{
  "credential_issuer": "https://credential-issuer.example.com",
  "credential_configuration_ids": [
    "proof_of_age",
  ],
  "grants": {
    "authorization_code": {
      }
    }
  }
}
```

The same credential offer in a form that can be included in a QR code or a link used
to invoke an AVI

```
av://?credential_offer=%7B%22credential_issuer%22%3A%22https%3A%2F%2Fcredential-issuer.example.com%22%2C%22credential_configuration_ids%22%3A%5B%22proof_of_age%22%5D%2C%22grants%22%3A%7B%22authorization_code%22%3A%7B%22issuer_state%22%3A%22Yp3vX9a7QTmc12bZfL8eRA5ow0UNjhKG%22%7D%7D%7D

```

### Authorization endpoint
Authorization request sent from AVI to the Authorization endpoint of the AP
```
GET /authorize?
  response_type=code
  &scope=proof_of_age
  &client_id=s6BhdRkqt3
  &code_challenge=E9Melhoa2OwvFrEMTJguCHaoeK1t8URWbuGJSstw-cM
  &code_challenge_method=S256
  &redirect_uri=av%3A%2F%2Fcallback
Host: server.example.com
```

The response sent from the Authorization endpoint of the AP to the AVI
```
HTTP/1.1 302 Found
Location: av://callback?
  code=SplxlOBeZQQYbYS6WxSbIA
```

### Token endpoint
#### Request 
A token request sent from the AVI to the token endpoint of the AP 
in an authorization code flow

```
POST /token HTTP/1.1
Host: server.example.com
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code
&code=SplxlOBeZQQYbYS6WxSbIA
&code_verifier=dBjftJeZ4CVP-mB92K27uhbUJU1p1r_wW1gFWFOEjXk
&redirect_uri=av%3A%2F%2Fcallback
```

A token request sent from the AVI to the token endpoint of the AP 
in a pre-authorized code flow

```
POST /token HTTP/1.1
Host: server.example.com
Content-Type: application/x-www-form-urlencoded

grant_type=urn:ietf:params:oauth:grant-type:pre-authorized_code
&scope=proof_of_age
&pre-authorized_code=SplxlOBeZQQYbYS6WxSbIA
&tx_code=493536
```

#### Response
A token response sent from the token endpoint of the AP to the AVI

```
HTTP/1.1 200 OK
Content-Type: application/json
Cache-Control: no-store

{
  "access_token": "czZCaGRSa3F0MzpnWDFmQmF0M2JW",
  "token_type": "Bearer",
  "expires_in": 86400,
}
```

### Credential endpoint
#### Request 
A credential request sent from the AVI to the credential endpoint of the AP
```
POST /credential HTTP/1.1
Host: server.example.com
Content-Type: application/json
Authorization: Bearer czZCaGRSa3F0MzpnWDFmQmF0M2JW

{
  "proofs": {
    "jwt": [
      "eyJ0eXAiOiJvcGVuaWQ0dmNpLXByb29mK2p3dCIsImFsZyI6IkVTMjU2IiwiandrIjp7Imt0eSI6IkVDIiwiY3J2IjoiUC0yNTYiLCJ4IjoiblVXQW9BdjNYWml0aDhFN2kxOU9kYXhPTFlGT3dNLVoyRXVNMDJUaXJUNCIsInkiOiJIc2tIVThCalVpMVU5WHFpN1N3bWo4Z3dBS18weGtjRGpFV183MVNvc0VZIn19",
      "eyJraWQiOiJkaWQ6ZXhhbXBsZTplYmZlYjFmNzEyZWJjNmYxYzI3NmUxMmVjMjEva2V5cy8xIiwiYWxnIjoiRVMyNTYiLCJ0eXAiOiJKV1QifQ"
    ]
  }
}
```
#### Response
A credential response sent from the credential endpoint of the AP to the AVI
```
HTTP/1.1 200 OK
Content-Type: application/json

{
  "credentials": [
    {
      "credential": "LUpixVCWJk0eOt4CXQe1NXK....WZwmhmn9OQp6YxX0a2L"
    },
    {
      "credential": "YXNkZnNhZGZkamZqZGFza23....29tZTIzMjMyMzIzMjMy"
    }
  ]
}
```

### Verifiable Presentation
#### Request

An authorization request includes a DCQL query. The following is a DCQL query for requesting a Proof of Age attestation 
```
{ 
  "credentials": [ 
    { 
      "id": "proof_of_age", 
      "format": "mso_mdoc", 
      "meta": { 
        "doctype_value": ["eu.europa.ec.av.1 "] 
      }, "claims": [ 
         {"path": ["eu.europa.ec.av.1 ", "age_over_18"]}      
       ] 
    } 
  ] 
}
```
An authorization request sent from a RP to the AVI. The request includes the DCQL query included in the previous example. 
```
GET /authorize?
  response_type=vp_token
  &response_mode=direct_post
  &client_id=redirect_uri%3Ahttps%3A%2F%2Fclient.example.org%2Fpost
  &response_uri=https%3A%2F%2Fclient.example.org%2Fpost
  &dcql_query=%7B%22credentials%22%3A%5B%7B%22id%22%3A%22proof_of_age%22%2C%22format%22%3A%22mso_mdoc%22%2C%22meta%22%3A%7B%22doctype_value%22%3A%5B%22eu.europa.ec.av.1 %22%5D%7D%2C%22claims%22%3A%5B%7B%22path%22%3A%5B%22eu.europa.ec.av.1 %22%2C%22age_over_18%22%5D%7D%5D%7D%5D%7D
  &nonce=n-0S6_WzA2Mj HTTP/1.1
```
#### Response
A response sent from the AVI to the RP

```
POST /post HTTP/1.1
Host: client.example.org
Content-Type: application/x-www-form-urlencoded

  vp_token=..
```