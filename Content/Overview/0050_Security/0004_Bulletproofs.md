---
title: "Confidential Transactions"
description: "The fastest Bulletproofs implementation ever, featuring single and aggregated range proofs, strongly-typed multiparty computation, and a programmable constraint system API for proving arbitrary statements (under development)."
use_file: "mobilecoinfoundation/bulletproofs/README.md"
---
CryptoNote ledgers are significantly more secure than Bitcoin, but they still leave the amount of each transaction in plain sight. 

On the MobileCoin ledger, the monetary value of each txo is encrypted using the method of Ring Confidential Transactions (RingCT). Only the receiver of the transaction can reveal the encrypted monetary value and spend the new txos that are written to the ledger. The recipient’s cryptographic control over spending ensures that all transactions in MobileCoin are irreversible, similar to cash transactions in the real world.

The Ring Confidential Transactions (RingCT) was published in 2016 by Shen Noether. RingCT protects the amount exchanged in each transaction using cryptography. Rather than publish the value that is exchanged, RingCT transactions include a mathematical proof that the transaction is balanced, meaning that the recipient didn't receive more money than the sender spent. However, this originally required a computationally intensive proof.

RingCT is implemented using bulletproofs for improved performance. Bulletproofs is an efficient (fast) algorithmic approach introduced by Bünz in 2017 that has greatly improved performance. It is now possible to use transactions with protected amounts without reducing the throughput of the payments network. 

Mobilecoin uses the[Dalek](https://github.com/dalek-cryptography/bulletproofs) library which is the fastest [Bulletproofs][bp_website] implementation ever, featuring single and aggregated range proofs, strongly-typed multiparty computation, and a programmable constraint system API for proving arbitrary statements (under development). The Dalek library implements Bulletproofs using [Ristretto][ristretto], using the `ristretto255` implementation in
[`curve25519-dalek`][curve25519_dalek]. 

Please see [this document](https://github.com/mobilecoinfoundation/bulletproofs/blob/main/README.md) for more details.
