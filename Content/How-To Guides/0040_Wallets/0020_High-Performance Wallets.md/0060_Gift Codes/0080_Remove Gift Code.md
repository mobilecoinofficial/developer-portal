---
title: Remove Gift Code
---
Remove a gift code from the database.

## Parameters

| Required Param | Purpose | Requirements |
| -------------- | ------- | ------------ |
| `gift_code_b58` | The base58-encoded gift code contents. | Must be a valid b58-encoded gift code. |

## Example

**Request Body**

```
{
  "method": "remove_gift_code",
  "params": {
    "gift_code_b58": "3DkTHXADdEUpRJ5QsrjmYh8WqFdDKkvng126zTP9YQb7LNXL8pbRidCvB7Ba3Mvek5ZZdev8EXNPrJBpGdtvfjk3hew1phmjdkf5mp35mbyvhB8UjRqoJJqDRswLrmKQL",
  },
  "jsonrpc": "2.0",
  "id": 1
}
```

**Response**

```
{
  "method": "remove_gift_code",
  "result": {
    "removed": true
  },
  "error": null,
  "jsonrpc": "2.0",
  "id": 1
}
```