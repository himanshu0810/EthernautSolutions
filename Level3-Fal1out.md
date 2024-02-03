# Welcome to the Third Challenge!

It's a great feeling to reach Level 3 after diving into the fascinating world of technology. This level might seem simple, but it teaches us the importance of focusing on code details to catch even the smallest errors.

In this challenge, we encounter a basic smart contract that handles the allocation, sending, and collection of balances, while also allowing users to view their balances. The goal here is to become the owner of this contract.

Let's take a close look at the contract code. Have you scrutinized it line by line? If not, no worries! We're all here to learn and progress step by step.

The issue lies in the constructor. Did you notice that the developer misspelled "Fallout"?

  solidity
  /* constructor */
  function Fal1out() public payable {
    owner = msg.sender;
    allocations[owner] = msg.value;
  }

As it stands, anyone can call this function and become the owner. Ideally, this function should only be called at the creation time, not during runtime. Currently, any user can call it and become the owner, which is not the intended behavior.

To fix this, execute the following command to become the owner and verify it:

   javascript
    await contract.Fal1out({value: toWei("0.00001")})

Afterward, confirm the ownership using:

   javascript
    await contract.owner()

Feel free to proceed by triggering the submit instance button and advancing to Level 4. **Happy learning!**
