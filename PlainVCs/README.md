# Plain VCs - SISSI Protocol

In this directory, you find the files for the protocol instance employed by [SISSI architecture](https://dl.acm.org/doi/abs/10.1145/3543507.3583409): Plain Verifiable Credentials (VCs) presented in Verifiable Presentations (VPs) using DIDComm with session key pairs.
In addition, there is a modified verision of the protocol that is modified to not use DIDComm/Authcrypt all the way, but instead use Diffie-Hellmann (DH): Plain Verifiable Credentials (VCs) presented in Verifiable Presentations (VPs) using DIDComm with a DH handshake.


## Outline

$Protocol$ | $Property$ | $No.$ | $Relative Path$ | $OK$ | $Attack$ 
---|---|---|---|---|---
|||||
[Plain VCs](DIDComm/) | Secrecy | 1 | [sissi.pv#L287](DIDComm/sissi.pv#L287) |  [x]  | [ ]
 | | | 2 | [archive/sissi_forward_secrecy.pv](DIDComm/archive/sissi_forward_secrecy.pv) |  [x]  | [ ]
 | | Agreement | 3 | [sissi.pv#L309](DIDComm/sissi.pv#L309) |  [x]  | [ ]
 | | | 4 | [sissi_ok_VP_leaked.pv](DIDComm/sissi_ok_VP_leaked.pv) |  [x]  | [ ]
 | | | 5 | [sissi_unforgeable_VC.pv](DIDComm/sissi_unforgeable_VC.pv) |  [x]  | [ ]
| | | 6 | [sissi_attack_domain_missing_replay.pv](DIDComm/sissi_attack_domain_missing_replay.pv) |  [ ] | [x] 
| | | 7 | [sissi_attack_no_nonce_VP_leaked.pv](DIDComm/sissi_attack_no_nonce_VP_leaked.pv) |  [ ] | [x] 
| | | 8 | [sissi_attack_VC_reissued.pv](DIDComm/sissi_attack_VC_reissued.pv) |  [ ] | [x] 
| | Unlinkability | 9 | [sissi_unlinkable.dps](DIDComm/sissi_unlinkable.dps) |  [x]  | [ ]
| | | 10 | [sissi_attack_verifier_unlinkablity.dps](DIDComm/sissi_attack_verifier_unlinkablity.dps) |  [ ] | [x] 
|||||
[Plain VCs + Diffie-Hellman](DIDComm%2BDH/) | Secrecy | 14 | [sissi.pv#L302](DIDComm%2BDH/sissi.pv#L302) |  [x]  | [ ]
 | | Agreement | 15 | [sissi.pv#L324](DIDComm%2BDH/sissi.pv#L324) |  [x]  | [ ]
|||||

## Documentation (doc)

In [doc](doc), you will find Message Sequence Charts (MSCs) that illustrate the protocol and variations of the protocol that are prone to attacks. 

$MSC$ | $Description$
---|--- 
[msc-full-protocol.pdf](doc/msc-full-protocol.pdf) | An illustration of the protocol, using the current notation (as introduced in the top-level [README.md](../README.md)). It corresponds to the Proverif code in [DIDComm/sissi.pv](DIDComm/sissi.pv), and the DeepSec code in [DIDComm/sissi_unlinkable.dps](DIDComm/sissi_unlinkable.dps).
[msc-protocol.pdf](doc/msc-protocol.pdf) | An illustration of the protocol, using an old notation. It corresponds to the archived Proverif code in [DIDComm/archive/sissi_multiparty_agreement.pv](DIDComm/archive/sissi_multiparty_agreement.pv).
[msc-mitm-attack.pdf](doc/msc-mitm-attack.pdf) | An illustration of an attack on the protocol, when the domain, i.e., the intended verifier, of an credential presentation, is omitted. An attack is able to replay a credential presentation. The MSC uses the old notation and corresponds to the Proverif code in __No. 6__, [DIDComm/sissi_attack_domain_missing_replay.pv](DIDComm/sissi_attack_domain_missing_replay.pv), which we already updated to the new notation. (Updating the MSC notation is on the TODO list.)
[msc-njagreement-attack.pdf](doc/msc-njagreement-attack.pdf.pdf) | An illustration of an attack on the protocol, when the holder, i.e., the recipient of a freshly issued credential, does not check and verify the received credential. This results in a potential re-issuance by an attacker using an already issued credential by an honest issuer, thereby violating multi-party agreement. The MSC uses the old notation and corresponds to the Proverif code in __No. 8__, [DIDComm/sissi_attack_VC_reissued.pv](DIDComm/sissi_attack_VC_reissued.pv), which we already updated to the new notation. (Updating the MSC notation is on the TODO list.)
[msc_anon_multiparty_agreement_dh.pdf](doc/msc_anon_multiparty_agreement_dh.pdf) | A hand-scribbled concept on how to use a Diffie-Hellmann (DH) handshake within the protocol. This was the basis for exploring DIDComm+DH where we do not use DIDComm's _authcrypt_ on every message but instead use a DH handshake to then use a shared secret for encrypting messages.

---

# Description of Attacks

When not following the protocol but, e.g., omitting fields that are marked as "optional" by SSI specifications, the following attacks may be possible.

