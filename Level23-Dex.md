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

