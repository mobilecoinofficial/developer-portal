---
title: "Full Service API"
use_file: "mobilecoinofficial/full-service/README.md"
---

The MobileCoin Full Service API Node is a ledger validator and wallet designed for high performance use-cases.

The Full-Service Node is a platform which you can use to build wallet services. It provides ledger syncing and validation, account management, and funds transfer and receiving. It uses a JSONRPC API, so you can connect to it from command line tools or build services around its functionality. It serves the use cases of single user (and is the backing to the MobileCoin Desktop Wallet), while also serving high performance, multi-account, multi-subaddress needs (such as backing merchant services platforms).

Please see the full API documentation at: [High-Performance Wallet API](https://mobilecoin.gitbook.io/full-service-api/)

It supports:

* multiple accounts
* subaddresses (unique public addresses for incoming transactions, to assign per-user)
* balance-checking (totals, and per-user-incoming)
* transaction construction & submission
* transaction history store
* hot and cold operating modes

For the Full-Service API documentation and tutorials, see the [Full-Service Documentation](https://mobilecoin.gitbook.io/full-service-api/).

### Show Me the Code

* [Full-Service Node](https://github.com/mobilecoinofficial/full-service): Implementation of the Full-Service Node
