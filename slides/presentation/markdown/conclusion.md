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
