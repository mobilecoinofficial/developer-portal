---
title: Update Account Name
---
Rename an account.

## Parameters

| Required Param | Purpose | Requirements |
| -------------- | ------- | ------------ |
| `account_id` | The account on which to perform this action. | Account must exist in the wallet. |
| `name` | The new name for this account. |  |

## Example

**Request Body**

```
{
  "method": "update_account_name",
  "params": {
    "acount_id": "3407fbbc250799f5ce9089658380c5fe152403643a525f581f359917d8d59d52",
    "name": "Carol"
  },
  "jsonrpc": "2.0",
  "id": 1
}
```

**Response**

```
{
  "method": "remove_account",
  "result": {
    "removed": true
  },
  "error": null,
  "jsonrpc": "2.0",
  "id": 1
}
```