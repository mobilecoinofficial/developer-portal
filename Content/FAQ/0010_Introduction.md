---
title: "Frequently Asked Questions"
description: "MobileCoin developer FAQ"
---

1. ### What is the impact of an Intel SGX compromise on transaction privacy?

    Secure enclaves can provide improved integrity and confidentiality while functioning as intended. Like most complex new technologies, we should anticipate that design flaws will inevitably be discovered. Several side channel attacks against secrets protected by Intel SGX have been published, and subsequently patched or otherwise mitigated. MobileCoin is designed to provide "defense in depth" in the event of an attack based on a secure enclave exploit. MobileCoin transactions use CryptoNote technology to ensure that, even in the clear, the recipient is concealed with a one-time address, the sender is concealed in a ring signature, and the amounts are concealed with Ring Confidential Transactions (RingCT).

    In the event of an Intel SGX compromise, the attacker's view of the ledger inside the enclave would still be protected by both ring signatures and one-time addresses, and amounts would remain concealed with RingCT. These privacy protection mechanisms leave open the possibility of statistical attacks that rely on tracing the inputs in ring signatures to determine probabilistic relationships between transactions. This attack is only applicable to transactions made during the time that the secure enclave exploit is known, but not patched. Once the Intel SGX vulnerability is discovered and addressed, statistical attacks are no longer possible, therefore forward secrecy is preserved.

2. ### Can I run a validator node without Intel SGX?

    You can run the consensus-service using Intel SGX in simulation mode, however you will not be able to participate in consensus with other validator nodes. Your software measurement will be different from hardware-enabled Intel SGX peers and remote attestation will fail.

3. ### Can I run a watcher node without Intel SGX?

    Yes, you can operate a watcher node and validate block signatures by running the mobilecoind daemon, which does not require Intel SGX.

4. ### I thought you were called MobileCoin. Where is the code for mobile devices?

    Please see [fog](https://github.com/mobilecoinfoundation/mobilecoin/blob/master/fog), [android-bindings](https://github.com/mobilecoinfoundation/mobilecoin/blob/master/android-bindings), and [libmobilecoin](https://github.com/mobilecoinfoundation/mobilecoin/blob/master/libmobilecoin), to see how the balance checking and transaction building process works on mobile devices that don't sync the ledger.

    Please see also the MobileCoin [Android](https://github.com/mobilecoinofficial/android-sdk/) and [iOS](https://github.com/mobilecoinofficial/MobileCoin-Swift) SDKs, which can be integrated into your app to enable MobileCoin payments.

5. ### Will I need to put my keys on a remote server to scan the blockchain for incoming transactions?

    Keys will never leave your mobile device. For more details on how this works, please see the [MobileCoin Fog README](https://github.com/mobilecoinfoundation/mobilecoin/blob/master/fog).

6. ### Is Fog decentralized?

    Fog is a scalable service that helps users find their transactions, conduct balance checks, and build new transactions. Fog does so without requiring a local copy of the blockchain and without revealing a user's activities or giving away their private keys.

    Fog is intended to be run by app providers to provide their users with a private and positive mobile experience. Users need only trust the integrity of SGX, and not the service provider, for their privacy.

    Fog is thus not a single, decentralized network, but can be deployed as needed by each party willing to offer this service. Fog can be treated as critical infrastructure for an app, and can be scaled to meet each party's needs.

7. ### What is the hint field? Can I put anything in there?

    The purpose of the hint field is to send an encrypted message to the Fog ingest enclave, which it finds when it post-processes the blockchain. A conforming client puts only an mc-crypto-box ciphertext of a specific size there. For non-fog transactions, a ciphertext encrypted for a random public key should be put there. Putting something in the hint field which is distinguishable from this may degrade privacy.

8. ### How do I get support?

   For troubleshooting help and other questions, please visit our [community forum](https://community.mobilecoin.foundation/).

   You may also open a technical support ticket via [email](mailto://support@mobilecoin.foundation)