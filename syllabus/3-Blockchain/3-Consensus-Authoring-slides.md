---
title: Consensus Systems
description: Consensus systems for Web3 Engineers.
instructors: ["Joshy Orndorff"]
teaching-assistants: ["Sacha Lansky", "Dan Shields", "Emre Surmeli"]
---

# Consensus Systems

---

## Consensus is

A decision making process that strives to achieve acceptance of a decision by all participants.

Notes:

If we have external factors like trust relationships or Social hierarchy it can be quite easy.

Trust example: If everyone trusts each other, then any one person can propose an outcome, and all the others will trust that it is in the community's best interest.

Social Hierarchy example: If a community has an executive (President, King, CEO), the executive can propose her preference and everyone will accept it based on the external mechanism that keeps her as the executive.

But these conditions are not interesting, and are often not realistic. It gets interesting when we don't make these simplifying assumption.

---

## Adversarial Conditions

- The participants want to agree on _something_
- They don't trust each other
- Some could be faulty, or make mistakes
- Some could be straight up malicious

---

## Desired Properties

All honest participants...

- Termination - Eventually reach some decision
- Agreement - Reach the same decision
- Integrity - Reach at most one decision, which was proposed by at least one participant.

---v

## Also Validity

If all participants unanimously propose the same decision , all honest participants reach that decision.

Notes:

Another desireable property that is sometimes too obvious to say.

---

## Example 1: Where to go for dinner

<widget-columns>
<widget-column>

<img style="width: 20em" src="../../assets/img/3-Blockchain/3.2-south-park-consensus.png"/>

</widget-column>

<widget-column>

- Termination - Stan, Kyle, and Kenny will each eventually decide where to eat.
- Agreement - Stan, Kyle, and Kenny will each decide to eat at the same place.
- Integrity - Stan, Kyle, and Kenny will eat at either Whistlin' Willies or Casa Bonita.

</widget-column>
</widget-columns>

Notes:

Stan, Kyle, Kenny, and Cartman are trying to decide where to have dinner.
Stan, Kyle, and Kenny want to have something yummy and spend time together.
Cartman is secretly pursuing his own goals and doesn't care whether dinner is yummy.
In fact he secretly hopes they won't decide at all so he can go home and eat KFC with his mom.

Stan and Kyle both propose Whistlin' Willie's for dinner.
Kenny and Cartman propose Casa Bonita.

In the end Kenny may not agree that Whistlin' Willie's is the best option for dinner, but he will agree that it is the legitimate group decision.

Image source: https://southparkstudios.mtvnimages.com/images/shows/south-park/clip-thumbnails/season-14/1402/south-park-s14e02c03-ill-show-them-f-obscene-16x9.jpg

---v

## Example 2: Which side of the road to drive on

In the last module we discussed the coordination game and its two Nash equilibria. We discussed that if there is no communication allowed among participants, a Shelling point may help.

Luckily in real life, communication is possible so we can run a consensus algorithm instead of rely on a Shelling point.

- Termination -
- Agreement -
- Integrity -

Notes:

Imagine all the drivers are allowed to participate in the consensus. Most are honest because they don't want to crash, but
some may be Byzantine.
Someone please volunteer to describe what one of these means in this example.

- Termination - All honest drivers will eventually decide which lane to drive in
- Agreement - All honest drivers will agree on the same lane to drive in
- Integrity - The decision will be one of the participants suggestions. Doesn't mean much in this case because there are only two choices. It's basically guaranteed that they will both be suggested by at least _someone_

---

## Byzantine Generals Problem

<widget-columns>
<widget-column>

<img style="width: 26em" src="../../assets/img/3-Blockchain/3.2-Byzantine-Generals.png"/>

</widget-column>

<widget-column>

- Divisions plan to attack
- Must make coordinated attack to succeed
- Some generals might be traitors and behave maliciously

</widget-column>
</widget-columns>

Notes:

Several divisions of the Byzantine army are camped around an enemy city. They plan to attack the city, but have not yet decided when to attack.
If the attack is coordinated, they will probably be successful, but if it is uncoordinated, they will probably be defeated.
What do our three properties mean in this scenario?

- Termination -
- Agreement -
- Integrity -

---v

## Mistakes vs Malice

**Crash Tolerance** - A system can keep operating normally when some actors crash or go offline.

**Byzantine Fault Tolerance** - A system can keep operating normally when some actors are intentionally malicious. Byzantine actors may:

- Crash - Or pretend to have crashed - Byzantine faults are a superset of crash faults
- Lie - Sending false information to peers
- Equivocate - Send inconsistent information to different peers

Notes:

These examples and many others are _instances_ of the Byzantine Generals Problem

---v

## Mistake or Malice?

<widget-columns>
<widget-column>

<img style="width: 600px" src="https://thepointsguy.global.ssl.fastly.net/uk/originals/2020/12/pitot-ice-scaled.jpg"/>

</widget-column>

<widget-column>

- Consider an Airplane flight computer.
- The pilot _must_ know the airspeed at all times.
- Airspeed sensors can fail.
- Sensors can be buggy.
- Are these malicious?

</widget-column>
</widget-columns>

Notes:

Consider an Airplane flight computer. It is critical that the pilot (human or automated) knows the aircraft's airspeed at all times.
Airspeed sensors can fail due to extreme temperatures, icing, solar radiation, and other reasons.
For this reason, there are redundant sensors, and they run a consensus protocol.

Imagine that one of the sensors has an overflow _bug_ such that when the airspeed exceeds a certain threshold, maybe `u8::max_value()`, it actually reports a much lower speed.

Are these crash faults or byzantine?

In a blockchain system, bugs in the code may cause nodes whose operators are intending to be honest, to deviate from the protocol anyway. This is why client diversity is important.

---

## Synchrony

A system is one of:

- **Synchronous** - When a message is sent, it is received immediately by all recipients.
- **Asynchronous** - When a message is sent it may be received after some delay, or not at all. The sender doe not know whether it is received. Messages may be received in different orders by different parties.
- **Partially Synchronous** - When a message is sent, it may be received after some delay up to a maximum delay, $T$. It may not be dropped entirely. Messages may be received in different orders by different parties.

Roughly analogous to real-time (async) vs. turn-based (sync) games.

---v

## Sync or Async?

<widget-text center>

Classify each of these:

- Telephone call
- Email
- Text message
- Jitsi video call
- Element chat

Notes:

Answers

- Telephone call - sync
- Email - async
- Text message - sync
- Jitsi video call - sync
- Element chat - IDK actually

---

## Determinism

A system is one of:

- **Deterministic** - The same inputs give the same outputs every single time.
- **Probabilistic** - The same inputs may not give the same outputs. Requires a source of entropy. eg coin flipping.

---

## FLP Impossibility

<img style="padding: 0 200px 0 0" src="../../assets/img/3-Blockchain/3.2-FLP-impossible.png"/>

Notes:

Once it was shown that deterministic consensus is impossible in an async network, the field split into two major parts. Either you:

- Assume the network is (at least partially) synchronous
- Introduce non-determinism

One interesting thing about Nakamoto pow consensus is that it does _both_.

- Nondeterminism because you don't know who will solve the pow first
- Partial synchrony because it only works if the time to download and execute the block is much less than the time to solve the PoW

---

## Blockchain Consensus

So what are we even trying to decide? A few things:

<widget-text center>

- **Author selection** - Who gets to author the next block
- **Finality** - Which blocks are final

Notes:

Finality is basically like safety. Finality can be proved to entities that are not involved in consensus.

These two decisions can be entirely orthogonal to one another, or wrapped up together. I'll focus on finality first

---

## Long-running Systems

Blockchains are intended to keep processing and finalizing transactions forever. This is more like deciding where to go to dinner every night over and over.

<widget-text center>

We frame the desireable properties differently. At a very high level they are:

- **Liveness** - Something good will eventually happen
- **Safety** - Nothing bad will happen

---v

## Example: Finalizing blocks

<widget-text center>

- Liveness - Another block will eventually be finalized.
- Safety - No two conflicting blocks will ever be finalized by honest participants.

Notes:

Liveness here obviously depends on liveness in the authoring. No more blocks will be finalized if no more blocks are authored.

---v

## Liveness vs Termination

Earlier I described Termination as desireable, now I'm saying Liveness is desireable.

Are these at odds with each other?

Notes:

Termination guarantees that, for a given decision, each honest participant will eventually decide something. This concept is relevant when there is a single decision to be made. In the blockchain context, it guarantees that we will eventually know which block is final at height n.

Liveness guarantees that a system that is intended to continue making progress, will indeed eventually make progress. In the context of the blockchain, liveness means that once we've decided what block is final at height n, we will eventually go on to decide what block is final at height n + 1.

---

## Choosing Authorities

We saw that some consensus engines have high communication overhead, $O(n^2)$. Other systems have lower overhead, but there is always _some_ practical bound on the size of the authority set when deterministic finality is used.

Therefore we need a fair way to choose who gets to participate. Typically there is an onchain economic game to select the participants.

---

## Keeping Authorities Honest

All the blockchain consensus engines we looked at assume some threshold of honest participants. So how do we ensure they are honest? We use an on-chain economic game to do so.

<widget-text center>

- Why would anyone even want to participate?
- How do we define misbehavior?

---v

## Game Theory!

Abstractly: You participate when the utility of doing so exceeds the cost.

<widget-text center>

Incentive designers may potentially:

- Reward honest behavior
- Punish (aka slash) dishonest behavior

Notes:

Many systems use both of these, but doing so is not strictly necessary. Even without slashes, the opportunity cost of staking and the missed rewards from authoring invalid blocks may be sufficient.

Algorand tried to get away without block rewards either, but ran into a problem. Any guess? -- Nobody wanted to participate.

---v

### Potential Rewards

<widget-text center>

- Explicit: Block reward paid by inflation
- Explicit: Transaction fees
- Implicit: Your dApp stays live and you get utility from the dApp
- Implicit: Street cred 😎

---v

## Potential Punishment

PoW has inherent punishment through wasted energy. Other system's don't.

Instead, aspiring participants will typically lock up a security deposit which can be slashed in part or in whole.

---v

## What is potentially punishable?

<widget-text center>

- Authoring when you aren't supposed to
- Failing to author when you are supposed to
- Casting finality votes for conflicting candidate blocks
- Casting a finality vote for a block (or chain) that includes an invalid state transition.

_How severe are each of these offences? Do they all warrant a slash? A full slash?_

---v

### Concrete Punishment Example

Let's say a slash is 100 units, and the reporter gets 10%. I plan to attack the network. If my attack is successful, I expect to gain roughly 200 units worth of utility.

I ask another authority to cooperate with me: "I'll pay you 20 units to _not_ rat me out for my attack".

Question: How would you respond?

Notes:

"I don't believe you can carry out the attack because _someone else_ will report you and take the 10 units, leaving me with 0."

---

## Case Study: Nakamoto Consensus

- Aka Proof of Work
- Anyone can participate anonymously
- Can start and stop participating at will without notice
- Support is signaled by "mining" (performing a hash preimage search) on blocks

Notes:

In Nakamoto consensus, the mining contributes to finality as well as selects the next author. At this point the author selection is orthogonal. We're focusing on safety and finality here.

---v

### Finality in Nakamoto Consensus

- Longest chain rule\* - Longest chain is "best"... for now
- Someone could always start mining a chain and, with low but non-zero probability, end up with it longer.
- There could _already_ be a longer chain that you just haven't heard of.

The finality is only probabilistic. Nakamoto consensus in only safe in a synchronous network.

Notes:

This is to say that Nakamoto consensus is NOT safe in on the real asynchronous internet. In practice, as long as
blocks can be downloaded and executed much more quickly that the target block time, it is usually good enough.

- Longest could also mean most accumulated work

---v

## Punishment and Reward in Nakamoto Consensus

- Implicit punishment: Mining a block costs energy. If that block is not in the best chain you wasted the energy.
- No reward in the consensus layer
- (Usually) an explicit reward in the economic game: Block authors are paid with inflation and transaction fees.

---

## Case Study: Tendermint

- Assume partial synchrony - time is divided into slots
- One block is up for Byzantine agreement in each slot.
- If the block has enough votes by the end of the round, it is finalized. Otherwise, it is rejected via timeout.

Notes:

Tendermint assumes partially (aka weakly) synchronous network - That is to say that messages may not arrive immediately, but will arrive within a finite time bound.
Practically speaking, time is divided into "slots" of equal length, and (at most) one round of Byzantine agreement happens in each slot.

---v

## Finality in Tendermint

- Deterministic finality - Once a block is finalized, we know it is finalized\*.
- Next round doesn't start until the previous one ends.
- Forkless - Forks are disallowed because blocks can only be authored on finalized parents.

Notes:

Because it is based on Byzantine agreement, we know that a finalized block will never be revierted _as long as our assumptions about honest participant threshold are met_. As we discussed, game theory can help ensure the honesty assumptions are met.

Tendermint is often touted as "instant finality". It is instant in the sense that finality is tied to authorship. In practice this means that authorship, which is inherently O(n), is throttled down to stay in sync with finality which is O(n^2).

---v

## Punishment and Reward in Tendermint

Tendermint is based on PBFT - There are no incentives in the consensus layer.

<widget-text center>

In the economic game:

- (Usually) an explicit reward just like in PoW - inflation, fees
- Requires a security deposit which incurs opportunity cost, and may be slashable

---v

### Hybrid Consensus

Separating the block production from finality.

This allows the block production to stay live and regular even if finality is lagging. It also allows lower overhead in the finality layer.

This approach is used in Substrate.

---

## Case Study: Grandpa

<widget-text center>

- Deterministic finality _only_
- Requires an external block authoring scheme with its own liveness proof.
- Two rounds of voting - the details of the voting will be discussed next week

---v

## Vote on Chains, not Blocks

Finalizing a block in a BFT system with $n$ participants is in $O(n^2)$, In Tendermint this communication has to happen at each and every block which puts a bound on the size of the validator set.

<widget-text center>

Now that finality is separated, we treat each vote as a vote not only for one block, but also for each ancestor block. This significantly reduces the number of total messages sent.

- Allows the chain to stay live even when many validators are offline
- Allows for a challenge period to delay finality if needed

---

## Authoring Schemes(Finite Participant)

Regardless of whether finality is separated from block authoring or not, we need a way to decide who can author the next block.

First, a question: Why not just let anyone author a block any time they want?

Notes:

Answer: Because it would overwhelm the network. It takes time to download and execute blocks. Restricting which nodes can author and when they can author, throttles block production to a manageable rate.

---

## Case Study: Aura

The simplest possible idea: The participants are given a total ordering, and then they take turns, one-author-per-slot in order forever.

---

## Case Study: Babe / Ouroborous

The two are quite similar. Participants reveal a VRF output once per slot. If your output is below a pre-determined threshold, you are eligible to author in this slot.

Question: Why is this better than Aura?

Notes:

Answer: Because in Aura you know who the next leader will be well in advance. This allows an adversary to perform a targeted attack. By using randomness in this way, you don't know who will eligible until the slot is upon us. This is similar to PoW.

---

## Summary

<widget-text center>

- Networks can be {Synchronous, Asynchronous}
- Consensus systems can be {Deterministic, Probabilistic}
- Consensus systems can be {Open participation, Finite participation}
- There is always an assumption that at least {1/2, 2/3} participants are honest
- In decentralized systems, we use Economics and Game Theory to incentivize honest execution of the consensus protocol