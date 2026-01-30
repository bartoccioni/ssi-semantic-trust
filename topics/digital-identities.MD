# Digital Identities
Digital identities are the first important piece to deal with when approaching "Semantic Trust" and also the easiest to be confused about; they can be seen as built on layers that always start from the **Cryptographic Identity Layer**.

## Digital Identities Layers
The idea behind introducing layers in digital identities is to provide a unique way to identify and categorise any digital identity and its related information. Each layer is characterised by different properties and information, which are important and used as a starting point for the next layer.
Here are the three layers:
- Cryptographic Identity Layer
- Trusted Identity Layer
- Semantic Identity Layer

![Layers](/media/digital-identity-layers.jpg)


### Cryptographic Identity Layer

This layer is always present and topmost, representing the foundation for all the other layers, and provides information regarding the keypair and its controller of the *private key* and how control is performed:
- **Anonymous** - no way to identify whether a human or a machine is the controller
- **Human controlled** - the controller is a human and the private key is **Hardware Protected** on a human-controlled device (or on a remote device + multifactor auth device)
- **Machine controlled** - the controller is a machine/service/device and has **Direct Access**  to the private key (that can be copied/stolen).
Surely, further combinations of controller/access are possible, but they do not really bring any value in identifying the controller

### Trusted Identity Layer

This layer is present when an **Intermediary of Trust** (also referred to as **Trust Service Provider - TSP**) is needed to ensure that, for a certain scope, a Cryptographic Digital Identity is controlled by a **known trusted party**. The several types of intermediaries of trust that can be selected depending on the scope and the needed level of trust are grouped in two main categories:

- **Self-Sovereign Identity-based** (or SSI-based) that relies on a *distributed ledger* (DLT) and *decentralized identifiers* (DID) to ensure trust across all parties of a scope under control of the intermediary itself in a privacy-preserving way.
  - **Standard** - This kind of intermediary trusts the DID and its controller. NOTE that the list of cryptographic identities contained in the DID is up to the DID Controller, and for that reason, the intermediary cannot trust a specific cryptographic identity.
  - **Strongly Bound** - This kind of intermediary trusts ONLY a specific cryptographic identity (its public key) contained in the DID document.
- **Public Key Infrastructure-based** (or PKI-based) that relies on a standard PKI stack with a Certificate Authority (CA) that issues and revokes **X509 certificates** that are always bound to the public key of a cryptographic identity, and that contain additional information of the trusted party.

For both categories, there are three levels of trust
- **Scoped** - This kind of intermediary is self-operated by the subject who wants to provide trust in a certain scope (e.g. a data space governance authority, an ecosystem operator, an organisation, etc).
- **Standard** This kind of intermediary is part of an existing trusted PKI valid across a state or worldwide(like CA/B Forum for SSL X509 Certificates).
- **Qualified (Europe Only)** same as previous, but in this case, the intermediary is part of the European eIDAS framework and is a **Qualified Trust Service Provider** and is also used to perform Qualified Electronic Signatures.

Note: The two kinds of intermediary can be combined using an X509 certificate as one of the public keys of a DID Document, so that the best of both worlds work together

#### Cryptographic and Trusted Identity Layers combined
Here is a list showing the result of the most commonly used combinations of the two Identity Layers used to classify actors :

| Combination | Description |
| --- | ----------- |
| Scoped Anonymous | Keypair bound to scope-issued credential (VC, X509) and no about keypair control |
| Scoped Machine | Keypair bound to scope-issued credential (VC, X509) and direct access control by machine or process  |
| Scoped Human | Keypair bound to scope-issued credential (VC, X509) and managed by a device wallet controlled by a human (multi-factor authenticated) |
| Trusted Machine | Keypair bound (associated) to a credential (VC, X509) issued by a Trust Service Provider (TSP) and fully controlled by a machine or process |
| Trusted Human | Keypair bound (associated) to a credential (VC, X509) issued by a Trust Service Provider (TSP) and managed by a device wallet controlled by a human (multi-factor authenticated) |
| eIDAS Trusted Machine | Keypair bound (associated) to a credential (VC, X509) issued by an eIDAS Qualified Trust Service Provider (TSP) and fully controlled by machine or process |
| eIDAS Trusted Human | Keypair bound (associated) to a credential (VC, X509) issued by an eIDAS Qualified Trust Service Provider (TSP) and managed by a device wallet controlled by a human (multi-factor authenticated) |


### Semantic Identity Layer

This layer is where a **trusted digital identity** is enriched with semantic information used to interact with it by a **Semantic Intermediary** in a certain **Scope**, that is basically defined by the following elements:
- The Trust Anchor or controller -> who is defining the scope
- The semantic model -> which information is useful to define the scope and the actors participating in it
- The trusted issuers -> who is entitled to issue and revoke the above information to other trusted digital identities
- The technical details -> how and if one semantic information can be issued/verified in real-time

Below are the most common/spread scopes and their description

#### Organisation 
This scope is defined by an organisation that onboards its employees and manages their Identity, Roles, Permissions, etc to be used internally within the organisation.

| Scope element | Description |
| --- | ----------- |
| Trust Anchor | The organisation itself |
| Semantic Model | Identity information, Hierarchy information, Roles, Permissions |
| Trusted Issuers | The trusted digital identities controlled by the organisation |
| Technical Info | SSI-Based Strongly Bound + Bitstring Status List|


#### Generic Ecosystem
This scope is defined by an ecosystem governance that onboards members, assigning them specific **Ecosystem Roles and Identity Attributes** used to establish which kind of interactions a member can perform with the other members in the ecosystem.

| Scope element | Description |
| --- | ----------- |
| Trust Anchor | The Ecosystem Governace |
| Semantic Model | Identity information, Roles, Permissions |
| Trusted Issuers | The trusted digital identities controlled by the governance |
| Technical Info | SSI-Based Strongly Bound + Distributed Ledger based trusted list |

#### Simpl-Open Dataspace
This scope is defined by a data space Governance Authority that wants to define and "run" a data space using the Simpl-Open middleware

| Scope element | Description |
| --- | ----------- |
| Trust Anchor | The data space governance authority |
| Semantic Model | Identity information, Roles, Rights, Permissions (defined using Identity Attributes) |
| Trusted Issuers | The trusted digital identity deployed in the data space governance authority Agent  |
| Technical Info | PKI-Based + Security Attributes Provider |

#### EDC Dataspace
This scope is defined by a data space Governance Authority that wants to define and "run" a data space using the EDC (Eclispe Data space Component) platform

| Scope element | Description |
| --- | ----------- |
| Trust Anchor | The data space governance authority |
| Semantic Model | Identity information, Roles, Rights, Permissions (defined using Verifiable Credentials) |
| Trusted Issuers | The trusted digital identity controlled by the data space governance authority  |
| Technical Info | SSI-Based + Bitstring Status List |


The two most used forms of information enrichment are:
- Using Attributes in conjunction with PKI-based trusted identities
- Using Verifiable Credentials in conjunction with SSI-based trusted identities

