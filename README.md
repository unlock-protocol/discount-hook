# Discount Hook for Locks

This project implements an Unlock [PublicLock Hook](https://docs.unlock-protocol.com/core-protocol/public-lock/hooks) that can be used on Locks smart contracts to support "Discount Code".

This process is _secured_ and cannot be bypassed by calling the contract directly as the password is used to submit the transaction on-chain.

When the user enters a discount code on the frontend application, it is used to generate a private key that is then used to sign the recipient's address. That signature is passed as the data argument on the `purchase` call.

A lock manager can add any number of discount code to their lock by calling the function `setDiscountCodeForLock`.

The Unlock Protocol team has deployed and verified a version of this hook on the following networks:

Production networks:

- [Optimism](https://optimistic.etherscan.io/address/0x8e0B46ec3B95c81355175693dA0083b00fCc1326)
- [Polygon](https://polygonscan.com/address/0x93E160838c529873cB7565106bBb79a3226FE07A)

Test networks:

- [Goerli](https://goerli.etherscan.io/address/0x850c015A6A88756a59Dc025fca988494fF90DBB7)

## Example

[This lock](https://goerli.etherscan.io/address/0x2490f447fdb7b259bc454871806b6b794de65944) deployed is deployed on Goerli and uses this discount hook with 2 different discounts:

- `FRIENDS` for a 50% discount
- `FAMILY` for a 100% discount

This means you can only purchase a key if you go [through this checkout URL](https://app.unlock-protocol.com/checkout?paywallConfig=%7B%22locks%22%3A%7B%220x2490f447fdb7b259bc454871806b6b794de65944%22%3A%7B%22network%22%3A5%2C%22name%22%3A%22%22%2C%22captcha%22%3Afalse%2C%22password%22%3Afalse%2C%22promo%22%3Atrue%2C%22emailRequired%22%3Afalse%2C%22maxRecipients%22%3Anull%2C%22dataBuilder%22%3A%22%22%2C%22skipRecipient%22%3Afalse%7D%7D%2C%22pessimistic%22%3Atrue%2C%22skipRecipient%22%3Atrue%2C%22title%22%3A%22Try+Discounts%21%22%2C%22icon%22%3A%22%22%7D) and enter any of the 2 discount codes you will see a discounted price on the final confirmation screen!

If you don't enter a discount code, you will pay the full price of 0.01 Eth.

## Using the hook for your own lock

1. First, you need to generate promo codes, then [go to this page to generate the corresponding Ethereum address](https://unlock-protocol.github.io/password-required-hook/). You can also generate this locally if needed by checking out the repo and switching to the `gh-page` branch.

2. Then, click on which network your lock has been deployed on (list above) and head `Contract` > `Write Contract`. Connect your wallet (you need to be connected as a lock's manager) and click on `setDiscountForLock`. There, enter the lock address, and then the signer address generated in the previous step and the discount to be applied. Since the EVM does not support floating numbers, you have to enter the value in "basis points". For example for a 100% discount, you would enter `10000`. For a 3% discount, you would enter `300`.

3. Finally, you need to point your lock to the hook. You can do that by going to your lock's settings page on the Unlock Dashboard. Then check the Advanced tab and the Hooks section.

4. Finally, [build a Checkout URL](https://docs.unlock-protocol.com/tools/checkout/configuration) and make sure you tick `Promo Codes` option for the lock to which you are applying a discount!

## Dev

You can deploy the hook on other chains by adding the chain to the `hardhat.config.js` config file and calling:

```
yarn run hardhat run scripts/deploy.js --network my-network
```

To verify the contract on block explorers, call :

```
yarn run hardhat verify --network my-network 0xhook-address
```

Running tests:

```
yarn run hardhat test test/sample-test.js
```
