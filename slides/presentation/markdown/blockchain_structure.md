<!-- .slide: data-background-color="#8D3AED" -->

## Blockchain Structure

---

<!-- **Goal:** Evaluate the blockchain structure of this project; name at least two things you like and two things you would want to improve. -->

<!-- - What is the state transition function of this blockchain/project? -->

<!-- - What are the core elements of the application stack? -->

<!-- - What is the anatomy of a block in this system? -->

<!-- - What is the consensus algorithm that is used? How does it work? -->

## Overview

- Tendermint Core is the application-agnostic engine that handles the networking and consensus layers of a blockchain.
- In practice, this means that Tendermint is responsible for propagating and ordering transaction bytes.
- Tendermint passes transactions to the application through an interface called the ABCI (Application Blockchain Interface).
- The state is updated by the application based on the messages received from Tendermint core.


---

## State Transition Function

State transition functions are implemented in the application based on messages recieved from the Tendermint consensus engine.

- The main messages that are used in state transition are:
  - InitChain.
  - DeliverTx.
  - EndBlock.
  - Commit.

> The first time a new blockchain is started, Tendermint calls InitChain.
> From then on, the following sequence of methods is executed for each block:
> BeginBlock, [DeliverTx], EndBlock, Commit
> where one DeliverTx is called for each transaction in the block.
> The result is an updated application state
[Source](https://docs.tendermint.com/master/spec/abci/abci.html#block-execution)

---

### InitChain and EndBlock

- Updates the validator set.

> The application may set the validator set during InitChain,
> and may update it during EndBlock
> [source](https://docs.tendermint.com/master/spec/abci/apps.html#updating-the-validator-set)

> Each abci end block call, the operations to update queues and validator set changes are specified to execute
> [source](https://docs.cosmos.network/v0.45/modules/staking/05_end_block.html#end-block)

---

### DeliverTx

When a valid block is received by Tendermint Core, each transaction in the block
is passed to the application via DeliverTx in order to be processed.
It is during this stage that the state transitions occur.

[Source](https://docs.cosmos.network/main/intro/sdk-app-architecture.html)

---

### Commit

Commit signals the application to persist application state. It takes no parameters

[Source](https://docs.tendermint.com/master/spec/abci/abci.html#commit)

---

## Core elements of the application stack

<pre>
                ^  +-------------------------------+  ^
                |  |                               |  |   Built with Cosmos SDK
                |  |  State-machine = Application  |  |
                |  |                               |  v
                |  +-------------ABCI--------------+
                |  |                               |  ^
Blockchain node |  |           Consensus           |  |
                |  |                               |  |
                |  +-------------------------------+  |   Tendermint Core
                |  |                               |  |
                |  |           Networking          |  |
                |  |                               |  |
                v  +-------------------------------+  v

ABCI - Application Blockchain Interface

</pre>

The core element of the application includes:

- A state machine.
- Custom business logic.

---

## Anatomy of a block

- Header
  - Contains information used throughout consensus and other areas of the protocol, for example:
    - Address of the validator that proposed the block.
    - MerkleRoot of the current validator set.
    - BlockID of the previous block.
- Data
  - Data contains a list of transactions.
  - The contents of the transaction is unknown to Tendermint.
- Evidence.
  - Evidence contains a list of infractions committed by validators.
  - This is used for slashing.
  - No more than 1/10th of the maximum block size (ConsensusParams.Block.MaxBytes) of evidence with each block.

---

## Anatomy of a block (cont'd)

- LastCommit.
  - A simple wrapper for a list of signatures.
  - Represents the votes validators casted. 
  - The commit in a block refers to the vote for previous block.
  - The number of votes in a commit is limited to 10000.

[Source](https://github.com/tendermint/tendermint/blob/master/spec/core/data_structures.md)

---
## Anatomy of a transaction

Transactions are arbitrary byte arrays. 

Transactions are meaningless to the Tendermint. It is only interpreted by the application. 

---

## Consensus algorithm

- Consensus algorithm used is Tendermint BFT.
- A variation of Practical Byzantine Fault Tolerance.
- Introduced for partially synchronous networks.

---

## Overview of the Tendermint BFT

- The tendermint BFT algorithm is multi-phased. It consists of the following phases:
- A validator within a validator set is selected using a deterministically algorithm to build the next block.
- The deterministc algorithm used is a weighted round-robin algorithm.
  - The frequency of being chosen is proportional to the voting power (i.e. amount of bonded ATOM) of the validator.
- **This is the _Propose_ phase**
- After building the next block the validator broadcasts it to the network.
- Every validator that sees the block votes on the validity of the block. They broadcast also broadcast their
  votes to everyone else on the network. Every validator waits till it gets votes from 2/3 of the validator set.
- **The phase where validators wait for Prevotes is called _Prevote phase_**
- Every validator that got a 2/3 of prevotes then prepares to precommit the block. They broadcast this to the rest of the network.
  and wait till they get same Precommit from 2/3 of the validator sets.

---

- **The phase where validators wait for Precommit is called _Precommit phase_**
- Once a validator receives Precommit votes from 2/3 of the validator set, they commit the block and add it to their copy of the chain.

---

<img src="https://docs.tendermint.com/master/assets/img/consensus_logic.e9f4ca6f.png" width="60%" />

[Source](https://docs.tendermint.com/master/introduction/what-is-tendermint.html#consensus-overview)
