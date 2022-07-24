<!-- .slide: data-background-color="#8D3AED" -->

## Threat Models

---

## Attack - Replay

Transactions have a chain id and a nonce to protect against replay attacks.

---

## Attack - Byzantine Generals Problem

Tendermint BFT provides protection against this attack as long as at least 2/3 of the nodes are not byzantine.

---

## Attack - 51%

If the stake is evenly distributed, then this attack is really hard and expensive.

With the current distribution of stake in Cosmos Hub, this could be possible with a collusion of not so many validators.

---

## Attack - Sybil

Proof of stake protects against a Sybil Attack because the number of accounts doesn't matter, what matters is the stake.

---

## Attack - Node DoS

- The block proposer is chosen by a deterministic round robin algorithm in proportion to their voting power.

- It's possible to know beforehand who is going to be the next proposer.

- If the node has no protection, such as having a sentry node network topology, it can be a victim to a DoS attack.

---

## Attack - Equivocation

- Faulty validators sign multiple vote messages (prevote and/or precommit) for different values during the same round.

- It requires 1/3 or more of voting power to be executed.

---

## Equivocation

**Scenario**

- CA - a set of correct validators with less than 1/3 of the voting power

- CB - a set of correct validators with less than 1/3 of the voting power

- CA and CB are disjoint

- F - a set of faulty validators with 1/3 or more voting power

---

## Equivocation

**Execution**

- A faulty proposer proposes block A to CA

- A faulty proposer proposes block B to CB

- Validators from the set CA and CB prevote for A and B, respectively.

- Faulty validators from the set F prevote both for A and B.

---

## Equivocation

**Execution (cont'd)**

- The faulty prevote messages

- For A arrive at CA long before the B messages

- For B arrive at CB long before the A messages

- Therefore correct validators from set CA and CB will observe more than 2/3 of prevotes for A and B and precommit for A and B, respectively.

- Faulty validators from the set F precommit both values A and B.

- Thus, we have more than 2/3 commits for both A and B.

---

## Equivocation

**Consequences**

- Creating evidence of misbehavior is simple in this case as we have multiple messages signed by the same faulty processes for different values in the same round.

- We have to ensure that these different messages reach a correct process (full node, monitor?), which can submit evidence.

- This is an attack on the full node level (Fork-Full).

- It extends also to the light clients,

- For both we need a detection and recovery mechanism.
