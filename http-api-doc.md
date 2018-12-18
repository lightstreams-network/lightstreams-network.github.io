---
layout: page
title:  "API documentation"
---

## Index
- [User Management](#User Management)
- [Wallet](#Wallet)
- [ERC20 Tokens](#ERC20)
- [Storage](#Storage)
- [Error Types](#Error Types)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
## <a name="User Management" />User Management

### Sign up

Base URL
```
https://gateway.lightstreams.network:9091/user/signup
```

Params
- password -> Password used to create the new account

Example request
```bash
curl -X POST \
  https://gateway.lightstreams.network:9091/user/signup \
  -H 'Content-Type: application/json' \
  -d '{"password":"password"}'
```

Output
```json
{
    "account": "0xE41b5C21671C22f1f054F1896d18b4773Dd9deC0"
}
```

> Note: If you are running the HTTPs node on localhost you will likely need to run the curl call using `--insecure`
as follow `curl --insecure localhost:8080/user/signup....`

### Sign in

Base URL
```
https://gateway.lightstreams.network:9091/user/signin
```

Params
- account -> Account address
- password -> Account password

Example request
```bash
curl -X POST \
  https://gateway.lightstreams.network:9091/user/signin \
  -H 'Content-Type: application/json' \
  -d '{"account":"E41b5C21671C22f1f054F1896d18b4773Dd9deC0","password":"password"}'
```

Output
```
{
    "token": "eyJibG9ja2NoYWluIjoiRVRIIiwiZXRoX2FkZHJlc3MiOiIweDRGNWFERWRDYTZkODY5RTlGNUY3ZENmNEI3QTlkRmE4MjMxYTA5NWYiLCJpYXQiOjI1LCJlYXQiOjEwMjV9.78mSE4Z9SiHO9fcY5vtCZpK-rdDGuJXpW4qEOzyH9-Zy5HySKGVH9aB-j_ixUb0q91S-I9_Cktj_OVl0LcRvmgE",
}
```

## <a name="Wallet" />Wallet

### Get balance

Base URL
```
https://gateway.lightstreams.network:9091/wallet/balance
```

Params
- account -> Account address

Example request
```bash
curl -X GET \
  'https://gateway.lightstreams.network:9091/wallet/balance?account=0x0dd46808e9780e4a23dd562962300ba029bcffef'
```

Output
```json
{
    "balance": "100000000000000000000",
}
```


### Send funds

Base URL
```
https://gateway.lightstreams.network:9091/wallet/transfer
```

Params
- from -> Source account address
- password -> Source account password
- to -> Destination account address
- amount_wei -> Amount of wei to transfer

Example request
```bash
curl -X POST \
  https://gateway.lightstreams.network:9091/wallet/transfer \
  -H 'Content-Type: application/json' \
  -d '{"from":"0x50c9406f0942711b6e2e28301CE86bFbF42eBE3F","password":"password","to":"E41b5C21671C22f1f054F1896d18b4773Dd9deC0","amount_wei":"1000000000000000000"}'
```

Output
```json
{
    "balance": "99000000000000000000",
}
```

## <a name="Storage" />Storage

### Add new file

Base URL
```
https://gateway.lightstreams.network:9091/storage/add
```

Params
- owner -> Account address who owns the file
- password -> Account's password
- file -> Path of file to upload

Example request
```bash
curl -X POST \
  https://gateway.lightstreams.network:9091/storage/add \
  -H 'Content-Type: multipart/form-data' \
  -F owner=4f5adedca6d869e9f5f7dcf4b7a9dfa8231a095f \
  -F password=password \
  -F file="@/tmp/shared-pic.png"
```

Output
```json
{
    "meta": "QmceeUk5pbXoRRuZs7syD7jtUosPH2K6wuQaWCFi8ba2dc",
    "acl": "0x5307C0F1146233884B3A9BC857738d8bDe0802E4"
}
```

### Grant permissions

Base URL
```
https://gateway.lightstreams.network:9091/acl/grant
```
Params
- acl -> ACL address of stored file
- owner -> Account address of file's owner
- password -> Password of owner's account
- to -> Account address to grant permissions
- permission -> Permissions to be granted (`read`, `write`, `admin`)


Example request
```bash
curl -X POST \
  https://gateway.lightstreams.network:9091/acl/grant \
  -H 'Content-Type: application/json' \
  -d '{"acl":"0x5307C0F1146233884B3A9BC857738d8bDe0802E4","owner":"4f5adedca6d869e9f5f7dcf4b7a9dfa8231a095f","password":"password","to":"0xE41b5C21671C22f1f054F1896d18b4773Dd9deC0", "permission": "read"}'
```

Output
```json
{
    "is_granted": "true"
}
```

### Fetch file

Base URL
```
https://gateway.lightstreams.network:9091/storage/fetch
```

Params
- token -> Token provided after sign in request
- meta -> Meta address for stored file

Example request
```bash
curl -X GET \
  'https://gateway.lightstreams.network:9091/storage/fetch?meta=QmceeUk5pbXoRRuZs7syD7jtUosPH2K6wuQaWCFi8ba2dc&token=eyJibG9ja2NoYWluIjoiRVRIIiwiZXRoX2FkZHJlc3MiOiIweDRGNWFERWRDYTZkODY5RTlGNUY3ZENmNEI3QTlkRmE4MjMxYTA5NWYiLCJpYXQiOjI1LCJlYXQiOjEwMjV9.78mSE4Z9SiHO9fcY5vtCZpK-rdDGuJXpW4qEOzyH9-Zy5HySKGVH9aB-j_ixUb0q91S-I9_Cktj_OVl0LcRvmgE'
```

Output
```
*** STORE FILE RAW CONTENT ****
```

## <a name="ERC20" />ERC20 Tokens

### Get balance

Base URL
```
https://gateway.lightstreams.network:9091/erc20/balance
```

Params
- account -> Account address to request balance from
- erc20_address -> ERC20 token address

Example request
```bash
curl -X GET \
  'https://gateway.lightstreams.network:9091/erc20/balance?erc20_address=0x7251e7005dba3abb0aee4e772a5ff378a8eea885&account=4f5adedca6d869e9f5f7dcf4b7a9dfa8231a095f'
```

Output
```json
{
    "balance": "100"
}
```

### Purchase tokens

Base URL
```
https://gateway.lightstreams.network:9091/erc20/purchase
```

Params
- erc20_address -> ERC20 token address
- account -> Account address who performs the purchasing action
- password -> Account password
- amount_wei -> Amount of wei send for the purchasing action

Example request
```bash
curl -X POST \
  https://gateway.lightstreams.network:9091/erc20/purchase \
  -H 'Content-Type: application/json' \
  -d '{"erc20_address":"0x7251e7005dba3abb0aee4e772a5ff378a8eea885","password":"password","account":"4f5adedca6d869e9f5f7dcf4b7a9dfa8231a095f","amount_wei":"100000000000000000"}'
```

Output
```json
{
    "balance": "622"
}
```

### Transfer tokens

Base URL
```
https://gateway.lightstreams.network:9091/erc20/transfer
```

Params
- erc20_address -> ERC20 token address
- account -> Source account address
- password -> Source account password
- to -> Destination account address
- amount -> Amount of tokens sent

Example request
```bash
curl -X POST \
  https://gateway.lightstreams.network:9091/erc20/transfer \
  -H 'Content-Type: application/json' \
  -d '{"erc20_address":"0x7251e7005dba3abb0aee4e772a5ff378a8eea885","account":"4f5adedca6d869e9f5f7dcf4b7a9dfa8231a095f","password":"password","to":"0xE41b5C21671C22f1f054F1896d18b4773Dd9deC0","amount":"10"}'
```

Output
```json
{
    "balance": "612"
}
```

## <a name="Error Types" />Error Types

### HTTP status code summary

```
200 - OK                Everything worked as expected.
400 - Bad Request       Sent parameters were invalid.
403 - Unauthorized      Not authorized request or invalid API token.
500 - Server Error      Something went wrong on server side.
```
