<!-- .slide: data-background-color="#8D3AED" -->

## Blockchain Guarantees

---

## Liveness and Safety Guarantee

- Tendermint guarantees liveness so long as less than 1/3 of the total weight of the Validator set is malicious or faulty.

- In the consensus algorithm, a validator's Precommit vote is _locked_ to the block, preventing from Precommit voting for another block
    - > a PreCommit for a block must come with justification, in the form of a Polka for that block. If the validator has already PreCommit a block at round R1, we say they are _locked on that block, and the Polka used to justify the new PreCommit at round R_2 must come in a round R_polka where R_1 < R_polka <= R_2 [source: Whitepaper](https://v1.cosmos.network/resources/whitepaper).
- Validators must Propose and/or PreVote the block they are locked on
   - > Second, validators must Propose and/or PreVote the block they are locked on. Together, these conditions ensure that a validator does not PreCommit without sufficient evidence as justification, and that validators which have already PreCommit cannot contribute to evidence to PreCommit something else. This ensures both safety and liveness of the consensus algorithm [source: Whitepaper](https://v1.cosmos.network/resources/whitepaper).

---

## Fairness Guarantee

> In a consensus scheme like Tendermint where there is infinite number of processes, 
and where each block is produced by a subset of processes, the protocol is fair if any correct
process (a process that followed the protocol) that wield φ fraction of the total merit in the system
will get more or less φ fraction of the total reward that is given in the system.

[source: Correctness and Fairness of Tendermint-core Blockchains](https://eprint.iacr.org/2018/574.pdf)

- Fairness around the way rewards are distributed.
- Fairness around selection of voting committees.

---

### Fairness around the way rewards are distributed

Properties for characterizing the fairness of a reward mechanism.

1. For each block in the blockchain, all correct validators for that block have a reward parameter
equal to 1.
2. For each block in the blockchain, all faulty validators and the processes that are not validators
should have a reward parameter for that block equal to 0.
3. A process gets a reward for a block if and only if it has a reward parameter for that block
equal to 1.
4. There exists a height H such that for a block in the blockchain at height H0 > H a process gets a reward for that block if and only if it has a reward parameter for that block equal to 1.

Definition: 
- A reward mechanism is fair if it satisfies the conditions 1, 2 and 3.
- A reward mechanism is eventually fair if it satisfies the conditions 1, 2 and 4.

---

### Tendermint’s Reward Mechanism

Tendermint's reward mechanism works as follows:

- Once a new block is decided for height H, processes wait for _TimeOutCommit_ time to collect
the decision from the other validators for H, and put them in their set _toReward_.
- During the consensus at height H, let us assume that p<sub>i</sub> proposes the block that will get
decided in the consensus. pi proposes to reward processes in its set toReward.
That is, only the processes from which pi delivered a commit will get a reward for the block
at height H − 1.

Repercussion: The reward mechanism of Tendermint is not eventually fair.

---

### Tendermint’s Reward Mechanism (Cont'd)

Tendermint’s reward mechanism can be made to be at least eventually fair
if the _TimeOutCommit_ is increased for each round until it catches up the message delay.

When the commit messages are sufficient to detect a process that did not send expected
messages, Tendermint’s reward mechanism with modulable timeouts is eventually fair.

---

### Tendermint’s Reward Mechanism (Cont'd)

Changes to the reward mechanism in Tendermint to make it eventually fair:

- Once a new block is decided, say for height H, processes wait for at most _TimeOutCommit_
to collect the decision from the other validators for that height, and put them in their set
toReward.

- If a process did not get the commits from all the validators for that height before the expiration
of the time-out, it increases the time-out for the next height.
    - To prevent time-out from increasing indefinitely due to an unresponsive validator, the scheme does the following: 
    From the time t, at height H all correct processes know the exact set of validators that committed the block from H − 1, and from those commit messages, they can exclude the set of processes that did not participate to the consensus

---

### Tendermint’s Reward Mechanism (Cont'd)

- During the consensus at height H, let us assume that pi proposes the block that will get
decided in the consensus. pi gives the reward to the processes in its _toReward_.

In this reward mechanism, _TimeOutCommit_ is increased whenever a process does not have the
time to deliver all the commits for the previous round making the reward mechanism
eventually fair.

[source: Correctness and Fairness of Tendermint-core Blockchains](https://eprint.iacr.org/2018/574.pdf)

---

### Tendermint’s Reward Mechanism (Conclusion)

There exists a reward mechanism in repeated-consensus blockchains that is (eventually) fair if and only if the system communication is (eventually) synchronous. Specifically for Tendermint if the protocol evolves in an eventual synchronous
setting, it is not eventually fair. However, it becomes eventually fair when timeouts are carefully
tuned, and under the assumption that commit messages contains enough information to distinguish
between correct and Byzantine processes in the synchronous period.

[source: Correctness and Fairness of Tendermint-core Blockchains](https://eprint.iacr.org/2018/574.pdf)

---

### Fairness around selection of voting committees

- The frequency of selection is related to their voting power, which is related to amount of stake they hold.
- A new validator might have to wait for long time before being selected. 
    - A validator V that has just been elected is moved to the end of the queue. If the validator set is large and/ or other validators have significantly higher power, V will have to wait many runs to be elected.
- The selection mechanism also has a weakness that it can lead to very few candidates consolidating vote support over time, in detriment to less popular candidates who perhaps are generally considered to be more competent. See the _Governance_ section for more information on this.
    
---

## Censorship-resistance Guarantee

The censorship-resistance guarantees only holds as long as malicious validators are not more than 1/3 of the validator set.

As at 21st July, 2022, there are 175 validators on the Cosmos Hub. The relatively small number increases the possibility for validators to collude, making censorship easier. 


