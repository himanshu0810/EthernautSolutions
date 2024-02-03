# Welcome to the Third Challenge!

It's a great feeling to reach Level 3 after diving into the fascinating world of technology. This level might seem simple, but it teaches us the importance of focusing on code details to catch even the smallest errors.

In this challenge, we encounter a basic smart contract that handles the allocation, sending, and collection of balances, while also allowing users to view their balances. The goal here is to become the owner of this contract.

Let's take a close look at the contract code. Have you scrutinized it line by line? If not, no worries! We're all here to learn and progress step by step.

The issue lies in the constructor. Did you notice that the developer misspelled "Fallout"?

```solidity
/* constructor */
function Fal1out() public payable {
  owner = msg.sender;
  allocations[owner] = msg.value;
}

