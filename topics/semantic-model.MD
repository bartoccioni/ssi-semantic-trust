# Semantic Model
The semantic model is a set of base entities expressed using the [Verifiable Credentials Data Model v2.0](https://www.w3.org/TR/vc-data-model-2.0/) that are extensible, customisable and specialisable and can be used implement almost any **Trust Scope** Use Case. 

## Trust Scope

The Trust Scope has the purpose of providing a trustworthy representation of all meaningful information for a specific scope. This credential enables the usage of Party Credentials, described later in this chapter, by defining their accredited issuers. Furthermore, the Trust Scope Credential supports the cooperation and interoperability between organisations/ecosystems/data spaces, by easing the use of external Trust Service Providers.

The **TrustScopeCredential** mainly defines:

- The **Scope** within which the Trust Service Provider is accredited (within which the issued credentials are considered valid).  
- The **TrustedIssuers** <!-- Is there a reason why not to name it TrustServiceProvider? --> entitled to issue **Credentials** in the above **Scope**
- The **Vocabularies** (expressed in SHACL) that semantically define the **Credentials** which can be issued by the **TrustedIssuer** in the defined **Scope**.
- The **Trusted List** used to check DIDs issued by the Trust Service Provider.

The `Trust Scope Credential` is defined by the following attributes:

<!-- are there any other attributes to be considered?where do we see how long the TA Credential is valid?Maybe the revocation mechanism has to be defined better --> 

| Attribute                | Type.Value/Voc    | Mandatory | Comment                                                                                                                                                                                                                                  |
|--------------------------|-------------------|-----------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `scopeDescription`    | String            | No        | A description of the scope of the Trust Service Provider                                                                                                                                                                                           |
| `trustIssuers`         | DID[]             | Yes       | A list of resolvable link(s) to the issuer verificationMethod to be used to uniquely identify ONE and ONLY key pair                                                                                                                            |
| `vocabularies`        | URI[]             | No        | A list of URIs pointing to the vocabularies/schemas (SHACL) semantically describing the Trust Service Provider's scope <!-- could we be clearer here?reference to the semantic applicable to issue attestations in the defined scope would be clearer? --> |
| `trustedListKind`     | KindOfTrustedList | No        | Which kind of implementation of the trusted list is used to check DID issued by Trust Anchors                                                                                                                                            | 
| `trustedListEndpoint` | URI               | No        | The address of the above trusted list                                                                                                                                                                                                    | 

The **KindOfTrustedList** type defines the list of the identified implementations of Trusted Lists:

- Gaia-X Trusted List Generic REST API Specification
- Gaia-X IPFS (with ETSI TS 119 612 format)
- TRAIN
- EBSI
- (other possible implementations)


**Important Note**
The *Gaia-X Trusted List Generic REST API Specification* is meant to be a simple API contract that for a given DID can return "true" if it is still valid. This will be very useful to enable integration of any custom trusted list not included in the defined list.


### Trust Scope Credential specialisation examples

Several specialisations of the **TrustScopeCredential** can be easily defined, for example:

#### **Organization Trust Scope** 
This specialisation is a self-issued credential that entitles the issuing organization to define its *Roles/Identity Attributes* to be used by identify and authorise its parties:

   - **scope** - *Organization Credential Management* (OCM).
   - **trusted issuers** - the organisation itself.
   - **vocabularies** - defines the semantics of *Roles/Identity Attributes* and **Domain Specific Credentials** that are valid in the defined scope.
   - **trusted list** - to assign and revoke *Roles/Identity Attributes* and **Domain Specific Credentials** to its parties (users, natural persons, endpoint services, etc) by issuing **PartyCredentials**.

#### **Gaia-X Compliance Trust Scope** 
This credential is issued by  Gaia-X and entitles an organisation to issue attestations about specific claims in the context of Gaia-X Compliance.

   - **scope** - issue attestations used to attest Gaia-X Compliance.
   - **trusted issuers** - the organisation accredited by Gaia-X to issue attestations in the defined scope.
   - **vocabularies** - defines the semantic applicable to issue attestations in the defined scope.
   - **trusted list** - to assign and revoke attestations issued to Gaia-X Providers. Trust Anchor Credential specialisation examples

#### **Ecosystem Trust Scope**
This specialisation is a self-issued credential that entitles an Ecosystem operator to define:

   - **scope** - manage an *Ecosystem* (onboarding/offboarding/role assignment etc.).
   - **trusted issuers** - the Ecosystem itself. <!-- the Ecosystem Governance Authority? -->
   - **vocabularies** - defines the semantic of *Roles/Identity Attributes* and **Domain Specific Credentials** valid in the defined scope.
   - **trusted list** - to assign and revoke *Roles/Identity Attributes* and **Domain Specific Credentials** to its members (other Participants) issuing **MembershipPartyCredentials**.


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
The general purpose party credential is intended to be extended into specialized Credentials (see **Party Credential Specializations**) 

The `Party Credential` is defined by the following attributes:

| Attribute                  | Type.Value/Voc   | Mandatory                                                | Comment                                                                                                                                        |
|----------------------------|------------------|----------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------|
| `holder`                | DID              | Yes                                                      | A resolvable link to the holder verificationMethod to be used to uniquely identify ONE and ONLY key pair                                       |
| `hasPolicy`           | policy[] in ODRL | No                                                       | A list of `policy` expressed using ODRL                                                                                                        |
| `identityAttributes`    | String[]         | No                                                       | A list of literals representing Identity Attributes to be used in a ABAC context                                                               | 
| `identityRoles`         | String[]         | No                                                       | A list of literals representing Identity Roles to be used in a RBAC context                                                                    | 
| `parentPartyCredential` | URI              | Yes if delegated by another existing Party Credential | A resolvable link to the parent Party Credential where the gx:holder MUST be equal to the signer(issuer verificationMethod) of this credential | 

**VERY IMPORTANT NOTE**

The **credentialSubject.id** MUST identify the **DID Document** that contains the **verificationMethod** (keypair)  referenced by `gx:holder` property that *belong to/is controlled by* the **Credential Holder**.
With this solution, the PartyCredential VC can be either publicly published in case it contains no sensitive data or kept private in case it contains PII - Personal Identifiable Information.

### Private Party Credential

The **Party Credentials** containing PII are not considered to be published and reachable via their **id** to everybody, instead, they are intended to be stored in secure storage such as a wallet, secure storage device, secure vault storage, etc.
An example of this type of credential is the **NaturalPersonCredential**, issued by a Legal Participant to one of his users/employees, with the purpose to entitle a Natural Person to interact with Relying Parties(RP) in a certain context
This credential contains Name, Surname, identityAttributes, Roles, etc. and MUST NOT be published as Public Party Credentials (see later in this section) are. "Selective disclosure" during interaction with RP can be considered.

#### Private Party Credential Example

The following **NaturalPersonPartyCredential** example represents the scenario where the Participant **did:web:did.actor:alice** is issuing a credential that <!-- self? --> asserts several claims (givenName, surname, idRoles etc).
This Credential is not published (the **id** is not public) and MUST be stored in the wallet of the holder. 

Note that the `gx:holder` property is a did:key referencing a keypair owned by the **Holder** that identifies the public key of the target wallet.

 <!-- in this example we can see more fields than those explained above - are they necessary?something more should be explained? -->



### Public Party Credential

A public party credential is a **Party Credential** containing data that can be publicly accessed and queried.



### Party Credential Specialisation examples


Here, some of the possible Party Credential specialisations are defined.

#### Natural Person Party Credential

This credential is issued by a Participant to entitle a natural person (usually one of his users/employees) to interact with other Relying Parties belonging to other Participants.

This is how `Natural Person Party Credential` will be defined by the following attributes in addition to Party Credential attributes:

<!-- I guess that the table below should be reviewed -->

| Attribute      | Type.Value/Voc | Mandatory | Comment                       |
|----------------|----------------|-----------|-------------------------------|
| `givenName` | String         | Yes       | Name of the natural person    |
| `surname`   | String         | Yes       | Surname of the natural person |

#### Legal Person Party Credential

This credential is issued by a Legal Person Participant to entitle another legal person (usually one of his users <!-- could we perhaps use a better word than users? -->  ) to interact with other Relying Parties belonging to other Participants on its behalf.

This is how `Legal Person Party Credential` will be defined by the following attributes in addition to Party Credential attributes:

| Attribute                   | Type.Value/Voc | Mandatory | Comment                                         |
|-----------------------------|----------------|-----------|-------------------------------------------------|
| `organizationIdentifier` | String         | Yes       | Organization Identifier used in the eIDAS rules |

#### Service Party Credential

This credential is issued by a Participant to entitle an automated service (usually an automated process) to interact with other Relying Parties belonging to other Participants.

This is how `Service Party Credential` will be defined by the following attributes in addition to Party Credential attributes

| Attribute    | Type.Value/Voc | Mandatory | Comment                                               |
|--------------|----------------|-----------|-------------------------------------------------------|
| `baseURL` | URI            | Yes       | The base URL endpoint where the service is accessible |

#### Membership Party Credential

This credential is issued by a LegalParticipant that runs an <!-- we could replace ecosystem by data space --> ecosystem to another Participant in order to attest his Membership status.

<!-- to be added attributes specific to the Membership Credential/check with the Lab about our Membership Credential -->


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

