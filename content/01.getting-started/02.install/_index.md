+++
title = "02. Install"
toc = true
+++

## Install

We have compiled binaries for macOS and Linux available (Windows not yet available).

### Install from prebuilt package

#### macOS

```
wget "https://s3.amazonaws.com/lightstreams/leth-osx" -O /usr/local/bin/leth
wget "https://s3.amazonaws.com/lightstreams/ipfs-osx" -O /usr/local/bin/ipfs
wget "https://s3.amazonaws.com/lightstreams/geth-osx" -O /usr/local/bin/geth
```

#### Linux

```
wget "https://s3.amazonaws.com/lightstreams/leth-linux-amd64" -O /usr/local/bin/leth
wget "https://s3.amazonaws.com/lightstreams/ipfs-linux-amd64" -O /usr/local/bin/ipfs
wget "https://s3.amazonaws.com/lightstreams/geth-linux-amd64" -O /usr/local/bin/geth
```

#### Windows

Not yet available.

### Set permissions

```
chmod u+x /usr/local/bin/leth
chmod u+x /usr/local/bin/ipfs
chmod u+x /usr/local/bin/geth
```

### Install with Docker

```
docker run -it -p 9091:9091 --name ls-node1 -d lightstreams/node
```

## Check installation

```
> leth version
Version: 0.1.0-alpha

> ipfs version
ipfs version 0.4.9-rc2

> geth version
Geth
Version: 1.8.20-stable
Git Commit: 24d727b6d6e2c0cde222fa12155c4a6db5caaf2e
Architecture: amd64
Protocol Versions: [63 62]
Network Id: 1
Go Version: go1.10.4
Operating System: linux
GOPATH=/home/a/go
GOROOT=/usr/lib/go-1.10
```

## Run a Lightstreams node

Initialize the node; you need to specify a --nodeid and a --network to connect to (rinkeby or sirius)
```
leth init --nodeid=1 --network=sirius
```

Run the node; you need to specify a --nodeid and a --network to connect to (rinkeby or sirius)
```
leth run --nodeid=1 --network=sirius
```

## Create an account

```
leth user signup --nodeid=1 --network=sirius
```
