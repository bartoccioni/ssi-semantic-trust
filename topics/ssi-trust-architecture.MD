# Trust Architecture Document 
This document has the purpose of describing in a very concise and understandable way the **Architecture of Trust** implemented by one or more **Trust Scopes** and their **relations**. At its core, it must use the semantic model to answer the following questions:
- [**Who**](#who) 
- [**What**](#what)
- [**How**](#how)

## Who
The **Who** section must describe all the involved Actors in each Trust Scope, starting from the **Owner** who establishes both **Identity and Semantic Providers** up to all the **Parties**.

### Trust Scope Owner
Is a [Cryptographic Digital Identity](digital-identities.MD#cryptographic-identity-layer) that represents the ["Trust Anchor"](https://en.wikipedia.org/wiki/Trust_anchor) (precisely as intended in cryptography) of a Trust Scope, and not only has full control over it, but is also the **starting point** of the "chain of trust" of the **Trust Scope** itself. The **Trust Scope Owner** is the issuer of the [TrustScope](digital-identities.MD#trust-scope) credential that contains all information defining the **Trust Scope** itself.

### Trusted Identity Provider
One or more [Cryptographic Digital Identities](digital-identities.MD#cryptographic-identity-layer) that are selected by the **Trust Scope Owner** issuing a [TrustedDigitalIdentity](digital-identities.MD#trusted-digital-identity) credential, that must guarantee trustworthiness of all the **Parties** at the cryptography level issuing/revoking [TrustedIdentityProvider](digital-identities.MD#trusted-identity-provider) credential. The **Trust Identity Provider** also has the very important responsibility of ensuring the reliability of the **ControllerKind** property of the Parties, especially when the "Human in the Loop" is mandatory.

### Semantic Identity Provider
One or more [Trusted Digital Identities](digital-identities.MD##trusted-digital-identity) that are selected by the **Trust Scope Owner**, that must provide the semantic model of what is defined in the [**What**](#what) section and ensure the application all the **rules** defined in the [**How**](#how) section.

## What
The **What** section describes the **Domain** of the **Trust Scope** that will be used by all parties of the **Trust Scope**. 

### Standard Domain

### Custom Specific Domain

## How
The **How** section describes the **Rules** that apply to all parties identified in the [**Who**](#who) section when acting on the context of the **Semantic Domain** defined in the [**What**](#what) section

