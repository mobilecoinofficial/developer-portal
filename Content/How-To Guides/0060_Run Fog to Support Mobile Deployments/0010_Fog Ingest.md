# About this Manual

MobileCoin Fog is a scalable service infrastructure that enables a smartphone to manage a privacy-preserving cryptocurrency with locally-stored cryptographic keys. Unique from the public ledger, the Fog Service returns all or some portion of user transactions obliviously to enable operators to look up transactions. The MobileCoin Fog Service is composed of three services and a ledger sync process:

-   Ingest: Processes each transaction in the public ledger according to registered users.

-   Report: Serves the Ingest's pubkey and SGX report to construct transactions (Txos).

-   View: Obliviously provides Txos belonging to registered users.

-   Ledger: Serves the contents of the public ledger obliviously.

![](https://lh3.googleusercontent.com/AaHG9Iez8m4ODL5ZK6DE3zo5AH8cP3PIqpYFauqsnVGev1oiMi1IsQ1bRbc7YdjfHmwbxhmEgkUaXg8FCJsGOrDZ53F1RDRPU4o25P_GACHvs3ilOMRD--Igmx5kx1INYwKt8lNL)
Figure 1: The Fog System Overview.

This manual contains a comprehensive overview of one part of the MobileCoin Fog Server, the Fog Ingest Service, and instructions on how to set up and manage a remote Fog Ingest node on the MobileCoin Network.

For more information about the MobileCoin Network, check out the [MobileCoin Ecosystem](https://docs.google.com/document/d/1vbnqfwW6sED7t61YKDIAItK4eWM3lSIDXoT18Nl9KfY/edit?usp=sharing) - a brief document about the MobileCoin open-source software network.

## Who Should Read this Manual

This manual should be read and referenced by operators, who have been entrusted with setting up and managing the MobileCoin Fog services in the MobileCoin network. 

## How to Use this Manual

This manual provides operators with quick help for setting up and operating the MobileCoin Fog Ingest Server. This document is separated into three sections:

1.  Overview of the Fog Ingest Server

2.  Step-by-step instructions for common tasks

3.  Lists of errors and alerts the Fog Ingest Server may generate with solutions.

Special symbols are used throughout the manual to make you aware of important information and valuable tips.

## Special Symbols Used in this Manual

The following symbols represent important information and valuable tips associated with the MobileCoin Fog Ingest Server and are displayed throughout the manual. Every noted symbol has significant meaning that you should heed:

![](https://lh5.googleusercontent.com/B88UcyxpWYktoK4neFxCbGHrAW3yg9ciF9dd-IsHL2a79iIADuTd96ikGe8DcTQ9bN87KIwi1xb8HSTjlKmyc2KhU7fEfB_uSdw0WDJmgmrN4kydIXAc8mEM2QV6_ikxTeNPctSM)  Note: Important information concerning operating the Fog Ingest Server.

![](https://lh5.googleusercontent.com/jnmJKdfktFYNYmiMMSyT9xByfEgefebqp_x4qJTxKh4KscODPU_vs_WbN7F2L21M3wMQbvPOg1IrzXv-IRqmC102YpgdPE_6w8alUEdEps00kPoGT5TtuClMEJAYBDivyaLy67By) Warning: Failure to follow directions may result in preventing your Fog Ingest Server from operating successfully in the MobileCoin Network.

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

![](https://lh4.googleusercontent.com/cyESseWMzaYwMEHzPCEp2G-8ul_q5wwV6u38wvxpfsCN9wPPwr60XEn2Kq36CvWogjjDhyuHGY0lY7c05Ong7KfW9J4VkH7neYFa3F9ULyfrwlJ7ooyh9JHi7NHcqlENlpEYIrUb)  Note:  You can establish your ingress mapping in whatever manner is convenient to your infrastructure. We present an example of the default MobileCoin ingress configuration.

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

![](https://lh5.googleusercontent.com/Zy1AuDGV5JC333f3mNSsrFNzJQGOeNP39Qdu9OsETTT9wabkgKOy3W7M2Po22NSeO9be2yJkuoOp9F0WxM9eAkjknQE_jyQj6nEaA7zfg4tdJ67FtRpw8XyhRq753B91PowdqxgG)  Note:  The ingest container contains multiple processes working together to provide the full fog-ingest functionality.

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
|  
ACCT_INGEST_CLIENT_LISTEN_URI | fog_ingest_server
 |
URI, e.g. insecure-fog://0.0.0.0:3226/\
 |
URI on which to listen for requests. Note: Ingest is not exposed to Fog users, so this is in fact an admin port. There is an admin client which registers users with the ingest service, and it talks to this port. 
 |
| 
INGEST_ADMIN_URI
 |
fog_ingest_server
 |
URI, e.g. insecure-mca://127.0.0.1:8003/
 |
URI on which the in-container admin service connects to gather info from the running ingest service.
 |
| 
PEER_LISTEN_URI
 |
fog_ingest_server
 |
URI, e.g. insecure-fog-ingest://0.0.0.0:8090/
 |
URI on which to listen for ingest backup peers. When a node is configured with the --primary-node-uri parameter, it acts as a backup and forms an attested connection with the primary ingest node.
 |
| 
INGEST_PEER_RESPONDER_ID
 |
fog_ingest_server
 |
Address and port, such as fog-ingest.alpha.mobilecoin.com:443
 |
"Responder ID" used in attestation.
 |
| 
ACCOUNT_DB_DIR
 |
fog_ingest_server, fog_lmdb_recovery_distribution_upload
 |
Path to account db
 |
The account db stores users' txos.
 |
| 
REPORT_DB_DIR
 |
fog_ingest_server, report_server
 |
Path to report db
 |
The report db provides attestation reports for the ingest service. These reports are verified by clients before sending a transaction.
 |
| 
NODE_LEDGER_DIR
 |
fog_ingest_server, mobilecoind
 |
Path to ledger
 |
The location of the ledger. We suggest that this should be persistently stored outside the container and mounted in. mobilecoind syncs the ledger starting from the last local block.
 |
| 
WATCHER_DB_DIR
 |
fog_ingest_server, mobilecoind
 |
Path to watcher db
 |
The watcher db provides block timestamps. mobilecoind syncs the block timestamps along with the ledger.
 |
| 
IAS_SPID
 |
fog_ingest_server
 |
IAS credential
 |
Required for ingest backup peer attestation.
 |
| 
IAS_API_KEY
 |
fog_ingest_server
 |
IAS credential
 |
Required for ingest backup peer attestation.
 |
| 
REPORT_SERVER_CLIENT_LISTEN_URI
 |
report_server
 |
URI, e.g. insecure-fog://0.0.0.0:3222/\
 |
URI on which to listen for client requests to the report server. The report server provides attestation reports for the ingest enclave, which contain the ingest's public key. Users encrypt the hint field with this public key.
 |
| 
AWS_ACCESS_KEY_ID
 |
fog_lmdb_recovery_distribution_upload
 |
AWS Credential
 |
Should have write access to your S3 recovery db bucket. 
 |
| 
AWS_SECRET_ACCESS_KEY
 |
fog_lmdb_recovery_distribution_upload
 |
AWS Credential
 |
Should have write access to your S3 recovery db bucket. 
 |
| 
AWS_PATH
 |
fog_lmdb_recovery_distribution_upload
 |
S3 URL, e.g. s3://mobilecoin.fog/fog-ingest.alpha.mobilecoin.com?region=us-east-2\
 |
The S3 location of your S3 recovery db bucket. 
 |
| 
PEERS
 |
mobilecoind
 |
Comma-separated consensus peers, e.g. mc://node1.alpha.mobilecoin.com/,mc://node2.alpha.mobilecoin.com
 |
Consensus "peers" from which to query the block height, in order to sync the ledger to the appropriate height. 
 |
| 
TX_SRC_URLS
 |
mobilecoind
 |
Comma-separated S3 URLS, e.g. https://s3-us-west-1.amazonaws.com/mobilecoin.chain/node1.alpha.mobilecoin.com/,https://s3-us-west-1.amazonaws.com/mobilecoin.chain/node2.alpha.mobilecoin.com/
 |
S3 source URLs from which to sync archive blocks. 
 |

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
| client-listen-uri | URI, for example: <br /> insecure-fog-ingest://0.0.0.0:3226/ | URI on which to listen for requests. Note: Ingest is not exposed to Fog users, so this is in fact an admin port. There is an admin client which registers users with the ingest service, and it talks to this port.  |
| peer-listen-uri | URI, for example: \n insecure-igp://0.0.0.0:8090/ | URI on which to listen for ingest backup peers. When a node is configured with the --primary-node-uri parameter, it acts as a backup and forms an attested connection with the primary ingest node. |
| ias-api-key | IAS credential | Your API key for attesting with the Intel Attestation Service. See [Getting Intel Attestation Service Credentials](https://docs.google.com/document/d/1iSHIi18Y7UTqzi0V4zy3NbP49ObxeHExsdmqoGHTRa0/edit#heading=h.6udbg7jtjqpq). |
| ias-spid | IAS credential | Your Service Provider ID for attesting with the Intel Attestation Service. See [Getting Intel Attestation Service Credentials](https://docs.google.com/document/d/1iSHIi18Y7UTqzi0V4zy3NbP49ObxeHExsdmqoGHTRa0/edit#heading=h.6udbg7jtjqpq). |
| local-node-id | address:port | The "Responder ID" used in ingest peer attestation for private key backup. |
| sealed-key | Path to local sealed key | Local location to store the sealed enclave key. If the ingest server restarts on the same machine, it can restore its private key from the sealed-key. |
| admin-listen-uri | URI, for example: &nbsp; insecure-mca://127.0.0.1:9091/ | URI on which the in-container admin service connects to gather info from the running ingest service. |

The arguments to the `mobilecoind` include the following:

| Argument | Value | Function |
| ----------- | ----------- | ----------- |
|
peer 
|
Consensus node peer URI. Can provide multiple.

Example:

mc://node1.demo.mobilecoin.com/\
|
Consensus "peers" from which to query the block height, in order to sync the ledger to the appropriate height.
|
|
tx-src-url
|
S3 Path to ledger archive blocks. Can provide multiple.

Example:

https://s3-us-west-1.amazonaws.com/mobilecoin.chain/node1.alpha.mobilecoin.com/\
|
S3 source URLs from which to sync archive blocks. 
|
|
ledger-db
|
Path to ledger db
|
Ledger-db location
|
|
watcher-db
|
Path to watcher db
|
Watcher-db location
|
|
poll-interval 
|
2
|\
|

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

# Glossary

| Term | Definition |
| ----------- | ----------- |
| **Consensus Server** | The MobileCoin Consensus Service facilitates transactions between MobileCoin users. The MobileCoin Consensus Protocol solves the Byzantine Agreement Problem by requiring each user to specify a set of peers that they trust, called a quorum.  |
| **Consensus Validator Node** | Runs Intel's Software Guard eXtensions (SGX), which provides defense-in-depth improvements to privacy and trust. |
| **Fog Service** | Composed of three services and a ledger sync process: Ledger, Ingest, View, and Report. |
| **IAS Account** | An account after you have obtained your Intel Attestation Service credentials. |
| **Ingest Server** | Part of the Fog Service, the Ingest Server processes each transaction in the ledger according to registered users. |
| **Ledger Server** | Part of the Fog Service, the Ledger server serves the contents of the ledger obliviously. |
| **MobileCoin LedgerDB** | Storage location for the LedgerDB containing downloaded blocks. |
| **MobileCoin Tokens (MOB)** | MobileCoin tokens (MOB, pronounced moh-bee) are a new cryptocurrency that you can send over the internet. |
| **Mobilecoind** | A payment system that lets you send money over the internet using a new currency called "mobilecoins" or MOB. |
| **mobilecoindDB** | Storage location for the mobilecoindDB containing indexed transactions for registered accounts. |
| **MRENCLAVE** | The measurement of the enclave. A stronger measurement of the key that includes all of the contents of the enclave, as well as MRSIGNER. |
| **MRSIGNER** | A measurement of the key, which was used to sign the enclave, but does not contain any contents of the enclave itself. |
| **Quorum Set: Trusted and Untrusted Peers**  | To run consensus, you must develop a quorum set that provides the basis of trust required to solve the Byzantine Agreement Problem. You must also link your identity to your node for both Intel's attestation verification, as well as to sign messages in consensus. Determine who you trust for your quorum. By figuring out who to include in your quorum set, you specify peers that you trust. MobileCoin uses Universal Resource Identifiers (URIs) to specify peers. These include the address of the peer, the peer's public key, and can optionally include connection information, such as the certificate authority bundle, and tls-hostname. |
| **Report Server** | Part of the Fog Service, the Report Server serves the Ingest's pubkey and SGX report to construct transactions. |
| **Secure Enclaves** | The MobileCoin Network implements secure enclaves using Intel's Software Guard eXtensions (SGX) to process new transactions according to the MobileCoin Consensus Protocol for integrity and confidentiality. Recent advances in trusted computing make it possible to run software on a remote host without exposing sensitive data to that server's operator, even when she has complete control of the remote computer's operating system (i.e. root access). |
| **Universal Resource Identifiers (URIs)** | Used by MobileCoin to specify peers. The information includes the address of the peer, the peer's public key, and can optionally include connection information, such as the certificate authority bundle, and the tls-hostname. |
| **View Server** | Part of the Fog Service, the View Server obliviously provides TxOs belonging to registered users. |

# Frequently Asked Questions

This section includes some of the most frequently asked questions (FAQs) by node operators:

### Certs

Question: Is it possible in general to generate self-signed certs and put their own cert in front of it?

Answer:

-   It is possible to use self-signed certs behind a load-balancer, as long as each consensus node is uniquely addressable by a URI without a path, e.g. mcp://somenode:12345

-   Platform providers can use a wildcard cert at the load balancer. Currently, MobileCoin nodes operate this way. All nodes connect via TLS to the frontend NGINX ingress, but the nginx ingress then connects back to the nodes in the clear. The cert needs to be pinned internally, so that nginx knows that this cert belongs to this specific host. 

-   You will need to share your public keys with every partner and update them when they change.

Question: Can platform providers use a letsencrypt cert that they are already using?

     Answer: You could use a wildcard cert that you already have, but you need a new cert (even if self-signed) for every consensus participant.

### Intel Attestation Service

Question: Do customers need their own Intel Attestation Service (IAS) Service Provider ID (SPID) key and IAS API Key?

Answer: No. All customers of a provider can use the same IAS SPID and IAS API key.

### Nodes

Question: Can platform providers launch nodes in different ways?

Answer:  [Need answer]

### S3

Question: Do customers control whether their node pushes blocks to S3?

Answer: Either way - platform providers can provide an option to provision bucket for ledger distribution (per-org, per customer)

### TLS Certifications

Question: Do all nodes need to have unique TLS certs / can platform providers manage bastion hosts and have insecure connections between nodes internally?

Answer: All nodes do need to have TLS certs

Question: Is there something specific that has to be in the TLS certs?

     Answer: All by DNS and not by IP

Question: If we renew a cert do we have to restart the node?

     Answer: Yes, because there is a TLS listener inside the nodes