---
title: "Daemon CLI Tool"
use_file: "mobilecoinfoundation/mobilecoin/mobilecoind/README.md"
---
The MobileCoin Daemon, or `mobilecoind`, is a standalone executable which provides a command line interface (CLI) for blockchain synchronization and wallet services.

It creates encrypted, attested connections to validator nodes who are participating in federated voting in order to get the current block height, block headers, and to submit transactions. These validator nodes are considered highly trusted due to their use of SGX, and they are used as a reliable source for block information as well as to validate and process the proposed transactions from `mobilecoind`.

Please see [this document](https://github.com/mobilecoinfoundation/mobilecoin/blob/master/mobilecoind/README.md) for details.
