# Trust Architecture Document 
This document has the purpose of describing in a very concise and understandable way the **Architecture of Trust** implemented by one or more **Trust Scopes** and their **relations**. At its core, it must use the semantic model to answer the following questions:
- Who 
- What
- How

## Who
The **Who** section must describe all the involved Actors in each Trust Scope, starting from the **Owner** who establishes both **Identity and Semantic Providers** up to all the **Parties**.

### Trust Scope Owner
Is the ["Trust Anchor"](https://en.wikipedia.org/wiki/Trust_anchor) (precisely as intended in cryptography) of a Trust Scope, and not only has full control over it, but is also the starting point of the "chain of trust" of the **Trust Scope** itself.

### Trusted Identity Providers


- How the Actors in a Scope can interact with each other
- How to extend trust across Scopes and enable inter-scope Actors' interactions
- Require a human action in interactions between Actors
