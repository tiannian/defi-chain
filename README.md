# findora-defi-chain

## Build project

``` shell
$ cargo build --release
```

## Features

- Use `ChainBridge` to cross ETH and `Findora Defi Chain`'s ETH.
- Use `Findora Defi Chain`'s pallet to cross Findora's assets.

## Related project

- [compound-finance/gateway](https://github.com/compound-finance/gateway)
- [frontier](https://github.com/paritytech/frontier)
- [substrate](https://substrate.dev/)
- [chainbridge](https://github.com/ChainSafe/ChainBridge)
- [Aurora](https://near.org/zh/blog/aurora-launches-near/)
- [SputnikVM](https://github.com/rust-blockchain/evm)

## Plan

Step 1 (Demo & MVP):

```
Ethereneum <-> Findora Defi Chain(Substrate based, ETH compact) <-> Findora
            ^                                                    ^
            |                                                    |
        ChainBridge                                         Bridge (need develop)
```

Step 2:

```
Ethereneum <-> Findora Defi Chain(Substrate based, ETH compact) <-> Findora
            ^                                                    ^
            |                                                    |
Crosschain(by substrate pallet and contracts)               Crosschain(by substrate pallet)
```

## Plan of Demo (WIP)

1. Findora Defi Chain (A ETH compact chain based on the substrate (frontier)).
2. Deploy ETH/ERC20/ERC721 (contracts) in ETH testnet.
3. Use `ChainBridge` to cross these assets.
4. Support vanilla ETH ? (It seems support. Map ETH on A chain to ERC20 on B Chain)
5. Map FRA to Findora Defi Chain in ERC20 by js quickly.

### Findora Defi Chain Demo

[frontier](https://github.com/paritytech/frontier)

Verify this chain can use all tools for ETH.
- [X] Basic Transfer.
- [X] Smart Contract.
- [X] ERC20/ERC721 Token.

## Plan of Step 2:

### Findora Defi Chain

This chain based on frontier or substrate.

1. Totally compact ETH.
2. Crosschain bridge in decentralized on Findora Defi Chain.
2. Write a pallet to cross ETH. Reference: [compound gateway](https://github.com/compound-finance/gateway)
3. Write a pallet to cross Findora.
    - [ ] Add basic Event for transaction on finadora.
    - [ ] Add lock account support on finadora.
    - [ ] Bridge.

## Step for Demo.

### ERC20 & ERC721

> Token on rinkeby.

```bash
https://rinkeby.etherscan.io/token/0xaa4fb0541d18aa4b0b89eb706263ea9c56589698
```

### Start environment

#### ChainBridge

Use this command to deploy smart contract on ETH. We use Rinkeby testnet.

``` bash
./index.js --url https://rinkeby-light.eth.linkpool.io/ --privateKey $PRIVATE_KEY --gasPrice deploy --all --relayers 0xE2B23454bEFe73Cf3e74cE9533F5FfFA091094a5 --relayerThreshold 1 --chainId 0
```

Output:

``` bash
Deploying contracts...
✓ Bridge contract deployed
✓ ERC20Handler contract deployed
✓ ERC721Handler contract deployed
✓ GenericHandler contract deployed
✓ ERC20 contract deployed
WARNING: Multiple definitions for safeTransferFrom
✓ ERC721 contract deployed
✓ CentrifugeAssetStore contract deployed

================================================================
Url:        https://rinkeby-light.eth.linkpool.io/
Deployer:   0xE2B23454bEFe73Cf3e74cE9533F5FfFA091094a5
Gas Limit:   8000000
Gas Price:   10000000000
Deploy Cost: 0.15561065

Options
=======
Chain Id:    0
Threshold:   1
Relayers:    0xE2B23454bEFe73Cf3e74cE9533F5FfFA091094a5
Bridge Fee:  0
Expiry:      100

Contract Addresses
================================================================
Bridge:             0xB7223605039dD9BFAE528B71eA7157725b9d8416
----------------------------------------------------------------
Erc20 Handler:      0x5F88E272fd66B4182A1705B6Ca51BC4658e00E0A
----------------------------------------------------------------
Erc721 Handler:     0xBc55db4d86b1CaBD46901A81D832fd0ce001d939
----------------------------------------------------------------
Generic Handler:    0xa5d10fA7aB8EC1535C135F88E6d477c994388B1A
----------------------------------------------------------------
Erc20:              0x14F92974D3980D74cf64BF83bc9f2c4b420e1A10
----------------------------------------------------------------
Erc721:             0xd73BE42678b94Cb75e8Bb840b2df1BEe1a26320A
----------------------------------------------------------------
Centrifuge Asset:   0xAed12A4f43ca3B97f234c61D7d637cC1534C396b
================================================================
```

Then configure contracts on ETH.

``` bash
cb-sol-cli --url https://rinkeby-light.eth.linkpool.io/ --privateKey 9fab3886bdf3281b8ace6957efb19ddd5bb6d32416ec408e6b0ce34e3a6eb732 --gasPrice 10000000000 bridge register-resource --bridge 0xB7223605039dD9BFAE528B71eA7157725b9d8416 --handler 0x5F88E272fd66B4182A1705B6Ca51BC4658e00E0A --resourceId 0x000000000000000000000000000000c76ebe4a02bbc34786d860b355f5a5ce00 --targetContract 0xaa4fb0541d18aa4b0b89eb706263ea9c56589698
```

Deploy chainbridge contract on FDC.

``` bash
cb-sol-cli --url http://127.0.0.1:9933 --privateKey 9fab3886bdf3281b8ace6957efb19ddd5bb6d32416ec408e6b0ce34e3a6eb732 --gasPrice 10000000000 deploy --all --relayers 0xE2B23454bEFe73Cf3e74cE9533F5FfFA091094a5 --relayerThreshold 1 --chainId 1
```
Output:

``` bash
✓ Bridge contract deployed
✓ ERC20Handler contract deployed
✓ ERC721Handler contract deployed
✓ GenericHandler contract deployed
✓ ERC20 contract deployed
WARNING: Multiple definitions for safeTransferFrom
✓ ERC721 contract deployed
✓ CentrifugeAssetStore contract deployed

================================================================
Url:        http://127.0.0.1:9933
Deployer:   0xE2B23454bEFe73Cf3e74cE9533F5FfFA091094a5
Gas Limit:   8000000
Gas Price:   10000000000
Deploy Cost: 0.15526877

Options
=======
Chain Id:    1
Threshold:   1
Relayers:    0xE2B23454bEFe73Cf3e74cE9533F5FfFA091094a5
Bridge Fee:  0
Expiry:      100

Contract Addresses
================================================================
Bridge:             0x60dF29E1ACD488919341c63FA2BfB7F05Daa7C7A
----------------------------------------------------------------
Erc20 Handler:      0xB7223605039dD9BFAE528B71eA7157725b9d8416
----------------------------------------------------------------
Erc721 Handler:     0x5F88E272fd66B4182A1705B6Ca51BC4658e00E0A
----------------------------------------------------------------
Generic Handler:    0xBc55db4d86b1CaBD46901A81D832fd0ce001d939
----------------------------------------------------------------
Erc20:              0xa5d10fA7aB8EC1535C135F88E6d477c994388B1A
----------------------------------------------------------------
Erc721:             0x14F92974D3980D74cf64BF83bc9f2c4b420e1A10
----------------------------------------------------------------
Centrifuge Asset:   0xd73BE42678b94Cb75e8Bb840b2df1BEe1a26320A
================================================================
```

Configure bridge

```bash
cb-sol-cli --url http://127.0.0.1:9933 --privateKey 9fab3886bdf3281b8ace6957efb19ddd5bb6d32416ec408e6b0ce34e3a6eb732 --gasPrice 10000000000 bridge register-resource \
    --bridge 0x60dF29E1ACD488919341c63FA2BfB7F05Daa7C7A \
    --handler 0xB7223605039dD9BFAE528B71eA7157725b9d8416 \
    --resourceId 0x000000000000000000000000000000c76ebe4a02bbc34786d860b355f5a5ce00 \
    --targetContract 0xa5d10fA7aB8EC1535C135F88E6d477c994388B1A
```

Set burn

``` bash
cb-sol-cli --url http://127.0.0.1:9933 --privateKey 9fab3886bdf3281b8ace6957efb19ddd5bb6d32416ec408e6b0ce34e3a6eb732 --gasPrice 10000000000 bridge set-burn \
    --bridge 0x60dF29E1ACD488919341c63FA2BfB7F05Daa7C7A \
    --handler 0xB7223605039dD9BFAE528B71eA7157725b9d8416 \
    --tokenContract 0xa5d10fA7aB8EC1535C135F88E6d477c994388B1A
```

Set mint

``` bash
cb-sol-cli --url http://127.0.0.1:9933 --privateKey 9fab3886bdf3281b8ace6957efb19ddd5bb6d32416ec408e6b0ce34e3a6eb732 --gasPrice 10000000000 erc20 add-minter \
    --minter 0xB7223605039dD9BFAE528B71eA7157725b9d8416 \
    --erc20Address 0xa5d10fA7aB8EC1535C135F88E6d477c994388B1A
```

Configure and start chainbridge

Approve token:

``` bash
cb-sol-cli --url https://rinkeby-light.eth.linkpool.io/ --privateKey 9fab3886bdf3281b8ace6957efb19ddd5bb6d32416ec408e6b0ce34e3a6eb732 --gasPrice 10000000000 erc20 approve \
    --amount 100 \
    --erc20Address 0xaa4fb0541d18aa4b0b89eb706263ea9c56589698 \
    --recipient 0x5F88E272fd66B4182A1705B6Ca51BC4658e00E0A
```

Deposit token

``` bash
cb-sol-cli --url https://rinkeby-light.eth.linkpool.io/ --privateKey 9fab3886bdf3281b8ace6957efb19ddd5bb6d32416ec408e6b0ce34e3a6eb732 --gasPrice 10000000000 erc20 deposit \
    --amount 100 \
    --dest 1 \
    --bridge 0xB7223605039dD9BFAE528B71eA7157725b9d8416 \
    --recipient 0xA346505ABcD3670812dc8DDCC10535baeAe78541 \
    --resourceId 0x000000000000000000000000000000c76ebe4a02bbc34786d860b355f5a5ce00
```

### Bridge (d)FRA

A plan to bridge Findora and Findora Defi.

In demo step, we need cross dFRA to FRA.

``` bash
[Findora Defi Chain] <--RPC--> Bridge --RPC--> [Findora Main Chain]
        ^                ^               ^
        |                |               |
Smart Contract    ETH compact RPC    Findora RPC
```

