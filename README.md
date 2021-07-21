# Habarigt

# Diaspora Transfer API documentation

Payment feature that allows you make payments to anyone in the world.

#### Contents

- [Overview](#1-overview)
- [Resources](#2-resources)
  - [Routes](#21-routes)
  - [Lookup](#22-lookup)
  - [Transfer](#23-transfer)

## 1. Overview

This API is a JSON-based OAuth2 API. All requests are made to endpoints beginning:
`https://api.habarigt.com/v1`

All requests must be secure, i.e. `https`, not `http`.

## 2. Resources

The API is RESTful and arranged around resources. All requests must be made using `https`.

Typically, the first request you make should be to lookup the bank account. This will confirm that your detail entered is valid, and give you a success reponse that you will need for subsequent requests.

### 2.1. Routes
| Method               | HTTP request      | Description                            |
| ---------------------|-------------------|----------------------------------------|
| POST                 | POST /lookup/     | Submits the bank lookup details        |
| POST                 | POST /transfer/   | Submit the transfer request            |
| POST                 | POST /query/:ref  | Submit the transaction reference ID    |


### 2.2. Lookup
Submitting the entered account details and returns details of the bank account in the lookup.

#### Parameters
bank <sup>`REQUIRED`</sup>
Input recipient bank name intended to be collected by this payment. (e.g, bank xyz).

country_code <sup>`REQUIRED`</sup>
Enter the country code of the bank account to be searched for. (e.g, 00001)

account_name <sup>`REQUIRED`</sup>
Enter the recipient account name (e.g, John Doe).

Where required information is:

| Field                | Type   | Description                                     |
| ---------------------|--------|-------------------------------------------------|
| bank                 | string | A bank name to search for                       |
| country_code         | string | The recipient bank country code                 |
| account_name         | string | The recipient account name                      |

```
POST https://api.habarigt.com/v1/lookup
```

Example request:

```
POST /v1/lookup HTTP/1.1
Host: api.habarigt.com
Content-Type: application/json
Accept: application/json
Accept-Charset: utf-8
```

The response is an object within a data envelope.

Example success response:

```
{
  HTTP/1.1 200 OK
  Content-Type: application/json; charset=utf-8
  message: success
{
  "data": {
    "bank_name": 'bank xyz',
    "bank_code": '00001',
    "account_name": 'john doe'
  }
}
}
```

Example error response:

```
{
  HTTP/1.1 400
  Content-Type: application/json; charset=utf-8
  message: invalid bank name
{
  "data": []
  }
}
```

Possible errors:

| Error               | Description                  |
| --------------------|------------------------------|
| 400 (Bad request)   | The `bank_name ` is invalid. |


### 2.3. Transfer
Transferring to the submitted bank account in the lookup.

```
POST https://api.habarigt.com/v1/transfer
```

Example transfer request:

```
POST /v1/transfer HTTP/1.1
Host: api.habarigt.com
Content-Type: application/json
Accept: application/json
Accept-Charset: utf-8
```

The response is an object within a data envelope.

Example successful transfer response:

```
{
  HTTP/1.1 200 OK
  Content-Type: application/json; charset=utf-8
  message: transfer successful
{
  "data": {
    "bank_name": 'bank xyz',
    "sort_code": '00001',
    "amount": '10000'
  }
}
}
```

Example error response:

```
{
  HTTP/1.1 400
  success: false
  Content-Type: application/json; charset=utf-8
  message: invalid bank name
{
  "data": []
  }
}
```

Possible errors:

| Error               | Description                  |
| --------------------|------------------------------|
| 400 (Bad request)   | The `bank_name ` is invalid. |



===========================
```
POST https://api.habarigt.com/v1/query/:ref
```

Example request:

```
POST /v1/query/:ref HTTP/1.1
Host: api.habarigt.com
Content-Type: application/json
Accept: application/json
Accept-Charset: utf-8
```

The response is an object within a data envelope.
