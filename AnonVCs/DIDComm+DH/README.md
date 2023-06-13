# # AnonVCs + DIDCommDH

In this directory, you find the files for a anonymous credential version of the protocol employed by [SISSI architecture](https://dl.acm.org/doi/abs/10.1145/3543507.3583409), and it is modified to not use DIDComm/Authcrypt all the way, but instead use Diffie-Hellmann (DH). Anonymous Verifiable Credentials presented as derived credentials using DIDComm with a DH handshake.

## Outline

$Protocol$ | $Property$ | $No.$ | $Relative Path$ | $OK$ | $Attack$ 
---|---|---|---|---|---
|||||
Anon VCs + Diffie-Hellmann | Secrecy | 16 | [sissi.pv#L312](sissi.pv#L312) |  [x]  | [ ]
 | | Agreement | 17 | [sissi.pv#L334](sissi.pv#L334) |  [x]  | [ ]
|||||