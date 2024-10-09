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

## 3. Transaction Broadcasting
**Explanation**: The signed transaction is sent to an Ethereum node to be broadcasted to the network.
**Pseudocode**:
```javascript
const provider = ethers.getDefaultProvider("mainnet"); // Connect to Ethereum network
const txResponse = await provider.sendTransaction(signedTransaction); // Broadcast the transaction
```

## 4. Transaction Propagation
**Explanation**: The transaction propagates through the network as nodes share it with their peers.

## 5. Transaction Validation
**Explanation**: Each node that receives the transaction validates it by checking the signature, nonce, sender's balance, and gas parameters.
**Pseudocode**:
```javascript
function validateTransaction(tx) {
  if (
    verifySignature(tx) &&
    checkNonce(tx.nonce, tx.from) &&
    checkBalance(tx.from, tx.value + tx.gasLimit * tx.gasPrice) &&
    checkGasLimits(tx.gasLimit)
  ) {
    addToMempool(tx);
  } else {
    discardTransaction(tx);
  }
}

```

## 6. Inclusion in Mempool
**Explanation**: Validated transactions are stored in the mempool, a pool of pending transactions awaiting inclusion in a block and nodes automatically handle mempool management.

## 7. Transaction Selection by Validators
**Explanation**: Validators select transactions from the mempool, often prioritizing those offering higher gas prices to maximize their rewards.

**Pseudocode**:
```javascript
function selectTransactionsForBlock(mempool) {
  // Sort transactions by gas price in descending order
  const sortedTransactions = mempool.sort((a, b) => b.gasPrice - a.gasPrice);
  const transactionsToInclude = [];
  let totalGasUsed = 0;

  for (const tx of sortedTransactions) {
    if (totalGasUsed + tx.gasLimit <= blockGasLimit) {
      transactionsToInclude.push(tx);
      totalGasUsed += tx.gasLimit;
    } else {
      break; // Stop if gas limit for the block is reached
    }
  }
  return transactionsToInclude;
}
```

## 8. Block Proposal and Creation
**Explanation**:  A validator assembles the selected transactions into a new block and proposes it to the network.

**Pseudocode**:
```javascript
const transactions = selectTransactionsForBlock(mempool);
const block = createNewBlock(transactions, previousBlockHash, validatorAddress);
broadcastBlock(block);
```

## 9. Block Validation and Consensus
**Explanation**:  Other validators verify the block's validity and, through the consensus mechanism, agree to add it to the blockchain.

**Pseudocode**:
```javascript
function validateBlock(block) {
  if (
    verifyBlockHeader(block.header) &&
    verifyTransactions(block.transactions) &&
    verifyStateTransitions(block)
  ) {
    return true;
  } else {
    return false;
  }
}

if (validateBlock(receivedBlock)) {
  addBlockToChain(receivedBlock);
  broadcastAcceptance(receivedBlock);
} else {
  rejectBlock(receivedBlock);
}
```

## 10. Block Finalization
**Explanation**:  The block becomes finalized after a supermajority of validators attest to its validity, making included transactions irreversible. Finalization is managed by Ethereum's Proof of Stake consensus mechanism.

## 11. State Update and Confirmation
**Explanation**:  The blockchain state is updated to reflect the transactions, and users can see their transaction as confirmed once it's included in the finalized block.



# Ethereum Transaction Lifecycle Chart

The following diagram illustrates the 11 stages of the Ethereum transaction lifecycle in a circular manner:

```mermaid
flowchart TD
    %% Define the nodes
    A[1. Transaction Creation]
    B[2. Transaction Signing]
    C[3. Transaction Broadcasting]
    D[4. Transaction Propagation]
    E[5. Transaction Validation]
    F[6. Inclusion in Mempool]
    G[7. Transaction Selection by Validators]
    H[8. Block Proposal and Creation]
    I[9. Block Validation and Consensus]
    J[10. Block Finalization]
    K[11. State Update and Confirmation]

    %% Arrange nodes in a circle by linking them in a loop
    A --> B
    B --> C
    C --> D
    D --> E
    E --> F
    F --> G
    G --> H
    H --> I
    I --> J
    J --> K
    K --> A

    %% Style the nodes (optional)
    classDef stage fill:#f9f,stroke:#333,stroke-width:2px
    class A,B,C,D,E,F,G,H,I,J,K stage

