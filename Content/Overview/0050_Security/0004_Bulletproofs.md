---
title: "Confidential Transactions"
description: "The fastest Bulletproofs implementation ever, featuring single and aggregated range proofs, strongly-typed multiparty computation, and a programmable constraint system API for proving arbitrary statements (under development)."
use_file: "mobilecoinfoundation/bulletproofs/README.md"
---
CryptoNote ledgers are significantly more secure than Bitcoin, but they still leave the amount of each transaction in plain sight. One solution was published in 2016 by Shen Noether, called Ring Confidential Transactions (RingCT). RingCT protects the amount exchanged in each transaction using cryptography. Rather than publish the value that is exchanged, RingCT transactions include a mathematical proof that the transaction is balanced, meaning that the recipient didn't receive more money than the sender spent. However, this originally required a computationally intensive proof.

Bulletproofs is an efficient algorithmic approach introduced by Bünz et al. in 2017 that has greatly improved performance. It is now possible to use transactions with protected amounts without reducing the throughput of the payments network. The [Dalek-cryptography/bulletproofs](https://github.com/dalek-cryptography/bulletproofs) library implements fastest [Bulletproofs][bp_website] implementation ever, featuring single and aggregated range proofs, strongly-typed multiparty computation, and a programmable constraint system API for proving arbitrary statements (under development).

This library implements Bulletproofs using [Ristretto][ristretto], using the `ristretto255` implementation in
[`curve25519-dalek`][curve25519_dalek].  When using the [parallel formulas][parallel_edwards] in the `curve25519-dalek` AVX2 backend, it can verify 64-bit rangeproofs **approximately twice as fast** as the original `libsecp256k1`-based Bulletproofs implementation.

Please see [this document](https://github.com/mobilecoinfoundation/bulletproofs/blob/main/README.md) for more details.
