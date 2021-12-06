# Common Tasks

This runbook is specifically for operators who will be setting up and operating Fog Ingest nodes on the MobileCoin network. Part of the Fog Service, the Fog Ingest Server processes each transaction in the ledger according to registered users.

| Relevant Information | Solution |
| ----------- | -----------|
| How to report bugs | |
| POCs | |
| Links to Documentation | |
| Assumptions: | |

## Understanding the Fog Ingest Server

The Fog Ingest Server processes each transaction in the public ledger according to registered users. By performing a key exchange within the enclave, the Ingest Server registered a Public Address with the Fog service. This adds a row to the seeds table. This currently happens automatically when a user's public address is decrypted by the Fog Service, but after [Fog-19](https://mobilecoin.atlassian.net/browse/FOG-19), the Ingest operators can register Public Addresses for their users.

### Key Rotation

A separate service (Report Service) provides key rotation of the Ingest's public key. The public key is required in order to create a transaction for a Fog recipient. This works by encrypting the recipient's Public Address with the Ingest Service's Public Key, and including this in the hint field.

### Write

When a transaction is written to the public ledger, the Ingest Server processes the transaction, producing a key and value.

#### Transaction Output in Public Ledger

| Stealth Address | Transaction Details | Fog Hint |
| ----------- | ----------- | ----------- |
| Cryptonote one-time recipient subaddress | Input Ring, Merkle Proof, etc. | Recipient's Public View Key and (encrypted with the ingest server's public key). |

#### Row in Fog Index Data Structure

| Key |Value |
| ----------- | ----------- |
| Search Key Token (Indistinguishable from Random) | Full Transaction Output Details, encrypted with user's subaddress public view key. |

A new write is (obliviously) appended as a row to the Fog Index data structure.

### Read

In order to perform a read, the Client must reconstruct the tokens for which it will query the Fog Ingest.

The Client maintains the following information:

| Persistent Storage | Cached on Phone | Queryable |
| ----------- | ----------- | -----------  |
| 
-   Root Entropy (From which to derive Cryptonote KeyPairs)

-   Fog Server PubKey/FQDN (or implicit, e.g. this app always uses this fog server)
 | 
-   Seeds (Reconstructable from Public Keys via Fog's mapping of public keys to seeds)

-   PRNGs cursor (Note: can be reconstructed from seed, and sending queries to View Node.)

-   Previous Transactions

-   Seeds Cursor (number of seeds received so far)
 | 
-   Seeds (from View)

-   TxOuts (from View)

-   Ingest PubKey (from Report service)
 |

# Running the Node

## Network Ports

The following ports need to be exposed. The actual ports assigned may vary according to your needs, but will need to be communicated for e.g. node and client connection. Presented here are the defaults.

?> You can establish your ingress mapping in whatever manner is convenient to your infrastructure. We present an example of the default MobileCoin ingress configuration.

| Purpose |  Service Port  | Ingress Mapping | Dependencies |
| ----------- | ----------- | ----------- | ----------- |
| Client report query | 3222 | 443 | Clients obtain the ingest's attestation report via the report server port. Not attested. |
| Ingest Peer Backup | 8090 | 443 | Ingest nodes create attested peer connections to share the ingest private key for backup. |
| Ingest Admin Service | 3226 | None | Operators can communicate with the ingest for admin operations such as "Add User" to register users. |
| Admin (metrics) | 9090 | 9090 |
Prometheus metrics are provided on the 9090 port. |

## Running the Node In a Container (Preferred Method)

The preferred method for running the node is in a container because MobileCoin engineers have assimilated and compiled the optimum running environment to improve setup time and efficiencies in operation of the node.

### Requirements

The following requirements for running the node in a container include:

-   TLS Certificates for your Node

-   S3 Storage Location

### Entrypoint and Container Processes

Familiarize yourself with the [entrypoint for the fog-ingest docker container](https://github.com/mobilecoinofficial/internal/blob/master/ops/entrypoints/fogingest.sh).

?> The ingest container contains multiple processes working together to provide the full fog-ingest functionality.

These multiple processes include the following:

| Process | Function  |
| ----------- | ----------- |
| mobilecoind | Syncs the ledger from archive blocks on S3. |
| fog_lmdb_recovery_distribution_upload | Uploads the recovery database (db) to S3. |
| report_server | Runs the MobileCoin Report service. |
| fog_ingest_server | Runs the MobileCoin Fog Ingest service. |

#### Environment Variables: How to Configure Your Node

The following environment variables should be provided when configuring your node:

| Variable | Service(s) | Value | Function |
| ----------- | ----------- | ----------- | ----------- |
|  ACCT_INGEST_CLIENT_LISTEN_URI | fog_ingest_server | URI, e.g. insecure-fog://0.0.0.0:3226/ | URI on which to listen for requests. Note: Ingest is not exposed to Fog users, so this is in fact an admin port. There is an admin client which registers users with the ingest service, and it talks to this port.  |
| INGEST_ADMIN_URI | fog_ingest_server | URI, e.g. insecure-mca://127.0.0.1:8003/ | URI on which the in-container admin service connects to gather info from the running ingest service. |
| PEER_LISTEN_URI | fog_ingest_server | URI, e.g. insecure-fog-ingest://0.0.0.0:8090/ | URI on which to listen for ingest backup peers. When a node is configured with the --primary-node-uri parameter, it acts as a backup and forms an attested connection with the primary ingest node. |
| INGEST_PEER_RESPONDER_ID | fog_ingest_server | Address and port, such as fog-ingest.alpha.mobilecoin.com:443 | "Responder ID" used in attestation. |
| ACCOUNT_DB_DIR | fog_ingest_server, fog_lmdb_recovery_distribution_upload | Path to account db | The account db stores users' txos. |
| REPORT_DB_DIR | fog_ingest_server, report_server | Path to report db | The report db provides attestation reports for the ingest service. These reports are verified by clients before sending a transaction. |
| NODE_LEDGER_DIR | fog_ingest_server, mobilecoind | Path to ledger | The location of the ledger. We suggest that this should be persistently stored outside the container and mounted in. mobilecoind syncs the ledger starting from the last local block. |
| WATCHER_DB_DIR | fog_ingest_server, mobilecoind | Path to watcher db | The watcher db provides block timestamps. mobilecoind syncs the block timestamps along with the ledger. |
| IAS_SPID | fog_ingest_server | IAS credential | Required for ingest backup peer attestation. |
| IAS_API_KEY | fog_ingest_server | IAS credential | Required for ingest backup peer attestation. |
| REPORT_SERVER_CLIENT_LISTEN_URI | report_server | URI, e.g. insecure-fog://0.0.0.0:3222/ | URI on which to listen for client requests to the report server. The report server provides attestation reports for the ingest enclave, which contain the ingest's public key. Users encrypt the hint field with this public key. |
| AWS_ACCESS_KEY_ID | fog_lmdb_recovery_distribution_upload | AWS Credential | Should have write access to your S3 recovery db bucket. |
| AWS_SECRET_ACCESS_KEY | fog_lmdb_recovery_distribution_upload | AWS Credential | Should have write access to your S3 recovery db bucket.  |
| AWS_PATH | fog_lmdb_recovery_distribution_upload | S3 URL, e.g. s3://mobilecoin.fog/fog-ingest.alpha.mobilecoin.com?region=us-east-2 | The S3 location of your S3 recovery db bucket. |
| PEERS | mobilecoind | Comma-separated consensus peers, e.g. mc://node1.alpha.mobilecoin.com/,mc://node2.alpha.mobilecoin.com | Consensus "peers" from which to query the block height, in order to sync the ledger to the appropriate height.  |
| TX_SRC_URLS | mobilecoind | Comma-separated S3 URLS, e.g. https://s3-us-west-1.amazonaws.com/mobilecoin.chain/node1.alpha.mobilecoin.com/,https://s3-us-west-1.amazonaws.com/mobilecoin.chain/node2.alpha.mobilecoin.com/ | S3 source URLs from which to sync archive blocks. |

#### Run It!

The following code is the actual docker run command that pulls it all together.

```
docker run \\
--detach=true \\
--init \\
--name fogingest

--network=containers

--publish 3222:3222

--publish 3226:3226

--publish 8090:8090

--publish 9090:9090

--cap-add=SYS_PTRACE

--env AWS_PATH=s3://mobilecoin.fog/fog-ingest.alpha.mobilecoin.com?region=us-east-2

--env AWS_SECRET_ACCESS_KEY=$AWS_SECRET_ACCESS_KEY

--env AWS_ACCESS_KEY_ID=$AWS_ACCESS_KEY_ID

--env IAS_SPID=$IAS_SPID

--env IAS_API_KEY=$IAS_API_KEY

--env INGEST_PEER_RESPONDER_ID=ingest.alpha.mobilecoin.com:443

--env PEER_LISTEN_URI=insecure-igp://0.0.0.0:8090/

--env PEERS=mc://node1.alpha.mobilecoin.com/,mc://node2.alpha.mobilecoin.com

--env TX_SOURCE_URLS=https://s3-us-west-1.amazonaws.com/mobilecoin.chain/node1.alpha.mobilecoin.com/,https://s3-us-west-1.amazonaws.com/mobilecoin.chain/node2.alpha.mobilecoin.com/

--env NODE_LEDGER_DIR=/data/ledger

--env WATCHER_DB_DIR=/data/watcher

--env REPORT_DB_DIR=/data/report

--env REPORT_SERVER_ADMIN_URI=insecure-mca://127.0.0.1:8007/

--env INGEST_ADMIN_URI=insecure-mca://127.0.0.1:8003/

--device /dev/isgx

-it mobilecoin/fogingest:<image_tag>
```

The entrypoint does not take any arguments.

## Using Your Own Binaries Built from Source, Without Docker

### Things You Need

-   TLS certificates for your node

-   Ledger Storage Location

-   S3 storage location

### The Various Binaries that You Need to Run

-   fog_ingest_server

-   fog_lmdb_recovery_distribution_upload

-   mobilecoind

-   report_server

### Run It!

-   Command line for fogingest service

-   Command line for mobilecoind

-   Command line for report server

-   Command line for fog_lmdb_recovery_distribution_upload

### Running the Fog Ingest Services

The arguments to the Fog Ingest server include the following:

| Argument | Value | Function |
| ----------- | ----------- | ----------- |
| recovery-db | Path to recovery-db /account | Recovery-db location |
| ledger-db | Path to ledger db | Ledger-db location |
| watcher-db | Path to watcher db | Watcher-db location |
| client-listen-uri | URI, for example:<br />insecure-fog-ingest://0.0.0.0:3226/ | URI on which to listen for requests. Note: Ingest is not exposed to Fog users, so this is in fact an admin port. There is an admin client which registers users with the ingest service, and it talks to this port.  |
| peer-listen-uri | URI, for example:<br />insecure-igp://0.0.0.0:8090/ | URI on which to listen for ingest backup peers. When a node is configured with the --primary-node-uri parameter, it acts as a backup and forms an attested connection with the primary ingest node. |
| ias-api-key | IAS credential | Your API key for attesting with the Intel Attestation Service. See [Getting Intel Attestation Service Credentials](https://docs.google.com/document/d/1iSHIi18Y7UTqzi0V4zy3NbP49ObxeHExsdmqoGHTRa0/edit#heading=h.6udbg7jtjqpq). |
| ias-spid | IAS credential | Your Service Provider ID for attesting with the Intel Attestation Service. See [Getting Intel Attestation Service Credentials](https://docs.google.com/document/d/1iSHIi18Y7UTqzi0V4zy3NbP49ObxeHExsdmqoGHTRa0/edit#heading=h.6udbg7jtjqpq). |
| local-node-id | address:port | The "Responder ID" used in ingest peer attestation for private key backup. |
| sealed-key | Path to local sealed key | Local location to store the sealed enclave key. If the ingest server restarts on the same machine, it can restore its private key from the sealed-key. |
| admin-listen-uri | URI, for example:<br />insecure-mca://127.0.0.1:9091/ | URI on which the in-container admin service connects to gather info from the running ingest service. |

The arguments to the `mobilecoind` include the following:

| Argument | Value | Function |
| ----------- | ----------- | ----------- |
| peer | Consensus node peer URI. Can provide multiple.<br />Example:<br />mc://node1.demo.mobilecoin.com/ | Consensus "peers" from which to query the block height, in order to sync the ledger to the appropriate height. |
| tx-src-url | S3 Path to ledger archive blocks. Can provide multiple.<br />Example:<br />https://s3-us-west-1.amazonaws.com/mobilecoin.chain/node1.alpha.mobilecoin.com/ | S3 source URLs from which to sync archive blocks. |
| ledger-db | Path to ledger db | Ledger-db location |
| watcher-db | Path to watcher db | Watcher-db location |
| poll-interval | 2 | \ |

The arguments to the `fog_lmdb_recovery_distribution_upload` include the following:

| Argument | Value | Function |
| ----------- | ----------- | ----------- |
| recovery-db | Path to recovery db | \ |
| dest | AWS url  |\ |
| state-file | Path to state file | \ |

# Playbook: Common Errors & Alerts

## Common Errors

| Common Error | Error Message | Solution |
| ----------- | ----------- | ----------- |
|\ |\ |\ |
|\ |\ |\ |
|\ |\ |\ |
|\ |\ |\ |
|\ |\ |\ |
|\ |\ |\ |
|\ |\ |\ |
|\ |\ |\ |
|\ |\ |\ |
|\ |\ |\ |

## Alerts

| Common Alert | Alert Message | Solution |
| ----------- | ----------- | ----------- |
|\ |\ |\ |
|\ |\ |\ |