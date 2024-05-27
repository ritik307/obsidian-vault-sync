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
- 
