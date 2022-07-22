<!-- .slide: data-background-color="#8D3AED" -->

## Governance

<!-- **Goal:** Evaluate the use of governance in this project; name at least two things you like and two things you would want to improve. -->

<!-- - How does this project solve the collective decision making problem? -->

<!-- - What are the structures of governance in this project? How are changes proposed and voted on? -->

<!-- - What voting mechanisms are used in this project? -->

<!-- - Who is allowed to vote in this project? What are the voting criteria? -->

---

## Topics

- Validator Selection
- On-Chain Governance

---

## Proof Of Stake Recap
- Representative Democracy
- Run a multi-winner election mechanism to elect a set of k validators
- In this election, the voting power of each voter is proportional to the number of tokens they own (their "stake"), so that if a voter owns 10 tokens, this is equivalent to there being 10 voters with a single token each and with identical ballots.

---

## Proof of Stake Recap
- Tokens are used as protection against a Sybil attack by an adversary
- A malicious entity may control a large number of validator candidates, but it should be very difficult to get them elected in terms of the numbers of tokens that need to vote for them.
---

## Proof of Stake Goals
- Maximize the total amount at stake.
- Maximize the stake behind the minimally staked validator.
- Minimize the variance of the stake in the set.
---
## Validator Selection (Theory)
- Multiwinner election mechanism to select k=175 validators
- Use k-plurality to elect validator set
---

## Shortcomings (Theory)
- Encourages a tactical voting technique known as compromising
- Voters vote for one or few candidates most likely to win even if that is not their preference. 
- In turn, this leads to very few candidates consolidating more and more vote support over time, in detriment to less popular candidates who perhaps are generally considered to be more competent.
- Coordination Game!
---

## Don't Trust. Verify.
---
## Putting theory to test
<img src="../../assets/img/3-Governance/Atom-staking.png" alt="Staking my atom">
---

## Cosmos vs Polkadot
### Total validators
- Cosmos: 175
- Polkadot: 297  
</br></br></br></br></br>
<div style="text-align: right"> (As on 21st July, 2022) </div>
---

## Cosmos vs Polkadot
### Total Stake
- Cosmos: 191,331,986 ATOMS ≈ 2,100,000,000 USD
- Polkadot: 628,940,586 DOTS ≈ 4,700,000,000 USD
  
</br></br></br></br></br>
<div style="text-align: right"> (As on 21st July, 2022) </div>
---

## Cosmos vs Polkadot
### Highest Stake
- Cosmos: 10,885,889 ATOMS ≈ 120,000,000 USD (5.69%)
- Polkadot: 2,000,000 DOTS ≈ 15,000,000 USD (0.32%)
    
</br></br></br></br></br>
<div style="text-align: right"> (As on 21st July, 2022) </div>
---

## Cosmos vs Polkadot
### Lowest Stake
- Cosmos: 39,277 ATOMS ≈ 430,000 USD (0.02%)
- Polkadot: 1,850,000 DOTS ≈ 13,870,000 USD (0.29%)
  
</br></br></br></br></br>
<div style="text-align: right"> (As on 21st July, 2022) </div>
---


## Cosmos vs Polkadot
- Drops to 10x lower than the highest stake for validator#42.
- Lowest stake 277 times lower than the highest stake. Compared to 8% lower in case of Polkadot.
- Top 22 validators have more than 66% of [voting power](https://www.mintscan.io/cosmos/validators).

---
## POS Goals (Again)
- 🤔 Maximize the total amount at stake.
- ❌ Maximize the stake behind the minimally staked validator.
- ❌ Minimize the variance of the stake in the set.
---
## On Chain Proposal and Governance
- 14 days deposit period for the required 64 ATOMS.
- Voting options: `Abstain`, `Yes`, `No`, `NoWithVeto`.
- Deposit Burned if 33.4% or more participating voting power votes for `NoWithVeto`.

---

## For a proposal to pass
- 64 ATOMS need to be deposited.
- 40% of network voting power needs to participate.
- Simple Majority (>50%) for `Yes` is needed for a proposal to pass.
- Deposit burned if quorum is not reached.

---

## Who can Vote?
- Validators in the active set at end of voting period.
- Delegators staking on validators in the active set at the end of voting period.
- If delegators don't vote, the corresponding validators inherit their voting power.
---
## Concerns
- No Fast track governance. All proposals are treated in the same bucket.
- Deposit burning when quorum is not reached discourage genuine users from proposing. 
- No incentive for voting power to participate unless the proposal affects them directly.
- Not every ATOM holder can vote.
- 74 proposals in last 39 months.
---

## Two things we like
- Having on-chain governance and a community pool.
- Having a deposit burning mechanism to prevent spam (though need to keep a balance to not discourage genuine proposals).

---
## Two (of many) things we would improve
- Improve validator selection mechanism so that voting power is more uniformly spread.
- Encourage more genuine proposals and more use of community pool.
- Add different tiers of governance proposal and not treat all proposals as equal.

<!-- ## Notes -->

<!-- > Governance -->
<!-- - 64 atoms deposit -->
<!-- - anyone can deposit -->
<!-- - Options: Abstain, Yes, No, NoWithVeto -->
<!-- - Minimum voting power 40% required -->
<!-- - if 33.4% Veto, deposit is burned -->
<!-- - fixed 14 day voting period -->
<!-- - Only validators and delegators in active set has voting power -->
<!-- - Total validators 175 -->
