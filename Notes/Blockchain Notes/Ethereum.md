## SmartContract
- Its a piece of code that runs on blockchain.
- Needs transaction to change from one state to another.
- Can do logic operations.
- Deployed as EVM ByteCode.
- **Note** : State change happens through mining + transactions.
- **Its turing complete**: That means it can solve ANY computational problem.
## Solidity
- Complied language. 
- Its compared to JS(ECMAScript)
- Every "high language" code compiles to ByteCode.
- Every Ethereum node in the network execute the same code bcz every node has the same copy of the chain.
## Deployment
- During deployment we sent off transactions.
- The data field was populated.
- The data in data field is the compiled byte-code.

# Code Syntax
## Overflow and Underflow
- In previous versions of Solidity (prior Solidity 0.8.x) an integer would automatically roll-over to a lower or higher number. If you would decrement 0 by 1 (0-1) on an unsigned integer, the result would not be -1, or an error, the result would simple be: type(uint).max.
- For this example I want to use uint8. In our previous example worked with uint256 and uint8 and did not roll over. Uint8 is going from 0 to 2^8 - 1. In other words: uint8 ranges from 0 to 255. If you increment 255 it will automatically be 0, if you decrement 0, it will become 255 if the operation is unchecked. No warnings, no errors. For example, this can become problematic, if you store a token-balance in a variable and decrement it without checking.
```jsx
//SPDX-License-Identifier: MIT

pragma solidity 0.7.0;

  

contract ExampleWrapAround {
	uint8 public myUint = 250;
	
	
	function decrement() public{
		myUint--;
	}
	
	function increment() public{
		myUint++; 
		// If you deploy this and run "increment" more than 5 time, the myUint8 will just magically start from 0 again.
	}
}

```

- _Normally_, we don't want an integer to roll over. That's why in 0.8 it changed to be the default behavior to error out if the maximum/minimum value is reached. But you can still enforce this behavior. With an `unchecked` block. Let's see an example.
```jsx
//SPDX-License-Identifier: MIT

pragma solidity 0.8.15; // New solidity version

  

contract ExampleWrapAround {

	uint8 public myUint = 250;

	function decrement() public{
		myUint--;
	
	}
	function increment() public{
	
		unchecked{
		
			myUint++;
		
		}
	
	}

}
```
## Strings and Bytes
- Natively, there are no String manipulation functions.
- No even string comparison is natively possible.
- There are libraries to work with Strings
- Strings are expensive to store and work with in Solidity (Gas costs)
- **Note**: try to avoid storing Strings, use Events instead.
### Comparing two strings
- There are no native string comparison functions in Solidity. There is still a way to compare two strings: by comparing their `keccak256` hashes.
	```jsx
//SPDX-License-Identifier: MIT

pragma solidity 0.8.15;

  

contract StringExample{

	string public myString = "My String";
	
	  
	
	function compareTwoStrings(string memory _myString) public view returns(bool){
		return keccak256(abi.encodePacked(myString)) == keccak256(abi.encodePacked(_myString));
	}
}
```
### Strings and Bytes
- Strings are actually arbitrary long bytes in UTF-8 representation. Strings do not have a length property, bytes do have that.
```jsx
//SPDX-License-Identifier: MIT

pragma solidity 0.8.15;

  

contract StringExample{

	string public myString = "My String";
	//unicode to print 0x48656c6c6f2057c3b6726c64
	bytes public myBytes = unicode"My String"; 
	
	function compareTwoStrings(string memory _myString) public view returns(bool){
		return keccak256(abi.encodePacked(myString)) == keccak256(abi.encodePacked(_myString));
	}
	
	function getMyBytesLength()public view returns(uint){
		return myBytes.length;
	}

}
```
## Address Types
- a variable of the type _address_ holds **20 bytes**. That's all that happens internally. 
- **Notes**: 
	- The Smart Contract is stored under its own address.
	- The Smart Contract can store an address in the variable "someAddress", which can be its own address, but can be any other address as well.
	- All information on the blochain is public, so we can retrieve the balance of the address stored in the variable "someAddress".
	- The Smart Contract can transfer funds _from_ his own address _to_ another address. But it cannot transfer the funds from another address.
	- Transferring Ether is fundamentally different than transferring ERC20 Tokens or NFTs.
```jsx
//SPDX-License-Identifier: MIT

pragma solidity 0.8.15;

contract AddressExample {

	address public myAddress;
	
	function setAddress(address _newAddress) public {
		myAddress = _newAddress;
	
	}
	function getBalanace() public view returns(uint){
		return myAddress.balance;
	}

}
```

- **Ethereum Denominations**
![[Pasted image 20240519012638.png]]
## msg.sender Object
- The msg-object is a special variables which always exist in the global namespace and is used to provide information about the blockchain or the transaction.
```jsx
//SPDX-License-Identifier: MIT

pragma solidity 0.8.15;

contract AddressExample {

	address public myAddress;
	
	function getSendersAddress() view public returns(address){
		return msg.sender;
	}
	
	function updateMyAddress() public{
		myAddress = msg.sender;
	}
}
```
## Types of functions
### Writing function
- a writing function can have return variables, **but** they won't actually return anything to the transaction initializer. There are several reason for it, but the most prominent one is: at the time of sending the transaction the actual state is unknown. It is possible that someone sends a competing transaction that alters the state and from there it only depends on the transaction ordering - which is something that happens on the miner side.
	- **What's the return variable for then?**
		- It's for Smart Contract communication. It is used to return something when a smart contract calls another smart contract function.
	- **How to return variables then in Solidity?**
		- With Events
```jsx
//SPDX-License-Identifier: MIT
pragma solidity 0.8.15;

contract ExampleViewPure {
	uint public myStorageVariable;
	
	function setMyStorageVariable(uint _newVar) public returns(uint) {
		myStorageVariable = _newVar;
		return _newVar;
	}
}
```

### View Functions:
-  A view function is a function that reads from the state but doesn't write to the state. A classic view function woule be a getter-function. 
```jsx
//SPDX-License-Identifier: MIT
pragma solidity 0.8.15;

contract ExampleViewPure {
	uint public myStorageVariable;
	
	function getMyStorageVariable() public view returns(uint) {
		return myStorageVariable;
	}
	
	function setMyStorageVariable(uint _newVar) public returns(uint) {
		myStorageVariable = _newVar;
		
		return _newVar;
	}

}
```

### Pure Functions:
- A pure function is a function that neither writes, nor reads from state variables. It can only access its own arguments and other pure functions. Let me give you an example:
```jsx
//SPDX-License-Identifier: MIT
pragma solidity 0.8.15;

contract ExampleViewPure {

	uint public myStorageVariable;
	
	function getMyStorageVariable() public view returns(uint) {
	
		return myStorageVariable;
	
	}
	
	function getAddition(uint a, uint b) public pure returns(uint) {
	
		return a+b;
	
	}
	
	function setMyStorageVariable(uint _newVar) public returns(uint) {
		myStorageVariable = _newVar;
		
		return _newVar;
	}
}
```
## Constructor
- a _special_ function that is called only once during contract deployment. It can have arguments, like here. But it also has the same global objects available as in any other transaction. So in `msg.sender` is the address of the person who deployed the Smart Contract.
```jsx
//SPDX-License-Identifier: MIT
pragma solidity 0.8.15;

contract ExampleConstructor {

	address public myAddress;
	
	constructor(address _someAddress) {
	
		myAddress = _someAddress;
	
	}
	
	function setMyAddress(address _myAddress) public {
	
		myAddress = _myAddress;
	
	}
	
	function setMyAddressToMsgSender() public {
	
		myAddress = msg.sender;
	
	}
}
```

## Metamask (Behind the Scene)
![[Pasted image 20240520011233.png]]

- Nodes are the piece of s/w that implements the Ethereum protocol.
- You need a blockchain node to interact with the blockchain.
- The blockchain is the single source of truth.
- There are infrastructure providers to abstract running blockchains.
## Ethereum Transaction
![[Pasted image 20240520013852.png]]
- If **to** field is empty then the data field must be populated bcz then its a **Contract**.

### How does blockchain know its allowed to transfer [value] from account [from] to account [to] ?
- By creating a **Signature**.
- ![[Pasted image 20240520014509.png]]
- ![[Pasted image 20240520014536.png]]
- ![[Pasted image 20240520014616.png]]
- ![[Pasted image 20240520015010.png]]
- ![[Pasted image 20240520015107.png]]
- ![[Pasted image 20240520015225.png]]
- **Note**:  the seed phrase is run through a special kind of algorithm and uses an index to create **private keys** so you can have a virtually unlimited number of private keys, and then eventually unlimited number of public keys and virtually unlimited number of Ethereum accounts from just one seed phrase.

## Cryptographic Hashing
- The ideal cryptographic hashing function should have 5 main properties.
	- it is deterministic so the same message always results in the same hash.
	- it is quick to compute the hash value for any given message.
	- it is infeasible to generate a message from its hash value except by trying all possible messages.
	- a small change to a message should change the hash value so extensively that the new hash value appears uncorrelated with the old hash value.
	- it is infeasible to find two different messages with the same hash value.

- Blocks have hashes of previous block.
	- Are 'chained together'.
- Changing info on prev. block changes all blocks thereafter.
- Blocks are stored decentralised.
	- on every participating blockchain-node.
	
![[Pasted image 20240520021016.png]]
- **Decentralisation**: Take the blockchain hashes and copy it to every blockchain-node that is running. So if you want to change something somewhere in the blockchain hash you need to convince every blockchain-node that they should do the same. You cant bcz Ethereum protocol doesn't allow that.

## Update / Delete transactions
- **Gas price auction**:  The higher the gas price , the more likely it get mined.
- **Send the same transaction nonce with higher gas fees**
	- Update: Higher gas fees
	- Cancel: Send NO data and to == from. (with same nonce)

## Payable Modifier
-  you need to add the payable modifier. That is the keyword "payable" right next to the visibility specifier "public".
-  you need to understand the global msg-objects property called value. `msg.value`.
```jsx
//SPDX-License-Identifier: MIT
pragma solidity 0.8.15;

contract SampleContract {

	string public myString = "Hello World";
	
	function updateString(string memory _newString) public payable {
	
		if(msg.value == 1 ether){
		
			myString = _newString;
		
		}else{
		
			payable(msg.sender).transfer(msg.value);
		
		}
	
	}

}
```

## Receiver and fallback Modifiers
- `receive()` is a function that gets priority over `fallback()` when a calldata is empty. But fallback gets precedence over receive when calldata does not fit a valid function signature.

```jsx
//SPDX-License-Identifier: MIT
pragma solidity 0.8.15;

contract SampleFallback{

	uint public lastValueSent;
	
	string public lastFunctionCalled;
	
	receive() external payable{
		lastValueSent = msg.value;
		lastFunctionCalled = "receive";
	}
	
	fallback() external payable {
		lastValueSent = msg.value;
		lastFunctionCalled = "fallback";
	}
}
```

![[Pasted image 20240521152831.png]]

## Setter and Getter Functions
- writing transactions: transactions.
- reading transactions:  calls.
- Calls are against the local blockchain node.
	- **Remember:** Everyone has a copy of the blockchain.
	- If you dont need to change anything, you dont need to inform other participants.
	- Reading is virtually FREE.
- **View Function:** Reading from the state and from other view functions.
- **Pure Function:** NOT reading or modifying the state.

## Function Visibility
- **Public:** Can be called internally and externally.
- **Private:** ONLY for the contract, not externally reachable and NOT via derived contract.
- **External:** Can be called from other contracts. Can be called externally.
- **Internal:** Only from the contract itself or from derived contracts. Cant be invoked by a transaction.

## Constructor
- A func. with the name "constructor()".
- Called ONLY during deployment.
- Cant be called afterwards.
- Is either public or internal.

## Fallback function
- A function with the name "fallback() external [payable]" to receive calldata.
- A function with the name "receive() external payable" to receive a value without calldata.
- Called when the transaction with the wrong function signature is sent to the smart contract.
- Called when the function in the transaction isnt found.
- Can ONLY be external.
- Fallback payable is optional.
- Contract receiving Ether without a fallback function and without a function call will throw an exception.
- You cannot completely avoid receiving Ether.(Miner reward or selfdestruct(address) will forcefully credit ether).
- Worst case: You can only rely on 2300 gas
	- ﻿_contractAddress.transfer(1 ether); send only 2300 gas along
- ﻿﻿Forcefully prevent contract execution if called with contract data
	- ﻿﻿require(msg.data.length == 0)

## Msg.value  and address
- Global msg-object contains a value property (in wei)
- ﻿﻿How much wei was sent during this call?
- ﻿﻿Address-type variables have a balance (address X = 0x123...) X.balance
- ﻿﻿Address-type variable can be payable (address payable x)
- ﻿﻿Payable addresses can receive a value (x.transfer(...wei))
- ﻿﻿The contract itself can have a balance (address(this).balance)

## Mapping (similar to Hashmaps)
```jsx
//SPDX-License-Identifier: MIT

pragma solidity 0.8.15;

contract SendWithdrawMoney{
	
	mapping(address => uint) balanceReceived;
	
	function deposit() payable public {
		balanceReceived[msg.sender] += msg.value;
	}
	
	function getContractBalance() view public returns(uint){
		return address(this).balance;
	}
	
	function withdrawAllMoney(address payable _to) public {
		uint balanceToSendOut = balanceReceived[_to];
		balanceReceived[msg.sender] = 0;
		_to.transfer(balanceToSendOut);
	}
	
	function checkAddressBalance(address _addr) view public returns(uint) {
		return balanceReceived[_addr];
	}
	
	function myBalance() public view returns(uint){
		return address(this).balance;
	}

}
```

## Struct vs Child Contract
### Child Contract: 
```jsx
//SPDX-License-Identifier: MIT

pragma solidity 0.8.15;

contract PaymentReceived {

	address public from;
	uint public amount;
	
	constructor(address _from, uint _amount) {
		from = _from;
		amount = _amount;
	}
}

contract Wallet {

	PaymentReceived public payment;
	
	function payContract() public payable {
		payment = new PaymentReceived(msg.sender, msg.value);
	}
}	
```
	
- **PROBLEM:** If you deploy the "Wallet" smart contract and send 1 wei to the payContract function (1) , it will use up 221530 gas (2). Why? Because it deploys a new contract "PaymentReceived", then links it.

### Struct:
```jsx
//SPDX-License-Identifier: MIT
pragma solidity 0.8.15;

contract Wallet2 {

	struct PaymentReceivedStruct {
		address from;
		uint amount;
	}
	
	PaymentReceivedStruct public payment;

	function payContract() public payable {
		payment = PaymentReceivedStruct(msg.sender, msg.value);
	}

}
```
- Gas fees suddenly costs only 75394 gas (almost 3 times less!)
- You can directly interact with the payment public getter function without interacting with a separate contract.
### What is better: Solidity struct vs Child Contract?
- **Saving Gas Costs!** Deploying Child Contracts is simply more expensive.
- **Saving Complexity:** Every time you need to access a child contracts property or variable, it needs to go through the child contracts address. For structs, internally, that's just an keccak hash for a lookup at the storage location.
- **DRY/KISS principle (Don't repeat yourself, keep ii simple, stupid):** Contracts are code. Every time you deploy the same contract with the same code you basically repeated yourself. And its not really simple either.
- **Attack Vector:** Contracts are running on its own address. If you have a set of getter and setter functions, there is a chance you need to have access lists or permissions that need to be managed separately.
### Good reasons for using Child Contracts over Structs
- Contract can have any code you want and that means the power of composability. You can put the contracts together like lego pieces and each contrac can have its own logic built in.
- Smaller Contracts if the code is split across several contracts. In the particular example above, you don't need to get payments from the wallet contract, you can get the payments from the child contract.
- Access: Contracts have their own address, can have their own data structures and their own interfaces.

## Struct Mapping Example
```jsx
//SPDX-License-Identifier: MIT

  

pragma solidity 0.8.15;

  

contract StractMappingExample{

  
	
	struct Transaction{
	
		uint amount;
		
		uint timestamp;
	
	}
	
	  
	
	struct BalanceInfo{
	
		uint totalBalance;
		
		uint depositIdx;
		
		mapping(uint => Transaction) depositLogs;
		
		uint withdrawalIdx;
		
		mapping(uint => Transaction) withdrawLogs;
		
	}
	
	mapping(address => BalanceInfo)public balanceLogs;
	
	  
	
	function getBalance() view public returns(uint){
	
		return balanceLogs[msg.sender].totalBalance;
	
	}
	
	  
	
	function deposit() payable public {
	
		balanceLogs[msg.sender].totalBalance += msg.value;
		
		Transaction memory deposiTransaction = Transaction(msg.value,block.timestamp);
		
		uint depositIdx = balanceLogs[msg.sender].depositIdx;
		
		balanceLogs[msg.sender].depositLogs[depositIdx] = deposiTransaction;
		
		balanceLogs[msg.sender].depositIdx++;
	
	}
	
	  
	
	function withdraw(address payable _to,uint _amount) payable public{
	
		balanceLogs[msg.sender].totalBalance -= _amount;
		
		Transaction memory withdrawTransaction = Transaction(msg.value,block.timestamp);
		
		uint withdrawalIdx = balanceLogs[msg.sender].withdrawalIdx;
		
		balanceLogs[msg.sender].depositLogs[withdrawalIdx] = withdrawTransaction;
		
		balanceLogs[msg.sender].withdrawalIdx++;
		
		_to.transfer(_amount);
	
	}

}
```

## Exception Handling
### require
- **Note**: If the require statement turns out to be TRUE the gas fees will be returned back to the user.
```jsx
//SPDX-License-Identifier: MIT
pragma solidity 0.6.12;

contract ExceptionExample {
	mapping(address => uint) public balanceReceived;
	
	function receiveMoney() public payable {
		balanceReceived[msg.sender] += msg.value;
	}
	
	function withdrawMoney(address payable _to, uint _amount) public {
		require(_amount <= balanceReceived[msg.sender], "Not Enough Funds, aborting");

		balanceReceived[msg.sender] -= _amount;
		
		_to.transfer(_amount);
	}

}
```

### assert
- Assert is used to check invariants. Those are states our contract or variables should never reach, ever. For example, if we decrease a value then it should never get bigger, only smaller.

```jsx
//SPDX-License-Identifier: MIT
pragma solidity 0.6.12;

contract ExceptionExample {

	mapping(address => uint8) public balanceReceived;
	
	function receiveMoney() public payable {
	
		assert(msg.value == uint8(msg.value));
		
		balanceReceived[msg.sender] += uint8(msg.value);
		
		assert(balanceReceived[msg.sender] >= uint8(msg.value));
	
	}
	
	function withdrawMoney(address payable _to, uint8 _amount) public {
	
		require(_amount <= balanceReceived[msg.sender], "Not Enough Funds, aborting");
		
		assert(balanceReceived[msg.sender] >= balanceReceived[msg.sender] - _amount);
		
		balanceReceived[msg.sender] -= _amount;
		
		_to.transfer(_amount);
	
	}

}
```

### try/catch block
```jsx
//SPDX-License-Identifier: MIT

pragma solidity 0.8.15;

contract WillThrow{
	
	error NotAllowedError(string); //defining custom exceptions
	
	function aFunction() pure public{
		// require(false, "This is an example error message");
		// assert(false);
		revert NotAllowedError("You are not allowed");
	}
}
	
contract ErrorHandlinhg{
	
	event ErrorLogging(string reason);
	
	event ErrorLogCode(uint code);
	
	event ErrorLogBytes(bytes lowlevelData);
	
	function catchTheError() public{
	
		WillThrow will = new WillThrow();
		
		try will.aFunction(){
			// if code works
		}catch Error(string memory reason){ //Trigger with require
			emit ErrorLogging(reason);
		}catch Panic(uint errorCode){ //Trigger with assert
			emit ErrorLogCode(errorCode);
		} catch (bytes memory lowlevelData){ //Trigger with custom messages
			emit ErrorLogBytes(lowlevelData);
		}
	
	}

}
```


## Difference .send and .transfer
- If the target address is a contract and the transfer fails, then .transfer will result in an exception and .send will simply return false, but the transaction won't fail.
```jsx
//SPDX-License-Identifier: MIT

pragma solidity 0.8.15;

contract Sender {

	receive() external payable {}
	
	function withdrawTransfer(address payable _to) public {
		_to.transfer(10);
	}
	
	function withdrawSend(address payable _to) public {
		bool sentSuccessful = _to.send(10);
	}

}

contract ReceiverNoAction {

	function balance() public view returns(uint) {
		return address(this).balance;
	}
	receive() external payable {}
}

contract ReceiverAction {
	
	uint public balanceReceived;
	
	function balance() public view returns(uint) {
		return address(this).balance;
	}
	receive() external payable {
		balanceReceived += msg.value;
	}
}
```
Now, let's play around with this:
1. Deploy the `Sender` contract
2. fund the `Sender` contract with some 100 wei (hit _transact_ to let it go to the receive function)
3. Deploy the `ReceiverNoAction` and copy the contract address
4. Send 10 wei to the `ReceiverNoAction` with withdrawTransfer. It works, because the function receive in `ReceiverNoAction` doesn't do anything and doesn't use up more than 2300 gas
5. Send 10 wei to the `ReceiverNoAction` with withdrawSend. It also works, because the function still does not need more than 2300 gas.
6. Deploy the `ReceiverAction` Smart contract and copy the contract address
7. Send 10 Wei to the `ReceiverAction` with withdrawTransfer. It **fails**, because the contract tries to write a storage variable which costs too much gas.
8. Send 10 Wei to the `ReceiverAction` with withdrawSend. The transaction doesn't fail, but it also doesn't work, which leaves you now in an odd state. 👈🏻 That's the Problem right here.

**Note: Always check the return value of low level send functions**. Ideally with an `require(sentSuccessful)` or so.
## Pull Over Push
- It's always better to let users withdraw money instead of pushing the funds. Consider a game. Two players play against each other. Last round, a player wins. In the normal world, you'd directly _push_ the funds to the winning user. But that's a bad pattern. Better to _credit_ the user and let him _withdraw_ (pull!) the money in a separate withdraw-function later on.

## Sending More Gas to Smart Contracts
- it would be great if you can call smart contracts from other smart contracts and also send a value, as a well as, more gas.
- There are two ways to achieve that:
	1. External function calls on contract instances
	2. Low-Level calls on the address

### External Function Calls
- Sometimes you want to call another smart contract. But not just that, you also want to send eth/wei to another smart contract.
```jsx
// SPDX-License-Identifier: GPL-3.0

pragma solidity 0.8.15;

  

contract ContractOne {
	mapping(address => uint) public addressBalances;
	
	function getBalance() public view returns(uint) {
		return address(this).balance;
	}
	
	function deposit() public payable {
		addressBalances[msg.sender] += msg.value;
	}

}


contract ContractTwo {

	function deposit() public payable {}
	
	function depositOnContractOne(address _contractOne) public {
		ContractOne one = ContractOne(_contractOne);
		one.deposit{value: 10, gas: 100000}();
	}
}
```

## Low-Level Calls on Address-Type Variables

- **EXAMPLE 1**: 
```jsx
// SPDX-License-Identifier: GPL-3.0

pragma solidity 0.8.15;

  

contract ContractOne {
	mapping(address => uint) public addressBalances;
	
	function getBalance() public view returns(uint) {
		return address(this).balance;
	}


	function deposit() public payable {
		addressBalances[msg.sender] += msg.value;
	}

}

  

contract ContractTwo {

	function deposit() public payable {}
	
	function depositOnContractOne(address _contractOne) public {
		bytes memory payload = abi.encodeWithSignature("deposit()");
		(bool success, ) = _contractOne.call{value: 10, gas: 100000}(payload);
		require(success);
	}

}
```

- **EXAMPLE 2**
```JSX
// SPDX-License-Identifier: GPL-3.0

pragma solidity 0.8.15;

  

contract ContractOne {

	mapping(address => uint) public addressBalances;
	
	function getBalance() public view returns(uint) {
		return address(this).balance;
	}
	
	  
	
	receive() external payable {
	
	addressBalances[msg.sender] += msg.value;
	
	}

}

  

contract ContractTwo {

  

function deposit() public payable {}

  

function depositOnContractOne(address _contractOne) public {

(bool success, ) = _contractOne.call{value: 10, gas: 100000}("");

require(success);

}

}
```
