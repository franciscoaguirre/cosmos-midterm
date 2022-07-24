<!-- .slide: data-background-color="#8D3AED" -->

## Threat Models

---

## Security

<p style="text-align: left;">Byzantine Fault Tolerant:</p>

- 1/3 BFT assumption is predicated on the stake of each validator.

- Round-robin style format for doing the Block Proposal.

- Pre-vote phase is where the validators vote on the proposed block and proceed to the pre-commit phase if more than 2/3.

- If more than 2/3 of the validators pre-commit the pre-voted block, then the block is committed.

---

## Security

- Secured against a Sybil Attack for being a PoS Blockchain.

- Secured against a 51% by the staked ATOM value.
