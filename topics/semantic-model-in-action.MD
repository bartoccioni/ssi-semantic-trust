# Semantic Model in Action



## Trust Scope Credential

### TrustScope credential specialisation examples

Several specialisations of the **TrustScopeCredential** can be easily defined, for example:

#### **Organisation Trust Scope** 
This specialisation is a self-issued credential that entitles the issuing organisation to define its *Roles/Identity Attributes* to be used by identify and authorise its parties:

   - **scope** - *Organization Credential Management* (OCM).
   - **trusted issuers** - the organisation itself.
   - **vocabularies** - defines the semantics of *Roles/Identity Attributes* and **Domain Specific Credentials** that are valid in the defined scope.
   - **trusted list** - to assign and revoke *Roles/Identity Attributes* and **Domain Specific Credentials** to its parties (users, natural persons, endpoint services, etc) by issuing **PartyCredentials**.

#### **Gaia-X Compliance Trust Scope** 
This credential is issued by  Gaia-X and entitles an organisation to issue attestations about specific claims in the context of Gaia-X Compliance.

   - **scope** - issue attestations used to attest Gaia-X Compliance.
   - **trusted issuers** - the organisation accredited by Gaia-X to issue attestations in the defined scope.
   - **vocabularies** - defines the semantics applicable to issue attestations in the defined scope.
   - **trusted list** - to assign and revoke attestations issued to Gaia-X Providers. Trust Anchor Credential specialisation examples

#### **Ecosystem Trust Scope**
This specialisation is a self-issued credential that entitles an Ecosystem operator to define:

   - **scope** - manage an *Ecosystem* (onboarding/offboarding/role assignment etc.).
   - **trusted issuers** - the Ecosystem itself. <!-- the Ecosystem Governance Authority? -->
   - **vocabularies** - defines the semantic of *Roles/Identity Attributes* and **Domain Specific Credentials** valid in the defined scope.
   - **trusted list** - to assign and revoke *Roles/Identity Attributes* and **Domain Specific Credentials** to its members (other Participants) issuing **MembershipPartyCredentials**.

## Party Credential

### Private Party Credential

**Party Credentials** containing PII are not considered to be published and resolvable via their **id** to everybody, instead, they are intended to be stored in secure storage such as a wallet, secure storage device, secure vault storage, etc.
An example of this type of credential is the **NaturalPersonCredential**, issued by an Organisation to one of its users/employees, with the purpose to entitle a Natural Person to interact with Relying Parties(RP) in the context of the organisation.

### Public Party Credential

A public party credential is a **Party Credential** containing data that can be publicly accessed and queried.
An example of this is the **MembershipPartyCredential** issued by a Trusted Issuer of a specific Trust scope (Organisation, Ecosystem, Data space, etc) to a Trusted Digital Identity in order to attest its Membership status in the scope context.


#### Private Party Credential Example

The following **NaturalPersonPartyCredential** example represents the scenario where the Participant **did:web:did.actor:alice** is issuing a credential that asserts several claims (givenName, surname, idRoles etc).
This Credential is not published (the **id** is not public) and MUST be stored in the wallet of the holder. 

Note that the `holder` property is a did:key referencing a keypair owned by the **Holder** that identifies the public key of the target wallet.



### Party Credential Specialisation examples

Here, some of the possible Party Credential specialisations are defined.

#### Natural Person Party Credential

This credential is issued by a Participant to entitle a natural person (usually one of their users/employees) to interact with other Relying Parties belonging to other Participants.

This is how `Natural Person Party Credential` will be defined by the following attributes in addition to Party Credential attributes:

<!-- I guess that the table below should be reviewed -->

| Attribute      | Type.Value/Voc | Mandatory | Comment                       |
|----------------|----------------|-----------|-------------------------------|
| `givenName` | String         | Yes       | Name of the natural person    |
| `surname`   | String         | Yes       | Surname of the natural person |

#### Legal Person Party Credential

This credential is issued by a Legal Person Participant to entitle another legal person (usually one of its users) to interact with other Relying Parties belonging to other Participants on its behalf.

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

This credential is issued by a Trusted Issuer of a specific Trust scope (Organisation, Ecosystem, Data space, etc) to a Trusted Digital Identiti in order to attest his Membership status.




