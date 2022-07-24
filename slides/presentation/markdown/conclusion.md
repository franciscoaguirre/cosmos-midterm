<!-- .slide: data-background-color="#8D3AED" -->

## Conclusion

---

## Overall

- 🏗 Cosmos SDK provides a good foundation to build an application-specific blockchain.

- 🔑 The protocol is based on reliable cryptography.

- 📉 Validators are incentivized to secure the network, but zones compete for this security.

- ❌ Validator voting system is flawed. Voters are under pressure to vote for a likely winner.

---

## Attack - Node DDos

- The block proposer is chosen by a deterministic and non-choking round robin selection algorithm that selects proposers in proportion to their voting power.

- It's possible to know beforehand which is going to be the next one.

- If the node has no protection, such as having a sentry node network topology, it can be a victim to a DDos attack.

---

## Attack - Equivocation

- Faulty validators sign multiple vote messages (prevote and/or precommit) for different values during the same round.

- It requires 1/3 or more of voting power to be executed.

---

<!-- .slide: data-background-color="#8D3AED" -->

## Thank you!

---

## Attack Scenario - Equivocation

- CA - a set of correct validators with less than 1/3 of the voting power

- CB - a set of correct validators with less than 1/3 of the voting power

- CA and CB are disjoint

- F - a set of faulty validators with 1/3 or more voting power

---

## Attack Execution - Equivocation

- A faulty proposer proposes block A to CA

- A faulty proposer proposes block B to CB

- Validators from the set CA and CB prevote for A and B, respectively.

- Faulty validators from the set F prevote both for A and B.

---

## Attack Execution - Equivocation

- The faulty prevote messages

- For A arrive at CA long before the B messages

- For B arrive at CB long before the A messages

- Therefore correct validators from set CA and CB will observe more than 2/3 of prevotes for A and B and precommit for A and B, respectively.

- Faulty validators from the set F precommit both values A and B.

- Thus, we have more than 2/3 commits for both A and B.

---

## Attack Consequences - Equivocation

- Creating evidence of misbehavior is simple in this case as we have multiple messages signed by the same faulty processes for different values in the same round.

- We have to ensure that these different messages reach a correct process (full node, monitor?), which can submit evidence.

- This is an attack on the full node level (Fork-Full).

- It extends also to the light clients,

- For both we need a detection and recovery mechanism.
