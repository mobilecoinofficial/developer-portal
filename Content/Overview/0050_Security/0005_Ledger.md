---
title: MobileCoin Ledger
---
MobileCoin Ledger takes transactions built with CryptoNote signatures and RingCT and adds two new improvements: membership proofs for transaction inputs and redacted transaction records. 

When a transaction is prepared by a user, it contains a ring signature with a double-spend proof. What’s a double-spend proof? One of the many ground-breaking elements of Satoshi’s Bitcoin electronic payment system was that it solved the long-standing “double-spend” problem that plagued cashless spending. Through the implementation of time-stamped transactions that are unanimously verified by a distributed network of validators, it was no longer possible for a person to spend the same funds twice. 

In addition to the double-spend proof, the MobileCoin Ledger’s ring signature also contains the proofs-of-membership for transaction inputs. Membership proofs allow the transaction to be validated without requiring disk access to the corresponding entries in the ledger. This closes a potential access-pattern side channel that could allow a malicious actor to observe the particular set of inputs used to construct the transaction. When a valid transaction is ready to be published in the MobileCoin ledger, the membership proofs are deleted and only the double-spend proof and the new transaction output are added to the public record. These "redacted transactions" make it difficult to link and deanonymize users through analysis of the public blockchain.

Redacted transactions also necessarily imply that the public blockchain entries do not contain enough information for the complete transaction history to be revalidated in an audit. Our solution is to provide two separate mechanisms for audit support. All blocks are signed as they are published by the code that performs the validation and redaction for publication.
