# Auditing
- The deployment process normal is like so:
  ```
  unit testing -> tool for detecting vulnerabilities -> testnetwork deployment -> audit -> receive audit badge -> deploy into main net
  ```
- The main vulnerabilities are:
 ```
 -> Missing input or precondition checks (Dont making checks for variable that the user is passing)
 -> Phishing vulnerabilities with tx.origin (In case we use tx.origin instead of msg.sender, the attacker can make us run a intermediate contract and change things from our contract without us to know and because we use tx.origin, it will act in our behalf)
 -> Incorrect calculation of output token amount (for example trying to calculate the fee as blockchain does not use decimals ex: 1/100 = 0 in solidity. We should multiply the amount with the percent we want and only then divide by 100 ex: (1000*1)/100 instead of (1000)*(1/100)
 -> Timestamp manipulation (it is not recommended to use the timestamp as the random factor, because miners can establish the timestamp of the block and with that they can create a timestamp that satisfies themselfs. In order to avoid this we can use chainlink oracle to provide the randomness)
 -> Block gas limit vulnerabilities (if we put high limits of gas, then it will most likely increase the gas to other users, create congestion in the network, create stale blocks)
 ```
 - One of the tools to detect vulnerabilities is slither
 - In order to understand more we should read audit reports from for example CERTIK
# Reading a article (9 most common smart contract vulnerabilities) [INFO](https://blaize.tech/article-type/web3-security/9-most-common-smart-contract-vulnerabilities-found-by-blaize/)
## 1. Indirect Execution of Unknown Code
- This can happen due to the presence of a fallback function feature in smart contracts
- It payable fall back functions can be executed in case you make a bad call to other smart contract, which makes you loose ETH
## 2. Reentrancy
- Executing code more than once, passing the checks out because it got executed faster than the state update
## 3. Incorrect calculation of the output token amount
- Its due to decimals handling
- Incorrect order of operation during fees calculations, which leads to a significant accuracy loss
- the accucary constant that was actually forgotten in the math operations
## 4. Interface/Naming issues
- In prior versions of solidity the constructor was specified by using the contracts name instead of the contract keyword.. so if we change the name of the contract and the constructor name is the same, that function is no longer a constructor but a normal function that everyone can run.. since it establishes the owner normally, it can lead to resources loss
## 5. Dependency on the order of execution
- The state that was presented when we called a function could not be the same state that will be present when we actually run the contract because of transaction ordering
## 6. Time component
- If a smart contract is dependent of time, it becomes a vulnerabilitie since the time is given by the block timestamp and a miner can use that in his own benefit since he can change the timestamp
## 7. Using blockhash function
- A miner also establish the block hash, so if a smart contract is somehow dependent of this block hash, it can be a vulnerabilitie that may be exploited by the miner
## 8. Incorrectly Handled Exceptions
- It happens if we handle exceptions in the wrong way which can lead to roll back transactions but if you emit a event previously, the event is not dismissed leaving to undesired behavior
## 9. Incorrect work with ERC20 token
- In case you make a custom implementation of the ERC20 standard token, where you somehow fail something in the function that may lead to unexpected behavior