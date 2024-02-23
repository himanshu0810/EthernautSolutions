# Cracking a Solidity Contract: Denial Attack

In the realm of smart contracts, vulnerabilities often lurk in the shadows, waiting for astute eyes to uncover them. In this tutorial, we'll delve into a clever exploitation of a contract known as Denial.

## Understanding the Denial Contract

The Denial contract is designed to facilitate withdrawals, splitting the proceeds between a designated partner and the contract owner. Here's a brief overview of its functionality:

- The contract features a `partner` address, who collaborates in withdrawals.
- Withdrawals split 1% of the total balance between the partner and the owner.
- Anyone can become a partner by calling `setWithdrawPartner`.
- Withdrawals are performed through the `withdraw` function.

## The Vulnerability

The crux of the vulnerability lies in the withdrawal process. When `withdraw` is called, it transfers funds to the partner first, then to the owner. However, if the owner calls `withdraw`, there's an opportunity to manipulate the execution flow to prevent the transfer to the owner, effectively denying them their share.

## Crafting the Exploit

To exploit this vulnerability, we need to block the transfer to the owner's account. Since the contract doesn't check the return value of the `call` function, a straightforward return won't suffice. Instead, we utilize the fact that `partner.call{value:amountToSend}("");` invokes the fallback function of the partner contract.

By creating a contract with a fallback function containing an infinite loop, we can exhaust all gas passed during the call, halting the execution before the transfer to the owner occurs.

## The DenialHack Contract

Let's take a look at the DenialHack contract:

    ```solidity
    import "../instances/Ilevel20.sol";
    
    contract DenialHack {
        Denial level20 = Denial(0x1bd442053Af3e571eBbe11809F3cd207A0466A45);
    
        constructor() public {
            level20.setWithdrawPartner(address(this));
        }
    
        receive() external payable {
            while (true) {}
        }
    }

## Deploying the Exploit
Deploy the DenialHack contract.
The constructor sets the deploying address as the partner.
The fallback function contains an infinite loop, consuming all gas.


## Conclusion
With the DenialHack contract deployed, any attempt by the owner to withdraw funds will result in gas exhaustion, preventing the transfer to the owner. This exploit demonstrates the importance of thorough auditing and understanding of contract interactions in ensuring robust smart contract design.

Now armed with this knowledge, proceed with caution, and always prioritize security in smart contract development.






