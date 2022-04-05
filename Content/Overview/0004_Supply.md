---
title: Circulating Supply
---

### Supply

MobileCoin's total current supply of coins is 250 million MOB.  The total supply is fixed, and you can see this in our open source code here: [total current supply of coins](https://github.com/mobilecoinfoundation/mobilecoin/blob/master/transaction/core/src/constants.rs).  The live circulating supply is 
available here: [current circulating supply](https://mobilecoin.foundation/wp-json/mcfoundation/circulating-supply). 

The entire supply of MobileCoins was created in the very first block on the blockchain. MobileCoin uses 'Federated Byzantine Agreement' modeled on the the [Stellar Consensus Protocol](https://www.stellar.org/papers/stellar-consensus-protocol?locale=en). This sets MobileCoin apart from Bitcoin and other cryptocurrencies that use 'Proof-of-Work' a process that allows the bitcoin network to construct the ledger by making the process of recording transactions difficult, also known as 'bitcoin mining.' Minig requires a large amount of computing power and miners earn 'block rewards.' This encourages miners to invest in adding greater and greater computing resources to add more miners to the network. The more computers they put ont he network, the more bitcoin they can receive. MobileCoin does not incentivize this type of behavior with block rewards.

MobileCoin is a private cryptocurrency where amounts are hidden behind cryptographic constructions; therefore, it is not possible for observers to passively verify that the origin block contains 250 million MOB. Below you will find instructions on how to access and inspect the origin account.


### Verifying the Supply

You can verify the supply of MobileCoin using the Origin Block Account.

`Origin Block Account Entropy:\
aa9082086d4f341300d3ba08d41db566edc9fb61eec623870b80871681a589eb\
{\
"view_private_key":\
"d825a5c3aaef812d17feba5cf8f3019bcb0608c0a673200fa425e56eb1dc2d0b",\
"spend_private_key":\
"9e95df3801c4b6ab0be854377430e18528546be71335e2afb91f058b3013380f"\
}`

The MobileCoin ledger is denominated in pico MOB (pMOB), where 1 MOB = 10¹² pMOB.

This means the origin block should have 2.5 * 10²⁰ pMOB. You'll note that this exceeds the [maximum value of a u64](https://doc.rust-lang.org/std/primitive.u64.html#associatedconstant.MAX) (~1.8 * 10¹⁹), which is the maximum amount that can be stored in a MobileCoin Txo. For this reason, the origin block is split into 16 Txos, as opposed to a single Txo containing 250M MOB.

MobileCoin currently supports multiple wallet software implementations (see Forum discussion: [Desktop Wallets](https://community.mobilecoin.foundation/t/desktop-wallets/566)). We will describe how to verify the origin block using the Full-Service wallet.

### Verification Steps with Full-Service Desktop Wallet

1.  Download or build the latest MainNet-configured Full-Service binary. See the latest release, which has instructions for building.
2.  Run the MainNet-configured Full-Service binary. See the latest release for an example run command.
3.  Make an account with the origin entropy in a new terminal window via the [`import_account_from_legacy_root_entropy`](https://app.gitbook.com/@mobilecoin/s/full-service-api/accounts/account/import_account_from_legacy_root_entropy-deprecated) endpoint. See the [MobileCoin Key Derivation Upgrade: Migration to Mnemonics](https://medium.com/mobilecoin/mobilecoin-key-derivation-upgrade-migration-to-mnemonics-99a329a8b783) post to understand why root entropies are 'legacy' (i.e. no longer actively supported).\
    `curl -s localhost:9090/wallet \\
    -d '{\
    "method": "import_account_from_legacy_root_entropy",\
    "params": {\
    "entropy": "aa9082086d4f341300d3ba08d41db566edc9fb61eec623870b80871681a589eb",\
    "name": "Origin",\
    "next_subaddress_index": "2",\
    "first_block_index": "0"\
    },\
    "jsonrpc": "2.0",\
    "id": 1\
    }' \\
    -X POST -H 'Content-type: application/json' | jq`
4.  Note the `account_id` that is produced, and then check the balance via [`get_balance_for_account`](https://app.gitbook.com/@mobilecoin/s/full-service-api/accounts/balance/get_balance_for_account).\
    `curl -s localhost:9090/wallet \\
    -d '{\
    "method": "get_balance_for_account",\
    "params": {\
    "account_id": "<your-account-id-here>"\
    },\
    "jsonrpc": "2.0",\
    "id": 1\
    }' \\
    -X POST -H 'Content-type: application/json' | jq`\
    The account was already emptied, so `unspent_pmob = 0` (note that the `spent_pmob` value is nonsense caused by integer overflow). If you have lightning fingers and manage to check the balance fast enough (before more than the origin block has synced), you will see the balance is 250 million MOB.\
    `{\
    "method": "get_balance_for_account",\
    "result": {\
    "balance": {\
    "object": "balance",\
    "network_block_index": "10253",\
    "local_block_index": "1",\
    "account_block_index": "1",\
    "is_synced": false,\
    "unspent_pmob": "250000000000000000000",\
    "pending_pmob": "0",\
    "spent_pmob": "0",\
    "secreted_pmob": "0",\
    "orphaned_pmob": "0"\
    }\
    },\
    "error": null,\
    "jsonrpc": "2.0",\
    "id": 1\
    }`
5.  To dive in deeper, you can use the [`get_all_txos_for_account`](https://app.gitbook.com/@mobilecoin/s/full-service-api/transactions/txo/get_all_txos_for_account) endpoint to see which outputs were received, and which blocks they were spent in.\
    `curl -s localhost:9090/wallet \\
    -d '{\
    "method": "get_all_txos_for_account",\
    "params": {\
    "account_id": "<your-account-id-here>"\
    },\
    "jsonrpc": "2.0",\
    "id": 1\
    }' \\
    -X POST -H 'Content-type: application/json' | jq`\
    There are 19 outputs owned by the origin account, however only 16 are 'origin outputs' (with 15.625 mill MOB each). Outputs with `received_block_index = 0` were created in the origin block. The remaining 3 outputs correspond to either self-spends (sending MOB to yourself) or change outputs (left-over MOB from sending to other people).
