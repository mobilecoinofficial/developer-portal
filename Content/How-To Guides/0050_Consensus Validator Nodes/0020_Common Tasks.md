# Common Tasks
This runbook is specifically for node operators who will be setting up and operating validator nodes on the MobileCoin network. Running a modified version of the [Stellar Consensus Protocol](https://www.stellar.org/papers/stellar-consensus-protocol), nodes running the consensus service agree on the content of the blockchain and publish the results. By using Intel's SGX secure enclaves, transactions remain private.
| Relevant Information | Solution |
| -------------------- | -------- |
| How to report bugs   |          |
| POCs                 |          |
| Links to Documentation |         |
| Assumptions:          | You have a node and are ready to run the Consensus Service. |

## Understanding the Consensus Server
MobileCoin users must agree on the content of the blockchain for it to be useful as a record of accounts. Bad actors will have a financial motive to misrepresent the ledger to enable fraud and counterfeiting. In distributed computing, the challenge of reaching agreement in a group that can’t exclude malicious agents from participating is called the Byzantine Agreement Problem. All cryptocurrency payment networks must include code that solves the Byzantine Agreement Problem.

The MobileCoin Consensus Protocol solves the Byzantine Agreement Problem by requiring each user to specify a set of peers that they trust, called a quorum. Quorums are based on the real-life trust relationships between individuals, businesses, and other organizations that compose the MobileCoin Network. There is no central authority in the MobileCoin Network. Users accept statements about the blockchain ledger when their quorum convinces them that these statements are true. While the algorithmic design of MCP is based on the Stellar Consensus Protocol, the MobileCoin Network is not interoperable with the Stellar payment network. 

The MobileCoin Consensus Protocol avoids the environmentally-damaging mathematical “work” required by Proof-of-Work (PoW) consensus protocols like Bitcoin and realizes a much higher transaction rate than the Bitcoin consensus protocol. In contrast to Proof-of-Stake (PoS) consensus protocols, practical control of governance in MCP is ceded to the users who are trusted the most by the extended MobileCoin community, rather than to the wealthiest users who control the largest financial stakes. 

MCP ensures that all operators agree on the sequence of valid payments that are completed. New transactions are grouped in blocks and published approximately once every five seconds to the MobileCoin Ledger.

### Secure Enclaves

Recent advances in trusted computing make it possible to run software on a remote host without exposing sensitive data to that server’s operator, even when she has complete control of the remote computer’s operating system (i.e. root access). Originally designed for digital rights management, these secure enclave technologies behave like a black box that ensures confidentiality and integrity for its content. 

The MobileCoin Network implements secure enclaves using Intel’s Software Guard eXtensions (SGX) to process new transactions according to the MobileCoin Consensus Protocol. Any code that needs to observe the input rings of a new transaction executes inside the black box created by the SGX trusted execution environment. Remote attestation and end-to-end encryption are used to protect the communication channel between a user submitting a new transaction and the secure enclave running on the remote server. The operator of the remote server cannot access any data that the user submits to the secure enclave, and so cannot see the set of txos used in the transaction input ring.

Remote attestation and end-to-end encryption similarly protect the communication channels between secure enclaves running on different remote servers. When the SGX remote attestation system is functioning as Intel designed, it is not possible for any operator in the MobileCoin Network to observe the full content of transactions. Complete data is only shared between secure enclaves that safely delete the information that could otherwise be used to statistically associate payment senders to payment recipients.

### Trusted Enclave and CSS

## Prerequisites

### IAS Account 
(see [Getting Intel Attestation Service Credentials](#credentials))

### SGX-Enabled Machine
(see [SGX Provisioning](#provisioning))

### Downloading and Installing the Correct SGX Driver

## <a name="credentials"></a>Getting Intel Attestation Service Credentials 
The Intel Attestation Service (IAS) credentials allow node operators to register with Intel as Licensed Enclave operators. This credential links the operator identity with the node’s attestation evidence provided to other nodes.

**Step 1:** Apply for and obtain an Intel License Agreement to run MobileCoin. Please see: [Partner Intel License Agreement](https://docs.google.com/document/d/1ATv98iLMDlghbC0q8GmbpL6iSlcquy4sVTWAg4nxS6U/edit#heading=h.gxj509579nem). 

**Note:**  Approval can take up to two weeks.

**Step 2:** Once your request is approved and the license issued (confirmed via email), you can create your account at the Intel Trusted Portal using the email associated with your Partner Intel License Agreement. 

**Step 3:** Once you log in at the Trusted Portal, this landing page displays. Click on the **Intel SGX Attestation Service** link to create an EPID subscription. 
































##  <a name="provisioning"></a>SGX Provisioning
### Provisioning with Azure
Azure offers SGX machines through their Confidential Computing platform. Intel is in the process of adding SGX-capable machines in multiple regions and availability zones. 
 Note:  You will need to request SGX quota (per core) in specific regions. It can take a few days for quota requests to be fulfilled.

Specifications for provisioning with Azure:

Specification
Preferred 
Notes
Machine Type
DC4s, DC8s
The number refers to the number of cores available. DC2s are also available, but are too underpowered for MobileCoin workloads.
Persistent Storage
Premium SSDs
These are not available in all regions or with all core types. Standard SSD is also acceptable, but may introduce iops bottlenecks.
Regions
West EU, 
UK South
MobileCoin consensus is slightly latency sensitive, so provisioning in regions somewhat geo-located with existing consensus peers is beneficial to the whole network.



