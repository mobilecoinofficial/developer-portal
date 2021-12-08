---
title: Watcher Node
---
Watcher nodes perform an essential role in the MobileCoin network by verifying the signatures that the full validator nodes attach to each block. In this way the watcher nodes continuously monitor the integrity of the decentralized MobileCoin network.

### Timestamps

Watchers can also be used to sync metadata from multiple Consensus Validator Nodes. For example, the Watcher can be used
in systems that require the timestamp from each node per block.

### Blockchain Explorer

The Watcher provides the backend for the [Blockchain Explorer](https://block-explorer.0--0.net/) (Beta), which serves 
the block signatures, timestamps, TXO data, and key_images per block.

### Show Me the Code

* [Watcher](https://github.com/mobilecoinfoundation/mobilecoin/tree/master/watcher): Implementation of the watcher
* [mobilecoind](https://github.com/mobilecoinfoundation/mobilecoin/tree/master/mobilecoind): Ledger-syncing daemon that can be run in "Watcher Mode"