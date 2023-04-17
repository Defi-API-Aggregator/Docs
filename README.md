# Unifi Docs

UniFi provides access to any Defi protocol in just a single line of code. Currently Unifi has integrated Uniswap with methods to do swaps, approve and get swap quote in uniswap all in a single line, as of now we support goerli chain.

In the coming up weeks:
1) We will be integrating protocols like Aave, 1inch and more methods from Uniswap
2) Creation of AA wallet architecture and provide REST API endpoints

# Unifi Uniswap Methods

## Setup

Install Unifi SDK
```js
npm i unifi-sdk
```
Get your API key from our Website and pass it as an argument
```js
const Unifi = require("unifi-sdk");

let unifiApiKey = "xxxx";

let unifi = Unifi(unifiApiKey);
```

## Methods

### approve
Approve uniswap to handle your tokens. This method is required to be executed before you do unifi.uniswap.swap calls
This call returns an unsignedTxn object which you can sign with your wallet to submit your transaction

```js
let unsignedTxn = await unifi.uniswap.approve({
  walletAddress: wallet.address,
  tokenAddress: "0x70cBa46d2e933030E2f274AE58c951C800548AeF",
  amount: "100000",
  chainId: 5,
});
```

### getQuote
Get the best Quotes from Uniswap (V2 and V3). Returns an Object with all essential information about the swap 
```js
let unsignedTxn = await unifi.uniswap.getQuote({
  sellTokenAddress: "0x70cBa46d2e933030E2f274AE58c951C800548AeF",
  buyTokenAddress: "0x07865c6e87b9f70255377e024ace6630c1eaa37f",
  sellTokenAmount: "1000",
  chainId: 5,
});
```

### swap
Swaps Tokens, pass in the sellTokenAddress for the token you want to sell and buyTokenAddress for the token you want to buy.

You can either pass sellTokenAmount or buyTokenAmount depending on your need.

Passing sellTokenAmount results in generating an unsigned transaction object for selling sellTokenAmount worth sellTokens.

Passing buyTokenAmount results in generating an unsigned transaction object for buying buyTokenAmount worth buyTokens for respective amount of sellTokens to be sold.
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
