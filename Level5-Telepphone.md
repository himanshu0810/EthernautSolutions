# Level 5 Challenge: Mastering Contract Ownership

Welcome to Level 5, where we embark on a journey to unravel the intricacies of contract ownership. Our goal is to crack this level and claim ownership of the coveted contract. To achieve this, we'll delve into the workings of `tx.origin` and `msg.sender`, two key elements in Ethereum smart contracts.

## Understanding `tx.origin`:

`tx.origin` represents the original sender of the transaction. In simpler terms, it is the external account that initiated the entire transaction. Keep in mind that using `tx.origin` can have security implications and is generally discouraged in modern Ethereum development.

## Understanding `msg.sender`:

`msg.sender` denotes the account that triggered the current function call within the contract. It is crucial in determining the origin of a specific function invocation.

To crack this level, we'll employ a clever strategy involving two contracts: **Contract 1** and **Contract 2**.

## Strategy Overview:

1. **Understand `tx.origin` and `msg.sender`**: Familiarize yourself with the roles these elements play in contract ownership.

2. **Security Considerations**: Be cautious when using `tx.origin` due to potential security implications. Modern Ethereum development discourages its use.

3. **Two-Contract Strategy**: Employ a clever strategy involving two contracts - **Contract 1** and **Contract 2** - to claim ownership of the target contract.

## Implementation:

- Dive into the code for **Contract 1** and **Contract 2** to see how they interact using `tx.origin` and `msg.sender`.

- Explore how the strategy leverages these elements to secure contract ownership.


## Clever Maneuver:

When the call reaches the targeted contract, `msg.sender` and `tx.origin` will be distinct. `tx.origin` will point to Contract 1, while `msg.sender` will refer to Contract 2. This clever maneuver allows us to bypass the condition in the `changeOwner` function, enabling us to modify the owner successfully.

Click the "Submit Instance" button, check the owner, and get ready to move to the next level. Secure your socks, for the journey to mastery continues!
