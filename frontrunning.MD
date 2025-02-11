```mermaid
sequenceDiagram
participant Taiko Block Builder A
participant Taiko Block Builder B
participant Taiko Smart Contract on Ethereum


Taiko Block Builder A->>+Taiko Block Builder A: 1. Builds a block with pending transactions from Taiko's mempool.
Taiko Block Builder A->>+Taiko Smart Contract on Ethereum: 2. Propose the block.
Taiko Smart Contract on Ethereum-->>+Taiko Block Builder B: 3. Sees proposed block transaction on Ethereum mempool
Taiko Block Builder B->>+Taiko Block Builder B: 4. Builds its own block with pending transactions from Taiko's mempool.
Taiko Block Builder B->>+Taiko Smart Contract on Ethereum: 5. Propose its own block with a higher gas fee to frontrun existing block builder.
Taiko Smart Contract on Ethereum-->>+Taiko Block Builder B: 6. Collects most of the transaction fees.
Taiko Smart Contract on Ethereum-->>+Taiko Block Builder A: 7. Collects minimal transaction fees due to proposed transactions already being included.
```

Taiko Block Builder (Proposer) and Prover are part of taiko-client, which is the consensus client. Taiko-Geth is the execution client, and TaikoL1 (deployed on Ethereum) is a smart contract.
