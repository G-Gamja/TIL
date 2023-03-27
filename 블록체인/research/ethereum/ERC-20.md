Here’s a brief rundown of how the mandatory standards apply to the creation of tokens.

TotalSupply: Outlines the total number of tokens to be created.

Approve: Helps to eliminate the possibility of counterfeit tokens being created by requiring approval of smart contract functions.

BalanceOf: Allows users to check their balances by returning the total number of tokens held by an address.

TransferFrom: Allows for the automation of transactions when desired.

Transfer: Allows for the transfer of tokens from one address to another, like any other blockchain-based transaction.

Allowance: When a smart contract wants to execute a transaction, it has to be able to see the balance held by the Ethereum wallet trying to transact. The allowance function allows the contract to carry out the transaction if the user has sufficient balance or cancel the transaction if they do not.

These six rules must be programmed into a token for it to be considered ERC20. Without clear instructions for these rules or standards, the token wouldn’t be able to interact with smart contracts effectively, which could cause numerous issues.