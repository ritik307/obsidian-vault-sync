## Fetch accounts, balance and convert it to ether

```jsx
(async ()=>{

	let accounts = await web3.eth.getAccounts();
	
	console.log("--accounts: ",accounts);
	
	const balance = await web3.eth.getBalance(accounts[0]);
	
	console.log("---balance: ",balance);
	
	const balanceInEth = await web3.utils.fromWei(balance.toString(),"ether");
	
	console.log("---balance in Ether: ",balanceInEth);

})();
```

## What's this ABI array? And why do I need it for interacting with smart contracts.
- ABI stands for Application Binary Interface.

**NOTE:** When you compile a smart contract then you get bytecode which gets deployed. Inside there are assembly opcodes telling the EVM during execution of a smart contract where to jump to. Those jump destinations are the first 4 bytes of the keccak hash of the function signature.
### ABI Array
- The ABI Array contains all functions, inputs and outputs, as well as all variables and their types from a smart contract.
- If you want to interact with a smart contract from web3js, you need two things:
	1. the ABI array
	2. the contract address (or you deploy the contract through web3js).

**JS CODE**:
```jsx
(async ()=>{

const abi = [

	{
	
	"inputs": [],
	
	"name": "myUint",
	
	"outputs": [
		
		{
		
			"internalType": "uint256",
			
			"name": "",
			
			"type": "uint256"
	
	}
	
	],
		"stateMutability": "view",
		"type": "function"
	},
	
	{
	
	"inputs": [
	
		{
			"internalType": "uint256",
			
			"name": "newUint",
			
			"type": "uint256"
		
		}
	
	],
	
	"name": "setMyUint",
	
	"outputs": [],
	
	"stateMutability": "nonpayable",
	
	"type": "function"
	
	}

];

	const address = '0x56a2777e796eF23399e9E1d791E1A0410a75E31b';
	
	  
	
	const contractInstance = new web3.eth.Contract(abi,address);
	
	console.log("---before: ",await contractInstance.methods.myUint().call());
	
	let accounts = await web3.eth.getAccounts();
	
	await contractInstance.methods.setMyUint(3098).send({from: accounts[0]});
	
	console.log(await contractInstance.methods.myUint().call());
	
	console.log("---after: ",await contractInstance.methods.myUint().call());

})();
```

**Smart Contract**
```jsx
//SPDX-License-Identifier: MIT

pragma solidity 0.8.14;

contract MyContract {
	uint public myUint = 123;
	function setMyUint(uint newUint) public {
		myUint = newUint;
	}
}
```
