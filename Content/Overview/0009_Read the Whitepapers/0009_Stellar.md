---
title: Consensus Protocol
---
All cryptocurrencies rely on a distributed network of equivalent nodes to cooperatively agree on the validity and ordering of transactions. MobileCoin has developed a high-performance, byzantine fault tolerant protocol for distributed agreement called the MobileCoin Consensus Protocol (MCP), based on the “federated byzantine agreement” described in the Stellar Consensus Protocol by David Mazieres. 

Note that MobileCoin adds an additional security enhancement to the Stellar Consensus Protocol. Rather than agreeing on sets of transactions, nodes instead agree on the cryptographic hash of encrypted sets of transactions. This allows the consensus algorithm to operate independently of the MobileCoin Ledger protocol validation code so that the potential attack surface is minimized.

# [Download the Whitepaper](https://raw.githubusercontent.com/mobilecoinofficial/developer-portal/master/info/stellar.pdf)

[widget type='pdfViewer' pdf='//raw.githubusercontent.com/mobilecoinofficial/developer-portal/master/info/stellar.pdf']
