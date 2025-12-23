# Digital Identities
Digital identities are the first important piece to deal with when approaching "Semantic Trust" and also the easiest to be confused about; they can be seen as built on layers that always start from the **Cryptographic Identity Layer**.

## Digital Identities Layers
The idea behind introducing layers in digital identities is to provide a unique way to identify and categorise any digital identity and its related information. Each layer is characterised by different properties and information, which are used and important for the next layer.
Here are the three layers:
- Cryptographic Identity Layer
- Trusted Identity Layer
- Semantic Identity Layer

### Cryptographic Identity Layer

This layer is always present and topmost, representing the foundation for all the other layers, and provides information regarding the keypair, such:
- The **Actor** controlling the *private key*
  - **Anonymous** - no way to identify whether a human or a machine is the controller
  - **Human** - the controller is a human
  - **Machine** - the controller is a machine
- The **Way** this control is performed
  - **Unknown** - no info available.
  - **Direct access** - to the private key that can be copied/stolen.
  - **Hardware protected access** - to the private key on a local device (or on a remote device + multifactor auth device).

### Trusted Identity Layer

This layer is present when an **Intermediary of Trust** (also referred as Trust Service Provider - TSP) is needed to ensure that, for a certain scope, a Cryptographic Digital Identity is controlled by a known trusted party. There are several types of intermediaries of trust that can be selected depending on the scope and the needed level of trust:
- **SSI Intermediary** uses a distributed ledger to store/remove a Decentralized Identifier (DID) that contains the public key of the cryptographic identity. This kind of intermediary only trusts the DID controller with all the public keys contained in the DID Document and NOT the Cryptographic Digital Identity.
- **Strongly Bound SSI Intermediary** use a distributed ledger to store/remove a Decentralized Identifier (DID) together with the public key of the cryptographic identity. This kind of intermediary trust only the Cryptographic Digital Identity contained in the DID document.
- **Certificate Authority (CA) Intermediary** that issues/revokes a **X509 Certificate** bound to the public key of the cryptographic identity and can contains additional information of the trusted party. A certificate can be also used in conjuction with a DID Document to enable SSI capabilities. 
- **Qualified Certificate Authority (CA) Intermediary** same as previous but the CA is an eIDAS Qualified Trust Service Provider.

### Semantic Identity Layer



```
      Anonymous:
        description: Keypair self issued and no info on controller 
      AnonymousManchineControlled:
        description: Keypair self issued and controlled by machine or process
      AnonymousHumanControlled:
        description: Keypair managed by a device wallet controlled by a human (multi-factor authenticated)
      ManchineControlled:
        description: Keypair bound (associated) to a credential (VC, X509) issued by a Trust Service Provider (TSP) and fully controlled by machine or process
      HumanControlled:
        description: Keypair bound (associated) to a credential (VC, X509) issued by a Trust Service Provider (TSP) and managed by a device wallet controlled by a human (multi-factor authenticated)
      eIDASManchineControlled:
        description: Keypair bound (associated) to a credential (VC, X509) issued by an eIDAS Qualified Trust Service Provider (TSP) and fully controlled by machine or process
      eIDASHumanControlled:
        description: Keypair bound (associated) to a credential (VC, X509) issued by  an eIDAS Qualified Trust Service Provider (TSP) and managed by a device wallet controlled by a human (multi-factor authenticated)
