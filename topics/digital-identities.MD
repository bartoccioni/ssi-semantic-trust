# Digital Identities
Digital identities are the first important piece to deal with when approaching "Semantic Trust" and also the easiest to be confused about. For that reason, I decided to create a classification of the digital identities that takes into account several elements:

- The **Number** of involved keypairs
- The **Actor** controlling the *private key* 
- The **Way** this control is performed
- The **Intermediary of Trust** bound to the *public key* often referred as Trust Service Provider



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
