## Foundry

**Foundry is a blazing fast, portable and modular toolkit for Ethereum application development written in Rust.**

Foundry consists of:

-   **Forge**: Ethereum testing framework (like Truffle, Hardhat and DappTools).
-   **Cast**: Swiss army knife for interacting with EVM smart contracts, sending transactions and getting chain data.
-   **Anvil**: Local Ethereum node, akin to Ganache, Hardhat Network.
-   **Chisel**: Fast, utilitarian, and verbose solidity REPL.

## Documentation

https://book.getfoundry.sh/

## Usage

### Build

```shell
$ forge build
```

### Test

```shell
$ forge test
```

### Format

```shell
$ forge fmt
```

### Gas Snapshots

```shell
$ forge snapshot
```

### Anvil

```shell
$ anvil
```

### Deploy

```shell
$ forge script script/Counter.s.sol:CounterScript --rpc-url <your_rpc_url> --private-key <your_private_key>
```

### Cast

```shell
$ cast <subcommand>
```

### Help

```shell
$ forge --help
$ anvil --help
$ cast --help
```

## Importing from Chainlink

To import dependencies from Chainlink for use in our Contract, we use this command
```
forge install smartcontractkit/chainlink-brownie-contracts@0.6.1 --no-commit
```

Next, we go to our `foundry.toml` file and use `remappings` to redirect our imports to the location of the relevant file imports on our local machine. This is because sometimes, when we download dependencies on our local machine, the file path may vary from what the github repo is. We remap in `foundry.toml` using the following command
```
remappings = ["@chainlink/contracts/src/v0.8/vrf=lib/chainlink-brownie-contracts/contracts/src/v0.8"]
```

## Link Tokens for Chainlink Subscription
The `LinkToken.sol` file in `test/mocks` directory was created to enable us fund our subscription. Use the following command to download dependency files for the `LinkToken.sol`
```bash
forge install transmissions11/solmate --no-commit
```

We then edit `remappings` in our `foundry.toml` file to enable us pick the dependency on our local machine without changing the file path on our `LinkToken.sol` file. Our `remappings` now become

```
remappings = ["@chainlink/contracts/src/v0.8/vrf=lib/chainlink-brownie-contracts/contracts/src/v0.8", "@solmate=lib/solmate/src"]
```

## Install Foundry DevOps

The foundry devOps repo enables us to get the most recent deployment from a given environment in foundry. This way, we can do scripting off previous deployments in solidity.
```bash
forge install Cyfrin/foundry-devops --no-commit
```
Update your `foundry.toml` to have read permissions on the broadcast folder.
```
fs_permissions = [{ access = "read", path = "./broadcast" }]
```


Tasks Carried Out:
Note that the test script is `SocialMediaTest.t.sol`
1. Modified the `createPost` function to allow only registered users create a post.
2. Wrote a test `testUserCantCreatePostIfNotRegistered` to certify that a user can not create post if not registered. The test reverts as expected - test passed.
3. Created a mapping `s_userAddressToPostId` which maps every post to the author of that post. 
4. Wrote a test `testRegisteredUserCanCreatePost` to certify that a registered user can create post. The test passed.
5. Wrote a test `testRegisteredUserCanCreatePost` to certify that a registered user can create post. The test passed.
6. Improve test coverage from 33% to 49%. Target is for test coverage to be at least 90%


Tasks Carried Out:
Note that the test script is `SocialMediaTest.t.sol`
1. Introduced the `receive` function in the Smart Contract. This is to enable Smart Contract receive payment from users who want to edit or delete their posts. This feature is introduced to reduce misuse of platform from users.
2. Introduced the Chainlink Price Feed. This is useful so that for instance, if we peg the fee for deleting of posts as USD5, the Chainlink price feed will enable us know the required amount of ETH or any other asset that is worth 5 dollars using the current price.
3. Installed the `chainlink-brownie-contracts` to enable us use the chainlink price feed.
4. Added a `remappings` section in our `foundry.toml` file to ensure our contract can read the dependency files download to our project repository from chainlink-brownie-contracts.
5. Wrote a test `testCantEditPostIfNotTheOwner` to certify that a user can not edit a post if he is not the author. The test passed.
6. Wrote a test `testOwnerCantEditPostWithoutPaying` to certify that a user can not edit their post if they don't pay the required amount. The test passed.
7. Wrote a test `testOwnerCanEditPostAfterPaying` to certify that a user can edit their post if they pay the required amount. The test passed.
8. Wrote a test `testEventEmitsWhenPostIsEdited` to certify that an event will be emited whenever user edits their post. The test passed.