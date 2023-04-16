# Docs

UniFi provides makes access to any Defi protocol in just a single line of code. Currently Unifi has integrated Uniswap with methods to do swaps, approve and get swap quote in uniswap all in a single line, as of now we support goerli chain.

In the coming up weeks:
1) We will be integrating protocols like Aave, 1inch and more methods from Uniswap
2) Creation of AA wallet architecture and provide REST API endpoints

# Uniswap

## Setup

```js
const Unifi = require("./sdk/unifi.js");
const ethers = require("ethers");

let provider = new ethers.providers.JsonRpcProvider(
  "https://goerli.infura.io/v3/xxxx"
);

let wallet = new ethers.Wallet("xxxx", provider);

let unifiApiKey = "xxxx";

let unifi = Unifi(unifiApiKey);
```

## Methods

### approve

```js
let unsignedTxn = await unifi.uniswap.approve({
  walletAddress: wallet.address,
  tokenAddress: "0x70cBa46d2e933030E2f274AE58c951C800548AeF",
  amount: "100000",
  chainId: 5,
});
```

### getQuote

```js
let unsignedTxn = await unifi.uniswap.getQuote({
  sellTokenAddress: "0x70cBa46d2e933030E2f274AE58c951C800548AeF",
  buyTokenAddress: "0x07865c6e87b9f70255377e024ace6630c1eaa37f",
  sellTokenAmount: "1000",
  chainId: 5,
});
```

### swap

```js
let unsignedTxn = await unifi.uniswap.swap({
  walletAddress: wallet.address,
  sellTokenAddress: "0x70cBa46d2e933030E2f274AE58c951C800548AeF",
  buyTokenAddress: "0x07865c6e87b9f70255377e024ace6630c1eaa37f",
  buyTokenAmount: "1000",
  recipientAddress: wallet.address,
  chainId: 5,
});
```
