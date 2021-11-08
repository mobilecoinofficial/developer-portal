---
title: Get All TXOs for Address
---
Get the JSON representation of the "TXO" object in the ledger.

## Parameters

| Required Param | Purpose | Requirements |
| -------------- | ------- | ------------ |
| `address` | The address on which to perform this action |  |

## Example

**Request Body**

```
{
  "method": "get_all_txos_for_address",
  "params": {
    "address": "CaE5bdbQxLG2BqAYAz84mhND79iBSs13ycQqN8oZKZtHdr6KNr1DzoX93c6LQWYHEi5b7YLiJXcTRzqhDFB563Kr1uxD6iwERFbw7KLWA6",
  },
  "jsonrpc": "2.0",
  "id": 1
}
```

**Response**

```
{
  "method": "get_txo_object",
  "result": {
    "txo": ...
  }
}
```