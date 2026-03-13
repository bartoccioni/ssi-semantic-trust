# Semantic Model
The semantic model is a set of base entities expressed using the [Verifiable Credentials Data Model v2.0](https://www.w3.org/TR/vc-data-model-2.0/) that are extensible, customisable and specialisable and can be used to implement almost any **Trust Scope** Use Case. 

## Trust Digital Identity

The main idea behind this entity is to define and classify all the **Actors** (or parties) involved in a selected **Scope**, following the same approach and information described in the section [Digital Identities](digital-identities.MD).

The **Trusted Digital Identity Credential** has the purpose of describing the **kind** of party behind a [Cryptographic Digital Identity](digital-identities.MD#cryptographic-identity-layer), enriching the standard information of an SSI Holder with the following information guaranteed by a **Trusted Identity Provider** that is also the issuer of this type of credential:

- The **ControllerKind** of the [Cryptographic Digital Identity](digital-identities.MD#cryptographic-identity-layer) defined as follow:
   - Anonymous 
   - Human-controlled 
   - Machine-controlled 
- The **TrustLevel** of the **Trusted Identity Provider** itself as defined in the [Trusted Identity Layer](digital-identities.MD#trusted-identity-layer) section:
   - Scoped
   - Standard
   - Qualified
- The **PublicKeyId** in the form of a resolvable link to the digital identity's verificationMethod used to uniquely resolve **ONE and ONLY** key pair of the possible list contained in the DID.
- The **PublicKeyDigest** as defined in the [Subresource Integrity specification](https://www.w3.org/TR/sri/#the-integrity-attribute) of the unique public key resolved using **PublicKeyId** and to be used by the holder to demonstrate Proof of Possession.

The `TrustedDigitalIdentity` credential is defined by the following attributes:

| Attribute       | Type.Value/Voc    | Mandatory | Comment                         |
|-----------------|-------------------|-----------|---------------------------------|
| `controller`    | ControllerKind    | Yes       | The kind of controller of the Cryptographinc Digital Identity |
| `level`         | TrustLevelType    | Yes       | The self-declared level of trust of the **Trusted Identity Provider** that is also the issuer of this credential |
| `publicKeyId`   | URI               | Yes       | The resolvable link to the Cryptographic Digital Identity verificationMethod to be used to uniquely identify ONE and ONLY key pair |
| `publicKeyDigest` | KindOfTrustedList | No        | The optional digest (hash) of the above resolved public key, that is used to **Strongly Bind** it with this Credential. This property is used to enable the Proof of Possession  | 

The `ControllerKind` type defines the list of identified controllers :

| Value                | Description 
|----------------------|--------------------------------------------------------------------------------|
| `Anonymous`          | No info available or disclosable |
| `Human`              |  The control is performed by a Human (requires human acknowledgement every signature) |
| `Machine`            | The control is performed by a Machine ( process, application, service, agent, etc.)  |

The `TrustLevelType` defines the list of categories of trust of the **Trusted Identity Provider** :

| Value             | Description 
|-------------------|--------------------------------------------------------------------------------|
| `Scoped`          | Valid in a self-defined scope (e.g. data space, ecosystem, organisation, etc). |
| `Standard`        | A **Trust Service Provider** belonging to an internationally accepted Trust Framework( CA/B Forum for SSL X509 Certificates, Etc.) 
| `Qualified`       | A **Qualified Trust Service Provider** belonging to the eIDAS Framework   |


## Trust Scope

The Trust Scope has the purpose of providing a trustworthy representation of all meaningful information for a specific scope described by a **Trust Scope Owner** (the issuer). This credential enables the usage of Party Credentials, described later in this section, by defining their accredited issuers. Furthermore, the Trust Scope Credential supports the cooperation and interoperability between organisations/ecosystems/data spaces, used in conjunction with the TrustRelation Credetials.

The **Trust Scope Credential** issued by the **Trust Scope Owner** mainly defines:

- The **Scope** within which the TrustedIssuers are accredited (within which the issued credentials are considered valid).  
- The **TrustedIssuers** entitled to issue **Credentials** in the above **Scope**
- The **Vocabularies** (expressed in SHACL) that semantically define the **Credentials** which can be issued by the **TrustedIssuer** in the defined **Scope**.
- The **Trusted List** is used to check DIDs issued by the Trust Service Provider.

The `TrustScope` credential is defined by the following attributes:

| Attribute                | Type.Value/Voc    | Mandatory | Comment                                             |
|--------------------------|-------------------|-----------|-----------------------------------------------------|
| `scopeDescription`    | String            | No        | A description of the scope of the Trust Service Provider |
| `trustIssuers`        | TrustedDigitalIdentity[] | Yes       | A list of **Strongly Bound TrustedDigitalIdentity** identifying the Issuer(s) of the Scope |
| `vocabularies`        | URI[]             | No        | A list of URIs pointing to the vocabularies (SHACL) semantically describing the Scope |
| `schemas`             | URI[]             | No        | A list of URIs pointing to the schemas semantically describing the Scope |
| `trustedListKind`     | KindOfTrustedList | No        | Which kind of implementation of the trusted list is used to check DID issued by Trust Anchors | 
| `trustedListEndpoint` | URI               | No        | The address of the above trusted list | 

The `KindOfTrustedList` type defines the list of identified implementations of Trusted Lists:

| Value                | Description 
|----------------------|--------------------------------------------------------------------------------|
| `API`                | Generic Trusted List REST API Specification |
| `LOTLeIDAS` |  List Of Trusted Lists with ETSI TS 119 612 format (eIDAS Qualified Trusted Service Providers) |
| `TRAIN` | Trusted list of trust anchors using DNS(SEC) |
| `EBSI` | European Blockchain Services Infrastructure (EBSI) |
| `OTHER` | other possible implementations |


**Important Note**
The *Generic Trusted List REST API Specification* is meant to be a simple API contract that, for a given DID can return "true" if it is still valid. This will be very useful to enable the integration of any custom trusted list not included in the defined list.

## Trust Relation

The **Trust Relation** entity is both simple and powerful; it represents and defines the will of a certain Trust Scope Owner to **trust** another **Trust Scope** and how this relation is mapped/translated in the semantics of its own Trust Scope

### Federation using Trust Scope Credentials

The **TrustScopeCredential** is designed with interoperability in mind and easily enables implementation of selective, bidirectional and monodirectional trust, and a federation between two or more trusted scopes is the perfect use case. Below are some examples:

#### Monodirectional selective federation between 2 Ecosystems

Ecosystem A can enable federation with Ecosystem B by simply trusting the **trusted issuers** of the **EcosystemTrustScope** issued by Ecosystem B and selecting which subset of its **Domain Specific Credentials** to trust.

#### Bidirectional full federation between 2 Ecosystems

Ecosystem A enables federation with Ecosystem B by fully trusting the **EcosystemTrustScope** issued by Ecosystem B, and vice versa.

#### Federation managed by an operator that selectively trusts multiple Ecosystems

An Ecosystem operator maintains the list of the **trusted issuers** and relative **Domain Specific Credentials** of the trusted **EcosystemTrustScope** issued by Ecosystems.

**Important Note**
In all the above examples, the *trusting* relationship can be concretely implemented by issuing a [verifiable credential](https://www.w3.org/TR/vc-data-model-2.0/) whose [credentialSubject](https://www.w3.org/TR/vc-data-model-2.0/#credential-subject) is the ID of the **EcosystemTrustScope**. Alternatively, a more fine-grained **TrustRelationCredential** could be designed for that purpose.


## Party Credential 

The Party Credential is based upon the [Verifiable Credentials Data Model v2.0](https://www.w3.org/TR/vc-data-model-2.0/) and is the basis for all IAA Parties such as **Natural Persons** , **Services**, **Legal Persons**, etc. 
The general-purpose party credential is intended to be extended into specialized Credentials (see **Party Credential Specializations**) 

The `PartyCredential` is defined by the following attributes:

| Attribute               | Type.Value/Voc   | Mandatory | Comment                                           |
|-------------------------|------------------|-----------|---------------------------------------------------|
| `holder`                | TrustedDigitalIdentity | Yes | A resolavable link to a **Strongly Bound TrustedDigitalIdentity** identifying the Holder who can do the **Proof of Possesion** of this credential |
| `hasPolicy`             | policy[] in ODRL | No | A list of `policy` expressed using ODRL |
| `identityAttributes`    | String[]         | No | A list of literals representing Identity Attributes to be used in an Attribute-Based Access Control (ABAC) context | 
| `identityRoles`         | String[]         | No | A list of literals representing Identity Roles to be used in a Role-Based Access Control (RBAC) context | 
| `parentPartyCredential` | PartyCredential  | No | A resolvable link to the parent **PartyCredential** where the **holder** MUST be equal to the **signer** of this credential (same public key and digest) | 

**VERY IMPORTANT NOTE**
To enable the delegation of rights (Power Of Attorney-like ) using the **PartyCredential**, the `parentPartyCredential` property must refer to a credential that is a superset of the `identityAttributes` and `identityRoles` properties with no exception.

The **credentialSubject.id** MUST identify the **DID Document** that is referenced by the `holder` property that *belongs to/is controlled by* the **Credential Holder**.
With this solution, the PartyCredential VC can be either publicly published in case it contains no sensitive data or kept private in case it contains PII - Personal Identifiable Information.

### Private Party Credential

**Party Credentials** containing PII are not considered to be published and resolvable via their **id** to everybody, instead, they are intended to be stored in secure storage such as a wallet, secure storage device, secure vault storage, etc.
An example of this type of credential is the **NaturalPersonCredential**, issued by an Organisation to one of its users/employees, with the purpose to entitle a Natural Person to interact with Relying Parties(RP) in the context of the organisation.

### Public Party Credential

A public party credential is a **Party Credential** containing data that can be publicly accessed and queried.
An example of this is the **MembershipPartyCredential** issued by a Trusted Issuer of a specific Trust scope (Organisation, Ecosystem, Data space, etc) to a Trusted Digital Identity in order to attest its Membership status in the scope context.


## Signature Credential

The Signature Credential is a Verifiable Credential introduced to express digital signatures in a machine-readable and interoperable form enabling several scenarios ranging from data transaction permissions to agreement signing. It allows participants to cryptographically sign digital resources and bind the signature to the identity of the signer with verifiable integrity.  

By using a verification method associated with a certificate issued under a recognized legal trust framework (e.g. eIDAS), the Signature Credential may achieve legal equivalence with handwritten signatures, depending on the assurance level (e.g., Qualified Electronic Signature).  

A specialization of this credential, Signed Agreement Credential, introduces revocability to model agreements whose validity may be rescinded over time. These credentials can be issued independently by each participant and later aggregated into a Verifiable Presentation, thus enabling decentralized signature workflows for contracts such as data usage agreements.  


Important note: the issuance of a signature credential from a digital identity bound to a human-controlled device/wallet, can be used to realise the "human-in-the-loop" interaction.

This approach promotes semantic clarity, cryptographic assurance, and interoperability across use cases involving legally binding actions, whether initiated by humans or automated agents. It also enables trust anchors and policies defined in the Gaia-X Trust Framework to validate each Signature Credential independently. 

### Multiple Signatures using SignatureCredential specializations

The **SignatureCredential** in the trust model defined by the ICAM Document, represents a general purpose and machine readable *signature* that Participants are asked to provide in several contexts, one of the most important of these contexts is represented by the [Data Transaction](...) section of the Data Exchange Document(version...) where this credential will be used to sign **[Data Usage Agreement](...)** and **Data Product Usage Contract**.

The concept behind this credential is the same as the credentials issued by the compliance service, to issue Participant Credentials and  Service Offering Credentials (using the "credentialSubject" claim), where the **credentialSubject** consists in:

| Attribute   | Type.Value/Voc | Mandatory | Comment                                                                                                                                                                                                                                     |
|-------------|----------------|-----------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `type`      | string         | Yes       | Indicating the *type* of the subject to be Signed                                                                                                                                                                                           |
| `id`        | URI            | Yes       | A resolvable single URI as defined in the [VC Specification](https://www.w3.org/TR/vc-data-model-2.0/#identifiers) identifying the subject to be Signed                                                                                     |
| `digestSRI` | String         | Yes       | Integrity of related Resources as defined in [W3C VC Data Model v2.0](https://www.w3.org/TR/vc-data-model-2.0/#integrity-of-related-resources) to ensure that referenced resource is not changed/modified/tampered(still identical in time) | 

Another important factor is represented by the fact that, issuing and signing this credential with a **verificationMethod** bound to a certificate that has legal relevance (e.g eIDAS) gives it the same level of trust, enabling the possibility to check the `gx:legalValidity` property of the **Signature Check Type** (see `gx:signers` property).

#### SignedAgreementCredential

The **SignedAgreementCredential** is a specialization of **SignatureCredential** that represents a *Revocable Signed Agreement* given by a participant to a specific subject (e.g. Data Usage Agreement, GDPR Agreement, etc.) and the only difference is that it can be revoked.

 
#### Data Usage Agreement Example

In a Data Transaction the **Data Provider**(did:web:provider.com) and a **Data Consumer**(did:web:consumer.com) must agree to a **Data Usage Agreement** (**id** "https://provider.com/data-usage-contract.654321") and this is done treating "Signatures" as a **Verifiable Credential** that each Participant can issue and sign. 
In this way, when all Participants have issued their SignatureCredential referencing the contract it will be possible to create Verifiable Presentations that contain all the Signatures.

Here is an example of how CredentialSignature can be used:


```json

{
  "@context": [
    "https://www.w3.org/ns/credentials/v2",
    "https://w3id.org/gaia-x/development#"
  ],
  "type": [
    "VerifiableCredential"
  ],
  "id": "https://consumer.com/data-usage-contract-signatures.123456",
  "issuer": "did:web:consumer.com",
  "validFrom": "2023-07-28T12:31:49.074Z",
  "validUntil": "2023-10-26T12:31:49.074Z",
  "credentialSubject": {
    "type": "dataUsageContract",
    "id": "https://provider.com/data-usage-contract.654321",
    "digestSRI": "sha384-lHKDHh0msc6pRx8PhDOMkNtSI8bOfsp4giNbUrw71nXXLf13nTqNJoRp3Nx+ArVK"
  },
  "credentialStatus": {
    "id": "https://consumer.com/status/1#127",
    "type": "BitstringStatusListEntry",
    "statusPurpose": "revocation",
    "statusListIndex": "127",
    "statusListCredential": "https://consumer.com/credentials/status/1"
  }
}
```

