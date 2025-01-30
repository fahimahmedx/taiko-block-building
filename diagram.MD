```mermaid
sequenceDiagram
participant Proposer
participant ChainSyncer
participant TaikoGeth
participant Prover
participant TaikoL1
participant TaikoL2


Proposer->>+Proposer: Set a timer between 12 - 120 seconds
Proposer->>+TaikoGeth: fetchPoolContent()
TaikoGeth-->>Proposer: txLists
Proposer->>+TaikoL1: proposeBlocksV2()
TaikoL1->>+TaikoL1: Validate Block Parameters and store onchain
TaikoL1-->>ChainSyncer: event BlockProposedV2
TaikoL1-->>Prover: event BlockProposedV2
note over ChainSyncer,TaikoGeth: insertNewHead()
rect LightYellow
ChainSyncer->>+ChainSyncer: Insert anchor tx to txList
ChainSyncer->>+TaikoGeth: Create and execute payload of txs
TaikoGeth-->>+ChainSyncer: payload
end
TaikoL2->>+TaikoL2: event AnchorV2
Prover->>+Prover: Listens for BlockProposedV2 event and requests proof if needed
Prover->>+TaikoL1: Bond
Prover->>+TaikoL1: Submits proof via proveBlock()
Proposer->>+TaikoL1: verifyBlocks()
TaikoL1->>+TaikoL1: Block is verified, bond of original prover is released
TaikoL1-->>Proposer: event BlockVerifiedV2
Proposer->>+Proposer: logs the block was verified
```

ChainSyncer, Proposer, and Prover are part of taiko-client, which is the consensus client. Taiko-Geth is the execution client, and TaikoL1 (deployed on Ethereum) & TaikoL2 (deployed on Taiko) are smart contracts.
