# Semantic Model
The semantic model is a set of base entities expressed using the [Verifiable Credentials Data Model v2.0](https://www.w3.org/TR/vc-data-model-2.0/) that are extensible, customisable and specialisable and can be used to implement almost any **Trust Scope** Use Case. To give a quick and high-level idea of the full context of the model, the entities are grouped into several sections dedicated to covering specific topics:

- [Know who you trust](#know-who-you-trust)
- [Define what you trust](#define-what-you-trust)
- [Extend your trust](#extend-your-trust)
- [Trustworthy human in the loop](#trustworthy-human-in-the-loop)

# Know who you trust
This section is about choosing Actors and their Identity Providers

## Trusted Digital Identity

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
- The **PublicKeyDigest** of the unique public key resolved using **PublicKeyId** and to be used by the holder to demonstrate Proof of Possession (see note about digest).

**NOTE ABOUT DIGEST**
All the digests used in this document are to be considered as defined in the [Subresource Integrity specification](https://www.w3.org/TR/sri/#the-integrity-attribute) and have the purpose to ensure integrity external referenced resources.

The `TrustedDigitalIdentity` credential is defined by the following attributes:

| Attribute       | Type    | Mandatory | Comment                         |
|-----------------|-------------------|-----------|---------------------------------|
| `controller`    | ControllerKind    | Yes       | The kind of controller of the Cryptographinc Digital Identity |
| `level`         | TrustLevelType    | Yes       | The self-declared level of trust of the **Trusted Identity Provider** that is also the issuer of this credential |
| `publicKeyId`   | URI               | Yes       | The resolvable link to the Cryptographic Digital Identity verificationMethod to be used to uniquely identify ONE and ONLY key pair |
| `publicKeyDigest` | String | No        | The optional digest (hash) of the above resolved public key, that is used to **Strongly Bind** it with this Credential. This property is used to enable the Proof of Possession issuing Verifiable Presentations | 

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
| `Qualified`       | A **Qualified Trust Service Provider** belonging to the eIDAS Framework |

## Trusted Identity Provider
This entity is representing the **Trusted Identity Provider** itself as defined in the [Trusted Identity Layer](digital-identities.MD#trusted-identity-layer) section

The `TrustedIdentityProvider` credential is defined by the following attributes:

| Attribute       | Type    | Mandatory | Comment                                             |
|-----------------|-------------------|-----------|-----------------------------------------------------|
| `trustedIssuer` | TrustedDigitalIdentity | Yes        | A **Strongly Bound TrustedDigitalIdentity** used to resolve the public key of the provider | 
| `type`          | ProviderType | No        | Which type of trusted identity provider  | 
| `kind`          | TrustedListKind | No        | Which kind of implementation of the trusted list is used to check the issued trusted identity | 
| `endpoint`      | URI               | No        | The address of the above trusted list | 

The `ProviderType` defines the technology framework stack:

| Value                | Description 
|----------------------|--------------------------------------------------------------------------------|
| `PKI`                | Public Key Infrastructure - PKI-based Trusted Identity Provider that issues X509 Certificates |
| `SSI` |  Self Sovereign Identity - SSI-based Trusted Identity Provider that uses DIDs and Verifiable Credentials |
| `Both` |  The combination of SSI and PKI that uses X509 Certificates in the DIDs  |

The `TrustedListKind`  defines the list of identified implementations of Trusted Lists:

| Value                | Description 
|----------------------|--------------------------------------------------------------------------------|
| `API`                | Generic Trusted List REST API Specification |
| `OCSP` | Online Certificate Status Protocol (OCSP) on Trust Service Provider (TSP) or Qualified TSP in case of eIDAS framework  |
| `TRAIN` | Trusted list of trust anchors using DNS(SEC) |
| `EBSI` | European Blockchain Services Infrastructure (EBSI) |
| `OTHER` | other possible implementations |


**Important Note**
The *Generic Trusted List REST API Specification* is meant to be a simple API contract that, for a given DID can return "true" if it is still valid. This will be very useful to enable the integration of any custom trusted list not included in the defined list.

# Define what you trust
This section is about expressing and describing in an interoperable way a Scope, its Actors, its Semantic Model, and how they are correlated

## Trust Scope

The **Trust Scope** has the purpose of providing a trustworthy representation of all meaningful information for a specific scope described by a **Trust Scope Owner** (see [The Scope - all layers together](digital-identities.MD#the-scope---all-layers-together) section). This credential enables the usage of [Party Credentials](#party-credential), described later in this section, by defining their accredited issuers. Furthermore, the TrustScope Credential supports the cooperation and interoperability between organisations/ecosystems/data spaces, used in conjunction with the TrustRelation Credentials.

The **TrustScope Credential** issued by the **Trust Scope Owner** mainly defines:

- The description of the **Scope** within which the semantic model, the trusted identity providers, and the semantic identity providers are to be considered valid.  
- The **TrustedIssuers** entitled to issue **Credentials** in the above **Scope**
- The **Vocabularies** (expressed in SHACL) that semantically define the **Credentials** which can be issued by the **TrustedIssuer** in the defined **Scope**.
- The **Schemas** (expressed in JSON-LD Context) that are used to validate the **Credentials** which can be issued by the **TrustedIssuer** in the defined **Scope**.
- The **Trusted Identity Providers** are used to check if a Cryptographic Digital Identity is to be considered Trusted.

The `TrustScope` credential is defined by the following attributes:

| Attribute                | Type    | Mandatory | Comment                                             |
|--------------------------|-------------------|-----------|-----------------------------------------------------|
| `scopeDescription`       | String            | No        | A description of the scope of the Trusted Scope |
| `trustedIssuers`         | TrustedDigitalIdentity[] | Yes | A list of **Strongly Bound TrustedDigitalIdentity** identifying the **Semantic Identity Provider** issuer(s) of the Scope |
| `vocabularies`           | URI[]             | No        | A list of URIs pointing to the vocabularies (SHACL) semantically describing the Scope |
| `schemas`                | URI[]             | No        | A list of URIs pointing to the schemas (JSON-LD Context) used to validate credentials in the Scope |
| `trustedIdentityProviders` | TrustedIdentityProvider[] | Yes       | A list of **Trusted Identity Provider** considered trustworthy in the Scope used to check **Trusted Digital Identities** |

## Party Credential 

The Party Credential is the basis for describing and enabling Actors as IAA Parties such as **Natural Persons**, **Services**, **Legal Persons**, etc. 
The general-purpose Party Credential is intended to be extended into specialised Credentials (see **Party Credential Specialisations**) and designed to be trustfully "chained" so that a party receiving a  

The **Party Credential** issued by one of the **Trusted Issuers** of the **Trust Scope Owner** mainly defines:

- The **Strongly Bound TrustedDigitalIdentity** identifying the Holder who can do the **Proof of Possesion** of this credential
- The **Trust Scope Credential** that defines the **Issuers**, **Schemas**, **Vocabolaries** necessary to the verification of this credential.
- The **Policies** (expressed in ODRL) that define the conditions under which this credential is to be considered "active".
- The **Identity Role, Attributes and Permissions** defined in the **Scope** and assigned to the Party.
- The **Delegatable** flag that establishes if the credential can be delegated in a chain of delegation.
- The **Parent** and its **Digest** that are used to create and verify the chain of delegation.
- The **Credential Status** as defined in the [`credentialStatus` property of Verifiable Credential Data Model 2.0](https://www.w3.org/TR/vc-data-model-2.0/#status) to enable revocation/suspension.

The `PartyCredential` is defined by the following attributes:

| Attribute               | Type   | Mandatory | Comment                                           |
|-------------------------|------------------|-----------|---------------------------------------------------|
| `holder`                | TrustedDigitalIdentity | Yes | A resolavable link to a **Strongly Bound TrustedDigitalIdentity** identifying the Holder who can do the **Proof of Possesion** of this credential |
| `trustScope`            | TrustScopeCredential  | No | A resolvable link to a **TrustScopeCredential** used to validate this credential (issuers, attributes, roles) in the **Scope** | 
| `hasPolicy`             | policy[] in ODRL | No | A list of `policy` expressed using ODRL to be used to verify if the credential is to be considered "active" in the **Scope** |
| `identityAttributes`    | Attribute[]         | No | A list of Identity Attributes (defined in the scope schema/vocabulary), to be used in an Attribute-Based Access Control (ABAC) context | 
| `identityRoles`         | Role[]         | No | A list of Identity Roles (defined in the scope schema/vocabulary), to be used in a Role-Based Access Control (RBAC) context | 
| `identityPermissions`   | Permission[]         | No | A list of Identity Permissions (defined in the scope schema/vocabulary), to be used in a Permission-Based Access Control context | 
| `delegatable`           | Boolena  | No | A flag that establishes if the credential can be used as `parentPartyCredential` of a child party credential | 
| `parentPartyCredential` | PartyCredential  | No | A resolvable link to the parent **PartyCredential** where the **holder** MUST be equal to the **issuer** of this credential (same public key and digest) | 
| `parentPartyCredentialDigest` | String  | No | The digest (hash) of the `parentPartyCredential` resolved credential used to ensure the integrity of the chain | 
| `credentialStatus` | Status | No | Optional and must not be used for non-revocable assertions, see [VC Data Model v 2.0 - Status](https://www.w3.org/TR/vc-data-model-2.0/#status) |

The `Attribute`, `Role` and the `Permission` types of the **Scope** are defined in the **TrustScopeCredential** schema/vocabularies together with the Trusted Issuers. They can vary from a simple value/description type to a complex domain specific object.

Here is an example of a value/description type of Role:

| Value         | Description 
|---------------|--------------------------------------|
| `User`        | Generic User |
| `Admin`       | Admnistrator |
| `Manager`     | Manager |
| `HR-Manager`  | Human Resource manager |

**VERY IMPORTANT NOTE**
To enable the delegation (Power Of Attorney-like ) using the **PartyCredential**, the `parentPartyCredential` property must refer to a credential that is a superset of the `identityAttributes`, `identityRoles` and `identityPermissions` properties with no exception and the **TrustedDigitalIdentity** identified by the **parentPartyCredential.holder** property MUST match the **issuer** of this credential to prove that the delegation is valid.


# Extend your trust
This section is about enabling the extension of the trust of your Scope to other Scopes

## Trust Relation

The **Trust Relation** entity is both simple and powerful; it represents and defines the will of a certain **Trust Scope Owner** to **trust** another **Trust Scope** and describes how this relation is mapped/translated in the semantics of its own Trust Scope

The **TrustRelation Credential** issued by the **Trust Scope Owner** mainly defines:

- The **Trustor Scope** that defines the **Issuers**, **Schemas**, **Vocabolaries** necessary to the verification of this relation.
- The **Trusted Scope** that defines the **Issuers**, **Schemas**, **Vocabolaries** necessary to the verification of this relation.
- The **Policies** (expressed in ODRL) that define the conditions under which this relation is to be considered "active" and "valid".
- The **Identity Role and Attributes Mappings** that are used to perform the trusted translation (from the trusted to the trustor).

The `TrustRelationCredential` is defined by the following attributes:

| Attribute               | Type   | Mandatory | Comment                                           |
|-------------------------|------------------|-----------|---------------------------------------------------|
| `trustorScope`          | TrustScopeCredential  | Yes | A resolvable link to a **TrustScopeCredential** that extends is **trust** | 
| `trustedScope`          | TrustScopeCredential  | Yes | A resolvable link to a **TrustScopeCredential** to **trust**  | 
| `hasPolicy`             | policy[] in ODRL | No | A list of `policy` expressed using ODRL to be used to verify if the relation is to be considered "active" and "valid" |
| `mappings`              | TrustMapping[]   | No | A list of TrustMapping, to be used for translation | 

The `TrustMapping` defines how the translation between **Scope Specific Credentials** is to be performed:

| Attribute          | Type          | Mandatory | Comment                                           |
|--------------------|---------------|-----------|---------------------------------------------------|
| `trusted`          | Credential Type or Claim | Yes | A **Type** or one of its **Claim** defined in the schema/vocabulary of the **Trusted** scope | 
| `translated`        | Credential Type or Claim | Yes | A **Type** or one of its **Claim** defined in the schema/vocabulary of the **Trustor** scope | 



### Federation using Trust Scope Credentials

Both **TrustScope** and **TrustMapping** credentials are designed with interoperability in mind and easily enables implementation of selective, multidirectional and monodirectional trust, and a federation between two or more trusted scopes is the perfect use case. Below are some examples:

#### Monodirectional selective federation between two Data space scopes 

Data space A can enable federation with Data space B by simply trusting the **trusted issuers** of the **Ecosystem TrustScope** issued by Ecosystem B and selecting which subset of its **Domain Specific Credentials** to trust and mapping them in its own **Domain Specific Credentials** by issuing a **TrustMapping** credential.

#### Bidirectional full federation between 2 Trust Scopes 

Ecosystem A enables federation with Ecosystem B by fully trusting the **Ecosystem TrustScope** issued by Ecosystem B as described above, and vice versa.

#### Federation managed by an operator that selectively trusts multiple Ecosystems

An Ecosystem operator maintains the list of the **trusted issuers** and relative **Domain Specific Credentials** of the trusted **EcosystemTrustScope** issued by Ecosystems.

#  Trustworthy human in the loop
This section is dedicated to including the human assertions in the trusted interactions between actors of a scope

## Assertion

The assertion entity is designed to represent different types of **will** of an **actor** (or party), the will to sign a contract, the will to provide consent towards a certain action or purpose, the will to accept an agreement, the will to provide permission for a certain action and so on. The only condition needed to make assertions viable is that their reference must be another verifiable credential; for example, a Signature assertion must refer to a Contract in the form of a VC.
Assertions must not use a publicly resolvable [identifier](https://www.w3.org/TR/vc-data-model-2.0/#identifiers) and can be **Revocable** or **Irrevocable**, and this can be achieved simply by using the [`credentialStatus`](https://www.w3.org/TR/vc-data-model-2.0/#status) in conjunction with the [BitstringCredentialStatusList](https://www.w3.org/TR/vc-bitstring-status-list/)

The **Assertion Credential** issued by any **Actor** as trustworthy affirmation of **will** on **something** mainly defines:

- The **Strongly Bound TrustedDigitalIdentity** that identifies the Issuer and checks that the Controller **must** be a human.
- The **Type** of assertion the **Actor** want to express.  
- The **Date and Time** of when the assertion was expressed by the **Actor**.  
- The **Target Id** of **what** the assertion is expressed for, that can be resolved (at least from all verifiers) and verified using the **DigestSRI** 
- The **DigestSRI** that is used to check that the target is not tampered/modified.
- The **Credential Status** as defined in the [`credentialStatus` property of Verifiable Credential Data Model 2.0](https://www.w3.org/TR/vc-data-model-2.0/#status) for revocable/suspendable assertions.

The `Assertion` credential is defined by the following attributes:

| Attribute   | Type | Mandatory | Comment                             |
|-------------|------|-----------|-------------------------------------|
| `humanIdentity` | TrustedDigitalIdentity | Yes        | A **Strongly Bound TrustedDigitalIdentity** used to resolve the public key of the issuer, used to check that **`controller` = ControllerKind.Human** | 
| `type`      | AssertionType | Yes | Indicating the *type* of the Assertion to be expressed |
| `validFrom` | Validity    | Yes | See `validFrom` as defined in the [VC Data Model v 2.0 - Validity](https://www.w3.org/TR/vc-data-model-2.0/#validity-period) |
| `id`        | URI    | Yes | A resolvable single identifier as defined in the [VC Data Model v 2.0 - Identifiers](https://www.w3.org/TR/vc-data-model-2.0/#identifiers) identifying the target of the Assertion |
| `digestSRI` | String | Yes | Integrity of related Resources as defined in [VC Data Model v2.0 - Integrity of related resources](https://www.w3.org/TR/vc-data-model-2.0/#integrity-of-related-resources) to ensure that the referenced target is not changed/modified/tampered (still identical in time) | 
| `credentialStatus` | Status    | No | Optional and must not be used for non-revocable assertions, see [VC Data Model v 2.0 - Status](https://www.w3.org/TR/vc-data-model-2.0/#status) |

The `AssertionType` defines the type of **will** expressed by the **Actor** (the issuer) :

| Value             | Description 
|-------------------|--------------------------------------------------------------------------------|
| `Signature`       | A signature attached to the targeted resource (contracts, SLAs) |
| `Consent`         | A signed consent given to the targeted resource (purposes, GDPRs, terms and conditions, etc) |
| `Permission`      | A signed permission to allow the targeted resource (Data/Infra/App Provisionings, access to a service, etc) |
| `Acceptance`      | A signed acceptance of the targeted resource (agreements, constraints, terms and conditions, etc) |
| `Acknowledge`     | A signed acknowledgement of the targeted resource (rules, generic/custom defined targets) |

### Multiple Signatures of a Contract using AssertionCredential

Using the **AssertionCredential** to add multiple signatures to a verifiable credential that represents a contract is quite simple and consists only of requesting all the signer actors (human identities) to issue an unrevocable AssertionCredential of type `Signature` that targets the contract verifiable credential.

**IMPORTANT NOTE**
Assertions are fully **Machine Readable** and can easily be checked and verified by machines/services


