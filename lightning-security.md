# Overview

I examine the use of keys in Lightning and the resulting security implications.

We can see that the entire channel balance is at risk if a node is compromised.
The current Lightning protocol only allows the use of single keys.
Without the ability to use multisig this presents a significant risk of loss.

# Lightning in a nutshell

Lightning operates as follows.

Funding:
* A funding transaction is created, going to a 2-of-2 multisig controlled by Alice and Bob
* Commitment transactions are created, distributing the funds back to Alice and Bob
* The funding transaction is published to the blockchain

Movement of funds:
* A new commitment transaction is created (one for Alice and one for Bob), with a delayed closing path and a revocation path.
The revocation path allows the counterparty to take all funds as penalty if an old commitment transaction is published.
* Commitment transactions can also have HTLC outputs, allowing atomic multi-hop movement of funds
* The commitment transaction is not published unless something goes wrong

Closing:
* A closing transaction is published.  The destinations are specified in the closing message.

# Keys

Lightning uses the following types of keys:
* Hot wallet keys, used to create the funding transaction
* Funding keys, used to sign commitment transactions
* Channel closing keys, the destination of remote funds on abrupt channel close
* Delayed channel closing keys, for sending local funds on abrupt channel close with a delay to allow for penalty transaction
to be published by the remote
* HTLC keys
* Revocation keys

During normal channel operation, the funding keys are used to sign new commitment transactions.
All of the keys, other than the funding keys, are derived from a secret tweaked with a per-commitment factor,
making them unpredictable.

The revocation keys have further structure, allowing a penalty-enforcing watcher to work with limited storage.

# Attack Vectors
## Hot wallet keys

These control plain bitcoin balances, and their compromise leads to the loss of the hot wallet.

## Funding keys

If the funding keys are compromised, any amount can be pushed through the channel.  If the remote end is malicious,
this will result in total loss of the channel balance.

Also, if the funding keys are compromised, a closing transaction can output our balance to any destination.

## Revocation keys

If the revocation keys are compromised, and the remote end is malicious, it can exercise the penalty transaction.

## Channel closing destination (normal close)

These can be anything, and if a node is compromised channel balance will be lost.
The channel closing transaction is signed by the funding keys.

## Delayed channel closing keys (abrupt close)

If the delayed channel closing keys are compromised, and the remote end is malicious, an old commitment can be published
and the penalty transaction will take all channel funds.

Mitigation: these keys are not needed during normal channel operation and don't need to be continuously online.

## Channel closing keys (abrupt close)

These keys don't need to be continously online.

# Discussion

In the current protocol, the entire channel balance is at risk.
As more payments move to lightning channels, the liquidity controlled by lightning nodes will be a significant fraction of all Bitcoin, so this is a significant barrier to adoption.

Here are some potential ways to secure Lightning channel balances:

* enhance the current protocol to allow for multisig wherever CHECKSIG is used
* adopt Schnorr signatures to allow for off-chain multisig setup and signing
 * TBD need to verify that the key derivation mechanisms doesn't interfere the Schnorr multisig protocol

Schnorr would be preferred, due to the lower on-chain space utilization and increased utility.
However, the timeline for Schnorr adoption in Bitcoin is unclear.

# References

* Schnorr multisig protoco, by sipa - https://github.com/sipa/secp256k1/blob/968e2f415a5e764d159ee03e95815ea11460854e/src/modules/schnorr/schnorr.md
* BOLT-3 blog post, by Rusty - https://medium.com/@rusty_lightning/the-bitcoin-lightning-spec-part-4-8-bitcoin-transaction-and-script-formats-246609394b0a
