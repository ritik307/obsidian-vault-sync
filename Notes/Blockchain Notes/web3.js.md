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

## web3.js example project
- **HTML**
```jsx
<button class="enableMetamask">
Enable MetaMask
</button>
<h2><span class="statusText"></span></h2>

<input type="text" id="address" placeholder="Contract Address" disabled="true" />
<button class="startStopEventListener" disabled="true" >
Listen to Events
</button>

<span class="eventResult">
  
</span>
<br />
<small>
This is an example of Events from <a href="https://ethereum-blockchain-developer.com">Ethereum-Blockchain-Developer.com</a>
</small>
```

- **JS**
```jsx
const enableMetaMaskButton = document.querySelector('.enableMetamask');
const statusText = document.querySelector('.statusText');
const listenToEventsButton = document.querySelector('.startStopEventListener');
const contractAddr = document.querySelector('#address');
const eventResult = document.querySelector('.eventResult');

enableMetaMaskButton.addEventListener('click', () => {
  enableDapp();
});
listenToEventsButton.addEventListener('click', () => {
  listenToEvents();
});

let accounts;
let web3;

async function enableDapp() {

  if (typeof window.ethereum !== 'undefined') { //if undefined than metamask is not installed
    try {
      accounts = await ethereum.request({
        method: 'eth_requestAccounts'
      });
      web3 = new Web3(window.ethereum); //we can use window.ethereum as web3 provider
      statusText.innerHTML = "Account: " + accounts[0];

      listenToEventsButton.removeAttribute("disabled");
      contractAddr.removeAttribute("disabled");
    } catch (error) {
      if (error.code === 4001) {
        // EIP-1193 userRejectedRequest error
        statusText.innerHTML = "Error: Need permission to access MetaMAsk";
        console.log('Permissions needed to continue.');
      } else {
        console.error(error.message);
      }
    }
  } else {
    statusText.innerHTML = "Error: Need to install MetaMask";
  }
};

let abi = [ //update ABI when you are changing your contract
	{
		"inputs": [
			{
				"internalType": "address",
				"name": "_to",
				"type": "address"
			},
			{
				"internalType": "uint256",
				"name": "_amount",
				"type": "uint256"
			}
		],
		"name": "sendToken",
		"outputs": [
			{
				"internalType": "bool",
				"name": "",
				"type": "bool"
			}
		],
		"stateMutability": "nonpayable",
		"type": "function"
	},
	{
		"inputs": [],
		"stateMutability": "nonpayable",
		"type": "constructor"
	},
	{
		"anonymous": false,
		"inputs": [
			{
				"indexed": false,
				"internalType": "address",
				"name": "_from",
				"type": "address"
			},
			{
				"indexed": false,
				"internalType": "address",
				"name": "_to",
				"type": "address"
			},
			{
				"indexed": false,
				"internalType": "uint256",
				"name": "_amount",
				"type": "uint256"
			}
		],
		"name": "TokensSent",
		"type": "event"
	},
	{
		"inputs": [
			{
				"internalType": "address",
				"name": "",
				"type": "address"
			}
		],
		"name": "tokenBalance",
		"outputs": [
			{
				"internalType": "uint256",
				"name": "",
				"type": "uint256"
			}
		],
		"stateMutability": "view",
		"type": "function"
	}
];
async function listenToEvents() {
  let contractInstance = new web3.eth.Contract(abi, contractAddr.value);
  console.log("---contract Instance ",contractInstance);
  contractInstance.events.TokensSent().on("data", (event) => {
  console.log("--event: ",event);
  	eventResult.innerHTML = JSON.stringify(event) + "<br />=====<br />" + eventResult.innerHTML;
  });
}
```

- **Contract**
```jsx
//SPDX-License-Identifier: MIT

pragma solidity 0.8.16;

contract EventExample {
	mapping(address => uint) public tokenBalance;
	
	event TokensSent(address _from, address _to, uint _amount);
	
	constructor() {
		tokenBalance[msg.sender] = 100;
	}
	
	function sendToken(address _to, uint _amount) public returns(bool) {
		require(tokenBalance[msg.sender] >= _amount, "Not enough tokens");
		tokenBalance[msg.sender] -= _amount;
		tokenBalance[_to] += _amount;
		emit TokensSent(msg.sender, _to, _amount);
		return true;
	}
}
```
