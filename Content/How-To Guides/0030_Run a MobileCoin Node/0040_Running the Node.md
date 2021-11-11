---
title: Running the Node
description: 
---
## What You Need to Know

### Consensus Network Concepts

Does anything need to go here?

### Network Ports

The following ports need to be exposed. The actual ports assigned may vary according to your needs, but will need to be communicated for e.g. node and client connection. Presented here are the defaults.

?> You can establish your ingress mapping in whatever manner is convenient to your infrastructure. We present an example of the default MobileCoin ingress configuration.

| Purpose | Service Port | Ingress Mapping | Dependencies |
| ------- | ------------ | --------------- | ------------ |
| Client transaction submission | 3223 | 443 | Clients must be aware of which port to submit TxPropose messages to. |
| Peer-to-peer consensus | 8443 | 443 | Other nodes in the network who wish to peer with your node must be aware of the consensus port. |
| Admin (logging) | 8000 | None | Admin port to obtain information and statistics about the currently running node. |
| Admin (metrics) | 9090 | 9090 | Prometheus metrics are provided on the 9090 port. |

### Description of the MobileCoin Ledger

In order for any payments network to function, it must be able to maintain a history of transactions. MobileCoin Ledger describes how the MobileCoin Network stores payment records in a public ledger. The ledger is implemented as a blockchain, in which each block contains transactions that include transaction outputs (txos) that might be spent in the future by their owners. Each transaction also includes a proof that all value spent in the transaction has never been spent before. The underlying design is based on the privacy-preserving CryptoNote ledger protocol, which obscures the identity of all txo owners using one-time recipient addresses. The link between sender and recipient is protected through the use of input rings that guard the actually-spent txo in a large set of possibly-spent txos.

The monetary value of each txo is encrypted using the method of Ring Confidential Transactions (RingCT). RingCT is implemented using bulletproofs for improved performance. Only the receiver of the transaction can reveal the encrypted monetary value and spend the new txos that are written to the ledger. The recipient's cryptographic control over spending ensures that all transactions in MobileCoin are irreversible, similar to cash transactions in the real world.

Each txo in the input ring of a transaction is annotated with a Merkle proof of inclusion in the MobileCoin Ledger blockchain. This allows new transactions to be validated with fewer blockchain read operations, improving efficiency and reducing information leaked to data-access side channels.

Additionally, MobileCoin Ledger dramatically improves on the baseline privacy offered by CryptoNote and RingCT by requiring that the input rings for every transaction are deleted before the new payment is added to the public ledger. Digital signatures are added to the ledger in place of the full transaction records to provide a basis for auditing.

### Ledger Distribution and S3 Setup

Does anything need to go here?

## Running the Node In a Container (Preferred Method)

### Requirements

-   **TLS Certificates for your Node**

-   **Node Signer Keys** (See [Signing Consensus Messages](/how-to-guides/run-a-mobilecoin-node/configuring-your-consensus-validator-node))

-   **Network Configuration File/Consensus config file** (network.toml - See [Configuring your Node to Connect to Trusted and Untrusted Peers](/how-to-guides/run-a-mobilecoin-node/configuring-your-consensus-validator-node))

-   **Ledger Storage Location**

-   **S3 Storage Location**

### Entrypoint and Container Processes

Familiarize yourself with the [entrypoint for the consensus docker container](https://github.com/mobilecoinofficial/internal/blob/master/testnet-deploy/ops/entrypoints/consensus_validator.sh).

?> It contains multiple processes working together to provide the full Consensus Validator functionality.

These processes are the following:

| Process | Function |
| ------- | -------- |
| aesm_service | Provides EPID provisioning for the enclave. |
| filebeat | Provides logging. |
| mc-ledger-migration | Performs the ledger migration, if necessary, then stops. |
| ledger-distribution | Distributes the ledger to S3 archive. |
| mc-admin-http-gateway | Provides the admin panel to the Consensus Service. |
| consensus-service | Runs the MobileCoin Consensus Service. |

### Environment Variables: How to Configure Your Node

The following environment variables should be provided:

| Variable | Value | Function |
| -------- | ----- | -------- |
| LOCAL_NODE_ID | address:port | Local node name for logging. Should ideally match the value of peer-responder-id as provided to consensus-service. |
| NODE_LEDGER_DIR | Path to ledger | The location of the ledger. We suggest that this should be persistently stored outside the container and mounted in. On startup, if the contents of NODE_LEDGER_DIR are empty, then the origin block from within the container should be copied into the NODE_LEDGER_DIR. |
| CONSENSUS_ADMIN_URI | URI | The URI for the admin http service which provides a management panel to the consensus service. The gateway is started in the container and is set to listen via this environment variable. Used by mc-admin-http-gateway. |
| AWS_ACCESS_KEY_ID | AWS Credential | Should have write access to your S3 ledger archive bucket. Used by ledger-distribution. |
| AWS_SECRET_ACCESS_KEY  | AWS Credential | Should have write access to your S3 ledger archive bucket.<br />Used by ledger-distribution. |
| AWS_PATH | S3 URL | The S3 location of your S3 ledger archive bucket. Used by ledger-distribution. |
| SGX_MODE | HW or SW | Indicates whether to run with SGX in hardware or simulation mode. (Should be HW). Used by consensus-service. |
| IAS_MODE | PROD or DEV | Indicates whether to hit the Intel Attestation Service's prod or dev endpoint. (Should be PROD). Used by consensus-service. |
| RUST_LOG | info, debug, trace | Sets the log level. Can also be tuned per specific rust crate. See [env_logger](https://docs.rs/env_logger/0.7.1/env_logger/). Used by ledger-distribution and consensus-service. |
| RUST_BACKTRACE | full, 1, 0 | Indicates how much detail to provide in the backtrace in the event of a panic. Used by ledger-distribution and consensus-service. |

### Run It!

**The actual docker run command that pulls it all together**

The entrypoint passes the arguments from the `docker` run command to the consensus service.

See [Configuring Your Node](/how-to-guides/run-a-mobilecoin-node/configuring-your-consensus-validator-node), below, for more details on how to set up the values for the parameters unique to your Consensus Validator Node.

The arguments to the consensus service are the following:

| Argument | Value | Function |
| -------- | ----- | -------- |
| peer-responder-id | address:port | The "Responder ID" which is used in attestation with peers and as an identifier for your node in broadcast messages.<br />This is also the value that peers will add to their quorum sets when they trust you. |
| client-responder-id | address:port | The "Responder ID" which is used in attestation with clients. |
| peer-listen-uri | URI, for example:<br />mcp://0.0.0.0:8443/?tls-chain=/certs/your-tls.crt&tls-key=/certs/your-tls.key | The URI on which your node is listening for consensus message traffic from peers.<br />See Accepting [Connections from Trusted and Untrusted Peers](/how-to-guides/run-a-mobilecoin-node/configuring-your-consensus-validator-node). |
| network | Path to network.toml | Defines the networking and trust configuration for your peer. See [Configuring your Node to Connect with your Trusted Peers](/how-to-guides/run-a-mobilecoin-node/configuring-your-consensus-validator-node), below. |
| ias-api-key | IAS credential | Your API key for attesting with the Intel Attestation Service. See [Getting Intel Attestation Service Credentials](/how-to-guides/run-a-mobilecoin-node/intel-attestation-credentials). |
| ias-spid | IAS credential | Your Service Provider ID for attesting with the Intel Attestation Service. See [Getting Intel Attestation Service Credentials](/how-to-guides/run-a-mobilecoin-node/intel-attestation-credentials). |
| msg-signer-key | Base64-encoded key | Used to sign every consensus message emitted by this node. See [Signing Consensus Messages](/how-to-guides/run-a-mobilecoin-node/configuring-your-consensus-validator-node). |
| network | Path to network.toml | Defines the networking and trust configuration for your peer. See [Configuring your Node to Connect with your Trusted Peers](/how-to-guides/run-a-mobilecoin-node/configuring-your-consensus-validator-node). |
| ledger-path | Path to local ledger | The purpose of consensus is to agree on the validity of transactions and append to this ledger. The ledger should be persistent in case consensus must be restarted. |
| sealed-block-signing-key | Path to local sealed backup location. | The block signing key signs each block to provide an audit trail of which enclaves participated in consensus for each block. The signing key is created in the enclave, and never leaves the enclave. The seal key is tied to the CPU, so the signing key is written as backup to the sealed-block-signing-key path, and can be read in the case that consensus restarts on the same CPU. If consensus is rescheduled on a different CPU, a new signing key will be created, and the signatures will indicate that a new enclave is participating in consensus. |
| origin-block-path | Path to local origin block, if ledger-path is an empty directory. | If the node is starting up without an existing ledger, you can provide the origin-block-path, which will copy the origin-block contents to the ledger-path on startup. |
| admin-listen-uri | URI, for example:<br />insecure-mca://127.0.0.1:9091/ | The admin http gateway sends gRPC requests to the admin-listen-uri for the consensus service, which is listening as specified. The scheme is either mca or insecure-mca to indicate whether TLS is used. Since the admin gateway runs inside the container, insecure-mca is sufficient. |

## Using Your Own Binaries Built from Source, Without Docker

### Things You Need

-   TLS certificates for your node

-   Node Signer Keys

-   Network Configuration file/Consensus config file (network.toml)

-   Ledger Storage Location

-   S3 storage location

### The Various Binaries that You Need to Run

-   Aesm service

-   Ledger distribution

-   Consensus-service

-   Admin/Metrics gateway

-   (Probably add a diagram here)

### Run It!

-   Command line for aesm service

-   Command line for ledger distribution

-   Command line for consensus-service

-   Command line for admin/metrics gateway

## Verifying Transactions

[Use information below in "Verifying Transactions are Running Correctly."]

## What Happens Next

### Upgrading the Enclave

Any introduction of new code into the consensus network first requires the operators to reach consensus among themselves about the code they will be running, as well as the time and manner in which the new code will be introduced. This is a privacy measure the consensus enclave enforces; it rejects connections from any presumptive peer that lacks an identical MRENCLAVE measurement. 

!> If this requirement were relaxed, that is, if the verification were incorrectly configured to only check the MRSIGNER values, such as where the three elements checked included MRSIGNER, product ID, and enclave security version, then key holders could be forced to sign malicious enclaves, which could intentionally leak all inbound user traffic. This malicious enclave could be attached to the consensus network by any existing member.

To introduce new enclave code, or also known as a consensus enclave upgrade, all operators should coordinate a "flag day."

?> It is possible to introduce new code that does not affect the enclave. If the new code does not impact the enclave, a "flag day" is not required.

The MobileCoin Foundation provides the MobileCoin Operators Group, which consensus node operators should utilize in order to schedule these "flag day" upgrades in response to enclaves issued by the MobileCoin Foundation Technical Committee and signed by the MobileCoin Foundation Key Management Group.

To upgrade a consensus node:

1.  The Node Operators Group (NOG), who exist under the MobileCoin Foundation, should agree on the new enclave to be deployed, as well as any upgrade-specific procedures and tests to be performed (which should be specified by the NOG in their charter). In addition, the upgrades should first be run on the testnet. 

    ?> An event coordinator is recommended to help with coordinating all of the upgrade functions.

2.  Once a maintenance window/video conference has been scheduled for upgrading the selected nodes, inbound client connection handlers (reverse proxy, load balancer, ingress controller, etc.) need to be configured to reject new connections.

    1.  Confirm no new transactions are being received/processed by the network.

    2.  Confirm that all nodes are at the same block height.

3.  Stop all nodes.

    1.  Snapshot all nodes' local databases. The files to be backed up and Copied on Write (that is, copying local files to a backup whenever they are written to), include:

        1.  Ledger-db (lmdb)

        2.  Ledger-distribution's statefile 

    2.  Apply any forward-DB migration scripts.

4.  Restart all nodes with the new enclave.

    1.  Confirm that all nodes are attesting to each other.

    2.  Perform transaction tests (e.g. self-payment, cross-payment, fog, non-fog).

    3.  Configure inbound client connection handlers to accept connections.

5.  Maintain the call/maintenance window for a predefined "live" period to ensure stability.

#### Unacceptable Degradation of Service

In the event of an unacceptable degradation of service, which cannot be practically mitigated outside of the enclave, the enclave upgrade should be rolled back. The exact rollback procedure may vary depending on the scope of the upgrade and its procedures.

To rollback an enclave upgrade:

1.  If outside the maintenance window, declare emergency and select an incident commander, and convene the Operators Group.

    1.  Agree that a rollback is necessary.

2.  Configure all inbound client handlers to reject new connections.

    1.  Confirm no new transactions are being received/processed by the network.

    2.  Confirm that all nodes are at the same block height.

3.  Stop all nodes.

    1.  Snapshot all node's local databases.

    2.  Apply any reverse-DB migration scripts.

4.  Restart all nodes with the old enclave.

    1.  Confirm that all nodes are attesting to each other.

    2.  Perform transaction tests.

    3.  Configure inbound client handlers to accept new connections.

5.  Maintain the call/maintenance window for a predefined "live" period to ensure stability.

#### Uneven Node Block Heights

In the event that all nodes are not at the same block height during a rollback, and cannot "settle" to the same level, the call participants must decide whether to roll back the ledger to a predetermined point (e.g. the lowest common block) or allow nodes to "catch up" to the highest available block, or some point in between. If a ledger rollback is decided, all operators must be able to delete some of their S3-published ledger material [TODO: discuss with Joe]. This must be accompanied by public announcement of the invalidity of certain blocks.

### Addressing Group Out-of-Date Attestation Status 

\[Should this be only in error messages, or should we describe this here and link to the error message, which is Group Out-of-Date error message?\]

### Monitoring (Joe, Sara, Ricky)

The consensus server exports metrics using [Prometheus](https://prometheus.io). The Prometheus metrics are exposed via the admin service, which is available on port TCP-8000 (as configured in the consensus server container) at the /metrics URL endpoint.  These metrics can be scraped by a prometheus server, and graphed using [Grafana](https://grafana.com).

Talk about interesting metrics here?

-   Pending values

-   Transaction rate

-   Transaction completion time

-   Consensus service errors

-   Blocks written rate

-   TX cache size

-   Etc...