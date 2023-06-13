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

For a description of the potential attacks when deviating from the protocol, visit the [Plain VCs](../README.md) parent directory.

A short description of the other files, marked as OK (the security properties are formally verified):

- __No. 1 & 3__, [sissi.pv](sissi.pv), is the instance of the protocol that is employed by the [SISSI architecture](https://dl.acm.org/doi/abs/10.1145/3543507.3583409), and that we commonly refer to as "our instance of the protocol". We use the notation as introduced in the top-level [README.md](../../README.md). The protocol is illustrated by this [MSC](../doc/msc-full-protocol.pdf). 
- __No. 2__, [archive/sissi_forward_secrecy.pv](archive/sissi_forward_secrecy.pv), proves that forward secrecy holds for the protocol, even when long-term private keys are leaked. This model of the protocol uses an old notation, and is archived. Nonetheless, the formal verification of the security property holds.
- __No. 4__, [sissi_ok_VP_leaked.pv](sissi_ok_VP_leaked.pv), models the protocol in the case that the Verifiable Presentation (VP) of the credential, which is used for authentication, is later leaked to an attacker. It proves that even with a leaked VP _secrecy_ and _agreement_ still hold, and that an attack is not able to capitalise on the leaked VP.
- __No. 5__, [sissi_unforgeable_VC.pv](sissi_unforgeable_VC.pv), proves that a Verifiable Credential (VC), which is issued by a particular actor, i.e. the issuer, can not be forged by an attacker mimicking the specific issuer.
- __No. 6__, [sissi_attack_domain_missing_replay.pv](sissi_attack_domain_missing_replay.pv), illustrates a replay attack on the protocol, when the `domain`, i.e., the intended recipient, of the VP is omitted. It is illustrated by this [MSC](../doc/msc-mitm-attack.pdf), and a description is available in the [Plain VCs](../README.md) parent directory.
- __No. 7__, [sissi_attack_no_nonce_VP_leaked.pv](sissi_attack_no_nonce_VP_leaked.pv), illustrates a replay attack on the protocol, when the `nonce` of the VP is omitted, and the VP is leaked. A description is available in the [Plain VCs](../README.md) parent directory.
- __No. 8__, [sissi_attack_VC_reissued.pv](sissi_attack_VC_reissued.pv), illustrates a replay attack on the protocol, when the holder, i.e., the recipient of a freshly issued credential does not check and verify the received credential. It is illustrated by this [MSC](../doc/msc-njagreement-attack.pdf), and a description is available in the [Plain VCs](../README.md) parent directory.
- __No. 9__, [sissi_unlinkable.dps](sissi_unlinkable.dps), illustrates _unlinkability_ from the perspective of the issuer. It proves that, after issuing a credential, it is impossible for the issuer to track how it is used with honest verifier.
- __No. 10__, [sissi_attack_verifier_unlinkablity.dps](sissi_attack_verifier_unlinkablity.dps), illustrates a stronger property, i.e., _unlinkability from the perspective of both the issuer
and verifier_, which further ensures that verifiers cannot link two uses of the
same credential. This __property cannot be achieved for plain Verifiable Credentials__, since the identifier of the holder appears in each Verifiable Presentation.