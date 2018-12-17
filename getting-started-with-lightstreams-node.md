---
layout: page
title:  "Getting started with Lightstreams Node"
---

| Version | Release Date |
|---------|--------------|
|0.7.0-alpha Meta && PoA Test Ntw|07.12.2018|

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
chmod u+x /usr/local/bin/leth && chmod u+x /usr/local/bin/ipfs
```

### Download Geth

Download your OS specific `geth` version 1.8.* from an official [Ethereum downloads page](https://geth.ethereum.org/downloads/).

Unzip and copy the downloaded `geth` executable as well into `/usr/local/bin/`.

Apply permissions:

```bash
chmod u+x /usr/local/bin/geth
```

## Validate installed binaries

![binaries versions](/public/images/verify_binaries.png)

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
  ...
  ...

Flags:
  -h, --help   help for leth

Use "leth [command] --help" for more information about a command.
```

Display current version:

```bash
leth version
Version: 0.6.0-alpha ACL Admin
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
{"msg":"IPFS: initializing IPFS node at $HOME/.lightstreams_1/ipfs"}
{"msg":"IPFS: generating 2048-bit RSA keypair...done"}
{"msg":"IPFS: peer identity: Qma1bKbQVYqHMhWzaRHAkKU5s5FsDnhR5bMzWLbjwxUaN6"}
{"msg":"IPFS successfully initialized.","dataDir":"$HOME/.lightstreams_1/ipfs"}
{"msg":"Rinkeby node successfully initialized.","dataDir":"$HOME/.lightstreams_1/rinkeby"}
{"msg":"Leth node fully initialized!!!","nodeDir":"$HOME/.lightstreams_1"}
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

### Expose Leth HTTP API

To initialize `leth` over HTTPs protocol you need to follow the next steps. At first you need to use
valid SSL certificates which can be generated, for instance, by running the following bash command:

```bash
mkdir -p /etc/ssl/leth
cd /etc/ssl/leth
openssl req -x509 -out localhost.crt -keyout localhost.key \
  -newkey rsa:2048 -nodes -sha256 \
  -subj '/CN=localhost' -extensions EXT -config <( \
   printf "[dn]\nCN=localhost\n[req]\ndistinguished_name = dn\n[EXT]\nsubjectAltName=DNS:localhost\nkeyUsage=digitalSignature\nextendedKeyUsage=serverAuth")
chmod a+r localhost.crt localhost.key
```

Once you have valid ssl certificates are allocated in your local machine, you have to edit
leth node configuration file, at `$HOME/.lightstreams_1/config.json`, and include the path
for your ssl certificates and the port where the HTTPs server is going to be exposed.

```js
{
	"https_server": {
		"port": 8080,
		"certificate_pem_filepath": "/etc/ssl/leth/localhost.crt",
		"certificate_pem_priv_key_filepath": "/etc/ssl/leth/localhost.key"
	},
....
```

At last, you need to run your `leth` server again, but this time using the `--https` flag as follow:
```bash
leth run --nodeid=1 --network=rinkeby --https
```

Within the first debug output shown in your terminal you will see the next one:
````
...
{"level":"debug","ts":1544091804.1871579,"caller":"http/server.go:28","msg":"Starting Leth HTTP server listening on port: 8080."}
...
````

Visit the following [link](/http-api-doc) to learn how to use leth http api.

## Get FREE Testing Tokens

### Obtaining Ether

Connect to the Leth node via IPC:

```bash
geth --datadir=$HOME/.lightstreams_1/rinkeby attach ipc:$HOME/.lightstreams_1/rinkeby/geth.ipc
```

Create a new Leth node account using the attached Geth JavaScript console and check the its balance:

```javascript
geth --datadir=$HOME/.lightstreams_1/rinkeby attach ipc:$HOME/.lightstreams_1/rinkeby/geth.ipc

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

### Leth storage file

Each file uploaded using Leth SDK generates 2 IPFS files.

1. A public Meta JSON file, accessible by everyone describing the protected file

```json
{
  "ext": "txt",
  "owner": "0xa92e3705e6d70cb45782bf055e41813060e4ce07",
  "hash": "QmZYSewpHNvdW1TTgska792QAT7Yd6yxZAoybpYFskTZSf", // of the protected file
  "acl": "0x5D780255679c55846c1fE1E738e7604425171B50" // smart contract access rules
}
```

2. The protected file itself.

### Adding a private file

Given a file: "secret_file.txt" with content "hello secret world".

To share a private file using `leth` execute the following command:

```bash
leth storage add --nodeid=1 --network=rinkeby --file=$HOME/Documents/secret_file.txt --owner=0xa92e3705e6d70cb45782bf055e41813060e4ce07
```

Output:

```bash
Enter keystore's password to unlock account:

...some logs, we keep logging on DEBUG level in Alpha version for debugging early bugs

{"meta":"QmZYSewpHNvdW1TTgska792QAT7Yd6yxZAoybpYFskTZSf","acl":"0xc2DBC8CdAba2df432C821639B80302f0675D6f74"}
```

Note:

- `--nodeid=1; --network=rinkeby` we are executing the command from Leth node 1 and deploying the ACL to file on Rinkeby network
- `--file=` flag is an absolute path to the file you want to share
- `--owner=` flag is file owner address who will pay for the file ACL. The account address was generated when you signed-up
- `"meta":"QmNkbFAo5jSKm7KLCdCr8c8ue2X53ShATD5yjyQq3ynoaf"` this is the address of a public Meta file linking to your private file in a secure IPFS storage
- `"acl"` this is the file's ACL. A smart contract addr controlling all the access rules. You can use it `leth acl grant` cmd to grant permissions to other accounts

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
leth storage fetch --nodeid=2 --network=rinkeby --meta=QmZYSewpHNvdW1TTgska792QAT7Yd6yxZAoybpYFskTZSf --account=0xnode2ethAddr0cb45782bf055e41813060e4ce89
```

Output:

```bash
{"error": {"code": "err_cli", "message": "unable to download file to IPFS storage. Error: ipfs cat cmd timed out"}
```

This error is expected because the file owner never actually granted permission to Leth node 2 account, 0xnode2ethAddr0cb45782bf055e41813060e4ce89.

Let's grant a read permission.

### Granting read access to the private file

Execute :

```bash
leth acl grant --nodeid=1 --network=rinkeby --permission=read --acl=0x2F15B633b4bC41BdFBBD8AAf2Be7Dae958D27C7E --owner=0xa92e3705e6d70cb45782bf055e41813060e4ce07 --account=0xnode2ethAddr0cb45782bf055e41813060e4ce89
```

Output:

```bash
{"msg":"Granting 'read' permission to account '0xnode2ethAddr0cb45782bf055e41813060e4ce89'..."}
{"msg":"Account '0xnode2ethAddr0cb45782bf055e41813060e4ce89' was granted 'read' permission."}

{"is_granted":"true"}
```

Let's try to read the `secret_file.txt` file now:

```bash
leth storage fetch --nodeid=2 --network=rinkeby --meta=QmZYSewpHNvdW1TTgska792QAT7Yd6yxZAoybpYFskTZSf --account=0xnode2ethAddr0cb45782bf055e41813060e4ce89
```

Leth SDK will resolve the linked protected file out of the JSON Meta file and save it to a tmp directory using the hash of the protected file itself
and the extension of the uploaded file resolved from the JSON Meta file.

Output:

```bash
{"output":"/tmp/QmProtIpfsHashdGXrvCgmQHo8Yqo8eLQTvC1sEJh6suBi.txt"}
```

Congratulation, you just shared a private file over internet in a decentralised manner.

### Granting admin access to the private file

Going step further. You can also grant admin rights to other accounts, devices so they can further have the privileges of granting read/admin access to other users.

Example, let's grant an `admin` right to the Leth Node 2 account. With such a privilege, Leth Node 2 account will be able to further grant access to other devices in the network/users.

```bash
leth acl grant --nodeid=1 --network=rinkeby --permission=admin --acl=0x2F15B633b4bC41BdFBBD8AAf2Be7Dae958D27C7E --owner=0xa92e3705e6d70cb45782bf055e41813060e4ce07 --to=0xnode2ethAddr0cb45782bf055e41813060e4ce89
```

### Reading the public Meta file

In case you want to get information about the privately stored file, you can do so using the `leth storage meta` command.

```bash
leth storage meta --nodeid=1 --network=rinkeby --meta=QmZYSewpHNvdW1TTgska792QAT7Yd6yxZAoybpYFskTZSf
```

Output:

```
{"filename":"secret_file.txt","ext":"txt","owner":"0xadC486F16F003897fb927e22438cb1b820f79879","hash":"QmRnXxBJg3NjXzuTi91iNYcMff4oz4NwjN7fgtBXp2UbG9","acl":"0x3cb99420c7F16f00ef41B5ace9e0C815F3736879"}
```

Note:

- `filename` is the original filename when file was uploaded
- `ext` is the original file extension
- `owner` who uploaded the file
- `hash` the hash of the protected file stored in IPFS (not the public Meta file hash)
- `acl` address of the contract handling file permissions

## SDK Help

If in doubt, you can always run any command with a `--help` flag to show and explain to you all the possible flags and cmd usages.

```bash
leth acl --help
```

## Need human help?

We are happy to get you started! Join our [telegram channel](https://t.me/lightstreams) and ask as many questions as you want!
