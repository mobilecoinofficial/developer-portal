---
title: Remote Attestation
---
In addition to secure enclaves, Intel SGX is used by MobileCoin as a "remote attestation" system that can be used to prove that code that claims to be running in a secure enclave is running in a secure enclave. This also proves the code is run on real hardware rather than a simulator.  Put simply, remote attestation gives us more confidence that a remote computer is running the right software, right now. This makes it much harder for operators to cheat in our system, and basically impossible for an operator to deny responsibility if they are ever caught cheating.

Consensus Validator Nodes in the MobileCoin network must use a Intel SGX secure enclave to validate transactions and produce each next block in the  ledger. When each Consensus Validator Node starts up, it initiates an attested connection with its peers. If they  are not running the exact same enclave code, the Enclave Measurements will not match, and the connection will be refused. Similarly, when clients submit a transaction to the network, they ask for the Attestation Evidence from the 
Consensus Validators, and only proceed with an attested connection if the Measurements are what they expect.

In short, the MobileCoin network becomes a network of blind oracles who simply agree on a set of encrypted strings. No information about who is transacting with whom, or for how much is revealed, even to the operators of the nodes that validate the transactions.

To understand how SGX improves the integrity of MobileCoin's Federated Byzantine Agreement Consensus Protocol, please see our Tech Talk: [Why we use SGX for MobileCoin](https://www.youtube.com/watch?v=Hwf_Q31woLo).

### Show Me the Code

* [Attestation](https://github.com/mobilecoinfoundation/mobilecoin/tree/master/attest/core): MobileCoin's attestation implementation
* [SGX](https://github.com/mobilecoinfoundation/mobilecoin/tree/master/sgx): A lightweight `no_std` Rust environment for use inside an SGX enclave.
* [Consensus Enclave](https://github.com/mobilecoinfoundation/mobilecoin/tree/master/consensus/enclave): MobileCoin's Consensus Enclave implementation and API
