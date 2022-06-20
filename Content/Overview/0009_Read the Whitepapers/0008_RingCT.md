---
title: Ring Confidential Transactions
---
CryptoNote ledgers are significantly more secure than Bitcoin, but they still leave the amount of each transaction in plain sight. On the MobileCoin ledger, the monetary value of each txo is encrypted using the method of Ring Confidential Transactions (RingCT) published in 2016 by Shen Noether and Adam Mackenzie of the Monero Research Lab.

RingCT protects the amount exchanged in each transaction using cryptography. Rather than publish the value that is exchanged, RingCT transactions include a mathematical proof that the transaction is balanced, meaning that the recipient didn’t receive more money than the sender spent. 

Only the receiver of the transaction can reveal the encrypted monetary value and spend the new txos that are written to the ledger. The recipient’s cryptographic control over spending ensures that all transactions in MobileCoin are irreversible, similar to cash transactions in the real world.

# [Download the Whitepaper](https://raw.githubusercontent.com/mobilecoinofficial/developer-portal/master/info/ringct.pdf)

[widget type='pdfViewer' pdf='//raw.githubusercontent.com/mobilecoinofficial/developer-portal/master/info/ringct.pdf']
