# Introduction

DRAFT 1, 2013-04-01

Bitcoin (BTC) has recently reached and passed US$1 Billion in total
valuation: ~11 million BTC at $91 per coin, up from $14 per coin in
January 2013.
As it starts to attract the attention of mainstream investors, Bitcoin
security practices will become critical.  Due to the non-reversible
nature of the Bitcoin protocol, it is nearly impossible to recover
stolen BTC.  BTC should therefore be protected similarly to other
high-density assets, such as gold and jewelry.
Because of the technical nature of the Bitcoin system, additional
tools are available to achieve security goals, such as encryption and
digital multi-signatures.  In order to protect BTC balances we should
make use of these building blocks to construct a system with security
features proportional to the expected threats.
# Summary
Security threats include: private key disclosure through the network,
key disclosure through physical access, transaction injection,
destination modification, coercion, embezzlement.  Some of these
attacks may be delivered through software vulnerabilities, others
through software trojans or physical access.
A related threat is private key loss due to data loss.  This risk must
be mitigated through backups.  Backups, in turn, increase the size of
the security perimeter that must be defended.
Security building blocks to mitigate the threat include: Offline
Storage, also known as Cold Storage, encrypted storage,
Multi-Signature coins, physical security and financial controls.
# Security Aspects of Bitcoin
## Controlled through public key cryptography
BTC are controlled by the use of private keys and are transmitted to
the associated public keys.  If the private keys are disclosed due to
a security breach, the coins can be quickly transferred to keys
controlled by the attacker.  A "coin" is a balance associated with a
private key that is recognized in the Bitcoin ledger.
## Very high asset density
One of the largest balances in existence is 111,111.11 BTC at address
1933phfhK3ZgFQNLGSDXvqCn32k2buXY8a.  Currently, this balance is worth
around US$10 million.  The private key for this address can be stored
in a few bytes of memory.  The value density of such a key is much
higher than any other value storage method.  In contrast $10 million
of gold currently weighs 200 Kg.
## Irreversible
A few seconds after a transaction is broadcast to the network, the
chances of reversing it (by broadcasting a conflicting transaction) is
low.  After a few ledger consensus cycles, at ~10 minutes each, the
chances of reversal become extremely remote barring unusual network
events.
## Pseudonymous
Bitcoins are controlled by mathematical constructs - private keys.
The connection between keys and people is not always known.  This
makes it difficult to identify the current owner of BTC.
However, due to the public nature of the ledger, analysis could be
performed on the flow of the coins.  For example, if a thief spends
some of the coins at a store that ships physical goods, investigators
could obtain the shipping address.
# Threats
## Private key disclosure through the network
Private keys may be inadvertently disclosed if the system where they
are stored is compromised, either through software defects or through
misconfiguration.
## Key disclosure through physical access
Private keys may be disclosed through physical access.  For example,
the system disk where the private key is stored may be stolen.
## Transaction injection
An improper transaction may be inadvertently digitally signed due to
inadequate controls.  This risk may be enabled either through software
that fails to check the transaction against the financial system of
record, or through inadequate organizational controls.
## Destination modification
A proper transaction may have its destination altered before it is signed.
## Coercion
A person with access to the private keys may be coerced to inject a
transaction.
## Embezzlement
An authorized person with access to the private keys may embezzle the
bitcoins.

# Security Building Blocks

## Cold Storage
Cold (offline) storage of private keys protects against disclosure of
the keys as a result of network attacks.  Cold storage is the minimum
level of protection for Bitcoin balances.  Many of the failures of
early Bitcoin exchanges would have been prevented through this process.
Balances that are transacted daily may be stored in a hot (online)
wallet while the bulk of BTC balances should be in cold storage.  For
some applications, 90%+ of balances are static.
The procedure for using offline keys is:
* Create a proposed transaction on the online system
* Copy the proposed transaction to the offline system
* Review the transaction on offline system (address and amount)
* Digitally sign the transaction offline
* Copy the signed transaction to the online system
* Broadcast the signed transaction
Threats mitigated: private key disclosure through the network,
transaction injection, destination modification.

## Encrypted storage
The cold storage private key may be encrypted with a password in order
to defend against physical theft.  In fact, it is often best to
encrypt the entire disk on the offline system, to make tampering more
difficult.  A further improvement is powering down the offline system
so that the keys are not available unencrypted in memory.
Threats mitigated: private key disclosure through physical access.
## Multi-signatures
As an extension to the normal BTC coin, a multi-signature coin
requires that multiple private keys be used to sign spending of the
coin.  For example, 3 out of 5 signatures from company officers may be
required in order to create a spending transaction.  Keys may be
distributed to people within one company, outside trustees and other
participants.
This powerful technique mitigates attacks against a minority of key
holders since only a successful coordinated attack can move the coins.
Threats mitigated: all.  Each of the threats described above is
mitigated by replacing single points of potential failures with
multiple ones.  Multiple point of failure must be exploited
concurrently, making the attack much more difficult.

## Secret Sharing
Secret-sharing (ref. Shamir) algorithms can be use to spread the
control of a single key among multiple people.  However, this method
is not as powerful as multi-signatures, because a single point of
failure exists once the key is reconstructed during the signing process.

## Physical security
Private keys may be further protected from disclosure by physical
access controls.

## Financial controls
Participants can compare the proposed transaction to financial
records.  For example, if the withdrawal beneficiary did not actually
have the balance to be moved, or there is no signed request from the
beneficiary, the transaction can be vetoed.
Threats mitigated: transaction injection, embezzlement.

# Sample Security Designs
## Level 0
Cold/encrypted storage.

## Level 1
Cold/encrypted storage with 2-of-3 multi-signature.  Three officers of
the company hold keys offline.

## Level 2
Cold/encrypted storage with 5-of-9 multi-signature.  Four officers and
five external trustees hold keys in secure vaults.  Some of the vaults
require additional procedures to unlock.  Each key is stored in three
vaults for backup.

# Conclusion
Although Bitcoin presents new security risks due to its irreversible
nature, it also provides new tools to mitigate these risks.  The
security of the system can be "dialed-up" to account for the potential
threats and size of balance.
