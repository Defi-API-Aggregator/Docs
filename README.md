# Unifi Docs

UniFi provides access to any Defi protocol in just a single line of code. Currently Unifi has integrated Uniswap with methods to do swaps, approve and get swap quote in uniswap all in a single line, as of now we support goerli chain.

In the coming up weeks:
1) We will be integrating protocols like Aave, 1inch and more methods from Uniswap
2) Creation of AA wallet architecture and provide REST API endpoints


## Setup

Install Unifi SDK
```js
npm i unifi-sdk
```
You can use our demo api-key and try out Unifi 
```js
let demoApiKey = "wutu5laotqfd8ofbn30px8sv7s9g9u3f";
```

```js
const Unifi = require("unifi-sdk");

let unifi = Unifi(demoApiKey);
```

## Methods

### Uniswap Methods

#### approve
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

#### getQuote
Get the best Quotes from Uniswap (V2 and V3). Returns an Object with all essential information about the swap 
```js
let unsignedTxn = await unifi.uniswap.getQuote({
  sellTokenAddress: "0x70cBa46d2e933030E2f274AE58c951C800548AeF",
  buyTokenAddress: "0x07865c6e87b9f70255377e024ace6630c1eaa37f",
  sellTokenAmount: "1000",
  chainId: 5,
});
```

#### swap
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
## Custom  Strategy

### targetImpactIntervalPurchase
implements a strategy similar to TWAP for swapping large amount of crypto over an interval of time at a targeted price impact in uniswap

The below will return a TIIPExecutor Object with which you can trigger to start the strategy and listen to events
```js
let TIIPExecutor = unifi.uniswapTools.targetImpactIntervalPurchase({
    walletAddress:wallet.address, 
    sellTokenAddress:'0x2260FAC5E5542a773Aa44fBCfeDf7C193bc2C599',
    buyTokenAddress:'0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2',
    maximumPriceImpact:0.05,
    priceImpactTolerance:0.01,
    desiredChunkSize:"20",
    targetQuantity:"2000",
    interval:60000,
    lowerBoundChunkSize:"10",
    chainId:1,
});
```

The following command with start the strategy
```js
TIPExecutor.runTIPExecutor()
```

The following is essential to listen to events which will contain the required transaction to be sent from your wallet at every interval which you have specified
```js
TIPExecutor.TIPExecutorEvents.on('executionOutput',(response)=>{
    console.log('response',response);
    //wallet.sendTransaction(response.unsignedTxnObject);
})
```

Full Implementation
```js
const Unifi = require('unifi-sdk');
let apiKey = 'wutu5laotqfd8ofbn30px8sv7s9g9u3f'//'wutu5laotqfd8ofbn30px8sv7s9g9u3f'//'2n2323vici73wr3vpoym9z0uqfndmdo5';
let unifi = Unifi(apiKey);
const fs = require('fs'); 
const ethers = require('ethers');
let provider =  new ethers.providers.JsonRpcProvider('https://goerli.infura.io/v3/175266848e3a488caba1109816167e8b');
let wallet = new ethers.Wallet('0xfe69d33a910098943ae2b3d06aacaef612bb45d88972484b95873450cdf64606',provider);


let TIPExecutor = unifi.uniswapTools.targetImpactIntervalPurchase({
    walletAddress:wallet.address, 
    sellTokenAddress:'0x2260FAC5E5542a773Aa44fBCfeDf7C193bc2C599',
    buyTokenAddress:'0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2',
    maximumPriceImpact:0.05,
    priceImpactTolerance:0.01,
    desiredChunkSize:"20",
    targetQuantity:"2000",
    interval:60000,
    lowerBoundChunkSize:"10",
    chainId:1,
});

const time = new Date().getTime();

TIPExecutor.runTIPExecutor()
TIPExecutor.TIPExecutorEvents.on('executionOutput',(response)=>{
    console.log('response',response);

    //writing to a file to log events for reference
    fs.appendFile('executionHistory_'+time+'.txt',JSON.stringify(response)+'\n', (err)=>{
        if(err) throw err;
    })

})
```

You can check out the code for the strategy at
```js
unifi-sdk/tools/uniswap/TIPExecutor.js
```

