# Mini - A minimal Cosmos SDK chain

This repository contains an example of a tiny, but working Cosmos SDK chain.
It uses the least modules possible and is intended to be used as a starting point for building your own chain, without all the boilerplate that other tools generate. It is a simpler version of Cosmos SDK's [simapp](https://github.com/cosmos/cosmos-sdk/tree/main/simapp).

`Minid` uses the **latest** version of the [Cosmos-SDK](https://github.com/cosmos/cosmos-sdk).

## How to use

In addition to learn how to build a chain thanks to `minid`, you can as well directly run `minid`.

### Installation

Install and run `minid`:

```sh
git clone git@github.com:cosmosregistry/chain-minimal.git
cd chain-minimal
make install # install the minid binary
make init # initialize the chain
minid start # start the chain
```

## Useful links

* [Cosmos-SDK Documentation](https://docs.cosmos.network/)

## mini chain sync node (first node)

* copy minid binary file or compile from source code

1. stop minid node ,and `minid snapshots export `
2. cat  snapshot 

```bash
$ minid snapshots list
height: 946 format: 3 chunks: 1  
```

3. dump snapshot `minid snapshots dump 946 3`

 > 946-3.tar.gz

4. copy 946-3.tar.gz to other node

### start second minid (other node)

5. load snapshot

> minid snapshots load 946-3.tar.gz

6. copy minid binary file or compile from source code
  copy `~/.minid/config/genesis.json`

7. add seeds `.minid/config/config.toml`

or

> minid start --p2p.seeds 619a20b42812e4e156795d66af0738a09fafa975@172.17.0.1:26656  |tee -a minid.log

```json
[p2p]
laddr = "tcp://0.0.0.0:26656"
external_address = ""
seeds = "eb24be3ac35037260b91906000606442b0e0c803@192.168.0.182:26656"

```
8. start second minid node

> minid start

```bash
minid config set client chain-id demo
minid config set client keyring-backend test
```

9. add validator

> minid comet show-validator
> minid tx bank send mini1577sjzu9z522tc9jv2hgn2kckztf3cln22m9lr mini1wtnf95x9ywdhv984fdpfs0ya0k074wf3ytkzyf 200000000mini
> minid tx staking create-validator ./validator.json --from mini1wtnf95x9ywdhv984fdpfs0ya0k074wf3ytkzyf

```json
{
 "pubkey": {"@type":"/cosmos.crypto.ed25519.PubKey","key":"xGlA/3aa/9812EwB30rf0PQzgu++qPPHWEp/MT+Vgpo="},
 "amount": "1000000mini",
 "moniker": "myvalidator3",
 "commission-rate": "0.1",
 "commission-max-rate": "0.2",
 "commission-max-change-rate": "0.01",
 "min-self-delegation": "1"
}
                                                                                                                                                                
```

10. start third minid node, and  create-validator

```bash
minid config set client chain-id demo
minid config set client keyring-backend test
```

> minid start --p2p.seeds ddde1ff60f2e7a82d6aaf3512f91919662605960@172.17.0.2:26656,619a20b42812e4e156795d66af0738a09fafa975@172.17.0.1:26656  |tee -a minid.log
> minid start --p2p.seeds 619a20b42812e4e156795d66af0738a09fafa975@172.17.0.1:26656,a5e695453ea30aa4af9fb61609b330f28ce45ca8@172.17.0.3:26656  |tee -a minid.log
> minid start --p2p.seeds ddde1ff60f2e7a82d6aaf3512f91919662605960@172.17.0.2:26656,a5e695453ea30aa4af9fb61609b330f28ce45ca8@172.17.0.3:26656  |tee -a minid.log


* At least three nodes are started, three validator are staking, three nodes are connected (by p2p.seeds), if one of nodes is down, the other two nodes are available