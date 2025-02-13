```mermaid
sequenceDiagram
participant Proposer
participant ChainSyncer
participant TaikoGeth
participant Prover
participant TaikoL1
participant TaikoL2


Proposer->>+Proposer: Set a timer between 12 - 120 seconds
Proposer->>+TaikoGeth: Fetch transactions from mempool via fetchPoolContent()
TaikoGeth-->>Proposer: txLists
Proposer->>+TaikoL1: proposeBatch() to propose multiple blocks and select prover from Prover Market
Prover->>+TaikoL1: Lock 125 Taiko Bond per Block
TaikoL1-->>+Prover: event BondDebited
TaikoL1->>+TaikoL1: Validate Block Parameters and store batch onchain
TaikoL1-->>ChainSyncer: event BatchProposed
TaikoL1-->>Prover: event BatchProposed
note over ChainSyncer,TaikoGeth: insertNewHead() to insert block into Taiko
rect LightYellow
ChainSyncer->>+ChainSyncer: Insert anchor tx to txList
ChainSyncer->>+TaikoGeth: Create and execute payload of txs via Engine API
TaikoGeth-->>+ChainSyncer: payload
end
ChainSyncer-->>+TaikoL2: Block is now inserted into Taiko, anchor tx in block executes
TaikoL2->>+TaikoL2: event AnchorV3
note over TaikoGeth,TaikoL1: Block Proving process. Waits for a BatchProposed event from the block building process.
rect LightYellow
Prover->>+Prover: Listens for BatchProposed event and generates proof for proposed blocks
Prover->>+TaikoL1: Lock 25 TAIKO Bond per block
TaikoL1-->>+Prover: event BondDebited
Prover->>+TaikoL1: Submits proof via proveBatch()
TaikoL1-->>+Prover: Unlock 125 TAIKO Bond and emit BondCredited
end
note over Proposer,TaikoL1: Block verification process.
rect LightYellow
Proposer->>+TaikoL1: verifyBatches()
TaikoL1->>+TaikoL1: Batch is verified
TaikoL1-->>Proposer: event BatchesVerified
TaikoL1-->>+Prover: Unlock 25 TAIKO Bond per block and emit BondCredited
Proposer->>+Proposer: logs the block was verified
end
```

ChainSyncer, Proposer, and Prover are part of taiko-client, which is the consensus client. Taiko-Geth is the execution client, and TaikoL1 (deployed on Ethereum) & TaikoL2 (deployed on Taiko) are smart contracts.
