---
title: "Consensus Enclave Omnibus"
description: "This collection of crates implements a \"fully formed\" interface for the consensus_service application to interact with its enclave, which is used to process inputs from clients. The \"outer\" mc-consensus-enclave crate is the one which the application will interact with, and it, in turn, will proxy requests to the actual enclave."
use_file: "mobilecoinfoundation/mobilecoin/consensus/enclave/README.md"
---
This collection of crates implements a "fully formed" interface for the `consensus_service` application to interact with its enclave, which is used to process inputs from clients. The "outer" `mc-consensus-enclave` crate is the one which the application will interact with, and it, in turn, will proxy requests to the actual enclave.

Please see [this doucment](https://github.com/mobilecoinfoundation/mobilecoin/blob/master/consensus/enclave/README.md) for more details.
