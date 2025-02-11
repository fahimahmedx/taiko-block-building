```mermaid
sequenceDiagram
participant Taiko Block Builder
participant Prover
participant Taiko Smart Contract on Ethereum


Taiko Block Builder->>+Taiko Block Builder: 1. Builds a block with pending transactions from Taiko's mempool.
Taiko Block Builder->>+Taiko Smart Contract on Ethereum: 2. Propose the block and select a prover for the block.
Prover->>+Taiko Smart Contract on Ethereum: 3. Lock 125 TAIKO and build proof.
Prover->>+Taiko Smart Contract on Ethereum: 4. Submits proof for proposed block, and locks 25 TAIKO until block is verified (proof can no longer be contested)
Taiko Smart Contract on Ethereum-->>+Prover: 5. 125 TAIKO bond is returned for submitting proof
Taiko Block Builder->>+Taiko Smart Contract on Ethereum: 6. Verifies the block.
Taiko Smart Contract on Ethereum-->>+Prover: 7. 25 TAIKO bond is released
```

Taiko Block Builder (Proposer) and Prover are part of taiko-client, which is the consensus client. Taiko-Geth is the execution client, and TaikoL1 (deployed on Ethereum) is a smart contract.
