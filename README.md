
A complete report on how to add Minting functionalities to a front end NFT website, meant as a basic plan and not in depth code.
## The Website
The Website provided is a front end app that allows people to buy a collection of NFTs directly before it goes to open sea.
With all assets premade, all that's left is adding a front end form to allow a user to choose a mint total with drop-down or arrows. 

<p align="center">
  <img src="mainpage.png" height="300" />
</p>

## Prerequisites
* Node.js (which is already implemented)
* Metamask or any other wallet API

## Connecting to the Wallet
The connect button should be used for wallet sync

We can add that functionality by using a library like ethers for blockchain connection interface, the ethers API is easier to implement than most.

Using ethers, we could create a Web3 Provider or JSONRPC provider and pass in `window.ethereum` as a node provider. `window.ethereum` should exist if the end user has metamask but we should do a check for this in the frontend code. The provider allows us to connect to the blockchain and make requests.

Add functionality to the connect button in the UI. If the user hasn't connected yet, call the `eth_requestAccounts` to make them connect their wallets. The end user will be prompted with a sign transaction.
## Minting
The mint button should be used perform the mint request.
Once the user is connected to the site with his wallet, conditionally add a mint button with a click handler to act upon. When the click happens of these functions will launch:

* Create a contract instance for the NFT using `ethers.Contract` We will need the abi from contract, contract address, and an ethers provider.

* Call mint function on the contract instance we made and pass in params and the value of the mint function. This function will require setting a gas price and gas limit.

End user will get the metamask prompt and then perform the transaction

## Sample Code
```// add a onClick handler when the link to Metaverse Society is clicked
// Add try/catch  to handle errors from the connection
const onMint = ({ amountToMint }) => {
   const provider = new ethers.providers.Web3Provider(window.ethereum)

   // MetaMask requires requesting permission to connect users accounts
   // This is what will "connect a wallet to the site", if this is already done it will basically act like a no-op
   const [account] = await provider.send("eth_requestAccounts", []);
   
   // User signs and account should be defined
   if(account){
     const contractAddress = "contractAddressHere";
     const abi = [] // ABI here you'd get this when you compile the contract
     const contract = new ethers.Contract(contractAddress, abi, provider);
     
     // Call contract mint, set gas price and gas limit
     // User prompts to sign transaction via metamask
     const tx = await contract.mint(amountToMint, {
        value: <Put the wei cost of the mint, use ethers.utils>,
        gasPrice: <Put dynamic gwei, use ethers.utils>,
        gasLimit: <Put gas limit or get gas estimate, e.g 100,000>
     });

     await tx.wait(); // Minting is happening

     // Do some sort of success
   }
}
```
When finished, add nice feedback and error handling on each step of the way for better UX.


