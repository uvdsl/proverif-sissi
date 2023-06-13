# PlainVCs + DIDComm+DH

In this directory, you find the files for the protocol instance employed by [SISSI architecture](https://dl.acm.org/doi/abs/10.1145/3543507.3583409) but modified to not use DIDComm/Authcrypt all the way, but instead use Diffie-Hellmann (DH). Plain Verifiable Credentials (VCs) presented in Verifiable Presentations (VPs) using DIDComm with a DH handshake.


## Outline

$Protocol$ | $Property$ | $No.$ | $Relative Path$ | $OK$ | $Attack$ 
---|---|---|---|---|---
|||||
Plain VCs + Diffie-Hellman | Secrecy | 14 | [sissi.pv#L302](sissi.pv#L302) |  [x]  | [ ]
 | | Agreement | 15 | [sissi.pv#L324](sissi.pv#L324) |  [x]  | [ ]
|||||