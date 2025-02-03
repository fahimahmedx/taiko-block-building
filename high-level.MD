```mermaid
sequenceDiagram
participant Proposer
participant Prover
participant TaikoL1


Proposer->>+Proposer: Builds a block with pending transactions from Taiko's mempool.
Proposer->>+TaikoL1: Propose the block
TaikoL1->>+TaikoL1: Validate the proposed block
Prover->>+TaikoL1: Submits proof for proposed block
Prover->>+TaikoL1: Locks Taiko as a Bond for the proof
Proposer->>+TaikoL1: Verifies the block.
TaikoL1->>+Prover: Block is verified, bond of original prover is released
```

Proposer and Prover are part of taiko-client, which is the consensus client. Taiko-Geth is the execution client, and TaikoL1 (deployed on Ethereum) is a smart contract.
