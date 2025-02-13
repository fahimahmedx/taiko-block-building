```mermaid
sequenceDiagram
participant Proposer
participant ChainSyncer
participant TaikoGeth
participant Prover
participant TaikoL1 Smart Contract
participant TaikoL2 Smart Contract


Proposer->>+Proposer: Set a timer between 12 - 120 seconds
Proposer->>+TaikoGeth: Fetch transactions from mempool via fetchPoolContent()
TaikoGeth-->>Proposer: txLists
Proposer->>+TaikoL1 Smart Contract: proposeBatch() to propose multiple blocks and select prover from Prover Market
TaikoL1 Smart Contract->>+TaikoL1 Smart Contract: Validate parameters of blocks in the batch, and store batch onchain
Prover->>+TaikoL1 Smart Contract: Lock 125 Taiko Bond per Block until proof is submitted.
TaikoL1 Smart Contract-->>+TaikoL1 Smart Contract: event BondDebited
TaikoL1 Smart Contract-->>ChainSyncer: event BatchProposed
note over ChainSyncer,TaikoGeth: insertNewHead() to insert block into Taiko
rect LightYellow
ChainSyncer->>+ChainSyncer: Insert anchor tx (used for syncing information between L1 and L2) to txList
ChainSyncer->>+TaikoGeth: Create payload for the block and insert it into Taiko chain via Engine API
TaikoGeth-->>+ChainSyncer: payload
end
TaikoGeth->>+TaikoL2 Smart Contract: Block is now inserted into Taiko, anchor tx in block executes
TaikoL2 Smart Contract->>+TaikoL2 Smart Contract: event AnchorV3
note over TaikoGeth,TaikoL1 Smart Contract: Block Proving process. Waits for a BatchProposed event from the block building process.
rect LightYellow
Prover->>+Prover: Listens for BatchProposed event for blocks it is assigned to prove, and generates proof for the batch of blocks.
Prover->>+TaikoL1 Smart Contract: Lock 25 TAIKO Bond per block until proof can no longer be contested.
TaikoL1 Smart Contract-->>+Prover: event BondDebited
Prover->>+TaikoL1 Smart Contract: Submits proof via proveBatch()
TaikoL1 Smart Contract-->>+Prover: Unlock 125 TAIKO Bond and 
TaikoL1 Smart Contract-->>+TaikoL1 Smart Contract: emit BondCredited
end
note over Proposer,TaikoL1 Smart Contract: Block verification process.
rect LightYellow
Proposer->>+TaikoL1 Smart Contract: verifyBatches()
TaikoL1 Smart Contract->>+TaikoL1 Smart Contract: Batch is verified
TaikoL1 Smart Contract-->>Proposer: event BatchesVerified
TaikoL1 Smart Contract-->>+Prover: Unlock 25 TAIKO Bond per block
TaikoL1 Smart Contract-->>+TaikoL1 Smart Contract: emit BondCredited
Proposer->>+Proposer: logs the block was verified
end
```

ChainSyncer, Proposer, and Prover are part of taiko-client, which is the consensus client. Taiko-Geth is the execution client, and TaikoL1 (deployed on Ethereum) & TaikoL2 (deployed on Taiko) are smart contracts.
