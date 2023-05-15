# SISSI Authentication and Authorization Protocol in ProVerif

This is a [ProVerif](https://bblanche.gitlabpages.inria.fr/proverif/) description of the protocol flow for authentication and authorization from the [SISSI architecture](https://dl.acm.org/doi/abs/10.1145/3543507.3583409).

The protocol is indended for a client-server scenario on the Web.
A client desires are Web resource served by the server.
Upon request, access is denied and the client is pointed towards the authentication-authorization endpoint.
The client acts as the prover and the authentication-authorization endpoint acts as the verifier.

The authentication-authorization endpoint enforces predefined access control rules (ACRs).
These ACRs specify which attribute the prover needs to have asserted by which issuer.

## Example
In a nutshell, Verifiable Credentials (VCs) enable an agent (the "holder") to prove to a second agent (the "verifier") that a third agent (the "issuer") has asserted and signed some statements or claims. 
In other words, with a presentation of a VC an agent proves to a verifying agent that 

1. they are in possession of the VC
2. the VC was issued by a particular issuer (agent)
3. the VC contains some claims, e.g., attributes of the holder
4. the VC was presented by the proving agent, e.g., the holder itself

For example, consider the use case of a student accessing online teaching material of some  guest professor.
Here, the student's university, the issuer, provides the student, the holder, a digital student credential, which asserts that the holder is a student signed by the university.
A guest professor at the university has a (private) Web server where online teaching material is accessible only to students of the university they are guest lecturing at.
As the professor does not know all the students, i.e., the group of all students is private, the professor can not grant this group access to the Web resources. 
To gain access to the online teaching material, the students have to prove that they are really enrolled at the university using a presentation, i.e., a VP, of the corresponding digital student credential, i.e., the VC.
This VP includes the VC and some additional meta data, and is typically signed by the holder to prove that the student assertion is really about them.
In this way, the professor's Web server can verify the above mentioned facts (1)-(4).
The use case is generaliseable to general accessing resources on the Web.



## Assumptions:
- The __Self-Sovereign Identity (SSI) assumption__:  
All agents can mint and manage a keypairs in a self-sovereign manner.
- The __Decentralised Identifier (DID) assumption__:  
All agents assume that the (integrity of the) link between DID of an agent, the thereby advertised public key and corresponding secret key can be trusted, in the sense that we assume that only the controller of the DID (the agent in control of the corresponding secret key) is able to modify/update the public key linked to the DID.
Each agent is thus assumed to keep the link between the DID, the thereby advertised their public key and their corresponding secret key, intact (meaning they use the corresponding secret key in communication).
Possible implementations e.g. via certification in Government Registries , Governed Blockchains or Smart Contracts.
- The __Verifier-Issuer assumption__:  
The verifier assumes the issuer to have due diligence on validating the assertions they make, e.g. that Holder is actually a student (which may be out-of-band).
The verifier thus assumes that the issuer behaves honestly according to the protocol (given the context of the use case).

## The Protocol with Plain VCs (not anonymous)

Find a Message Sequence Chart (MSC) of the plain model [here](./PlainVCs/doc/msc-full-protocol.pdf).

Here, we give the corresponding formal model (cf. [Proverif code](./PlainVCs/DIDComm/sissi.pv)):

### Grammar
$~$ | $~$ | $~$ | $~$ | $~$ 
---|---|---|---|---
M, N, K ::= | x | $\textit{variables}$ | |
$~$ | (M, $\ldots$, N) | $\textit{tuples}$ |
$~$ | $proj_{i}(M)$ | $i^{th} \textit{projection}$ | $proj_{i}\left(M_1, \ldots, M_i, \ldots, M_n \right)$ | $= M_i$
$~$ | $pk(K) $ | $\textit{public key}$ |
$~$ | $\{ M \}_{K}$ | $\textit{encryption}$ |
$~$ | $dec( M , K )$ | $\textit{decryption}$ | $dec( \{ M \}_{K} , K )$ | $= M$  
$~$ | $sig( M , K ) $| $\textit{signature}$ |
$~$ | $check( M , K )$ | $\textit{check signature}$ | $check( sig( M , K ) , K )$ | $= true$

### Part 1 - Issuance

Holder $(P, sk_P, I, pk_I, V, pk_V)$ | Issuer $(I, sk_I, attr, P, pk_P)$
--- | ---
$\textit{new } ssk_{PI}, n_p, n_h; $  | $\textit{new } ssk_I, n_i; $
$\textit{let } m'_{0} := (n_p,pk(ssk_{PI})) \textit{ in}$  | $\textit{ch}(m_{0}); $ 
$\textit{let } m_{0} := \{(m'_{0},\textit{sig}(m'_{0},ssk_{PI}))\}_{pk_I} \textit{ in}$  | $\textit{let } ((n_p,spk_{PI}),\textit{s}_0):= \textit{adec}(m_{0},sk_I) \textit{ in}$
$\overline{\textit{ch}}(m_{0}); $  | $\textit{if } \textit{check}((n_p,spk_{PI}),\textit{s}_0, spk_{PI}) \textit{ then}$
$\textit{ch}(m_{1}); $  | $\textit{let } m'_{1} := (n_p,n_i,pk(ssk_I)) \textit{ in}$
$\textit{let } ((n'_p,n_i,spk_I), \textit{s}_1) := \textit{adec}(m_{1},ssk_{PI}) \textit{ in}$  | $\textit{let } m_{1} := \{(m'_{1},\textit{sig}(m'_{1},sk_I))\}_{spk_{PI}} \textit{ in}$
$\textit{if } \textit{check}((n'_p,n_i,spk_I),\textit{s}_1,pk_I) \textit{ then}$  | $\overline{\textit{ch}}(m_{1}); $
$\textit{if } n'_p = n_p \textit{ then}$  | $\textit{ch}(m_{2}); $
$\textit{let } m'_{2} := ((n_i,P,I,n_h),\textit{sig}((n_i,P,I,n_h),sk_P)) \textit{ in}$  | $\textit{let } (((n'_i,P',I',n_h),\textit{s}_P),\textit{s}_2) := \textit{adec}(m_{2},ssk_I) \textit{ in}$ 
$\textit{let } m_{2} := \{(m'_{2},\textit{sig}(m'_{2},ssk_{PI}))\}_{spk_I} \textit{ in}$  | $\textit{if } \textit{check}(((n'_i,P',I',n_h),\textit{s}_P),\textit{s}_2,spk_{PI}) \textit{ then}$
$\overline{\textit{ch}}(m_{2}); $  | $\textit{if } \textit{check}((n'_i,P',I'),\textit{s}_P,pk_P) \textit{ then}$
$\textit{ch}(m_{3}); $  | $\textit{if } (n'_i,P',I') = (n_i,P,I) \textit{ then}$
$\textit{let } (((((P',attr,I'), \textit{s}_I), P'',n'_h), s_H), \textit{s}_3) := \textit{adec}(m_{3},ssk_{PI}) \textit{ in}$  | $\textit{let } {\textit{claims}} := (P, \textit{attr},I) \textit{ in}$
$\textit{if } \textit{check}(((((P',attr,I'), \textit{s}_I), P'', n'_h), s_H)\textit{s}_3, spk_I) \textit{ then}$  | $\textit{let } {\textit{VC}} := ({\textit{claims}} ,\textit{sig}({\textit{claims}}, sk_I)) \textit{ in}$ 
$\textit{if } \textit{check}((((P',attr,I'), \textit{s}_I), P'', n'_h), \textit{s}_H, spk_I) \textit{ then}$  | $\textit{let } m'_{3} := ((\textit{VC},P,n_H),\textit{sig}((\textit{VC},P,n_H),sk_I)) \textit{ in}$ 
$\textit{if } \textit{check}((P',attr,I'), \textit{s}_I, pk_I) \textit{ then}$  | $\textit{let } m_{3} := \{(m'_{3},\textit{sig}(m'_{3},ssk_I))\}_{spk_{PI}} \textit{ in}$
$\textit{if } (P',I',P''n'_h) = (P,I,P, n_h) \textit{ then}$  | $\overline{\textit{ch}}(m_{3}); $
$!Prover (P, sk_P, {\textit{VC}}, V, pk_V) $  |

### Part 2 - Provenance

Prover $(P, sk_P, \textit{VC}, V, pk_V)$ | Verifier $(V, sk_V, {\textit{RULE}}, pk_P, pk_I, {\textit{URI}})$ 
--- | ---
$\textit{new } ssk_{PV}, n_p; $  | $\textit{new } ssk_V, n_i, n_c, \textit{tkn}; $ 
$\textit{let } m'_{4} := (n_p,pk(ssk_{PV})) \textit{ in}$  | $\textit{ch}(m_{4}); $
$\textit{let } m_{4} := \{(m'_{4},\textit{sig}(m'_{4},ssk_{PV}))\}_{pk_V} \textit{ in}$  | $\textit{let } ((n_p,spk_{PV}),\textit{s}_4) := \textit{adec}(m_{4},sk_V) \textit{ in}$
$\overline{\textit{ch}}(m_{4}); $  | $\textit{if } \textit{check}((n_p,spk_{PV}),\textit{s}_4,spk_{PV}) \textit{ in}$
$\textit{ch}(m_{5}); $  | $\textit{let } m'_{5} := (n_p,n_v,pk(ssk_V)) \textit{ in}$
$\textit{let } ((n'_p,n_v,spk_V),\textit{s}_5) := \textit{adec}(m_{5},ssk_{PV}) \textit{ in}$  | $\textit{let } m_{5} := \{ (m'_{5},\textit{sig}(m'_{5},sk_V)) \}_{spk_{PV}} \textit{ in}$ 
$\textit{if } \textit{check}((n'_p,n_v,spk_V),\textit{s}_5,pk_V) \textit{ then}$  | $\overline{\textit{ch}}(m_{5}); $ 
$\textit{if } n'_p := n_p \textit{ then}$  | $\textit{ch}(m_{6}); $
$\textit{let } m'_{6} := (n_v, {\textit{URI}}) \textit{ in}$  | $\textit{let } ((n'_v,uri'),\textit{s}_6)  := \textit{adec}(m_{6},ssk_V) \textit{ in}$ 
$\textit{let } m_{6} := \{(m'_{6},\textit{sig}(m'_{6},ssk_{PV}))\}_{spk_V} \textit{ in}$  | $\textit{if } \textit{check}((n'_v,uri'),\textit{s}_6,spk_{PV}) \textit{ then}$  
$\overline{\textit{ch}}(m_{6}) $ | $\textit{if} (n'_v,{\textit{URI}\, \\'}) = (n_v,{\textit{URI}}) \textit{ then}$
$\textit{ch}(m_{7}); $  | $\textit{let } m'_{7} := (n_c,{\textit{RULE}}) \textit{ in}$ 
$\textit{let } ((n_c,\textit{RULE} ),\textit{s}_7) := \textit{adec}(m_{7},ssk_{PV}) \textit{ in}$  | $\textit{let } m_{7} := \{(m'_{7},\textit{sig}(m'_{7},ssk_V))\}_{spk_{PV}} \textit{ in}$
$\textit{if } \textit{check}((n_c,\textit{RULE} ),\textit{s}_7,spk_V) \textit{ then}$  | $\overline{\textit{ch}}(m_{7}); $ 
$\textit{let } (\textit{claims},\textit{s}_{I}):=\textit{VC}  \textit{ in}$  | $\textit{ch}(m_{8}); $
$\textit{if } \textit{claims}=\textit{RULE} \textit{ then}$  | $\textit{let } (((((P',attr',I'), \textit{s}_I),n'_c,V'), \textit{s}_P ),\textit{s}_8) := \textit{adec}(m_{8},ssk_V) \textit{ in}$
$\textit{let } {\textit{VP}} := (({\textit{VC}},n_c,V), \textit{sig}(({\textit{VC}},n_c,V),sk_P)) \textit{ in}$  | $\textit{if } \textit{check}(((((P',attr',I'), \textit{s}_I),n'_c,V'), \textit{s}_P ),\textit{s}_8,spk_{PV}) \textit{ then}$
$\textit{let } m_{8} := \{{\textit{VP}},\textit{sig}({\textit{VP}},ssk_{PV})\}_{spk_V} \textit{ in}$  | $\textit{if } \textit{check}((((P',attr',I'), \textit{s}_I),n'_c,V'), \textit{s}_P, pk_P) \textit{ then}$
$\overline{\textit{ch}}(m_{8}) $  | $\textit{if } \textit{check}((P',attr',I'), \textit{s}_I, pk_I) \textit{ then}$
$\textit{ch}(m_{9}) $  | $\textit{if } ((P',attr',I'),n'_c,V') = ((P,attr,I),n_c,V) \textit{ then}$
$\textit{let }((\textit{tkn},\textit{s}_\textit{tkn}),\textit{s}_9) := (\textit{adec}(m_{9},ssk),spk_V) \textit{ in}$  | $\textit{let } m'_{9} := (\textit{tkn},\textit{sig}(\textit{tkn},sk_V)) \textit{ in}$ 
$\textit{if } \textit{check}((\textit{tkn},\textit{s}_\textit{tkn}),\textit{s}_9,spk_V) \textit{ then}$  | $\textit{let } m_{9} := \{\textit{sig}(m'_{9},ssk_V)\}_{spk_{PV}} \textit{ in}$
$\textit{if } \textit{check}(\textit{tkn},\textit{s}_\textit{tkn},pk_V) \textit{ then}$  | $\overline{\textit{ch}}(m_{9}); $ 
### Setup


Issuer $(I, sk_I, attr, P, pk_P)$ | Holder $(P, sk_P, I, pk_I, V, pk_V)$ |  Verifier $(V, sk_V, {\textit{RULE}=(P,attr,I)}, pk_P, pk_I, {\textit{URI}})$ 
--- | --- | ---
$\textit{let } pk_I := \textit{getPubKey}(I) \textit{ in}$ | $\textit{let } pk_P := \textit{getPubKey}(P) \textit{ in}$ | $\textit{let } pk_V := \textit{getPubKey}(V) \textit{ in}$ 
$\textit{let } pk_I = \textit{getPubKey}(proj_{2}({\textit{VC}\,\\'}) $ | $\textit{let } pk_P := \textit{getPubKey}(proj_{1}(m'_{2})) \textit{ in}$ 


### Configuration

$~$ | $~$
--- | ---
$\textit{new } sk_I, sk_P, sk_V; $ | 
$\overline{ch}((pk(sk_I), pk(sk_P), pk(sk_V))); $ |
$!\textit{Issuer}^{honest}(I, sk_I, \textit{attr}, P, pk(sk_P))    \mid      $ |
$!\textit{Holder}^{honest}(P, sk_P, I, pk(sk_I), V, pk(sk_V))          \mid  $ |
$!\textit{Verifier}^{honest}(V, sk_V, {(  P,   attr,   I  )}, pk(sk_P), pk(sk_I), {\textit{URI}})     \mid  $ |
$connect(EasP, pk_{EasP}); $ |
$\textit{Issuer}^{open}(I, sk_I, \textit{attr}, EasP, pk_{EasP}) ~\mid$ |
$connect(EasI, pk_{EasI}, EasV, pk_{EasV}); $ |
$\textit{Holder}^{open}(P, sk_P, EasI, pk_{EasI}, EasV, pk_{EasV})          \mid $ |
$connect(EasP, pk_{EasP}, EasI, pk_{EasI}); $
$\textit{Verifier}^{open}(V, sk_V, {(  EasP,   attr,   EasI )}, pk_{EasP}, pk_{EasI}, {\textit{URI}}); $ |

