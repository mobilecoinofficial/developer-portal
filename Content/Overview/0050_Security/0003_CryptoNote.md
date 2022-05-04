---
title: CryptoNote
---
In order for any payments network to function, it must be able to maintain a history of transactions. The MobileCoin Ledger
describes how the MobileCoin Network stores payment records in a public ledger. The ledger is implemented as a 
blockchain, in which each block contains transactions that include transaction outputs (txos) that might be spent in 
the future by their owners. Each transaction also includes a proof that all value spent in the transaction has never 
been spent before. 

The underlying design is based on the privacy-preserving CryptoNote ledger protocol, which obscures 
the identity of all txo owners using one-time recipient addresses. In 2012, Nicolas van Saberhagen, an unknown anonymous author, published the CryptoNote whitepaper describing a new distributed ledger protocol called CryptoNote. At its core, CryptoNote is an attempt to improve on the design of Bitcoin to create an encrypted ledger.

CryptoNote introduced two innovations: ring signatures and one-time addresses. 

The link between sender and recipient is protected through the use of input rings that guard the actually-spent txo in a large set of possibly-spent txos. Ring signatures make it harder to statistically analyze the network by changing the direct links between buyers and sellers used in Bitcoin into probabilistic links between sets of possible buyers and sellers. Every transaction has a set of possible ancestors which makes tracking payments in the public ledger much more difficult. 

CryptoNote's one-time addresses allow payments to be received using numerical aliases that are indistinguishable from random numbers for everyone except the intended recipient. Essentially, every transaction in Bitcoin publicly shares the recipient's pseudonymous address, while CryptoNote ledgers publish the recipient's address in an encrypted format that protects user data.


### Show Me the Code

* [Ledger Crate](https://github.com/mobilecoinfoundation/mobilecoin/tree/master/ledger): the implementation of the MobileCoin ledger using LMDB as the data store.
* [Transaction Crate](https://github.com/mobilecoinfoundation/mobilecoin/tree/master/transaction/core): Implementation of MobileCoin transactions.

### Tech Talk: Transactions

For a walk through of how the MobileCoin Transaction Protocol works, see [MobileCoin Tech Talk #1: Transactions](https://www.youtube.com/watch?v=e9afDQ_M5CU)
