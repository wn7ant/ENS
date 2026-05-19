# ENS — Encrypted Neural Swarm

**ENS** is a prototype architecture and partial Rust implementation for a **Paillier-based encrypted compute swarm** with **blockchain-audited model-state updates**.

The project explores how encrypted inputs can be processed by untrusted or semi-trusted swarm nodes while model state, weight updates, signatures, and state hashes are tracked through a blockchain-style audit layer.

ENS is not currently a production system, a complete install-and-run framework, or a finished distributed AI platform. This repository contains architecture notes, protocol sketches, JSON/schema material, and partial Rust implementation examples.

---

## Project Summary

ENS combines several known primitives into one experimental architecture:

- Paillier-style additive homomorphic encryption
- distributed swarm-node computation
- signed model-state updates
- deterministic model-state hashing
- blockchain-style audit trails
- Tendermint/ABCI-inspired transaction validation
- Rust-based implementation sketches and utilities

The intended system flow is:

```text
encrypted client input
→ swarm-node encrypted computation
→ Model Keeper aggregation/update
→ signed model-state hash
→ blockchain-audited update record
```

The long-term goal is to explore whether small, distributed nodes can participate in privacy-preserving computation without directly seeing the underlying plaintext data.

---

## Current Status

This repository is currently a **design/prototype repository**.

It includes:

- high-level ENS architecture notes
- node role definitions
- communication and transaction design
- JSON Schema and OpenAPI sketches
- partial Rust modules for:
  - Paillier key generation
  - encrypted compute examples
  - model update structures
  - model-state hashing
  - JSON and SQLite weight storage
  - signed batch updates
  - ABCI-style transaction verification
- early protocol sketches for Model Keeper training rounds
- notes on Tendermint-style blockchain integration

It does **not** yet provide:

- a complete Cargo workspace
- a production-ready node implementation
- a complete peer-to-peer swarm runtime
- a fully implemented training pipeline
- production security guarantees
- compliance guarantees
- full homomorphic encryption
- encrypted comparison, ranking, or nonlinear neural-network operations

---

## What ENS Is

ENS is an experimental architecture for exploring this question:

> Can encrypted inputs be distributed across a swarm of compute nodes, processed without exposing plaintext, and tied to an auditable model-state ledger?

In the current design:

- **Clients** encrypt input data.
- **Swarm nodes** perform limited homomorphic computation over ciphertexts.
- **Model Keepers** coordinate model state, training rounds, and weight updates.
- **Blockchain nodes** audit and verify signed model-state changes.
- **State hashes** provide deterministic references to model versions.

The blockchain layer is not intended to run AI computation directly. Its purpose is to provide an auditable control plane for model-state changes.

---

## What ENS Is Not

ENS is not currently:

- a finished AI framework
- a replacement for TensorFlow, PyTorch, or existing ML runtimes
- a complete blockchain network
- a production privacy system
- a HIPAA-compliant system by default
- a defense-ready system
- a full homomorphic encryption platform
- a general-purpose encrypted neural-network runtime

ENS should currently be understood as a prototype architecture and partial implementation.

---

## Core Components

### Client Node

The client node is responsible for:

- generating or holding the private decryption key
- encrypting input data
- sending encrypted inputs to swarm nodes
- receiving encrypted outputs
- decrypting final results when appropriate

The client is the primary trust anchor for plaintext input data.

### Swarm Node

A swarm node is a compute worker.

It is intended to:

- receive encrypted input values
- perform limited homomorphic operations
- apply plaintext scalar weights where appropriate
- return encrypted partial results
- avoid learning the underlying plaintext input

Swarm nodes are designed to be stateless or minimally stateful where possible.

### Model Keeper

The Model Keeper is responsible for model coordination.

It may:

- maintain current model weights
- coordinate training rounds
- dispatch encrypted compute tasks
- aggregate results
- update model weights
- compute model-state hashes
- sign model updates
- submit updates to the audit ledger

The current architecture treats the Model Keeper as a coordinating component, not as a fully decentralized consensus system by itself.

### Blockchain / Audit Layer

The blockchain-style layer is used to track and verify:

- model IDs
- model versions
- signed update requests
- model-state hashes
- authorship
- timestamps
- transaction validity

The audit layer is intended to make model-state changes inspectable and difficult to silently rewrite.

---

## Cryptographic Limits

ENS currently uses Paillier-style additive homomorphic encryption concepts.

Paillier is suitable for operations such as:

- encrypted addition
- accumulation of encrypted values
- multiplication of encrypted values by plaintext scalars

Paillier does **not** directly provide:

- arbitrary encrypted computation
- native encrypted comparison
- encrypted ranking
- encrypted nonlinear activation functions
- full neural-network inference by itself
- full homomorphic encryption

Any future support for comparison, ranking, nonlinear activation, or more complex encrypted training logic would require additional protocols, approximations, leakage-aware design, or different cryptographic primitives.

---

## Implementation Scope

### Implemented or Partially Implemented

This repository currently includes partial implementation material for:

- Paillier key generation
- encrypted compute round-trip examples
- Rust structures for batch model updates
- deterministic model-state hashing examples
- JSON model-weight storage
- SQLite model-weight storage
- signed model update sketches
- ABCI-style transaction validation sketches
- JSON Schema and OpenAPI draft material

### Architectural / Design-Stage

The following areas are currently design-stage:

- full swarm-node lifecycle
- Model Keeper orchestration
- Tendermint integration
- decentralized discovery
- training-round coordination
- encrypted multi-node aggregation
- long-running peer-to-peer operation

### Not Currently Implemented

The following are not currently implemented:

- production deployment
- full neural-network training
- encrypted comparison/ranking
- nonlinear activation over encrypted values
- complete adversarial security model
- production identity and access management
- compliance controls
- hardened key management
- robust distributed fault handling

---

## Threat Model Draft

This prototype assumes:

- clients retain private decryption keys
- swarm nodes may be untrusted or semi-trusted
- swarm nodes should not learn plaintext inputs
- model update authorship is verified by signatures
- blockchain-style state tracking provides an audit trail for model updates
- deterministic state hashing can identify specific model versions

This prototype does not yet fully address:

- malicious Model Keepers
- malicious key distribution
- collusion between nodes
- side-channel leakage
- traffic analysis
- denial-of-service attacks
- compromised clients
- compromised signing keys
- production authorization policy
- regulated deployment requirements

This threat model is incomplete and should be treated as a starting point.

---

## Repository Guide

The repository currently contains a mixture of architecture documents, protocol notes, schema drafts, and Rust implementation sketches.

Suggested reading order:

1. High-level architecture
2. Node roles
3. Encryption layer architecture
4. Communication protocol design
5. Transaction structures
6. Model Keeper training round protocol
7. Model-state hashing
8. Batch model update structures
9. ABCI behavior and transaction verification
10. Storage examples
11. Paillier compute examples

The intended conceptual path is:

```text
client encrypts input
→ swarm node computes over ciphertext
→ Model Keeper coordinates model state
→ update is signed
→ state hash is recorded
→ blockchain layer verifies/audits the update
```

---

## Planned Repository Structure

The repository is being moved toward a clearer structure:

```text
ENS/
  README.md
  LICENSE

  docs/
    architecture/
      high-level-architecture.md
      node-roles.md
      encryption-layer.md
      blockchain-layer.md

    protocols/
      communication-protocol.md
      training-round-protocol.md
      transaction-types.md
      node-bootstrapping-discovery.md

    tendermint/
      tendermint.md
      multiple-nodes.md
      abci-app-behavior.md
      check-deliver-tx.md
      state-query-support.md

  schemas/
    ens_schemas.json
    ens_openapi_3.yaml
    api-json-schema.md

  rust/
    batch-model-updates.md
    model-state-hashes.md
    sqlite-storage.md
    json-storage.md
    paillier-keygen.md
    swarm-compute-paillier.md
    client-demo-roundtrip.md
    cli-sign-submit.md

  examples/
    sample-block.md
    test-transaction.md
```

This structure is not yet final, but it reflects the intended organization of the project.

---

## Roadmap

### Phase 1: Repository Cleanup

- clarify project scope
- organize architecture documents
- organize Rust examples
- document cryptographic limits
- identify which files are design notes versus implementation code

### Phase 2: Buildable Rust Workspace

- create a Cargo workspace
- separate reusable crates
- add unit tests
- add deterministic state-hash tests
- add signature verification tests
- add Paillier encrypted weighted-sum example

Possible workspace structure:

```text
crates/
  ens-types/
  ens-crypto/
  ens-model-state/
  ens-swarm-node/
  ens-model-keeper/
  ens-abci-app/
  ens-cli/
```

### Phase 3: Vertical Proof of Concept

Build the smallest end-to-end demonstration:

```text
client encrypts sample inputs
→ swarm node computes encrypted weighted sum
→ client decrypts result
→ Model Keeper signs weight update
→ ABCI app verifies update
→ model-state hash is recorded
```

### Phase 4: Distributed Prototype

- multiple swarm nodes
- basic discovery
- task dispatch
- result aggregation
- state synchronization
- signed model updates
- local testnet deployment

---

## Example Use Cases

ENS is not production-ready, but the architecture may eventually be useful for exploring:

- privacy-preserving distributed computation
- auditable model updates
- small-node encrypted compute
- local-first encrypted AI experiments
- verifiable model-state transitions
- decentralized or semi-decentralized compute coordination
- privacy-preserving research prototypes

Potential medical, defense, commercial, or regulated uses would require separate security analysis, compliance review, deployment controls, access controls, audit procedures, and legal review.

ENS should not be treated as compliant, certified, production-safe, or deployment-ready by default.

---

## Development Notes

The current Rust material should be treated as implementation sketches unless otherwise marked.

Important technical issues still need to be resolved, including:

- replacing floating-point model weights with deterministic fixed-point or integer representations
- defining canonical serialization for model-state hashing
- formalizing transaction formats
- formalizing key ownership and update authority
- defining node identity and authorization
- separating trusted and untrusted roles
- specifying how Model Keepers are selected, trusted, or replaced
- determining how training math works within Paillier’s limitations

---

## License

ENS is offered under a dual-license model.

### Non-commercial public use

This repository is publicly available for non-commercial use, including reading, studying, cloning, experimenting, academic review, personal research, and non-commercial prototyping.

You may use the public version of this work to understand the ENS architecture, evaluate the design, build non-commercial prototypes, and contribute improvements back to the project.

You may not use this work, in whole or in part, for commercial products, paid services, proprietary platforms, defense applications, government contracting, or revenue-generating systems without a separate written commercial license.

### Commercial licensing

Commercial use requires a separate license agreement.

This includes, but is not limited to:

- selling ENS-based software or services
- integrating ENS concepts or code into a commercial product
- using ENS in paid consulting, contracting, or managed services
- deploying ENS as part of a business, government, defense, intelligence, or industrial system
- using ENS to support proprietary AI, security, routing, blockchain, or distributed-computation platforms

For commercial licensing, contact the project owner.

### Contributions

Contributions are welcome, but all contributions must be made with clear licensing expectations.

By submitting a pull request, patch, issue comment containing code, documentation, design material, schemas, examples, or other project content, you agree that your contribution may be incorporated into ENS and distributed under the project’s current and future licensing model, including both the non-commercial public license and separate commercial licenses.

Do not submit contributions unless you have the right to provide them under these terms.

Contributors retain copyright to their own contributions unless otherwise agreed in writing, but grant the project owner a broad, perpetual, worldwide, royalty-free license to use, modify, distribute, sublicense, and relicense those contributions as part of ENS.

### No warranty

ENS is experimental research and prototype work. It is provided as-is, without warranty of any kind.

The project makes no guarantee of correctness, security, cryptographic safety, fitness for production use, regulatory compliance, or suitability for sensitive systems.

Do not use ENS in production, safety-critical, medical, financial, defense, infrastructure, or high-risk environments without independent review, testing, and a separate written license.

This section summarizes the intended licensing model. A formal `LICENSE` file and `CONTRIBUTING.md` file will be added before accepting outside contributions.

---

## Author

Created by Everett Vinzant.

ENS is an experimental research and prototype project exploring encrypted distributed computation, model-state auditability, and privacy-preserving swarm architecture.

---

## Disclaimer

This repository is for research, learning, architecture exploration, and prototype development.

It is not production-ready software.

It does not provide legal, medical, compliance, defense, or security guarantees.

Do not use ENS to process sensitive, regulated, classified, medical, financial, or production data without substantial additional engineering, review, testing, and legal/compliance analysis.
