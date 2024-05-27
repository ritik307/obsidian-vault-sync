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
- When you compile a smart contract then you get bytecode which gets deployed. Inside there are assembly opcodes telling the EVM during execution of a smart contract where to jump to. Those jump destinations are the first 4 bytes of the keccak hash of the function signature.
### ABI Array
- The ABI Array contains all functions, inputs and outputs, as well as all variables and their types from a smart contract.
- If you want to interact with a smart contract from web3js, you need two things:
	1. the ABI array
	2. the contract address (or you deploy the contract through web3js)
