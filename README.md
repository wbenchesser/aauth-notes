## AAuth: Agent Authorization and Authentication

### What is it?
* AAuth (short for Agent Authentication and Authorization) is an open-standard internet protocol designed specifically for the era of AI agents. It provides a cryptographic framework for agent identity, resource access, and user delegation.

* AAuth gives every automated client (like an AI agent) its own unique cryptographic identity. It uses signed requests rather than bearer tokens, combining identity and authorization claims into a single protocol flow. It is designed to complement existing standards like OAuth 2.0 and OpenID Connect (OIDC) rather than replace them entirely.

### Where did it come from?
* AAuth is an active Internet-Draft specification being developed within the [IETF (Internet Engineering Task Force)](https://www.ietf.org/archive/id/draft-hardt-aauth-protocol-00.html).

* Its primary creator and author is Dick Hardt, who is famously known as the original author of OAuth 2.0. Hardt recognized that the web security infrastructure he helped build decades ago for static web applications was fundamentally unequipped to handle autonomous, dynamic AI agents.

### What problem does it solve?
* Traditional web authentication protocols were built on the assumption that a human user is interacting with a static application that has pre-defined permissions and a known execution path. AI agents break this assumption. They act semi-independently, discover tools on the fly, and cross multiple clouds or organizations to complete a single task. AAuth fixes several flaws that occur when trying to force AI agents into traditional OAuth/OIDC systems:

    * <u>**Client Identities Don't Travel**</u>: In standard OAuth, a client_id issued by Google means nothing to GitHub. Because AI agents routinely jump between entirely different services, they need a universal, independent identity that they can carry across trust domains.

    * <u>**The Vulnerability of Bearer Tokens**</u>: OAuth relies heavily on bearer tokens. If an AI agent's bearer token is stolen or leaked in a prompt injection attack, the attacker gets full access. AAuth enforces proof-of-possession by default using HTTP Message Signatures. A token is cryptographically bound to the agent's private key, making stolen tokens useless to interceptors.

    * <u>**Decisions Happen Mid-Task**</u>: Traditional auth assumes permissions are granted upfront at login. However, AI agents assemble their toolchains live as a task unfolds. If an agent discovers it needs a new API mid-task, AAuth allows for "on-demand" authorization, allowing human consent pauses to be treated as a first-class state rather than a system error.

    * <u>**Static Scopes Don't Capture Intent**</u>: Traditional scopes are broad. AAuth introduces rich, *context-aware* authorization that evaluates what the agent is doing in the moment.
        * As an example, consider an action `mail.read`.
        * A security system cannot tell if an AI agent is reading your email just to summarize a flight confirmation, or if it has been compromised and is scraping your entire inbox.

    * <u>**Multi-Hop / Chained Calls**</u>: When an AI agent has a complex job, it might call `Service A`, which then needs to call downstream `Service B`. Traditional OAuth struggles to pass authorization securely down a chain. AAuth allows interaction and authorization requirements to cleanly bubble back up the chain to the user.
    
### Recent Updates
* In April 2026, author Dick Hardt updated and consolidated the core specification into a new [baseline draft](https://datatracker.ietf.org/doc/draft-hardt-aauth-protocol/#:~:text=draft%2Dhardt%2Daauth%2Dprotocol%2D02.%20Status.%20Versions%3A%2000.%2001.%2002.%20This,Formats.%20txt%20html%20xml%20htmlized%20bibtex%20bibxml.). Updates include: 
    1. Introduction of "Missions" and "Mission Logs"
        * A **Mission** is a cryptographic JSON object containing the context of what an agent is allowed to do (e.g., approved tools, approver details, and timestamp) along with a human-readable description.
        * It uses a **Mission Log**, which acts as an ordered, live ledger maintained by the authorization server. Every token request, tool execution, and user interaction is recorded here so the system can dynamically evaluate if the agent's behavior matches its original intent.
    2. 
