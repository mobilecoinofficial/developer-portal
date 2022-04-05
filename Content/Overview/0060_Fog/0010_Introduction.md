---
title: How does Fog work?
use_file: "mobilecoinfoundation/mobilecoin/fog/README.md"
---
MobileCoin Fog is a privacy-preserving service designed to support use of the MobileCoin Payments Network on mobile 
devices, which can use Fog to check their balance and send payments, without syncing the ledger.

Fog is designed so that the MobileCoin and Fog service operators have no nontrivial insight into your payment. Please
see the [threat model](https://github.com/mobilecoinfoundation/mobilecoin/blob/master/fog-threat-model-2.1.0.md) 
for a comprehensive explanation.

### Durable Oblivious Messaging Service

Fog provides scalable access to TXOs in the MobileCoin ledger, without clients needing to scan every TXO on their 
local device, while also not handing the users' private keys to any third party to scan on their behalf, which has
major security and privacy implications. 

Fog works by post-processing the MobileCoin ledger in Oblivious, Encrypted RAM, tagging TXOs with ratcheting random 
numbers that the client can uniquely generate to retrieve the TXOs, and serving those TXOs from Oblivious RAM (ORAM).

For MobileCoin payments to be practical, we cannot require the mobile device to sync the ledger or download the entire
blockchain. However, a so-called "thin wallet" won't work either, because the types of queries made by a thin wallet
generally reveal to the server the user's balance, when they got paid, etc. In typical thin wallet designs, the server
is trusted by the user.

MobileCoin has been engineered to eliminate this sort of trust in the service is "oblivious" to the nature of the user
requests, and the service operator is unable to harvest the users' data in exchange for running the service.

Because of this, off-the-shelf solutions to wallet services simply don't work. In many cases, if we naively make a
database query to handle a query that a wallet would make if it had access to the ledger, it reveals significant
information about e.g. whether Bob was paid or not in the last block, which payments Bob received, whether Alice paid
Bob, etc., any of which would not meet our privacy goals.

Instead, Fog makes heavy use of SGX enclaves and Oblivious RAM data structures to serve such queries privately and
without compromising scalability. Although such use of SGX may create potential operational challenges, the MobileCoin
system has been carefully designed to navigate these challenges.

#### Ingest

The "fog-ingest" service consumes and post-processes the blockchain, writing records to the recovery database. The service attempts to decrypt the e_fog_hint field of each TxOut, and then tags that TxOut with a random number from a random number generator specific to the fog user who received the TxOut (if any). This is an SGX service, and these computations are performed obliviously. The service additionally publishes a public key to the "fog-report" service, for the users to use to create fog hints.

#### View

The "fog-view" service provides an API for fog users to access this database, whose primary purpose is to deliver to the fog users their TxOut's. Some of the queries that the user needs to make to the database are sensitive. To protect them, this is an SGX service and some of the queries are resolved obliviously.

#### Ledger

The "fog-ledger" service provides several APIs for fog users to make queries against the MobileCoin ledger. Some of the queries that the user needs to make are sensitive; SGX also provides this service, and some of the queries are resolved obliviously.

#### Report

The "fog-report" service. The fog report service publishes a signed fog public key which comes from the fog-ingest SGX enclave. This public key must be available to anyone who wants to send MobileCoin to a fog user, so the fog report service is expected to be publicly accessible and require no authentication (the way the others probably would). In addition to the Intel report, there is an X509 certificate chain signing the report as well. The "fog-report" service is not an SGX service.

### Show Me the Code

* [MobileCoin Fog](https://github.com/mobilecoinfoundation/mobilecoin/tree/master/fog): Implementation of Fog
