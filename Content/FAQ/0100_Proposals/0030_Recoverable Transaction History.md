---
title: Recoverable Transaction History
use_file: "mobilecoinfoundation/mcips/text/0004-recoverable-transaction-history.md"
---
We propose to specify three memo types that support a "recoverable transaction history" feature.

(See [mobilecoinfoundation/mcips#3](https://github.com/mobilecoinfoundation/mcips/pull/3) for a
description of memos and memo types.)

When a sender sends money to a recipient, the "sender memo" is attached to the TxOut that the
recipient recieves, and permits them to identify the sender. The "destination memo" is attached
to the change TxOut that the sender recieves, and records details of the recipient and the amount sent.

In a mobile chat application, the use of these memos in payments permits the app to recover transaction
history given only the private keys of the user, by fetching all of the user's TxOut's from Fog and then
decoding and validating the memos.

Some additional ancillary parts of the proposal:
- Change subaddress is standardized to subaddress index 1.
- Clients should always produce a change output even if the change is zero, in order to have a place to put
  the destination memo.
- A standard 16-byte hash of a public address is specified, which may be useful elsewhere.
- A memo containing payment request id numbers is also introduced.

Please see [this doucment](https://github.com/mobilecoinfoundation/mcips/blob/main/text/0004-recoverable-transaction-history.md) for details.
