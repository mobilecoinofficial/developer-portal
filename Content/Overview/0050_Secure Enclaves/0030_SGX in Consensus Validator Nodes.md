---
title: What is Remote Attestation?
use_file: "mobilecoinfoundation/mobilecoin/sgx/README.md"
---
Consensus Validator Nodes include an SGX enclave which validates transactions and produces each next block in the 
ledger. When each Consensus Validator Node starts up, it initiates an attested connection with its peers. If they 
are not running the exact same enclave code, the Enclave Measurements will not match, and the connection will be
refused. Similarly, when clients submit a transaction to the network, they ask for the Attestation Evidence from the 
Consensus Validators, and only proceed with an attested connection if the Measurements are what they expect.

### Show Me the Code

* [Attestation](https://github.com/mobilecoinfoundation/mobilecoin/tree/master/attest/core): MobileCoin's attestation implementation
* [SGX](https://github.com/mobilecoinfoundation/mobilecoin/tree/master/sgx): A lightweight `no_std` Rust environment for use inside an SGX enclave.
* [Consensus Enclave](https://github.com/mobilecoinfoundation/mobilecoin/tree/master/consensus/enclave): MobileCoin's Consensus Enclave implementation and API
