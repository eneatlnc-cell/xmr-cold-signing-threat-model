# Cold Monero Signing on a Network-Less Android Device — Threat Model

**Status: draft, seeking critique. Nothing here is implemented. No Monero code has
been written.** This document exists so that people who know Monero can tell me
where I am wrong **before** anything gets built.

**Read it here: [xmr-cold-signing-threat-model.pdf](xmr-cold-signing-threat-model.pdf)**

## The one-sentence version

Move the **signing** step onto a device with no network stack, keep the **chain
state** step on a device that does, and make the boundary between them an
explicit, documented, attackable thing rather than an assumption.

## What's inside

| § | Topic |
| --- | --- |
| 2 | Assets and trust boundaries (B1–B4; B2 is where this design lives or dies) |
| 3 | Key custody: the honest constraint — Android Keystore cannot host ed25519, so the design is a *TEE-wrapped seed*, stated plainly as weaker than a hardware wallet, with the property table to prove it |
| 4 | Attacker positions P1–P5 |
| 6 | Attack surface A1–A8: change-address substitution, fee manipulation, key-image reuse, ring/proof-data poisoning, stale chain view, bundle parsing, confirmation-UI spoofing, … |
| 7 | The minimum the cold side must verify independently |
| 9 | What I am asking the Monero community — including whether the TEE-wrapped-seed model is worth building at all |

## Context

The cold device in this design is the [Vault](https://github.com/eneatlnc-cell/Engine3.0)
app of the Engine E2EE messaging project: a network-less Android app whose manifest
declares **zero INTERNET permission**, whose identity keys are generated inside
Android Keystore/TEE and are non-exportable, and which gates every private-key
operation behind biometric authentication. The open protocol/crypto stack of that
project is published at **[Engine3.0](https://github.com/eneatlnc-cell/Engine3.0)**
(AGPL-3.0) for public audit.

The intended use case is signing an OTC exit directly from cold storage, without
routing a large holding through a hot wallet first.

## Feedback

Section 9 lists the open questions. Issues on this repo are the right place for
critique; security-relevant remarks about the existing (non-Monero) codebase go
through the [disclosure policy](https://github.com/eneatlnc-cell/Engine3.0/blob/main/SECURITY.md).
