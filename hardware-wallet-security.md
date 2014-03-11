# The Problem with Bitcoin Hardware Wallets (and possible solutions)
DRAFT 1, 2014-03-09
Author: devrandom (Miron Cuperman)

Bitcoin hardware wallets are a very promising technology. Since the private key never has to leave the device, the security of the coins could approach that of cold storage.

However, with wide adoption hardware wallets present a very tempting target.   Once enough wealth is controlled by a specific hardware wallet model, attacking the supply chain of the wallet becomes attractive. Malware could be inserted in hardware or software. The random seed could be generated in a way that is predictable to the attacker or the seed could be leaked.

In this paper we present possible solutions to make hardware wallets trustworthy even when the hardware and software are not.

# Attack Vectors
The wallet supply chain could be attacked at several points:
* The microcontroller chip could have additional circuitry inserted at the fab
* The chip mask could be modified en-route to the fab
* The chip could be replaced during shipping of the chip or the finished product
* Any other chip on the wallet circuit board that can write to memory could inject code
* The software could be modified

The following specific attacks could be carried out:
* The random number generator could be compromised making the wallet seed predictable to the attacker.  The attacker would then scan the blockchain for predictable keys.
* The seed could be leaked to the blockchain through subliminal channels, such as the one present in ECDSA signatures.  The attacker would scan the blockchain for leaked seeds.

# The Solution
We have to close two venues of attack: the RNG and the subliminal channel.  We close them by the user of a computer that controls the communication to/from the hardware wallet.  This is known as a *Warden* in cryptography.

## The RNG
The random number generator could be subverted.  To protect against this, we have to inject additional randomness and verify the resulting keys.  This can be done by having two seeds - one generated by the hardware wallet device and one by the Warden computer.

The two seeds generate two different master keypairs.  Because of the linear property of [ECDSA](https://en.wikipedia.org/wiki/Elliptic_Curve_DSA), if we add the private keys to get a final private key the matching final public key is the addition of the individual public keys.  The protocol runs as follows:

* Each side generates a seed, and derives the master keypair
* Wallet sends wallet public key A
* Warden sends warden private key b
* Final public key is bA
* Device computes final private key ab
* abG = Ab
* Addresses are generated from the final public key recorded by the computer, while signatures are performed using the final private key recorded by the device

The device cannot generate a predictable final key because it cannot predict the key sent by the computer.

## The Subliminal Channel

The subliminal channel of ECDSA can be closed by forcing the free variables in the signature to be unpredictable to the hardware wallet.  Some approaches are presented in the citation section below.

## Multisignatures

When using multisignature wallets, a hardware wallet controls just one out of three keys.  Even if the hardware wallet is compromised, the coins are safe if the other two keys remain secret.

However, this solution is not fully satisfactory by itself:
* Information leakage is still a risk, possibly eliminating privacy
* Theft can still occur if one of the other keys is compromised

This solution could be combined with the other two for a stronger overall system.

Citations:

* [Simmons, Gustavus J. "The history of subliminal channels." Selected Areas in Communications, IEEE Journal on 16.4 (1998): 452-462.](http://www.cs.gmu.edu/~zduric/cs803/Simmons.pdf)
* [The subliminal channel and digital signatures](https://dl.acm.org/citation.cfm?id=20202)
* [Study on Closing the Subliminal Channel Based on ECDSA Digital
Signature Scheme](http://www.jofcis.com/publishedpapers/2011_7_4_1254_1261.pdf)
* [Bohli, Jens-Matthias, María Isabel González Vasco, and Rainer Steinwandt. "A subliminal-free variant of ECDSA." Information Hiding. Springer Berlin Heidelberg, 2007.] (http://link.springer.com/chapter/10.1007%2F978-3-540-74124-4_25)
* [Dong, Qingkuan, and Guozhen Xiao. "A Subliminal-Free Variant of ECDSA Using Interactive Protocol." E-Product E-Service and E-Entertainment (ICEEE), 2010 International Conference on. IEEE, 2010.](http://ieeexplore.ieee.org/xpl/login.jsp?tp=&arnumber=5660874&url=http%3A%2F%2Fieeexplore.ieee.org%2Fxpls%2Fabs_all.jsp%3Farnumber%3D5660874)