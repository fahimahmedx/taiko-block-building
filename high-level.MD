```mermaid
sequenceDiagram
participant Proposer
participant Prover
participant TaikoL1


Proposer->>+Proposer: 1. Builds a block with pending transactions from Taiko's mempool.
Proposer->>+TaikoL1: 2. Propose the block, and locks 125 TAIKO until block is proved
Prover->>+TaikoL1: 3. Submits proof for proposed block, and locks 25 TAIKO until block is verified
TaikoL1-->>+Proposer: 4. 125 TAIKO bond is returned
Proposer->>+TaikoL1: 5. Verifies the block.
TaikoL1-->>+Prover: 6. 25 TAIKO bond is released
```

Proposer and Prover are part of taiko-client, which is the consensus client. Taiko-Geth is the execution client, and TaikoL1 (deployed on Ethereum) is a smart contract.
