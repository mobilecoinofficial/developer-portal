---
title: Governance and Supply
---

<img src="https://mobilecoinwp.wpengine.com/wp-content/uploads/2021/06/padlocks.jpeg" width="400">

Governance
MobileCoin is a privacy-focused cryptocurrency launched on December 7th, 2020. Its initial code base was developed by MobileCoin Inc. and published via the MobileCoin Foundation with an open-source license (GPLv3).

The MobileCoin Foundation maintains the reference implementation of MobileCoin’s (the cryptocurrency’s) protocol, along with the ‘service-layer’ technology known as Fog that facilitates efficient use of MobileCoin in resource-constrained environments like mobile devices, without loss of privacy.

Changes to those reference implementations proposed by MobileCoin Inc. (for example) must be approved by the MobileCoin Foundation’s Technical Committee.

Fees
Each transaction submitted to the MobileCoin network must pay a fee to the MobileCoin Foundation. The purpose of this fee is to prevent unlimited submission of transactions, which otherwise could be easily abused to degrade users’ experience. At the time of writing this, the minimum fee per transaction is 0.01 MOB, and is always distributed to the following address (written in hexadecimal format):

public view key:
5222a1e9ae32d21c23114a5ce6bb39e0cb56aea350d4619d43b1207061b10346
public spend key:
26b507c63124a2f5e940b4fb89e4b2bb0a2078ed0c8e551ad59268b9646ec241

There is currently a plan for the minimum fee to be configurable in the next version of MobileCoin’s protocol. Changing the fee configuration would require a coordinated restart of all validator nodes in the network.

Supply

The origin block of MobileCoin created 250 million MOB, and the supply is fixed. The live circulating supply is 
available [here](https://mobilecoin.foundation/wp-json/mcfoundation/circulating-supply). 

Unlike many cryptocurrencies, where ‘Proof-of-Work’ plays a central role in constructing the ledger, MobileCoin uses an implementation of the Stellar Consensus Protocol, which relies on the ‘Federated Byzantine Agreement model’. An important consequence of that model is the infeasibility of ‘block rewards’ in MobileCoin, which in other cryptocurrencies is the primary mechanism for creating new coins.

Since block rewards could not be used, the entire supply of MobileCoins was created when the cryptocurrency was first launched. In other words, the entire supply was created in MobileCoin’s origin block.

However, MobileCoin is a private cryptocurrency where amounts are hidden behind cryptographic constructions; therefore, it is not possible for observers to passively verify that the origin block contains 250 million MOB. Below you will find instructions on how to access and inspect the origin account.

Verifying the Supply
You can verify the supply of MobileCoin using the Origin Block Account.

Origin Block Account Entropy:
aa9082086d4f341300d3ba08d41db566edc9fb61eec623870b80871681a589eb
{
“view_private_key”:
“d825a5c3aaef812d17feba5cf8f3019bcb0608c0a673200fa425e56eb1dc2d0b”,
“spend_private_key”:
“9e95df3801c4b6ab0be854377430e18528546be71335e2afb91f058b3013380f”
}

The MobileCoin ledger is denominated in pico MOB (pMOB), where 1 MOB = 10¹² pMOB.

This means the origin block should have 2.5 * 10²⁰ pMOB. You’ll note that this exceeds the maximum value of a u64 (~1.8 * 10¹⁹), which is the maximum amount that can be stored in a MobileCoin Txo. For this reason, the origin block is split into 16 Txos, as opposed to a single Txo containing 250M MOB.

MobileCoin currently supports multiple wallet software implementations (see Forum discussion: Desktop Wallets). We will describe how to verify the origin block using the Full-Service wallet.

Verification Steps with Full-Service Desktop Wallet
Download or build the latest MainNet-configured Full-Service binary. See the latest release, which has instructions for building.
Run the MainNet-configured Full-Service binary. See the latest release for an example run command.
Make an account with the origin entropy in a new terminal window via the import_account_from_legacy_root_entropy endpoint. See the MobileCoin Key Derivation Upgrade: Migration to Mnemonics post to understand why root entropies are ‘legacy’ (i.e. no longer actively supported).
curl -s localhost:9090/wallet \
-d '{
"method": "import_account_from_legacy_root_entropy",
"params": {
"entropy": "aa9082086d4f341300d3ba08d41db566edc9fb61eec623870b80871681a589eb",
"name": "Origin",
"next_subaddress_index": "2",
"first_block_index": "0"
},
"jsonrpc": "2.0",
"id": 1
}' \
-X POST -H 'Content-type: application/json' | jq
Note the account_id that is produced, and then check the balance via get_balance_for_account.
curl -s localhost:9090/wallet \
-d '{
"method": "get_balance_for_account",
"params": {
"account_id": "<your-account-id-here>"
},
"jsonrpc": "2.0",
"id": 1
}' \
-X POST -H 'Content-type: application/json' | jq
The account was already emptied, so unspent_pmob = 0 (note that the spent_pmob value is nonsense caused by integer overflow). If you have lightning fingers and manage to check the balance fast enough (before more than the origin block has synced), you will see the balance is 250 million MOB.
{
"method": "get_balance_for_account",
"result": {
"balance": {
"object": "balance",
"network_block_index": "10253",
"local_block_index": "1",
"account_block_index": "1",
"is_synced": false,
"unspent_pmob": "250000000000000000000",
"pending_pmob": "0",
"spent_pmob": "0",
"secreted_pmob": "0",
"orphaned_pmob": "0"
}
},
"error": null,
"jsonrpc": "2.0",
"id": 1
}
To dive in deeper, you can use the get_all_txos_for_account endpoint to see which outputs were received, and which blocks they were spent in.
curl -s localhost:9090/wallet \
-d '{
"method": "get_all_txos_for_account",
"params": {
"account_id": "<your-account-id-here>"
},
"jsonrpc": "2.0",
"id": 1
}' \
-X POST -H 'Content-type: application/json' | jq
There are 19 outputs owned by the origin account, however only 16 are ‘origin outputs’ (with 15.625 mill MOB each). Outputs with received_block_index = 0 were created in the origin block. The remaining 3 outputs correspond to either self-spends (sending MOB to yourself) or change outputs (left-over MOB from sending to other people).
