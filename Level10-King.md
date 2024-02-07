# Exploiting the King Contract

In the realm of smart contracts, security is paramount. Understanding vulnerabilities and how to exploit them is crucial for both developers and auditors. In this educational walkthrough, we'll explore a common vulnerability and demonstrate how to exploit it in the context of a simple Solidity contract.

## The King Contract

Let's begin with the target contract, aptly named `King`. Below is the Solidity code for the `King` contract:

    ```solidity
    // SPDX-License-Identifier: MIT
    pragma solidity ^0.8.0;
    
    contract King {
    
      address king;
      uint public prize;
      address public owner;
    
      constructor() payable {
        owner = msg.sender;  
        king = msg.sender;
        prize = msg.value;
      }
    
      receive() external payable {
        require(msg.value >= prize || msg.sender == owner);
        payable(king).transfer(msg.value);
        king = msg.sender;
        prize = msg.value;
      }
    
      function _king() public view returns (address) {
        return king;
      }
    }

This contract features a simple game where participants can become the "king" by sending a higher prize than the current one. The current king is the address associated with the king variable, and the prize value is stored in the prize variable. The receive() function handles incoming ether, updating the king and prize accordingly.

## Exploiting the Vulnerability
Our goal is to become the king without actually sending any ether to the contract. To achieve this, we'll deploy an exploit contract that intercepts the incoming ether and prevents the King contract from updating the king. Here's the exploit contract:

    ```solidity
    contract Exploit {
        King target;
    
        constructor(address _target) payable {
            target = King(_target);
            // Call the target contract's fallback function and send 0.001 ether
            (bool success, ) = _target.call{value: 0.001 ether}("");
            require(success, "Call failed");
        }
    }

This Exploit contract does not have any payable functions, meaning it rejects any ether sent to it.

## Exploiting the King Contract
To exploit the King contract, we deploy the Exploit contract and ensure that the King contract sends ether to it during deployment. Once deployed, the King contract will attempt to transfer ether to the current king upon receiving a higher prize. However, since the Exploit contract rejects any incoming ether, the king will not be updated.

By calling the _king() function on the King contract instance, we can confirm that our exploit contract address is now the king.

Congratulations! You have successfully exploited the vulnerability in the King contract.

## Conclusion
Understanding vulnerabilities and how to exploit them is crucial for ensuring the security of smart contracts. By exploring and experimenting with vulnerabilities in a controlled environment, developers and auditors can better protect against potential exploits in real-world contracts.

