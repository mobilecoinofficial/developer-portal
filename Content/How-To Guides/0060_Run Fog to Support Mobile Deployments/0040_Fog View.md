# Common Tasks

This runbook is specifically for operators who will be setting up and operating Fog View nodes on the MobileCoin network. Part of the MobileCoin Fog Service, the Fog View Server obliviously provides Txos belonging to registered users.

| Relevant Information | Solution |
| -------------------- | -------- |
| How to report bugs | \ |
| POCs | \ |
| Links to Documentation | \ |
| Assumptions: | \ |

## Understanding the Fog View Server

The Fog View Server obliviously provides Txos belonging to registered users.

### Fog Index Data Structure Description

The Fog Index data structure is an oblivious key-value store, which can be horizontally scaled (sharded across several machines). This Horizontally Scaled TxO Server is called the "View Node."

## Running the Node In a Container (Preferred Method)

#### Requirements

-   TLS Certificates for your Node

-   Enclave Private Key (See [Signing Consensus Messages](https://docs.google.com/document/d/1iSHIi18Y7UTqzi0V4zy3NbP49ObxeHExsdmqoGHTRa0/edit#heading=h.9sgb67tqikck)) ? find relevant link

-   S3 Storage Location

### Entrypoint and Container Processes

Familiarize yourself with the [entrypoint for the fog-ingest docker container](https://github.com/mobilecoinofficial/internal/blob/master/ops/entrypoints/fogingest.sh).

![](https://lh4.googleusercontent.com/zIh04JZnrx2s49oC0Evdx-1BqjJEZxFamPbzqVlfrSJCexy46EE7bjK2dJWpF2PQKMRXcXXyaLd-04b21IYi6aOd6o1sYsU8ZBQCXD-_cP5vs9DauMwWQzjS7rawIjBUjsFIxyDb)  Note:  It contains multiple processes working together to provide the full fog-ingest functionality.

These processes include the following:

| Process | Function |
| ------- | -------- |
| fog-view-server | \ |
| fog-lmdb-recovery-distribution-download | Uploads the recovery database (db) to S3 |

#### Environment Variables: How to Configure Your Node

The following environment variables should be provided:

| Variable | Value | Function |
| -------- | ----- | -------- |
| ACCOUNT_DB_DIR | Path to account db | \ |
| AWS_ACCESS_KEY_ID | AWS Credential | Should have write access to your S3 ledger archive bucket. Used by ledger-distribution.<br />And creds to recovery_db? Should these be separate? |
| AWS_SECRET_ACCESS_KEY | AWS Credential | Should have write access to your S3 ledger archive bucket.<br />Used by ledger-distribution. |
| AWS_PATH | S3 URL | The S3 location of your S3 ledger archive bucket. Used by ledger-distribution. |
| SGX_MODE | HW or SW | Indicates whether to run with SGX in hardware or simulation mode. (Should be HW). Used by consensus-service. |
| IAS_MODE | PROD or DEV | Indicates whether to hit the Intel Attestation Service's prod or dev endpoint. (Should be PROD). Used by consensus-service. |
| RUST_LOG | info, debug, trace | Sets the log level. Can also be tuned per specific rust crate. See [env_logger](https://docs.rs/env_logger/0.7.1/env_logger/). Used by ledger-distribution and consensus-service. |
| RUST_BACKTRACE | full, 1, 0 | Indicates how much detail to provide in the backtrace in the event of a panic. Used by ledger-distribution and consensus-service. |

#### Run It!

The code provides the actual docker run command that pulls it all together.

![](https://lh3.googleusercontent.com/Sdyfjy37STZR0ihyB9Xtaanm0OwHYvYwL1foWgnKAxPVfWTh_SY56RlKHjDvdP6mQzQhw8yZMaBkC2Es7q0U5pLDTuIvNjX2PROzWvsTPHnMJ6ZV3hWxSXSOZh8knVI-cfMCGaj9) Warning: Remove the end white space for copy and paste, or the command will fail.

The following code includes:

```
docker run \\
--detach=true

--init \\
--name fogview-test \\
--network=containers

--publish 3225:3225 \\
--publish 8090:8090

--cap-add=SYS_PTRACE \\
--env AWS_PATH=s3://mobilecoin.fog/fog-ingest.ibm.mobilecoin.com?region=us-east-2 \\
--env AWS_SECRET_ACCESS_KEY=$AWS_SECRET_ACCESS_KEY \\
--env AWS_ACCESS_KEY_ID=$AWS_ACCESS_KEY_ID

--env IAS_SPID=$IAS_SPID \\
--env IAS_API_KEY=$IAS_API_KEY

--env VIEW_CLIENT_RESPONDER_ID=respond.demo.mobilecoin.com:443

--env RUST_BACKTRACE=full

--env RUST_LOG=debug,rustls=warn,hyper=warn,tokio_reactor=warn,mio=warn,want=warn,reqwest=warn,rusoto_core=error,h2=error

--env SGX_MODE=HW

--env ACCOUNT_DB_DIR=/account

--env VIEW_ADMIN_URI=insecure-mca://127.0.0.1:8005/

--device /dev/isgx

-it mobilecoin/fogview:<image_tag>
```

The entrypoint passes the arguments from the docker run command to the fog ingest service.

See [Configuring Your Node](https://docs.google.com/document/d/1iSHIi18Y7UTqzi0V4zy3NbP49ObxeHExsdmqoGHTRa0/edit#heading=h.hxfz6iro492h), ? relink. below, for more details on how to set up the values for the parameters unique to your Fog Ingest Node.

![](https://lh3.googleusercontent.com/GTmdxdvA5Y0QpGFDjqOIZuFwgDW-sKg5RikJe1MNsaLLPEOOXJnmx479e8lk3WQ51LHErxSd0ZGOaK1iw9jnoNqI2Z-M8i8IwmuvRFPveLPi0lbhg24zmZm4YyYpKkF6icgW2LmH)  Note: The following processes have been separated. They may be consolidated.

The arguments to the Fog View server include the following:

| Argument | Value | Function |
| -------- | ----- | -------- |
| recovery-db | Path to recovery-db /account | Recovery-db location |
| client-listen-uri | URI, for example:<br />insecure-fog-view://0.0.0.0:3226/ | \ |
| ias-api-key | IAS credential | Your API key for attesting with the Intel Attestation Service. See [Getting Intel Attestation Service Credentials](https://docs.google.com/document/d/1iSHIi18Y7UTqzi0V4zy3NbP49ObxeHExsdmqoGHTRa0/edit#heading=h.6udbg7jtjqpq). |
| ias-spid | IAS credential | Your Service Provider ID for attesting with the Intel Attestation Service. See [Getting Intel Attestation Service Credentials](https://docs.google.com/document/d/1iSHIi18Y7UTqzi0V4zy3NbP49ObxeHExsdmqoGHTRa0/edit#heading=h.6udbg7jtjqpq). |
| client-responder-id | address:port | The "Responder ID" which is used in attestation with clients. |
| admin-listen-uri | URI, for example:<br />insecure-mca://127.0.0.1:9091/ | The admin http gateway sends gRPC requests to the admin-listen-uri for the consensus service, which is listening as specified. The scheme is either mca or insecure-mca to indicate whether TLS is used. Since the admin gateway runs inside the container, insecure-mca is sufficient. |

The arguments to the fog_lmdb_recovery_distribution_upload include the following:

| Argument | Value | Function |
| -------- | ----- | -------- |
| recovery-db | Path to recovery db | \ |
| src | AWS_PATH | Recovery-db location |

## Using Your Own Binaries Built from Source, Without Docker

### Things You Need

-   TLS certificates for your node

-   Ledger Storage Location

-   S3 storage location

### The Various Binaries that You Need to Run

-   Fog_ingest_server

-   fog_lmdb_recovery_distribution_upload

-   mobilecoind

-   Report_server

-   (Probably add a diagram here)

### Run It!

-   Command line for fogingest service

-   Command line for mobilecoind

-   Command line for report server

-   Command line for fog_lmdb_recovery_distribution_upload

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