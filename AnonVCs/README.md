# AnonVCs - Anonymous Version of the SISSI Protocol

In this directory, you find the files for a anonymous version of the protocol employed by [SISSI architecture](https://dl.acm.org/doi/abs/10.1145/3543507.3583409):
Anonymous Verifiable Credentials presented as derived credentials using DIDComm with session key pairs.
In addition, there is a modified verision of the protocol that is modified to not use DIDComm/Authcrypt all the way, but instead use Diffie-Hellmann (DH): Anonymous Verifiable Credentials presented as derived credentials using DIDComm with a DH handshake.

## Outline

$Protocol$ | $Property$ | $No.$ | $Relative Path$ | $OK$ | $Attack$ 
---|---|---|---|---|---
|||||
[Anon VCs](DIDComm/) | Secrecy | 11 | [sissi.pv#L297](DIDComm/sissi.pv#L297) |  [x]  | [ ]
 | | Agreement | 12 | [sissi.pv#L319](DIDComm/sissi.pv#L319) |  [x]  | [ ]
| | Unlinkability | 13 | [sissi_unlinkablity_ok_wrt_verifier.dps](DIDComm/sissi_unlinkablity_ok_wrt_verifier.dps) |  [x]  | [ ]
|||||
[Anon VCs + Diffie-Hellmann](DIDComm%2BDH/) | Secrecy | 16 | [sissi.pv#L312](DIDComm%2BDH/sissi.pv#L312) |  [x]  | [ ]
 | | Agreement | 17 | [sissi.pv#L334](DIDComm%2BDH/sissi.pv#L334) |  [x]  | [ ]
|||||