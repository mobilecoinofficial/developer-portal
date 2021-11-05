# This Section is In Development

This is a reminder that this "Run a Node" section is still being edited, and needs another pass.

# About this Manual

The MobileCoin Consensus Validator is a vital part of a scalable service infrastructure that enables a smartphone to manage a privacy-preserving cryptocurrency with locally-stored cryptographic keys. A Consensus Validator node is one type of node that runs in the MobileCoin ecosystem. The Consensus Validator node runs Intel's Software Guard eXtensions (SGX), which provides defense-in-depth improvements to privacy and trust. The MobileCoin Consensus Validator is part of the MobileCoin Network, an open-source software ecosystem. This manual contains a comprehensive overview of MobileCoin Consensus Validator and instructions on how to set up and manage a remote Consensus Validator node on the MobileCoin Network.

For more information about the MobileCoin Network, check out the [MobileCoin Ecosystem](https://docs.google.com/document/d/1hDU2hHjnMdqQtfkCPxcq__BbFNkDN1iDCm6Jw1kJD3g/edit?usp=sharing) - a brief document about the MobileCoin open-source software network.

## Who Should Read this Manual

This manual should be read and referenced by validator node operators, who have been entrusted with setting up and managing the nodes in the MobileCoin network. 

## How to Use this Manual

This manual provides node operators with quick help for setting up and operating the MobileCoin Consensus Server. This document is separated into three sections:

1.  Overview of the Consensus Server

2.  Step-by-step instructions for common tasks

3.  Lists of errors and alerts the Consensus Server may generate with solutions.

Special symbols are used throughout the manual to make you aware of important information and valuable tips.

### Special Symbols Used in this Manual

The following symbols represent important information and valuable tips associated with the MobileCoin Consensus Server and are displayed throughout the manual. Every noted symbol has significant meaning that you should heed:

![](https://lh4.googleusercontent.com/jm7-WYUB04M7RaXYXTqFTrxjF1WKGwn29AlsQwhnFREAFeXNmQIC80sMhD9WBsG-NjLqubnVrAveRu7SyesRMYCCj87egCWPMrov4kS49t5n4t6uu0NvO9dG76p7lQZAcMtR1jKR)  Note: Important information concerning operating the Consensus Server.

![](https://lh3.googleusercontent.com/wneoKld-9srG8N74K5oXUC6R3MBN6vwZwU79UTcTPOIRsh1bJMmIJ1o2iRJljfekmlh4ycXuB5JU60woEe-iAuQdtFM_s4vAVUYf2-_borl6-ooiO3NNNzP-GIVMey8Fr9inKl_j) Warning: Failure to follow directions may result in preventing your Consensus Server from operating successfully in the MobileCoin Network.

# Common Tasks

This runbook is specifically for node operators who will be setting up and operating validator nodes on the MobileCoin network. Running a modified version of the [Stellar Consensus Protocol](https://www.stellar.org/papers/stellar-consensus-protocol.pdf), nodes running the consensus service agree on the content of the blockchain and publish the results. By using Intel's SGX secure enclaves, transactions remain private.

| Relevant Information | Solution |
| -------------------- | -------- |
| How to report bugs | \ |
| POCs | \ |
| Links to Documentation | \ |
| Assumptions: | You have a node and are ready to run the Consensus Service. |

## Understanding the Consensus Server

MobileCoin users must agree on the content of the blockchain for it to be useful as a record of accounts. Bad actors will have a financial motive to misrepresent the ledger to enable fraud and counterfeiting. In distributed computing, the challenge of reaching agreement in a group that can't exclude malicious agents from participating is called the Byzantine Agreement Problem. All cryptocurrency payment networks must include code that solves the Byzantine Agreement Problem.

The MobileCoin Consensus Protocol solves the Byzantine Agreement Problem by requiring each user to specify a set of peers that they trust, called a quorum. Quorums are based on the real-life trust relationships between individuals, businesses, and other organizations that compose the MobileCoin Network. There is no central authority in the MobileCoin Network. Users accept statements about the blockchain ledger when their quorum convinces them that these statements are true. While the algorithmic design of MCP is based on the Stellar Consensus Protocol, the MobileCoin Network is not interoperable with the Stellar payment network.

The MobileCoin Consensus Protocol avoids the environmentally-damaging mathematical "work" required by Proof-of-Work (PoW) consensus protocols like Bitcoin and realizes a much higher transaction rate than the Bitcoin consensus protocol. In contrast to Proof-of-Stake (PoS) consensus protocols, practical control of governance in MCP is ceded to the users who are trusted the most by the extended MobileCoin community, rather than to the wealthiest users who control the largest financial stakes.

MCP ensures that all operators agree on the sequence of valid payments that are completed. New transactions are grouped in blocks and published approximately once every five seconds to the MobileCoin Ledger.

### Secure Enclaves

Recent advances in trusted computing make it possible to run software on a remote host without exposing sensitive data to that server's operator, even when she has complete control of the remote computer's operating system (i.e. root access). Originally designed for digital rights management, these secure enclave technologies behave like a black box that ensures confidentiality and integrity for its content.

The MobileCoin Network implements secure enclaves using Intel's Software Guard eXtensions (SGX) to process new transactions according to the MobileCoin Consensus Protocol. Any code that needs to observe the input rings of a new transaction executes inside the black box created by the SGX trusted execution environment. Remote attestation and end-to-end encryption are used to protect the communication channel between a user submitting a new transaction and the secure enclave running on the remote server. The operator of the remote server cannot access any data that the user submits to the secure enclave, and so cannot see the set of txos used in the transaction input ring.

Remote attestation and end-to-end encryption similarly protect the communication channels between secure enclaves running on different remote servers. When the SGX remote attestation system is functioning as Intel designed, it is not possible for any operator in the MobileCoin Network to observe the full content of transactions. Complete data is only shared between secure enclaves that safely delete the information that could otherwise be used to statistically associate payment senders to payment recipients.

### Trusted Enclave and CSS

## Prerequisites

- **IAS Account**
    <br />
    \(see [Getting Intel Attestation Service Credentials](/how-to-guides/run-a-mobilecoin-node/intel-attestation-credentials)\)

- **SGX-Enabled Machine**
    <br />
    \(see [SGX Provisioning](/how-to-guides/run-a-mobilecoin-node/configuring-your-consensus-validator-node)\)

- **Downloading and Installing the Correct SGX Driver**