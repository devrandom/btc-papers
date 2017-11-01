# Soft-fork Introduction of a New POW

## Motivation:

- Mitigate centralization pressures by introducing a POW that does not have economies of scale
- Introduce an intermediary confirmation point, reducing the impact of mining power fluctuations

Note however that choice of a suitable POW will require deep analysis.  Some pitfals include: botnet mining, POWs that seem ASIC resistant but are not, unexpected/covert optimization.

In particular, unexpected/covert optimizations, such as ASCIBOOST, present a potential centralizing and destabilizing force.

## Design

### Aux POW intermediate block

Auxiliary POW blocks are introduced between normal blocks - i.e. the chain alternates between the two POWs.
Each aux-POW block points to the previous normal block and contains transactions just like a normal block.
Each normal block points to the previous aux-POW block and must contain all transactions from the aux-POW block.
Block space is not increased.

The new intermediate block and the pointers are introduced via a soft-fork restriction.

### Reward for aux POW miners

The reward for the aux POW smoothly increases from zero to a target value (e.g. 1/2 of the total reward) over time.
The reward is transferred via a soft-fork restriction requiring a coinbase output to an address published in the
aux-POW block.

### Aux POW difficulty adjustment

Difficulty adjustments remain independent for the two POWs.

The difficulty of the aux POW is adjusted based on the average time between normal block found
to aux block found.

Further details are dependent on the specific POW.

### Heaviest chain rule change

This is a semi-hard change, because non-upgraded nodes can get on the wrong chain in case of attack.  However,
it might be possible to construct an alert system that notifies non-upgraded nodes of an upcoming rule change.
All blocks are still valid, so this is not a hardforking change.

The heaviest chain definition changes from sum of `difficulty` to sum of:

    mainDifficulty ^ x * auxDifficulty ^ y

where we start at:

    x = 1; y = 0

and end at values of x and y that are related to the target relative rewards.  For example, if the target rewards
are equally distributed, we will want ot end up at:

    x = 1/2; y = 1/2

so that both POWs have equal weight.  If the aux POW is to become dominant, x should end small relative to y.


## Questions and Answers

- What should be the parameters if we want the aux POW to have equal weight? A: 1/2 of the reward should be transferred
to aux miners and x = 1/2, y = 1/2.

- What should be the parameters if we want to deprecate the main POW?  A: most of the reward should be transferred to
aux miners and x = 0, y = 1.  The main difficulty will tend to zero, and aux miners will just trivially generate the
main block immediately after finding an aux block, with identical content.

- Wasted bandwidth to transfer transactions twice?  A: this can be optimized by skipping transactions already
transferred.

- Why would miners agree to soft-fork away some of their reward?  A: they would agree if they believe that
the coins will increase in value due to improved security properties.

## Open Questions

- After a block of one type is found, we can naively assume that POW will become idle while a block of the other type is being mined.  In practice, the spare capacity can be used to find alternative ("attacking") blocks or mine other coins.  Is that a problem?
- Is selfish mining amplified by this scheme for miners that have both types of hardware?

## POW candidates

- SHA256 (i.e. use same POW, but introduce an intermediate block for faster confirmation)
- Proof of Space and Time (Bram Cohen)
- Equihash
- Ethash

## Next Steps

- evaluate POW candidates
- evaluate difficulty adjustment rules
- simulate miner behavior to identify if there are incentives for detrimental behavior patterns (e.g. block withholding / selfish mining)
- Protocol details


## Credits

Bram Cohen came up with a similar idea back in March:
https://lists.linuxfoundation.org/pipermail/bitcoin-dev/2017-March/013744.html
