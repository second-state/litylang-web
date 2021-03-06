+++
title = "Free Gas"
date = 2018-06-22T15:00:50+08:00
weight = 8
chapter = true
disabletoc = false
+++

# "Free" Gas

One of the major hurdles of blockchain application adoption is that end users are asked to pay a `gas` fee in order to perform certain functions on the blockchain. The gas mechanism is crucial for the blockchain's security, as it prevents DoS attackers from overwhelming the blockchain nodes with computationally intensive requests. However, the gas requirement also means that new end users must be taught to purchase cryptocurrencies and manage private keys before they can even start to use decentralized applications.

Lity provides an alternative approach to onboard new users to blockchain applications. Through the `freegas` keyword, the smart contract owner can designate a contract function should have gas fees paid by the owner herself. When the user calls those functions, she would indicate that she is not paying gas by setting the `gasPrice` to 0.

* If the `gasPrice=0` transaction calls a function that is NOT `freegas`, the transaction would fail.
* If the `gasPrice=0` transaction calls a `freegas` function in a contract that does not have a balance, the transaction would fail.

> Of course, the caller can specify a regular `gasPrice` (e.g., 2Gwei), and in that case, the caller pays for gas 
> even if the contract function is `freegas`.

If the `gasPrice=0` transaction calls a `freegas` function in a contract that has sufficient balance, the function executes and the gas fee is deducted from the contract address. The caller pays nothing, and the contract pays gas at the system's standard gas price.

> There is a rate limit for making `gasPrice=0` transactions. The blockchain node software prevents 
> users from exploiting the system by sending a lot of free transactions.

Below is an example. On the CyberMiles blockchain, if an end user calls the `test` function with `gasPrice=0` and the contract address has a CMT balance, the transaction's gas fee will come out of the contract address.

```
pragma lity >= 1.2.7;

contract FreeGasDemo {
  int a;
  function test (int input) public freegas returns (int) {
    a = input;
    return a;
  }

  function () public payable {}
}
```

The `payable` function is important as it allows the contract to receive CMTs that will later be used as gas.

The screen shots below show the free gas contract function in action.

1 Compile and deploy the contract using Europa IDE and Venus Wallet on CyberMiles blockchain.

![Compile](compile.png)

2 Fund some CMTs to be used as gas fees to the contract address.

![Fund](fund.png)

3 Call the contract function with zero `gasPrice` and the user account balance stays the same. The gas fee is paid from the contract's address.

![Execute](execute.png)

[Read more](https://lity.readthedocs.io/en/latest/freegas.html) about the `freegas` transactions.

