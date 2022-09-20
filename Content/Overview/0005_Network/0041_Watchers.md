---
title: Watchers
---
Watcher nodes perform an essential role in the MobileCoin network by verifying the signatures that the full validator nodes attach to each block. In this way the watcher nodes continuously monitor the integrity of the decentralized MobileCoin network.

### Timestamps

Watchers can also be used to sync metadata from multiple Consensus Validator Nodes. For example, the Watcher can be used
in systems that require the timestamp from each node per block.

### Blockchain Explorer

The Watcher provides the backend for the [Blockchain Explorer](https://block-explorer.mobilecoin.foundation/), which serves 
the block signatures, timestamps, TXO data, and key_images per block.

### Show Me the Code

* [Watcher](https://github.com/mobilecoinfoundation/mobilecoin/tree/master/watcher): Implementation of the watcher
* [mobilecoind](https://github.com/mobilecoinfoundation/mobilecoin/tree/master/mobilecoind): Ledger-syncing daemon that can be run in "Watcher Mode"

Basic run command to sync block signatures from two nodes on the test network:

Create a `sources.toml` file, for example:

```source-toml
[[sources]]
tx_source_url = "https://s3-us-west-1.amazonaws.com/mobilecoin.chain/node1.test.mobilecoin.com/"
consensus_client_url = "mc://node1.test.mobilecoin.com/"

[[sources]]
tx_source_url = "https://s3-us-west-1.amazonaws.com/mobilecoin.chain/node2.test.mobilecoin.com/"
consensus_client_url = "mc://node2.test.mobilecoin.com/"
```

Run the watcher:

```source-shell
SGX_MODE=HW IAS_MODE=PROD MC_LOG=debug,hyper=error,want=error,reqwest=error,mio=error,rustls=error\
    cargo run -p mc-watcher --bin mc-watcher --\
    --sources-path sources.toml\
    --watcher-db /tmp/watcher-db
```

The watcher can also be incorporated into other programs, as in [`mobilecoind`](https://github.com/mobilecoinfoundation/mobilecoin/blob/master/mobilecoind/README.md), where the watcher continuously syncs block signatures, and `mobilecoind` offers an interface to query block signatures for watched nodes through the mobilecoind API.

In order to check that the watcher is running, you can send a gRPC request to the health check endpoint:

```source-shell
grpcurl -proto ./util/grpc/proto/health_api.proto -plaintext localhost:3226 grpc.health.v1.Health/Check
```
