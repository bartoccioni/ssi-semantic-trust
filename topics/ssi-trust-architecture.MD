# Trust Architecture Document 
This document has the purpose of describing in a very concise and understandable way the **Architecture of Trust** implemented by one or more **Trust Scopes** and their **relations**. At its core, it must use the semantic model to answer the following questions:
- [**Who**](#who) is involved
- [**What**](#what) is the subject 
- [**How**](#how) is done

## Who
The **Who** section must describe all the involved Actors in each Trust Scope, starting from the **Owner** who establishes both **Identity and Semantic Providers** up to all the **Parties**.

### Trust Scope Owner
Is a [Cryptographic Digital Identity](digital-identities.MD#cryptographic-identity-layer) that represents the ["Trust Anchor"](https://en.wikipedia.org/wiki/Trust_anchor) (precisely as intended in cryptography) of a Trust Scope, and not only has full control over it, but is also the **starting point** of the "chain of trust" of the **Trust Scope** itself. The **Trust Scope Owner** is the issuer of the following credentials:

- [TrustedDigitalIdentity](semantic-model.MD##trusted-digital-identity) credentials for all the selected [Trusted Identity Providers](#trusted-identity-providers) and [Semantic Identity Providers](#semantic-identity-providers)
- [TrustedIdentityProvider](semantic-model.MD#trusted-identity-provider) credentials for all the [Trusted Identity Providers](#trusted-identity-providers)
- [TrustScope](semantic-model.MD#trust-scope) credential that contains all information defining the **Trust Scope** itself.

### Trusted Identity Providers
One or more [Trusted Digital Identity](semantic-model.MD##trusted-digital-identity) that must guarantee trustworthiness of all the **Parties** at the cryptography level by issuing the [TrustedDigitalIdentity](semantic-model.MD#trusted-digital-identity) credentials and their [Type](#trusted-identity-types), especially when the "Human in the Loop" is mandatory.

### Trusted Identity Types
The list of the allowed Parties [cryptographic and trusted identity](digital-identities.MD#cryptographic-and-trusted-identity-layers-combined) combinations, clearly stating when the `ControllerKind` is **Human**

### Semantic Identity Providers
One or more [Trusted Digital Identities](semantic-model.MD##trusted-digital-identity) that are selected by the **Trust Scope Owner**, that must provide the semantic model of what is defined in the [**What**](#what) section and ensure the application all the **rules** defined in the [**How**](#how) section.

## What
The **What** section establishes how the [**Semantic Model**](semantic-model.MD) is used in the **Domain** of the **Trust Scope** and how it is extended to cover unforeseen use cases. Concretely, is the ontology of the semantic model translated into vocabulary and schema used to check all the Verifiable Credentials used within the context of the **Trust Scope**, including `TrustScope`, `TrustedIdentityProvider`, and `TrustedDigitalIdentity` that are used in the [**Who**](#who) section. 

### Use and customise the Model
Here is where the IAA scope is implemented and formalised, listing all the foreseen Attributes, Roles, and Permissions that will be used in the **Trust Scope** 

### Extend the model Model
Here is where all unforeseen IAA entities can be defined and added to the standard Model to be used in the **Trust Scope** 

### PartyCredential specialisations
If a [Semantic Identity Provider](#semantic-identity-provider) needs to have different specialisations of PartyCredential to identify different types of Party, like organisations, users, members, participants, etc, here is the place where those specialisations have to be defined.

### Assertions
Here is where the assertion specialisations are defined to be used by Parties

## How
The **How** section describes the **Rules** and **Constraints** that the Semantic Identity Provider apply to all parties identified in the [**Who**](#who) section when acting on the context of the **Semantic Domain** defined in the [**What**](#what) section. 
The rules are expressed using ODRL policies, and the rule "The party must be a human" becomes :
```
{
  "constraint": [
    {
      "leftOperand": "TrustedDigitalIdentity.ControllerKind",
      "operator": "eq",
      "rightOperand": "Human"
    }
  ]
}
```

