# PlainVCs + DIDComm

In this directory, you find the files for the protocol instance employed by [SISSI architecture](https://dl.acm.org/doi/abs/10.1145/3543507.3583409). Plain Verifiable Credentials (VCs) presented in Verifiable Presentations (VPs) using DIDComm with session key pairs.

## Outline

$Protocol$ | $Property$ | $No.$ | $Relative Path$ | $OK$ | $Attack$ 
---|---|---|---|---|---
|||||
Plain VCs | Secrecy | 1 | [sissi.pv#L287](sissi.pv#L287) |  [x]  | [ ]
 | | | 2 | [archive/sissi_forward_secrecy.pv](archive/sissi_forward_secrecy.pv) |  [x]  | [ ]
 | | Agreement | 3 | [sissi.pv#L309](sissi.pv#L309) |  [x]  | [ ]
 | | | 4 | [sissi_ok_VP_leaked.pv](sissi_ok_VP_leaked.pv) |  [x]  | [ ]
 | | | 5 | [sissi_unforgeable_VC.pv](sissi_unforgeable_VC.pv) |  [x]  | [ ]
| | | 6 | [sissi_attack_domain_missing_replay.pv](sissi_attack_domain_missing_replay.pv) |  [ ] | [x] 
| | | 7 | [sissi_attack_no_nonce_VP_leaked.pv](sissi_attack_no_nonce_VP_leaked.pv) |  [ ] | [x] 
| | | 8 | [sissi_attack_VC_reissued.pv](sissi_attack_VC_reissued.pv) |  [ ] | [x] 
| | Unlinkability | 9 | [sissi_unlinkable.dps](sissi_unlinkable.dps) |  [x]  | [ ]
| | | 10 | [sissi_attack_verifier_unlinkablity.dps](sissi_attack_verifier_unlinkablity.dps) |  [ ] | [x] 
|||||