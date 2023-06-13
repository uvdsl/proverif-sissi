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