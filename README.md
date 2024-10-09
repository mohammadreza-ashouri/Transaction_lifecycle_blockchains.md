# Stages of Transaction Lifecycle in Ethereum

### Table 1: Block and Transactions’ Attributes of the Ethereum Data

| **Attribute**             | **Description**                                                                                                                                  |
|---------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------|
| **Block Information**     |                                                                                                                                                  |
| **name**                  | A unique block identifier                                                                                                                        |
| **nonce**                 | A hash of proof-of-work                                                                                                                          |
| **hash**                  | A unique hash of the block                                                                                                                       |
| **miner**                 | A beneficiary address who receives mining reward                                                                                                 |
| **totalDifficulty**       | Indicating the total difficulty of the chain up to a specified block by an integer value                                                         |
| **difficulty**            | Specifying the difficulty level by an integer value                                                                                              |
| **extraData**             | A field containing additional data from a block                                                                                                  |
| **size**                  | The block size in bytes                                                                                                                          |
| **gasUsed**               | Total gas used by all transactions in a block                                                                                                    |
| **gasLimit**              | Maximum gas usage of all transactions in a block                                                                                                 |
| **timestamp**             | A UNIX timestamp when blocks were constructed                                                                                                    |
| **transactions**          | Unique ID of the transaction or a hash array of 32-byte transactions                                                                              |
| **uncles**                | Uncle block hashes array                                                                                                                         |
|                           |                                                                                                                                                  |
| **Transactional Information** |                                                                                                                                             |
| **nonce**                 | Before that transaction, total transactions made by similar sender                                                                               |
| **hash**                  | A unique transaction hash                                                                                                                        |
| **blockNumber**           | A unique block number for the committed transaction block                                                                                        |
| **blockHash**             | A unique hash for the committed transaction block                                                                                                |
| **from**                  | A unique hash string considered as sender’s address                                                                                              |
| **to**                    | A unique hash string considered as receiver’s address, resulted null if creating contract is the purpose of received transaction                 |
| **value**                 | The transferred amount in Wei where Wei is the unit of Ethereum                                                                                  |
| **gasPrice**              | Sender provided gas price in Wei                                                                                                                 |
| **gas**                   | Sender provided gas amount                                                                                                                       |
| **input**                 | Extra data sent with the transaction                                                                                                             |



## 1. Transaction Creation

**Explanation**: A user constructs a transaction by specifying the recipient's address, the amount of Ether to transfer, and sets parameters like gas limit and gas price.

**Pseudocode**:
```javascript
const transaction = {
  to: "0xRecipientAddress",
  value: ethers.utils.parseEther("1.0"), // Amount in Ether
  gasLimit: ethers.utils.hexlify(21000), // Standard gas limit for Ether transfer
  gasPrice: ethers.utils.parseUnits("50", "gwei"), // Gas price in Gwei
  nonce: currentNonce, // Must be fetched from the network for the sender's address
  data: "0x", // Optional data field (empty for Ether transfer)
};
```

## 2. Transaction Signing
**Explanation**: The user signs the transaction with their private key to ensure authenticity and prevent unauthorized modifications.

**Pseudocode**:
```javascript
const wallet = new ethers.Wallet(privateKey); // Initialize wallet with private key
const signedTransaction = await wallet.signTransaction(transaction); // Sign the transaction
```
