# About this Manual

MobileCoin Fog is a scalable service infrastructure that enables a smartphone to manage a privacy-preserving cryptocurrency with locally-stored cryptographic keys. Unique from the public ledger, the Fog Service returns all or some portion of user transactions obliviously to enable operators to look up transactions. The MobileCoin Fog Service is composed of three services and a ledger sync process:

-   Ingest: Processes each transaction in the public ledger according to registered users.

-   Report: Serves the Ingest's pubkey and SGX report to construct transactions.

-   View: Obliviously provides Txos belonging to registered users.

-   Ledger: Serves the contents of the public ledger obliviously.

![](https://lh4.googleusercontent.com/Wcl5VLbfIubWvtMZEKVTTIHro55Ofl22N6-bLqHpOZ-2QCLpXYkDEVpKvqqfLsSsXyKntM5pAIFGUM9xedbtKXsGablvSCuRsoYhtdc-4dWA_c8IxGAKZuNOPRBBSD2TlZx8dpPB)
Figure 1: The Fog System Overview.

This manual contains a comprehensive overview of one part of the MobileCoin Fog Server, the Fog Ledger Service, and instructions on how to set up and manage a remote Fog Ledger node on the MobileCoin Network.

For more information about the MobileCoin Network, check out the [MobileCoin Ecosystem](https://docs.google.com/document/d/1vbnqfwW6sED7t61YKDIAItK4eWM3lSIDXoT18Nl9KfY/edit?usp=sharing) - a brief document about the MobileCoin open-source software network.

## Who Should Read this Manual

This manual should be read and referenced by operators, who have been entrusted with setting up and managing the Fog services in the MobileCoin network. 

## How to Use this Manual

This manual provides operators with quick help for setting up and operating the MobileCoin Fog Ledger Server. This document is separated into three sections:

1.  Overview of the Fog Ledger Server

2.  Step-by-step instructions for common tasks

3.  Lists of errors and alerts the Fog Ledger Server may generate with solutions.

Special symbols are used throughout the manual to make you aware of important information and valuable tips.

### Special Symbols Used in this Manual

The following symbols represent important information and valuable tips associated with the MobileCoin Fog Ledger Server and are displayed throughout the manual. Every noted symbol has significant meaning that you should heed:

![](https://lh6.googleusercontent.com/8W6xOHPYS6FND5m51hh_u_3taqds9RxsQRcmBwvsGbErPM-mxXJ93peM0DMyNQhA10aXfh7HjkNr7Ldurb29aCiq8kETtrYL2yhd-o6Aom_tTsz6DTeISihITzVnMyAVh7Z4dgXG)  Note: Important information concerning operating the Fog Ledger Server.

![](https://lh3.googleusercontent.com/Ku0KWBwMkZc98ZpiZHkPVQyXtLgrkAqBNXQYjVKnPTLlbTD9KjjY8ARcfdJ9JaBqGUw-oh93cTTI4ryhzD0ttlb6R_koibEMkC3py1BPSDjpJ5kYtqXBI25T4JkNjuSIOyQfZinW) Warning: Failure to follow directions may result in preventing your Fog Ledger Server from operating successfully in the MobileCoin Network.

# Common Tasks

This runbook is specifically for operators who will be setting up and operating Fog Ledger nodes on the MobileCoin network. Part of the MobileCoin Fog Service, the Fog Ledger Server serves the contents of the public ledger obliviously.

| Relevant Information | Solution |
| --- | --- |
| How to report bugs | \ |
| POCs |\ |
| Links to Documentation | \ |
| Assumptions: | \ |

## Understanding the Fog Ledger Server

The Fog Ledger Server serves the contents of the public ledger obliviously. 

## Running the Ledger Server Node

### Network Ports

The following ports need to be exposed. The actual ports assigned may vary according to your needs, but will need to be communicated for e.g. node and client connection. Presented here are the defaults.

![](https://lh6.googleusercontent.com/xkQTxMiRAbbJ9kew63SEoNXtXL-xC8tiF5-1Rlui6He7fRBNXK90h-Q0YSu-up68ZNNSDRlFdpP6d20EBApCyb9w1HJryJPWdx97y6uu5i0P5i1bgN5yEd0s-_yjBZgncj2biaNJ)  Note:  You can establish your ingress mapping in whatever manner is convenient to your infrastructure. We present an example of the default MobileCoin ingress configuration.

| Purpose | Service Port | Ingress Mapping | Dependencies |
| ------- | ------------ | --------------- | ------------ |
| Client queries | 3228 | 443 | Clients obtain ledger materials. |
| Admin (metrics) | 9090 | 9090 | Prometheus metrics are provided on the 9090 port. |

## Running the Node In a Container (Preferred Method)

The preferred method for running the node is in a container because MobileCoin engineers have assimilated and compiled the optimum running environment to improve setup time and efficiencies in operation of the node.

### Requirements

-   TLS Certificates for your Ledger Node

-   S3 Location from which to sync the ledger

### Entrypoint and Container Processes

Familiarize yourself with the [entrypoint for the fog-ledger docker container](https://github.com/mobilecoinofficial/internal/blob/master/ops/entrypoints/fogledger.sh).

![](https://lh4.googleusercontent.com/-_2pFLsbb1eTkTHi_VYtKDuy_waGuNMfsFWQOhYbc7p75vXxewyaITic8cSIHqzgK-7E4QzYYg6nn_fe_YASws-oKgFBbYI-nzuNkZNzxasTgFZyv8agq8V162SQAWInFE2q_8rG)  Note:  The fog-ledger container contains multiple processes working together to provide the full fog-ledger functionality.

These processes include the following:

| Process | Function |
| ------- | -------- |
| ledger-server | Obliviously provides ledger materials necessary for constructing transactions and checking balance. |
| mobilecoind | Downloads the ledger database (ledger-db) from S3 |

#### Environment Variables: How to Configure Your Ledger Ledger Node

The following environment variables should be provided:

| Variable | Service(s) | Value | Function |
| -------- | ---------- | ----- | -------- |
| LEDGER_DIR | ledger_server, mobilecoind | Path to ledger db | Location of ledger. |
| WATCHER_DB_DIR | ledger_server, mobilecoind | Path to watcher db | The watcher db provides block timestamps. mobilecoind syncs the block timestamps along with the ledger. |
| FOG_LEDGER_CLIENT_LISTEN_URI | ledger_server | URI, e.g. insecure-fog-ledger://0.0.0.0:3226/ | Listening URI for client requests. |
| CLIENT_RESPONDER_ID | ledger_server | Address and port, e.g. fog-ledger.alpha.mobilecoin.com:443 | "Responder ID" for attestation with clients. |
| IAS_SPID | fog_ingest_server | IAS credential | Required for client attestation. |
| IAS_API_KEY | fog_ingest_server | IAS credential | Required for client attestation. |
| PEERS | mobilecoind | Comma-separated consensus peers, e.g. mc://node1.alpha.mobilecoin.com/,mc://node2.alpha.mobilecoin.com | Consensus "peers" from which to query the block height, in order to sync the ledger to the appropriate height. |
| TX_SRC_URLS | mobilecoind | Comma-separated S3 URLS, e.g. https://s3-us-west-1.amazonaws.com/mobilecoin.chain/node1.alpha.mobilecoin.com/,https://s3-us-west-1.amazonaws.com/mobilecoin.chain/node2.alpha.mobilecoin.com/ | S3 source URLs from which to sync archive blocks.  |

#### Run It!

The following code provides the actual docker run command that pulls it all together.

![](https://lh3.googleusercontent.com/Ku0KWBwMkZc98ZpiZHkPVQyXtLgrkAqBNXQYjVKnPTLlbTD9KjjY8ARcfdJ9JaBqGUw-oh93cTTI4ryhzD0ttlb6R_koibEMkC3py1BPSDjpJ5kYtqXBI25T4JkNjuSIOyQfZinW) Warning: Remove end white space for copy and paste, or the command will fail.

```
docker run

--detach=true

--init

--name fogledger-test

--network=containers

--publish 3228:3228

--publish 9090:9090

--cap-add=SYS_PTRACE

--env PEERS=mc://node1.alpha.mobilecoin.com/,mc://node2.alpha.mobilecoin.com

--env TX_SOURCE_URLS=https://s3-us-west-1.amazonaws.com/mobilecoin.chain/node1.alpha.mobilecoin.com/,https://s3-us-west-1.amazonaws.com/mobilecoin.chain/node2.alpha.mobilecoin.com/

--env AWS_SECRET_ACCESS_KEY=$AWS_SECRET_ACCESS_KEY

--env AWS_ACCESS_KEY_ID=$AWS_ACCESS_KEY_ID

--env IAS_SPID=$IAS_SPID

--env IAS_API_KEY=$IAS_API_KEY

--env VIEW_CLIENT_RESPONDER_ID=fog-ledger.alpha.mobilecoin.com:443

--env CLIENT_LISTEN_URI=insecure-fog-ledger://0.0.0.0:3228/

--env LEDGER_DIR=/ledger

--device /dev/isgx

-it mobilecoin/fogledger:<image_tag> 
```

There are no arguments to the entrypoint via the docker run command.

## Using Your Own Binaries Built from Source, Without Docker

### Things You Need

-   TLS certificates for your node

-   Ledger Storage Location

-   S3 storage location

### The Various Binaries that You Need to Run

-   ledger_server

-   mobilecoind

### Run It!

-   Command line for fog ledger service

-   Command line for mobilecoind

The arguments to the fog ledger server include the following:

| Argument | Value | Function |
| -------- | ----- | -------- |
| ledger-db | Path to ledger db | The ledger materials synced from the network, to provide obliviously to clients. |
| client-listen-uri | URI, for example:<br />insecure-fog-ledger://0.0.0.0:3226/ | The URI on which to listen for client requests. |
| ias-api-key | IAS credential | Your API key for attesting with the Intel Attestation Service. See [Getting Intel Attestation Service Credentials](https://docs.google.com/document/d/1iSHIi18Y7UTqzi0V4zy3NbP49ObxeHExsdmqoGHTRa0/edit#heading=h.6udbg7jtjqpq). |
| ias-spid | IAS credential | Your Service Provider ID for attesting with the Intel Attestation Service. See [Getting Intel Attestation Service Credentials](https://docs.google.com/document/d/1iSHIi18Y7UTqzi0V4zy3NbP49ObxeHExsdmqoGHTRa0/edit#heading=h.6udbg7jtjqpq). |
| client-responder-id | address:port | The "Responder ID" which is used in attestation with clients. |
| admin-listen-uri | URI, for example:<br />insecure-mca://127.0.0.1:9091/ | The admin http gateway sends gRPC requests to the admin-listen-uri for the consensus service, which is listening as specified. The scheme is either mca or insecure-mca to indicate whether TLS is used. Since the admin gateway runs inside the container, insecure-mca is sufficient. |

The arguments to mobilecoind include the following:

| Argument | Value | Function |
| -------- | ----- | -------- |
| peer | Consensus node peer URI. Can provide multiple.<br />Example:<br />mc://node1.demo.mobilecoin.com/ | Consensus "peers" from which to query the block height, in order to sync the ledger to the appropriate height. |
| tx-src-url | S3 Path to ledger archive blocks. Can provide multiple.<br />Example:<br />https://s3-us-west-1.amazonaws.com/mobilecoin.chain/node1.alpha.mobilecoin.com/ | S3 source URLs from which to sync archive blocks. |
| ledger-db | Path to ledger db | Ledger-db location |
| watcher-db | Path to watcher db | Watcher-db location. The watcher syncs additional per-node metadata about the ledger, such as block signatures and timestamps. |
| Poll-interval | 2 | Amount of time between querying for nodes' block heights. |

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