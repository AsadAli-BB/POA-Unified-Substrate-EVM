# CloudWalk Network Node 

<p align="center">
  <img src="/docs/media/cloudwalk-cover.png">
</p>

![example branch parameter](https://github.com/cloudwalk/cloudwalk-network/actions/workflows/test.yml/badge.svg?branch=master)
![example branch parameter](https://github.com/cloudwalk/cloudwalk-network/actions/workflows/check.yml/badge.svg?branch=master)

A [FRAME](https://substrate.dev/docs/en/next/conceptual/runtime/frame)-based PoA network with Ethereum-like capabilities.

Current release `cloudwalk-network-v1.0.2` using
- [Substrate](https://github.com/paritytech/substrate): `polkadot-v0.9.24`
- [Frontier](https://github.com/paritytech/frontier): `polkadot-v0.9.24`

## Build & Run

To build the chain, execute the following commands from the project root:

```
$ cargo build --release
```

To execute the chain, run:

```
$ ./target/debug/cloudwalk-network-node --dev
```

### Docker Based Development

Optionally, You can build and run the frontier node within Docker directly.  
The Dockerfile is optimized for development speed.  
(Running the `docker run...` command will recompile the binaries but not the dependencies)

Building (takes 5-10 min):

```bash
docker build -t cloudwalk-network-node-dev .
```

Running (takes 1 min to rebuild binaries):

```bash
docker run -t cloudwalk-network-node-dev
```

## Genesis Configuration

The development [chain spec](node/src/chain_spec.rs) included with this project defines a genesis
block that has been pre-configured with an EVM account for
[Alice](https://substrate.dev/docs/en/knowledgebase/integrate/subkey#well-known-keys). When
[a development chain is started](https://github.com/substrate-developer-hub/substrate-node-template#run),
Alice's EVM account will be funded with a large amount of Ether. The
[Polkadot UI](https://polkadot.js.org/apps/#?rpc=ws://127.0.0.1:9944) can be used to see the details
of Alice's EVM account. In order to view an EVM account, use the `Developer` tab of the Polkadot UI
`Settings` app to define the EVM `Account` type as below. It is also necessary to define the
`Address` and `LookupSource` to send transaction, and `Transaction` and `Signature` to be able to
inspect blocks:

```json
{
	"Address": "MultiAddress",
	"LookupSource": "MultiAddress",
	"Account": {
		"nonce": "U256",
		"balance": "U256"
	},
	"Transaction": {
		"nonce": "U256",
		"action": "String",
		"gas_price": "u64",
		"gas_limit": "u64",
		"value": "U256",
		"input": "Vec<u8>",
		"signature": "Signature"
	},
	"Signature": {
		"v": "u64",
		"r": "H256",
		"s": "H256"
	}
}
```

Use the `Developer` app's `RPC calls` tab to query `eth > getBalance(address, number)` with Alice's
EVM account ID (`0xd43593c715fdd31c61141abd04a99fd6822c8558`); the value that is returned should be:

```text
x: eth.getBalance
340,282,366,920,938,463,463,374,607,431,768,211,455
```

> Further reading:
> [EVM accounts](https://github.com/danforbes/danforbes/blob/master/writings/eth-dev.md#Accounts)
