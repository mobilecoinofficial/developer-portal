---
title: "Encrypted Memos"
use_file: "mobilecoinfoundation/mcips/text/0003-encrypted-memos.md"
---
We propose to extend mobilecoin transaction outputs to include an "encrypted memo" field of a fixed, 46 byte length. These memo fields have a standard encryption scheme and can be read by the sender and recipient, but no one else.

These memos are intended to hold data, rather than human-readable text. (Although that is one possible use.) We propose an extensible method for specifying a schema for the plaintext data in the memo. We propose that memo types are standardized via MCIP proposals, so that clients can interoperate seamlessly.

Please see [this doucment](https://github.com/mobilecoinfoundation/mcips/blob/main/text/0003-encrypted-memos.md) for details.
