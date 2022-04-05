---
title: Blockchain & Fog
---

![](https://raw.githubusercontent.com/mobilecoinofficial/developer-portal/main/images/Blockchain.png)

# Explain Like I'm 5:
## What is MobileCoin's Blockchain?
[What is MobileCoin's Blockchain?]: #what-is-blockchain

Blockchain is a new technology that is decentralizing trade and allowing peer-to-peer transactions to occur, removing the intermediaries. How does MobileCoin use this innovation to allow two people to send digital cash using their smartphones?

### To a Five-Year Old Child:

Imagine you have a bag of puzzle pieces and you give a piece to each of your friends. If you all get together, you can solve the puzzle. However, if someone brings a piece from another puzzle, or you are missing a piece, the puzzle remains unsolved. Whenever someone in the world wants to buy something, you or one of your friends needs to place a new puzzle piece down.

### To a Grandparent:

Blockchain is like a counting mechanism, a ledger. It is similar to the invention of double-entry accounting, which is a bookkeeping method that has been used since the 1400s. Double-entry accounting involves recording each transaction in two accounts, resulting in a credit to one account and a debit to another. The resulting transaction must balance out. Blockchains make it harder to cheat by having many different copies of the ledger, each held by a different person who all come together to agree on the information in their ledgers. The technology ensures that the accounts always balance, and no one can cheat.

### To a First Year College Student:

The importance of blockchain is that it democratizes information. It allows people to trade one-on-one. Anyone can validate that the transactions are valid. Blockchain is keeping track of data in a way that blocks large corporations or big tech companies from tracking the user's data to sell. For example, Google, AWS, and Microsoft provide cloud services, but the users' data is the product, which the technology companies can sell. Blockchain can be used for multiple uses, not just finance. It is a trustworthy database that can be used for running video games, tracking physical goods, recording weather and/or sports scores, or as a supply chain.

The blockchain itself is an append-only sequence (ledger) of all transactions. It uses cryptography to encode all of the transactions, so no one can see what has happened. This also means that previous transactions cannot be changed because the cryptography reveals if someone has tried to manipulate the data. To create a new entry in the ledger, you take the existing ledger and use a mathematical equation to add information to the chain which ensures that each new entry is provably linked to all prior transactions. The math proves truth -- any inconsistencies reveal that someone has tampered with the ledger.

### To a Software Engineer:

Blockchain is a decentralized approach to keeping track of data. It can be a ledger, a log, or a history of important items. On a blockchain network, there are multiple types of participants: servers, nodes, miners (in the case of Bitcoin's blockchain network), and/or computers. Each one of the nodes receives a proposed transaction. If one of the nodes in a trusted group of peers has a different set of transactions than the rest of the nodes, the other servers correct the mistake through consensus. As the consensus mechanism reaches its conclusion, a new block is appended to the chain using a cryptographic proof known as a hash. Succinctly, this is why it is called a blockchain: it is a chain of blocks linked by cryptographic knowledge.

The MobileCoin Blockchain is a digital ledger that records financial information of financial transactions, the giving and receiving of funds. The design of the MobileCoin Blockchain ledger is based on the privacy-preserving CryptoNote ledger protocol, which obscures the identity of all users' transaction outputs (TxOuts) using one-time recipient addresses. The transaction is encrypted by Ring Confidential Transactions (RingCT), which hides the actual transaction value amid decoys, a larger set of possibly-spent transactions. The MobileCoin Blockchain is encrypted, which means that the TxOuts are constructed in such a way that users can only receive funds or know how much they have spent by using their private keys to view or spend the transaction. Explicitly: only the counterparties in a transaction are privy to its contents.

Each TxOut is annotated with a Merkle proof of inclusion, a secret committed message in a finite group of messages, in the MobileCoin Blockchain ledger. A Merkle proof of inclusion (or membership) can be validated without going to the ledger database and in constant time -- without revealing which output the user is spending. Additionally, MobileCoin's Blockchain improves on the baseline privacy offered by CryptoNote and RingCT by deleting input rings for every transaction before the new payment is appended to the public ledger. This provably destroys the transaction graph.

One erroneous assumption about blockchains is that it is being used for both the consensus mechanism, how the servers decide on what data to append to the blockchain, and the record of those decisions, that is, the result of the consensus. Technically, the blockchain is just the record of those decisions finalized in "blocks." However, a lot of people conflate the two.

The blockchain is the result of the consensus validation. Consensus occurs after a certain number of servers (peers) vote together that the outputs being sent from one person to another are legitimate thereby coming to "consensus" on the next valid block. If they all agree the transactions are legitimate, the peers accept the transaction, and they append the encrypted part of that transaction to the blockchain. Only the recipient has the private key to decrypt it.

MobileCoin's Blockchain differs from other blockchains. Other networks retain complete copies of all transactions, so new participants can validate the chain independently. Alternatively, MobileCoin uses MobileCoin Fog to offload the transactional data. Once a transaction has been validated, most of its data is discarded. Only the information necessary to make new transactions are saved in the MobileCoin Blockchain, such as key images and outputs. MobileCoin protects users from data leakage, location monitoring, and surveillance.

Using MobileCoin Fog (discussed next), MobileCoin has made the blockchain private and secure for smartphone usage.
