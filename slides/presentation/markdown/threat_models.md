<!-- **Goal:** Evaluate the security protocols of this project; name at least two things you like and two things you would want to improve.

- Other Attacks

- How does this project provide secure transactions? //// idk if this goes here tho -->

<!-- .slide: data-background-color="#8D3AED" -->

## Threat Models

---

## Security

Byzantine Fault Tolerant:

- 1/3 BFT assumption is predicated on the stake of each validator

- Round-robin style format for doing the Block Proposal

- Pre-vote phase is where the validators vote on the proposed block and proceed to the pre-commit phase if more than 2/3

- If more than 2/3 of the validators pre-commit the pre-voted block, then the block is committed

---

## Security

Sybil Attack

- Proof-of-Stake Blockchain

51% Attack

- Secured by the ATOM value
