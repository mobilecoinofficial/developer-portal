---
title: Get Transaction Object
---
Get the JSON representation of the TXO object in the transaction log.

## Parameters

| Required Param | Purpose | Requirements |
| -------------- | ------- | ------------ |
| `transaction_log_id` | The transaction log ID to get. | Transaction log must exist in the wallet. |

## Example

**Request Body**

```
{
  "method": "get_transaction_object",
  "params": {
    "transaction_log_id": "4b4fd11738c03bf5179781aeb27d725002fb67d8a99992920d3654ac00ee1a2c",
  },
  "jsonrpc": "2.0",
  "id": 1
}
```

**Response**

```
{
  "method": "get_transaction_object",
  "result": {
    "transaction": ...
  }
}
```