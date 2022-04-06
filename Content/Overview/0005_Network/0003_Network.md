---
title: "MobileCoin Network"
description: "The MobileCoin open-source software ecosystem introduces several innovations to the cryptocurrency community, including its Ledger, Consensus Protocol, Secure Enclaves, and Fog."
hide_title: true
---
<div class="lines-header">

<h1> the <strong>MobileCoin</strong> network</h1>

The MobileCoin open-source software ecosystem introduces several innovations to the cryptocurrency community, including:

<h3 style="color: #6aacd3">MobileCoin Ledger</h3>

a new encrypted blockchain built on a technology foundation that includes CryptoNote and Ring Confidential Transactions (RingCT)

<h3 style="color: #a7a7a8">MobileCoin Consensus Protocol</h3>

a high-performance solution to the Byzantine Agreement Problem that allows new payments to be rapidly confirmed

<h3 style="color: #85ccba">Secure Enclaves</h3>

trusted execution environments using Intel's Software Guard eXtensions (SGX) to provide defense-in-depth improvements to encryption and trust 

<h3 style="color: #95b4cf">MobileCoin Fog</h3>

a scalable service infrastructure that enables a smartphone to manage an encrypted cryptocurrency with locally-stored cryptographic keys

</div>

![](/images/ecosystem_diagram_1.png)

<h2 style="color: #6aacd3">MobileCoin Ledger</h2>

In order for any payments network to function, it must be able to maintain a history of transactions. MobileCoin Ledger describes how the MobileCoin Network stores payment records in a public ledger. The ledger is implemented as a blockchain, in which each block contains transactions that include *transaction outputs (txos)* that might be spent in the future by their owners. Each transaction also includes a proof that all value spent in the transaction has never been spent before. The underlying design is based on the encrypted CryptoNote ledger protocol, which obscures the identity of all txo owners using one-time recipient addresses. The link between sender and recipient is protected through the use of input rings that guard the actually-spent txo in a large set of possibly-spent txos.

The monetary value of each txo is encrypted using the method of Ring Confidential Transactions (RingCT). RingCT is implemented using bulletproofs for improved performance. Only the receiver of the transaction can reveal the encrypted monetary value and spend the new txos that are written to the ledger. The recipient's cryptographic control over spending ensures that all transactions in MobileCoin are irreversible, similar to cash transactions in the real world.

Each txo in the input ring of a transaction is annotated with a Merkle proof of inclusion in the MobileCoin Ledger blockchain. This allows new transactions to be validated with fewer blockchain read operations, improving efficiency and reducing information leaked to data-access side channels.

Additionally, the MobileCoin Ledger dramatically improves on the baseline security offered by CryptoNote and RingCT by requiring that the input rings for every transaction are deleted before the new payment is added to the public ledger. Digital signatures are added to the ledger in place of the full transaction records to provide a basis for auditing.  

<h2 style="color: #a7a7a8">MobileCoin Consensus Protocol</h2>

MobileCoin users must agree on the content of the blockchain for it to be useful as a record of accounts. Bad actors will have a financial motive to misrepresent the ledger to enable fraud and counterfeiting. In distributed computing, the challenge of reaching agreement in a group that can't exclude malicious agents from participating is called the Byzantine Agreement Problem. All cryptocurrency payment networks must include code that solves the Byzantine Agreement Problem.

The MobileCoin Consensus Protocol solves the Byzantine Agreement Problem by requiring each user to specify a set of peers that they trust, called a *quorum*. Quorums are based on the real-life trust relationships between individuals, businesses, and other organizations that compose the MobileCoin Network. There is no central authority in the MobileCoin Network. Users accept statements about the blockchain ledger when their quorum convinces them that these statements are true. While the algorithmic design of MCP is based on the Stellar Consensus Protocol, the MobileCoin Network is not interoperable with the Stellar payment network. 

The MobileCoin Consensus Protocol avoids the environmentally-damaging mathematical "work" required by Proof-of-Work (PoW) consensus protocols like Bitcoin and realizes a much higher transaction rate than the Bitcoin consensus protocol. In contrast to Proof-of-Stake (PoS) consensus protocols, practical control of governance in MCP is ceded to the users who are trusted the most by the extended MobileCoin community, rather than to the wealthiest users who control the largest financial stakes. 

MCP ensures that all operators agree on the sequence of valid payments that are completed. New transactions are grouped in blocks and published approximately once every five seconds to the MobileCoin Ledger.

<h2 style="color: #85ccba">Secure Enclaves</h2>

Recent advances in trusted computing make it possible to run software on a remote host without exposing sensitive data to that server's operator, even when she has complete control of the remote computer's operating system (i.e. root access). Originally designed for digital rights management, these *secure enclave* technologies behave like a black box that ensures confidentiality and integrity for its content.

The MobileCoin Network implements secure enclaves using Intel's Software Guard eXtensions (SGX) to process new transactions according to the MobileCoin Consensus Protocol. Any code that needs to observe the input rings of a new transaction executes inside the black box created by the SGX trusted execution environment. Remote attestation and end-to-end encryption are used to protect the communication channel between a user submitting a new transaction and the secure enclave running on the remote server. The operator of the remote server cannot access any data that the user submits to the secure enclave, and so cannot see the set of txos used in the transaction input ring.

Remote attestation and end-to-end encryption similarly protect the communication channels between secure enclaves running on different remote servers. When the SGX remote attestation system is functioning as Intel designed, it is not possible for any operator in the MobileCoin Network to observe the full content of transactions. Complete data is only shared between secure enclaves that safely delete the information that could otherwise be used to statistically associate payment senders to payment recipients. 

<h2>MobileCoin Fog</h2>

MobileCoin Fog is a scalable service infrastructure developed by MobileCoin to enable cryptocurrencies to be safely managed from a smartphone. Fog solves two major technical challenges of encrypted cryptocurrencies on mobile devices:
1. Identifying received payments 
2. Constructing new payments 

![](/images/ecosystem_diagram_2.png)

## Identifying Received Payments

In order to check if they own any new transaction that appears in the ledger, a user must mathematically test each txo using their private keys. It is undesirable from a security standpoint to provision private keys to a remote server to monitor for received transactions, but it is impractical to perform the calculation for every new transaction on a smartphone because of the significant bandwidth and computation required

MobileCoin has developed an efficient system to help smartphone users locate their received transactions without having to test every transaction using their private keys. Each transaction output in the MobileCoin Ledger contains a discovery *hint* field. This field stores the recipient's public address, encrypted using a public key provided by MobileCoin Fog. The matching private key is stored exclusively inside a secure enclave. Each new transaction in the public ledger is processed within the secure enclave, and recognized transactions are organized for users who have registered their public address.

Additional data transformations are necessary to safely store persistent data across a scalable service infrastructure and significant care is applied to avoid leaking information through data-access side channels. The user's private keys remain on their smartphone at all times. This stands in contrast to existing *trusted query services* prevalent in other cryptocurrencies. When users provision a remote server with private keys, they are trusting that remote service's operators not to expose or share the private keys.

## Constructing New Payments

Users need access to the complete ledger in order to construct transaction input rings. It is undesirable for smartphone users to selectively download parts of the ledger from a remote server as needed, since this can potentially leak information about transaction ownership or the links between senders and recipients. The complete ledger may be many terabytes in size, which makes it impractical to download and store on a smartphone.

MobileCoin Fog allows smartphone users to selectively download ledger data using oblivious data access, meaning that the user can safely retrieve transactions or blocks from the ledger to build new payments without needing to store a complete copy of the blockchain. MobileCoin Cloud uses secure enclaves to implement oblivious data access with improved trust and efficiency.
