<!--
---uint256 _id)

MethodID: 0x40c10f19
[0]:  000000000000000000000000098cfce9ee4f12c3e9f605935c99f7d94b8e25c9
[1]:  0000000000000000000000000000000000000000000000000000000000015c3f
comments: true
---
-->

!!! info "Transitioning to POL"

    Polygon network is transitioning from MATIC to POL, which will serve as the gas and staking token on Polygon PoS. Use the links below to learn more:

    - [Migrate from MATIC to POL](../get-started/matic-to-pol.md)
    - [POL token specs](../concepts/tokens/pol.md)

## Overview

All your favorite Ethereum tools (Foundry, Remix, Web3.js) work seamlessly on Polygon, with the same familiar UX. Just switch to the [Polygon RPC](https://polygon-rpc.com/) and keep building.

Connect your wallet and deploy any decentralized application to either PoS mainnet or Amoy testnet (Sepolia-anchored).

Use the links below to find the right tooling and guides that suit your needs the best.

- [Faucets](../../tools/gas/matic-faucet.md) - Fetch test tokens
- [Polygon gas station](../../tools/gas/polygon-gas-station.md) - Gas estimation API
- [Polygon dApp Launchpad](../../tools/dApp-development/launchpad/intro.md) - dApp development CLI tool
- [Popular third-party tooling](../../tools/dApp-development/third-party-tutorials.md)
- [Matic.js library](../../tools/matic-js/get-started.md)

If you have no prior experience in dApp development, the following resources will help you get started with some essential tools for building, testing, and deploying applications on Polygon PoS.

- [Full Stack dApp: Tutorial Series](https://kauri.io/full-stack-dapp-tutorial-series/5b8e401ee727370001c942e3/c)
- [Web3.js](https://www.dappuniversity.com/articles/web3-js-intro)
- [Ethers.js](https://docs.ethers.io/v5/)
- [thirdweb](https://portal.thirdweb.com)
- [Remix](https://remix.ethereum.org/)
- [Hardhat](https://hardhat.org/hardhat-runner/docs/getting-started)
- [Foundry](https://github.com/foundry-rs/foundry/blob/master/README.md)
- [Metamask](https://support.metamask.io/getting-started/)
- [Venly (previously Arkane)](https://docs.venly.io/docs/getting-started-with-venly)
- [Develop a dApp using Fauna, Polygon, and React](https://github.com/hello-ashleyintech/polygon-fauna-app)


## Network details

To access network-related details, including the chain ID, RPC URL, and more, for both the mainnet and Amoy testnet, refer to the [network documentation](../reference/rpc-endpoints.md).

## Wallets

You'll need an Ethereum-based wallet to interact with Polygon because the network runs on Ethereum Virtual Machine (EVM). You can choose to set up a [MetaMask](https://support.metamask.io/getting-started/getting-started-with-metamask/), or [Venly](../../tools/wallets/venly/create-wallet.md) (Arkane) Wallet. 

There are several other third-party wallet options available to choose from, and you'll find them listed out on [this page here](../../tools/wallets/getting-started.md).

!!! tip "Set up web3 provider"

    Refer to the following guides and follow along to set up your wallet for making web3 function calls:
    
    - [Venly](../../tools/wallets/venly/index.md)

## Common tasks

Token bridging between Polygon PoS and Ethereum and vice-versa, and inter-layer communication are basic and essential actions that most dApps need to perform. Use the links below to navigate to guides that'll help you get started with these tasks.
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

import "./TransparentUpgradeableProxy.sol";
import "../../access/Ownable.sol";

/**
 * @dev This is an auxiliary contract meant to be assigned as the admin of a {TransparentUpgradeableProxy}. For an
 * explanation of why you would want to use this see the documentation for {TransparentUpgradeableProxy}.
 */
contract ProxyAdmin is Ownable {

    /**
     * @dev Returns the current implementation of `proxy`.
     *
     * Requirements:
     *
     * - This contract must be the admin of `proxy`.
     */
    function getProxyImplementation(TransparentUpgradeableProxy proxy) public view virtual returns (address) {
        // We need to manually run the static call since the getter cannot be flagged as view
        // bytes4(keccak256("implementation()")) == 0x5c60da1b
        (bool success, bytes memory returndata) = address(proxy).staticcall(hex"5c60da1b");
        require(success);
        return abi.decode(returndata, (address));
    }

    /**
     * @dev Returns the current admin of `proxy`.
     *
     * Requirements:
     *
     * - This contract must be the admin of `proxy`.
     */
    function getProxyAdmin(TransparentUpgradeableProxy proxy) public view virtual returns (address) {
        // We need to manually run the static call since the getter cannot be flagged as view
        // bytes4(keccak256("admin()")) == 0xf851a440
        (bool success, bytes memory returndata) = address(proxy).staticcall(hex"f851a440");
        require(success);
        return abi.decode(returndata, (address));
    }

    /**
     * @dev Changes the admin of `proxy` to `newAdmin`.
     *
     * Requirements:
     *
     * - This contract must be the current admin of `proxy`.
     */
    function changeProxyAdmin(TransparentUpgradeableProxy proxy, address newAdmin) public virtual onlyOwner {
        proxy.changeAdmin(newAdmin);
    }

    /**
     * @dev Upgrades `proxy` to `implementation`. See {TransparentUpgradeableProxy-upgradeTo}.
     *
     * Requirements:
     *
     * - This contract must be the admin of `proxy`.
     */
    function upgrade(TransparentUpgradeableProxy proxy, address implementation) public virtual onlyOwner {
        proxy.upgradeTo(implementation);
    }

    /**
     * @dev Upgrades `proxy` to `implementation` and calls a function on the new implementation. See
     * {TransparentUpgradeableProxy-upgradeToAndCall}.
     *
     * Requirements:
     *
     * - This contract must be the admin of `proxy`.
     */
    function upgradeAndCall(TransparentUpgradeableProxy proxy, address implementation, bytes memory data) public payable virtual onlyOwner {
        proxy.upgradeToAndCall{value: msg.value}(implementation, data);
    }
}mint(address _to,uint256 _id)

MethodID: 0x40c10f19
[0]:  000000000000000000000000098cfce9ee4f12c3e9f605935c99f7d94b8e25c9
[1]:  0000000000000000000000000000000000000000000000000000000000015c3fhttps://polygonscan.com/token/0x3c499c542cef5e3811e1192ce70d8cc03d5c3359?a=0x4cfb9604f3297dde00f83756651325cc2897ddd2#code#F1#L1
* [Bridge tokens from Ethereum to PoS](../how-to/bridging/ethereum-polygon/ethereum-to-matic.md)
* [Bridge tokens from PoS to Ethereum](../how-to/bridging/ethereum-polygon/matic-to-ethereum.md)
* [L1 - L2 communication and state transfer](../how-to/bridging/l1-l2-communication/state-transfer.md)

## Connecting to Polygon

You can add Polygon to MetaMask, or directly use Venly (Arkane), which allows you to connect to Polygon using RPC.

In order to connect with the Polygon PoS network to read blockchain information, you can use a node provider like Alchemy SDK.

```js
// Javascript
// Setup: npm install alchemy-sdk
const { Alchemy, Network } = require("alchemy-sdk");

const settings = {
  apiKey: "demo", // Replace with your API Key from https://www.alchemy.com
  network: Network.MATIC_MAINNET, // Replace with MATIC_AMOY for testnet config
};

const alchemy = new Alchemy(settings);

async function main() {
  const latestBlock = await alchemy.core.getBlockNumber();
  console.log("The latest block number is", latestBlock);
}

main();
```

!!! tip "Reach out to us!"

    If you're encountering problems while hacking or have questions about something, please use the following methods to contact us:

    1. If you come across a complex code repository, 404s on the docs site, or if you feel there's something missing - feel free to [open an issue on the Polygon Knowledge Layer's GitHub repository](https://github.com/0xPolygon/polygon-docs/issues). You can also open a PR if you're looking to [contribute](https://github.com/0xPolygon/polygon-docs?tab=readme-ov-file#contributing)!
    2. Get in touch with us via Discord:
        - [Community Discord](https://discord.com/invite/0xPolygonCommunity)
        - [Research and Development Discord](https://discord.com/invite/0xpolygonrnd)

## Already have a dApp?

If you already have a decentralized application (dApp) and are looking for a platform to help you scale efficiently, then you are at the right place because Polygon allows you to:

1. Easily migrate from Ethereum Virtual Machine (EVM) based chain: Polygon prides itself in being the ultimate Layer-2 scaling solution for Ethereum. You don't have to worry about the underlying architecture while moving or deploying your dApps to the Polygon PoS network as long as it is EVM-compatible.
2. Use Polygon PoS as a faster transaction layer: Deploying your dApp to the PoS mainnet allows you to leverage Polygon as a faster transaction layer for your dApp. Additionally, you can get your tokens mapped by us. You can join our [technical discussions group](http://bit.ly/matic-technical-group) on Telegram to learn more.
