# Stages of Transaction Lifecycle in Ethereum

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
