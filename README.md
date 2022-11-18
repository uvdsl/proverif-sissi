# SISSI Authentication and Authorization Protocol in ProVerif

This is a [ProVerif](https://bblanche.gitlabpages.inria.fr/proverif/) description of the protocol flow for authentication and authorization from the SISSI architecture.

The protocol is indended for a client-server scenario on the Web.
A client desires are Web resource served by the server.
Upon request, access is denied and the client is pointed towards the authentication-authorization endpoint.
The client acts as the prover and the authentication-authorization endpoint acts as the verifier.

The authentication-authorization endpoint enforces predefined access control rules (ACRs).
These ACRs specify which _attribute_ the prover needs to have asserted by which _issuer_.

```
Example: The prover P must be a student of university U.
The prover must hold a credential that says (P,studentOf,U) signed by U.
The prover must present this credential and prove that they are in-fact in control of P.
```
> Note that verifying the truthfullness of the asserted statement, i.e., that the prover P is actually a student of U, is not the task of the verifier but the task of the issuer, i.e., the university U.
The verifier trusts the issuer university U that the asserted statement is in-fact true.
This is also the reason why the verifier does not accept such credentials to be signed by anyone but only by issuers that the verifier trusts.
How that trust between verifier and issuer is established is out-of-scope. (Some ideas: personal knowledge, trusted accreditation bodies, DNS checks of well-known websites, ...)

## Assumptions:
- Agents are identified using DIDs [1].
- Each agent is in control of their DID, i.e., in control of the secret key associated to that DID.
- Prover P holds a credential (P,attr,I) signed by issuer I.
- Verifier holds an access control rule (P,attr,I) for which a matching and correctly signed credential is to be presented by any prover P.
- The desired resource is known to be available at URI `u`.
- The desired resource is served via HTTPS.

The HTTPS response from the desired resource indicates _the verifying agent_ via the verifier's DID.
Resovling the verifier's DID yields the corresponding DID document which includes a `serviceEndpoint`, the authentication-authorizaiton endpoint.

The authentication-authorization endpoint is a HTTP endpoint, which may or may not be served via HTTPS.

> Note: I am unsure if HTTPS has an impact here. Probably HTTPS is still beneficial. To be discussed!

### 1. Phase - Handshake: Prover/Client authenticates Verifier/Server

The prover initiates a new communication session, minting a new session key pair for encrypting any communication following DIDComm [2].
The initial message, i.e., HTTP(S) request to the authentication-authorization enpoint, includes the `prover's session public key` of the prover and a nonce `n_p`.
The prover signs the message with their `session secret key`. 
The prover uses the `verifier's DID public key` to encrypt the message.

The receiving agent, i.e., the verifier, decrypts the message with their `verifier's DID secret key`.
> Note: This ensures that only the agent that has control over the secret key from the DID that the prover received from the resource's HTTPS response can read the initial message (seeAlso threads 1 and 2).

The verifer extracts the `sender's session public key` from the message and authenticates the integrity of the message.
Then the verifier mints a new session key pair, mints their own nonce and creates a response message.
The message includes the sender's nonce `n_p`, the verifier's nonce `n_v` and the `verifier's session public key`.
>Note/Question: Do we need to include the public key of the sender here to prevent MITM/relay of the message? Or is this already sufficiently adressed? To be discussed!

The message is signed by the `verifier's DID secret key`.
> Note/Question: Do we need to sign the message using the session key as well? To be discussed!

The message is encrypted using the `sender's session public key`.

>Note: In our setting, this was one HTTP(S) request and one HTTP(S) response between the client (prover) and authentication-authorization endpoint (verifier).

The prover receives the message and decrypts it with its `session private key`, authenticates it with the `verifier's DID public key`.
If the received nonce `n'_p` matches the original `n_p`, the verifier is authenticated to be in control of `the verifier's DID secret key` and thus to be assumed to be the verifier identified by their DID.
Moreover, the prover now knows the `verifier's session public key`.
> Note: So far, only the prover has authenticated the verifier.
The verifier does not know anything yet to whom they are talking. So far - so standard for client-server interactions.

### 2. Phase - Verifier/Server authenticates & authorizes Prover/Client

The prover creates a new message that includes the desired resource's URI `u` and the verifier's nonce `n_v`. 
The message is signed and then encrypted with the session keys and then send to the verifier (HTTP(S) request).

The verifier decrypts and authenticates the integrity of the message using the session keys.
If the received nonce `n'_v` matches the original nonce `n_v`, the verifier knows that this request came from the same sender as before and the sender is really in control of the session secret key.
>Note: So far, the verifier only knows they are talking to the same sender as before.

The verifier looks up the ACR `r = (P,attr,I)` for resource u and mints a new nonce `n_c` which together form the new message.
The new message is signed and encrypted using the session keys and send back to the sender (HTTP(S) response).

> Note: Which ACR is chosen is up to the verifier. The verifier may simply send all the potentially applicable rules or just one of them - depending on how the rules are structured. For example, being a student may only be a first requirement. The prover would also need to be enrolled in a specific course as a second criterion. When the verifier wants to minimise disclosed information, they will only disclose the most general criterion, i.e., being a student, in the first round. Only after the prover has successfully proven to be a student, the verifier will ask for provenance of the course enrollment. 

The prover decrypts and authenticates the integrity of the message.
The prover checks if they hold any credential that matches the received rule `(P,attr,I)`.
The matching credential is packaged together with the nonce `n_c` in a _Verifiable Presentation (VP)_ that is signed by the prover with their DID P secret key.
This VP is then signed and encrypted using the session keys and send to the verifier (HTTP(S) request).

The verifier decrypts and authenticates the integrity of the message.
By extracting `P`from the credential presented in the VP and resolving the DID to its DID document, the verifier is able to obtain the `prover's DID public key`.
The verifier authenticates the signature on the VP using that public key.
If the received nonce `n'_c` matches the original nonce `n_c`, the verifier knows that this request came from the same sender as before and the sender is really in control of the session secret key and the `prover's DID secret key`.
>Note: As per assumption, agents are identified using DIDs and the assumption underlying any digital interaction is that secret keys are not shared with anyone else, the verifier could assume that the sender being in control over the prover's DID secret key is in-fact the prover. #authentication

The verifier further evaluates if the received credential is valid and satisfies the ACR:
The issuer DID is extracted from the credential.
If the issuer DID from the credential matches the issuer DID required by the ACR, the resolved thereby yielding the `issuer's DID public key`.
With that key, the signature of the credential is authenticated.
If the attribute asserted by the valid credential matches the attribute required by the ACR, the verifier accepts the ACR to be verified.
> Note: The verifier trusts that the asserted attributes of P are in-fact true. Moreover, the trust relationship from verifier to issuer already predates this interaction as the ACR predefined the specific issuer identified by their DID I to be required. #authorization

As the prover has been successfully been authenticated to be in control of DID P's secret key and P to have an asserted attribute issued by DID I which matches the required ACR, the prover is authorized to access the resource.
How that is implement exactly is a minor detail, be it via some access token (as indicated in our case) or direct resource access, is not relevant to the protocol at hand.


## Thread model
1. URI `u` is a malicious URI, and does not identify or yield the desired resource (_phishing attack_ -> no solution, just excluded per assumption).
2. The HTTPS response from the desired resource includes a malicious agent's DID as verifier (_man-in-the-middle attack_ -> adressed per HTTPS, except in case of thread 1).
3. Message `(n_p,n_v,spk_v)` could be relayed to any agent as it does not include the recipients session public key. (_man-in-the-middle attack_ -> is this really necessary or just necessary in general?)
4. ...?
5. Dishonest issuer: That is the problem of the verifier. If the verifier trusts a dishonest issuer, it can't be mitigated.
    - Take the dishonest CA for TLS certificates for example, just a big mess.
6. Dishonest verifier: 
    - a VC phisher?
    - needs to be in control over resource u 
7. Dishonest prover: 
    - case (a): prover not in control over DID P 
        -> cannot present a valid VP of a credential issued to P.
    - case (b): prover in control over DID P, but prover is not the same agent that was issued credential by I.
    (identity sharing)
        -> isnt this the same as for any identity sharing?

### References
[1] https://www.w3.org/TR/did-core/  
[2] https://identity.foundation/didcomm-messaging/spec/  
