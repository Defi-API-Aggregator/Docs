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
Approve uniswap to handle your tokens. This method is required to be executed before you do unifi.uniswap.swap calls.

If you do not pass amount, then default value is used. The default value is the maximum value possible in solidity 2^256-1.

This mean you will approve uniswap to handle 2^256-1 quantity of your tokens.

```js
let unsignedTxn = await unifi.uniswap.approve({
  walletAddress: wallet.address,
  tokenAddress: "0x70cBa46d2e933030E2f274AE58c951C800548AeF",
  amount: "100000",
  chainId: 5,
});
```
This call returns an unsignedTxn object which you can sign with your wallet to submit your transaction

Output of the above will be similar as shown below.
```js
{
  data: '0x095ea7b300000000000000000000000068b3465833fb72a70ecdf485e0e4c7bd8665fc4500000000000000000000000000000000000000000000000000000000000186a0',
  to: '0x70cBa46d2e933030E2f274AE58c951C800548AeF',
  nonce: 842,
  gasLimit: { type: 'BigNumber', hex: '0x0493e0' },
  gasPrice: { type: 'BigNumber', hex: '0x015b3970' }
}
```
Sample Implementation Below
```js
const Unifi = require('unifi-sdk')
const ethers = require('ethers');
let provider =  new ethers.providers.JsonRpcProvider('your-provider-url');
let wallet = new ethers.Wallet('your-private-key',provider);

let apiKey = 'your-api-key';
let unifi = Unifi(apiKey);

async function execute(){

    unsignedTxn = await unifi.uniswap.approve({
        walletAddress:wallet.address,
        tokenAddress:"0x70cBa46d2e933030E2f274AE58c951C800548AeF",
        amount:"100000",
        chainId:5,
    }
    )
    
    let sentTxn = await wallet.sendTransaction(unsignedTxn)
    let txnReceipt =  await sentTxn.wait();
    console.log('txnReceipt',txnReceipt)
}

execute()
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
This call returns an unsignedTxn object which you can sign with your wallet to submit your transaction

Output of the above will be similar as shown below
```js
{
  data: '0x5ae401dc00000000000000000000000000000000000000000000000000000000643c9683000000000000000000000000000000000000000000000000000000000000004000000000000000000000000000000000000000000000000000000000000000010000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000010442712a670000000000000000000000000000000000000000000000000000000000989680000000000000000000000000000000000000000000000000001be2e1dedca98f0000000000000000000000000000000000000000000000000000000000000080000000000000000000000000f522c2e707345475da50bd45a4c8061df4de722e000000000000000000000000000000000000000000000000000000000000000300000000000000000000000070cba46d2e933030e2f274ae58c951c800548aef000000000000000000000000b4fbf271143f4fbf7b91a5ded31805e42b2208d600000000000000000000000007865c6e87b9f70255377e024ace6630c1eaa37f00000000000000000000000000000000000000000000000000000000',
  to: '0x68b3465833fb72A70ecDF485E0e4C7bD8665Fc45',
  value: { type: 'BigNumber', hex: '0x00' },
  from: '0xf522C2E707345475dA50BD45a4C8061df4dE722E',
  gasPrice: { type: 'BigNumber', hex: '0x015b3970' },
  gasLimit: { type: 'BigNumber', hex: '0x043bfc' },
  nonce: 842
}
```

Sample implementation Below

```js
const Unifi = require('unifi-sdk')
const ethers = require('ethers');
let provider =  new ethers.providers.JsonRpcProvider('your-provider-url');
let wallet = new ethers.Wallet('your-private-key',provider);

let apiKey = 'your-api-key';
let unifi = Unifi(apiKey);

async function execute(){

    let unsignedTxn = await unifi.uniswap.swap({
        walletAddress:wallet.address,
        sellTokenAddress:"0x70cBa46d2e933030E2f274AE58c951C800548AeF",
        buyTokenAddress:"0x07865c6e87b9f70255377e024ace6630c1eaa37f",
        buyTokenAmount:"1000000",
        recipientAddress:wallet.address,
        chainId:5,
    }
    )
    
    let sentTxn = await wallet.sendTransaction(unsignedTxn)
    let txnReceipt =  await sentTxn.wait();
    console.log('txnReceipt',txnReceipt)
}

execute()
```
