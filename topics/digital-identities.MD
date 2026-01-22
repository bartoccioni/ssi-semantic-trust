# Digital Identities
Digital identities are the first important piece to deal with when approaching "Semantic Trust" and also the easiest to be confused about; they can be seen as built on layers that always start from the **Cryptographic Identity Layer**.

## Digital Identities Layers
The idea behind introducing layers in digital identities is to provide a unique way to identify and categorise any digital identity and its related information. Each layer is characterised by different properties and information, which are used and important for the next layer.
Here are the three layers:
- Cryptographic Identity Layer
- Trusted Identity Layer
- Semantic Identity Layer

### Cryptographic Identity Layer

This layer is always present and topmost, representing the foundation for all the other layers, and provides information regarding the keypair and its controller, such:
- The **Actor** controlling the *private key*
  - **Anonymous** - no way to identify whether a human or a machine is the controller
  - **Human** - the controller is a human
  - **Machine** - the controller is a machine
- The **Way** this control is performed
  - **Unknown** - no info available.
  - **Direct access** - to the private key that can be copied/stolen.
  - **Hardware protected access** - to the private key on a local device (or on a remote device + multifactor auth device).

### Trusted Identity Layer

This layer is present when an **Intermediary of Trust** (also referred to as **Trust Service Provider - TSP**) is needed to ensure that, for a certain scope, a Cryptographic Digital Identity is controlled by a **known trusted party**. The several types of intermediaries of trust that can be selected depending on the scope and the needed level of trust are grouped in two main categories:
- **Self-Sovereign Identity-based** (or SSI-based) that relies on a *distributed ledger* (DLT) and *decentralized identifiers* (DID) to ensure trust across all parties of a scope under control of the intermediary itself in a privacy preserving way.
  - **Standard Intermediary** - This kind of intermediary trusts the DID and its controller. NOTE that the list of cryptographic identities contained in the DID is up to the DID Controller, and for that reason, the intermediary cannot trust a specific cryptographic identity.
  - **Strongly Bound Intermediary** - This kind of intermediary trusts ONLY a specific cryptographic identity (its public key) contained in the DID document.
- **Public Key Infrastructure-based** (or PKI-based) that relies on a standard PKI stack with a Certificate Authority (CA) that issues and revokes **X509 certificates** that are always bound to the public key of a cryptographic identity, and that contain additional information of the trusted party. 
  - **Autonomous Intermediary** - This kind of intermediary is self-operated by the subject who wants to provide trust in a certain scope (e.g. a data space governance authority, an ecosystem operator, an organisation, etc).
  - **Standard Intermediary** This kind of intermediary is part of an existing trusted PKI valid across a state or worldwide(like CA/B Forum for SSL X509 Certificates).
  - **Qualified Certificate Authority (CA) Intermediary** same as previous, but in this case, the intermediary is part of the European eIDAS and is a **Qualified Trust Service Provider** and is also used to perform Qualified Electronic Signatures.

Note: The two kinds of intermediary can be combined using an X509 certificate as one of the public keys of a DID Document, so that the best of both worlds work together

#### Cryptographic and Trusted Identity Layers combined
Here is a list showing the result of some outecomes of combining  the two Identity Layers to classify actors :

| Combination | Description |
| --- | ----------- |
| Anonymous | Keypair self-issued and no info on way to control |
| Anonymous Machine Controlled | Keypair self-issued and direct access control by machine or process  |
| Anonymous Human Controlled | Keypair managed by a device wallet controlled by a human (multi-factor authenticated) |
| Manchine Controlled | Keypair bound (associated) to a credential (VC, X509) issued by a Trust Service Provider (TSP) and fully controlled by a machine or process |
| Human Controlled | Keypair bound (associated) to a credential (VC, X509) issued by a Trust Service Provider (TSP) and managed by a device wallet controlled by a human (multi-factor authenticated) |
| eIDAS Machine Controlled | Keypair bound (associated) to a credential (VC, X509) issued by an eIDAS Qualified Trust Service Provider (TSP) and fully controlled by machine or process |
| eIDAS Human Controlled | Keypair bound (associated) to a credential (VC, X509) issued by  an eIDAS Qualified Trust Service Provider (TSP) and managed by a device wallet controlled by a human (multi-factor authenticated) |


### Semantic Identity Layer

This layer is where a digital identity becomes meaningful and is enriched with information that is useful to interact with it in a certain scope. 
