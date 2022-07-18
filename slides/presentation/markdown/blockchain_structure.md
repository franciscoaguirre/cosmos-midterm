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
- Tendermint passes transactions to the application through an interface called the ABCI (Application Blockchain Interface)
- The state is updated by the application based on the messages received from Tendermint core

> The first time a new blockchain is started, Tendermint calls InitChain.
> From then on, the following sequence of methods is executed for each block:
> BeginBlock, [DeliverTx], EndBlock, Commit
> where one DeliverTx is called for each transaction in the block.
> The result is an updated application state
[Source: https://docs.tendermint.com/master/spec/abci/abci.html#block-execution]

- The main messages that are used in state transition are:
  - InitChain
  - DeliverTx
  - EndBlock
  - Commit
---

## InitChain and EndBlock

- Updates the validator set.

> The application may set the validator set during InitChain,
> and may update it during EndBlock
> [source: https://docs.tendermint.com/master/spec/abci/apps.html#updating-the-validator-set]

> Each abci end block call, the operations to update queues and validator set changes are specified to execute
> [source: https://docs.cosmos.network/v0.45/modules/staking/05_end_block.html#end-block]

---

## DeliverTx

When a valid block is received by Tendermint Core, each transaction in the block
is passed to the application via DeliverTx in order to be processed.
It is during this stage that the state transitions occur.

Source: https://docs.cosmos.network/main/intro/sdk-app-architecture.html

---

## Commit

Commit signals the application to persist application state. It takes no parameters

Source: https://docs.tendermint.com/master/spec/abci/abci.html#commit
