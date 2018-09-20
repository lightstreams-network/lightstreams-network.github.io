---
layout: page
title:  "Getting started with Lightstreams Node"
---

| Version | Release Date |
|---------|--------------|
|v0.0.6 (Alpha)|19.09.2018|

Lightstreams technical stack:

- Lightstreams Node `Leth` (Controller, **SDK**)
- Ethereum Node `Geth` (Rinkeby Test Network)
- IPFS Node `IPFS` (Secure Storage with custom **PermissionedBlocks**)

Lightstreams Node (`Leth`) is an executable binary compiled from our Lightstreams SDK, the souce code that will be publicly available in the near future.

`Leth` is used for initializing, running, controlling and interacting with the Lightstreams Node (Blockchain + Secured InterPlanetary File System).

`Geth` is currently used as a Rinkeby PoA test network instead of our Lightstreams PoA Test Network due to smoother, go to market strategy with release of an ERC20 PHT token.

`IPFS` is a heavily customized decentralised file system enhanced with our award winning **PermissionedBlocks** technology.

## Installing Leth node

### Download MacOS compiled `Leth` and `IPFS` from our AWS S3 Server:

```bash
wget "https://s3.amazonaws.com/lightstreams/leth-osx" -O /usr/local/bin/leth
wget "https://s3.amazonaws.com/lightstreams/ipfs-osx" -O /usr/local/bin/ipfs
```

### Download Liux compiled `Leth` and `IPFS` from our AWS S3 Server:

```bash
wget "https://s3.amazonaws.com/lightstreams/leth-linux-amd64" -O /usr/local/bin/leth
wget "https://s3.amazonaws.com/lightstreams/ipfs-linux-amd64" -O /usr/local/bin/ipfs
```

### Ensure the executables have all the necessary permissions:

```bash
chmod 777 /usr/local/bin/leth && chmod 777 /usr/local/bin/ipfs
```

### Download Geth

Download your OS specific `geth` version 1.8.* from an official [Ethereum downloads page](https://geth.ethereum.org/downloads/).
We recommend to download version: v1.8.15, Khazad-dûm².

Unzip and copy the downloaded `geth` executable as well into `/usr/local/bin/`.

Apply permissions:

```bash
chmod 777 /usr/local/bin/geth
```

## Validate installed binaries

![versions](/public/images/installing_leth_node_version_verification.png)

Perfect, binaries are successfully executable!

## Interacting with Leth node over CLI

Run `leth help` command to display all commands you have in disposition in the current version.

```
Lightstreams CLI for interacting with a configured node.

Usage:
  leth [flags]
  leth [command]

Available Commands:
  acl         Features LethACL's pkg capabilities over CLI such as granting ACL permissions.
  auth        Features LethAuth's pkg capabilities over CLI such as token generation, verification...
  docs        Generates LETH cmd usage docs based on code into the: 'docs/cmd/auto_generated'.
  help        Help about any command
  init        Initializes new LS local node for a chosen network.
  run         Runs full Leth node by spawning blockchain and IPFS daemons.
  storage     Features LethStorage's pkg capabilities over CLI such as file upload/download, access authorization...
  version     Describes version.

Flags:
  -h, --help   help for leth

Use "leth [command] --help" for more information about a command.
```

Display current version:

```bash
leth version
Version: 0.0.5 Alpha
```

Display instructions for initializing new Lightstreams Node:

```bash
leth init --help
Initializes new LS local node for a chosen network.

Usage:
  leth init [flags]

Flags:
  -h, --help             help for init
      --network string   Possible values: 'rinkeby'.
      --nodeid int       ID of the node in order to support multiple nodes on the same machine. 0 by default.
```

## Running Leth node

First, initialize new Leth node with ID 1 for Rinkeby network:

```bash
leth init --nodeid=1 --network=rinkeby

{"msg":"Initializing Leth node...","nodeID":1,"network":"rinkeby"}
{"msg":"IPFS: initializing IPFS node at /Users/enchanterio/.lightstreams_1/ipfs"}
{"msg":"IPFS: generating 2048-bit RSA keypair...done"}
{"msg":"IPFS: peer identity: Qma1bKbQVYqHMhWzaRHAkKU5s5FsDnhR5bMzWLbjwxUaN6"}
{"msg":"IPFS successfully initialized.","dataDir":"/Users/enchanterio/.lightstreams_1/ipfs"}
{"msg":"Rinkeby node successfully initialized.","dataDir":"/Users/enchanterio/.lightstreams_1/rinkeby"}
{"msg":"Leth node fully initialized!!!","nodeDir":"/Users/enchanterio/.lightstreams_1"}
```

Second, run Leth node:

```bash
leth run --nodeid=1 --network=rinkeby

{"msg":"Starting Leth node online services (blockchain, IPFS)..."}
{"msg":"GETH: INFO [09-13|11:30:40.138] Maximum peer count ETH=25 LES=0 total=25"}
{"msg":"GETH: INFO [09-13|11:30:40.148] Starting peer-to-peer node instance=Geth/v1.8.15-stable-89451f1c/darwin-amd64/go1.10.4"}
...
```

```bash
...
{"msg":"GETH: INFO [09-13|11:31:26.189] Imported new block receipts count=906 elapsed=9.521ms   number=960 hash=413833…8d126d size=4.13kB  ignored=0"}
{"msg":"GETH: INFO [09-13|11:31:26.390] Imported new block headers count=384 elapsed=149.265ms number=1344 hash=4524ae…5d3fff ignored=0"}
{"msg":"GETH: INFO [09-13|11:31:26.406] Imported new block receipts count=384 elapsed=2.144ms   number=1344 hash=4524ae…5d3fff size=1.54kB  ignored=0"}
```

Leave it running for few hours until you see your node is fully synced with Rinkeby Test Network:

```bash
GETH: INFO [09-13|10:31:14.044] Imported new chain segment blocks=1 txs=12 mgas=1.134 elapsed=83.860ms mgasps=20.680 number=2980046 hash=1b5fff…3bff4e cache=31.55mB"
```

## Get FREE Testing Tokens

### Obtaining Ether

Connect to the Leth node via IPC:

```bash
geth --datadir=/Users/enchanterio/.lightstreams_1/rinkeby attach ipc:/Users/enchanterio/.lightstreams_1/rinkeby/geth.ipc
```

Create a new Leth node account using the attached Geth JavaScript console and check the its balance:

```javascript
geth --datadir=/Users/enchanterio/.lightstreams_1/rinkeby attach ipc:/Users/enchanterio/.lightstreams_1/rinkeby/geth.ipc

Welcome to the Geth JavaScript console!

Instance: Geth/v1.8.15

> personal.newAccount()
>> ...type your password...
"0xa92e3705e6d70cb45782bf055e41813060e4ce07"
> eth.accounts
["0xa92e3705e6d70cb45782bf055e41813060e4ce07"]
> eth.coinbase
"0xa92e3705e6d70cb45782bf055e41813060e4ce07"
> eth.getBalance(eth.coinbase)
0
>
```

Request FREE ETH from Rinkeby Faucet:

- Publish your Ethereum address (0xa92e3705e6d70cb45782bf055e41813060e4ce07) anywhere in a public social network
- E.g, post your address to my [Google Plus group](https://plus.google.com/u/0/communities/115209806315551990293)
- Open [https://www.rinkeby.io/#faucet](https://www.rinkeby.io/#faucet) and paste the post url
- Wait few seconds, if the blockchain is fully synced, check the balance once again `eth.getBalance(eth.coinbase)` -> 3000000000000000000

### Obtaining Photons

*Todo: Add documentation how to obtain PHTs token in the next release of Lightstreams Faucet!*

## Private File Sharing for dApps, content producers

Once your Leth node is fully synced and you own some ETH, you can access our most important feature!

**Private file sharing**.

Files are protected with ACL and access is authenticated and authorized before any content seeding starts! Not just encrypted publicly in IPFS, what majority of projects do. Privacy vs Confidentiality in action.

### Uploading a private file

Given a file: "secret_file.txt" with content "hello secret world".

To share a private file using `leth` execute the following command:

```bash
leth storage upload --nodeid=1 --network=rinkeby --file=/Users/enchanterio/Documents/secret_file.txt --fileOwnerKeystore=/Users/enchanterio/.lightstreams_1/rinkeby/keystore/UTC--2018-09-13T12-55-28.129616463Z--a92e3705e6d70cb45782bf055e41813060e4ce07
```

Output:

```bash
Enter keystore's password to unlock account:

...some logs, we keep logging on DEBUG level in Alpha version for debugging early bugs

{"hash":"QmZYSewpHNvdW1TTgska792QAT7Yd6yxZAoybpYFskTZSf","aclid":"0xc2DBC8CdAba2df432C821639B80302f0675D6f74","error":""}
```

Note:

- `--nodeid=1; --network=rinkeby` we are executing the command from Leth node 1 and deploying the ACL to file on Rinkeby network
- `--file=` flag is an absolute path to the file you want to share
- `--fileOwnerKeystore=` flag is an absolute path to your Ethereum account key located inside of your Leth directory. If you have multiple accounts created, you will recognise the associated account keystore by the suffix `UTC--<created_at UTC ISO8601>-<your Ethereum account address hex>`
- `"hash":"QmNkbFAo5jSKm7KLCdCr8c8ue2X53ShATD5yjyQq3ynoaf"` this is the address of your private file in a secure IPFS storage
- `"aclid"` this is the smart contract addr controlling file's ACL. You can use it `leth acl grant` cmd to grant permissions to other accounts

PS: You can always run any command with `--help` flag to get explanation of all required/optional flags

```bash
leth acl grant --help
```

### Reading the private file

Now you can tell your friend running different Leth node to download your private file! If you would like to test it yourself, setup another Leth node on the same machine in another terminal, Leth node 2 and create also another account!

```bash
leth init --nodeid=2 --network=rinkeby
leth run --nodeid=2 --network=rinkeby

...wait a few hours for a full sync...
```

Once your Leth node is full synced, attempt to read the private file from your new Leth node 2:

```bash
leth storage read --nodeid=2 --network=rinkeby --ipfsFileHash=QmZYSewpHNvdW1TTgska792QAT7Yd6yxZAoybpYFskTZSf --keystore=/Users/enchanterio/.lightstreams_2/rinkeby/keystore/UTC--2018-10-13T12-55-28.129616463Z--0xnode2ethAddr0cb45782bf055e41813060e4ce89
```

Output:

```bash
{"content":"","error":"unable to download file to IPFS storage. Error: ipfs cat cmd timed out"}
```

This error is expected because the file owner never actually granted permission to Leth node 2 account, 0xnode2ethAddr0cb45782bf055e41813060e4ce89.

Let's grant a read permission.

### Granting read access to the private file

Execute :

```bash
leth acl grant --nodeid=1 --network=rinkeby --permission=read --aclid=0x2F15B633b4bC41BdFBBD8AAf2Be7Dae958D27C7E --keystore=/Users/enchanterio/.lightstreams_1/rinkeby/keystore/UTC--2018-09-13T12-55-28.129616463Z--a92e3705e6d70cb45782bf055e41813060e4ce07 --account=0xnode2ethAddr0cb45782bf055e41813060e4ce89
```

Output:

```bash
{"msg":"Granting 'read' permission to account '0xnode2ethAddr0cb45782bf055e41813060e4ce89'..."}
{"msg":"Account '0xnode2ethAddr0cb45782bf055e41813060e4ce89' was granted 'read' permission."}
{"error":"","is_granted":"true"}
```

Let's try to read the secret_file.txt file now:

```bash
leth storage read --nodeid=2 --network=rinkeby --ipfsFileHash=QmZYSewpHNvdW1TTgska792QAT7Yd6yxZAoybpYFskTZSf --keystore=/Users/enchanterio/.lightstreams_2/rinkeby/keystore/UTC--2018-10-13T12-55-28.129616463Z--0xnode2ethAddr0cb45782bf055e41813060e4ce89
```

Output:

```bash
{"content":"hello secret world\n","error":""}
```

Congratulation, you just shared a private file over internet in a decentralised manner.

## Need help?

We are happy to get you started! Join our [telegram channel](https://t.me/lightstreams) and ask as many questions as you want!
