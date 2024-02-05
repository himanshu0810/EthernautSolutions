# Embracing Blockchain Technology for a Better World

I am a big believer in blockchain technology, and I am excited about its potential to make the world a better place.  In this journey, I am happy to see that you are learning this cool 

technology to contribute to positive change.


## The Challenge: Owning Tokens to Conquer the Level

To advance in this quest, we need to acquire and manage some tokens. Currently, we have 20 tokens,  and there are specific rules to follow when transferring them. Let's dive into the

code to understand how we can own and manage these balances effectively.


# Exploiting Vulnerability with Transfer Function

In certain scenarios, the transfer function can be utilized to exploit vulnerabilities. Let's examine a  case where the result of an operation is -1 when the integer is 

signed (20-21).

    ```plaintext
    20 - 21 = -1


Now, consider the same operation with an unsigned integer. If the number is 2^255 - 1, the result of the operation is  
uint256(-1), equivalent to (2**256) – 1. In Solidity, especially with versions greater than 0.8 or when using SafeMath library, 
such an operation would typically result in a revert. SafeMath is a library commonly used to handle arithmetic operations safely.



Now, with this understanding, let's delve into an example where the transfer function is used:  

    ```solidity
    await contract.transfer(<RANDOM_CONTRACT_ADDRESS>, -21)
    


This snippet showcases an attempt to exploit the vulnerability by transferring a negative value. The success  

 of such an operation would depend on the implementation details of the contract, especially regarding integer handling and overflow protection.


Remember to exercise caution and adhere to best practices in smart contract development to prevent vulnerabilities   
and potential exploits.


