---
title: "What is Bulletproofs?"
description: "The fastest Bulletproofs implementation ever, featuring single and aggregated range proofs, strongly-typed multiparty computation, and a programmable constraint system API for proving arbitrary statements (under development)."
use_file: "mobilecoinfoundation/bulletproofs/README.md"
---
The fastest [Bulletproofs][bp_website] implementation ever, featuring
single and aggregated range proofs, strongly-typed multiparty
computation, and a programmable constraint system API for proving
arbitrary statements (under development).

This library implements Bulletproofs using [Ristretto][ristretto],
using the `ristretto255` implementation in
[`curve25519-dalek`][curve25519_dalek].  When using the [parallel
formulas][parallel_edwards] in the `curve25519-dalek` AVX2 backend, it
can verify 64-bit rangeproofs **approximately twice as fast** as the
original `libsecp256k1`-based Bulletproofs implementation.

Please see [this document](https://github.com/mobilecoinfoundation/bulletproofs/blob/main/README.md) for more details
