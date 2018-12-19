---
layout: default
title: "Try-out our HTTPs Gateway"
---

<br>

> IMPORTANT: Be aware that we are currently working on the development
of this API and therefore the environment might suffer short outages and
data could be removed without beforehand notice.

# Introduction

In order to facilitate the onboarding of new developers to our
[lightstreams node](/getting-started-with-lightstreams-node) we provide an open
**API Gateway** to interact with it via HTTPs protocol. The vast majority
of the functionalities implemented on lightstreams node is available
using our api.

The **API Gateway** aims to provide an additional layer of abstraction on the
[lightstreams node](/getting-started-with-lightstreams-node) to simplify
Ethereum Blockchain and IPFS protocol functionalities such as upload
new file to lightstreams IPFS network and create ethereum accounts and transfer
funds among them.

Every transaction is executed and validated over `rinkeby` network. IPFS content
is stored and protected by lightstreams authorization protocol,
and uploaded content will be only accessible by lightstreams nodes.

Leth Gateway is running on the following domain
```
https://gateway.lightstreams.network:9091
```

## How to use it

### Create an account
Example of CURL request to create a new account:
```
curl -X POST https://gateway.lightstreams.network:9091/user/signup \
  -H 'Content-Type: application/json' -d '{"password":"password"}'
```

Visit the following [link](/http-api-doc) to learn how about every
available entrypoint.

## Community

If you have any questions or requests related to our Gateway we will
happy to help in our public [forum](https://discuss.lightstreams.network/c/sdk)
