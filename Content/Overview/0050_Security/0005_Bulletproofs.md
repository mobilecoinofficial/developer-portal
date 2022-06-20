---
title: "Bulletproofs"
description: "The fastest Bulletproofs implementation ever, featuring single and aggregated range proofs, strongly-typed multiparty computation, and a programmable constraint system API for proving arbitrary statements (under development)."
---

MobileCoin uses Bulletproofs [link] in client transaction construction to obscure the transaction input and output amounts, while proving in zero-knowledge that the ledger will remain balanced.

Bulletproofs is an efficient (fast) algorithmic approach introduced by Bünz in 2017 that has greatly improved performance. It is now possible to use transactions with protected amounts without reducing the throughput of the payments network. 

Mobilecoin uses the[Dalek](https://github.com/dalek-cryptography/bulletproofs) library which is the fastest [Bulletproofs][bp_website] implementation ever, featuring single and aggregated range proofs, strongly-typed multiparty computation, and a programmable constraint system API for proving arbitrary statements (under development). 

The Dalek library implements Bulletproofs using [Ristretto][ristretto], using the `ristretto255` implementation in
[`curve25519-dalek`][curve25519_dalek]. When using the parallel formulas in the curve25519-dalek AVX2 backend, it can verify 64-bit rangeproofs approximately twice as fast as the original libsecp256k1-based Bulletproofs implementation.

Please see [this document](https://github.com/mobilecoinfoundation/bulletproofs/blob/main/README.md) for more details.
