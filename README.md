# WeaveChain

WeaveChain is a [CosmWasm](https://cosmwasm.com/)-based smartcontract network on [AO](https://cookbook_ao.g8way.io/).

## AO

AO is a hyper parallel computation protocol on top of [Arweave](https://arweave.org/). It runs any number of Wasm-based smart contract processes in parallel with [actor model](https://en.wikipedia.org/wiki/Actor_model) messaging mechanism.

- [AO Specification](https://hackmd.io/@YTnQkIXiSgyoU2Gnfq6BGg/r1Y3oDrda#About-this-Specification)

## Actor Model

CosmWasm is [actor model smart contract](https://book.cosmwasm.com/actor-model.html) initially built for, but not limited to, CosmosSDK blockchains. Actor model is the only viable way to hyper scale computation in decentralized networks and AO and CosmWasm turned out to be a perfect duo.

## Set up Local Nodes

To test it out, [ArLocal](https://github.com/textury/arlocal) and 3 [AO units](https://cookbook_ao.g8way.io/concepts/units.html) (MU, CU, SU) need to be running locally.

```bash
git clone https://github.com/weavedb/weavechain.git
cd weavechain/ao
yarn
yarn start
```

- Arweave Testnet (ArLocal) : [http://localhost:1984](http://localhost:1984)
- Messenger Unit (MU) : [http://localhost:1985](http://localhost:1985)
- Scheduler Unit (SU) : [http://localhost:1986](http://localhost:1986)
- Compute Unit (CU) : [http://localhost:1987](http://localhost:1987)

## Write CosmWasm Contract
Follow this [tutorial](https://book.cosmwasm.com/basics/rust-project.html) to write some CosmWasm contracts.

And compile it with the following command.

```bash
cargo build --target wasm32-unknown-unknown --release
```
The binary file to deploy will be at `target/wasm32-unknown-unknown/release/project_name.wasm`.
 
## CosmWasm AO SDK

### Installing SDK

```bash
yarn add cwao
```

### Using SDK in Node Script

```javascript
const CWAO = require("cwao")

// wallet = Arweave wallet jwk loaded with enough $AR
const cwao = new CWAO({ wallet })

// get module binary
const module_binary = require("fs").readFileSync(module_binary_file_path)

// deploy contract (module = CosmWasm contract binary)
const module_txid = await cwao.deploy(module_binary)

// get scheduler address for the process
const scheduler_address = await cwao.arweave.wallets.jwkToAddress(wallet)

// instantiate contract
const process = await cwao.instantiate({
  Module: module_txid,
  Scheduler: scheduler_address,
  input: { num: 1 },
})

// execute contract
await cwao.execute({ Process: process.id, func: "Add", input: { num: 2 } })

// query contract
const state = await cwao.query(process.id)

```
