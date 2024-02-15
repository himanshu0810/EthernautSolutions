# Understanding Solidity Storage Layout

In Solidity, understanding how variables are stored in storage is crucial for optimizing storage usage and accessing data efficiently. Solidity packs variables in storage to conserve space, which affects how data is stored and accessed.

      ```solidity
      // SPDX-License-Identifier: MIT
      pragma solidity ^0.8.0;
      
      contract Privacy {
      
        bool public locked = true;
        uint256 public ID = block.timestamp;
        uint8 private flattening = 10;
        uint8 private denomination = 255;
        uint16 private awkwardness = uint16(block.timestamp);
        bytes32[3] private data;
      
        constructor(bytes32[3] memory _data) {
          data = _data;
        }
        
        function unlock(bytes16 _key) public {
          require(_key == bytes16(data[2]));
          locked = false;
        }
      
        /*
          A bunch of super advanced solidity algorithms...
      
            ,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`
            .,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,
            *.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^         ,---/V\
            `*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.    ~|__(o.o)
            ^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'  UU  UU
        */
      }

## Storage Allocation
Each storage slot in Solidity is 32 bytes wide. Variables are packed into slots based on their size and type. Let's analyze the storage allocation for the provided contract:

- `bool public locked`: Takes 1 byte. Stored in slot 0.
- `uint256 public ID`: Takes 32 bytes. Stored in slot 1.
- `uint8 private flattening`: Takes 1 byte.
- `uint8 private denomination`: Takes 1 byte.
- `uint16 private awkwardness`: Takes 2 bytes. These three variables are packed together, occupying 32 bytes. Stored in slot 2.
- `bytes32[3] private data`: Each element takes 32 bytes. Stored from slot 3 to slot 5.

## Cracking the Level
To modify the `locked` variable to `false`, we need to unlock the contract using the `unlock` function. The unlocking process involves retrieving a key from storage slot 5, casting it to bytes16, and passing it to the `unlock` function.

### Steps:
1. Fetch the value from storage slot 5.
   ```javascript
   key = await web3.eth.getStorageAt(contract.address, 5)
