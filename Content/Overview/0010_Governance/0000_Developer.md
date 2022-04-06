---
title: Open Source Governance
---
Blockchains are distributed systems. They are essentially consensus protocols, which means that different nodes in the network (e.g. computers on the internet) have to be running compatible software.

**"Node operators"** are the owners and managers of nodes that run the protocol. Most node operators don't want to write much software, and it's a technical challenge for anyone to independently write compatible implementations of any consensus protocol even if they have a specification. As a result, node operators rely on software repositories (usually hosted on Microsoft/Github servers) to provide them with the software they choose to run.

**"Core developers"** of a blockchain are software developers who work on the software that implement that protocol. Developers have processes that are supposed to assure the quality of the software they release, and are generally very interested in maintaining the legitimacy of their software repositories because they want to see people using their software (as opposed to someone else's).

MobileCoin is a privacy-focused cryptocurrency [launched](https://mobilecoinfoundation.medium.com/mobilecoin-main-net-8e355d82c726) on December 7th, 2020. Its initial [code base](https://github.com/mobilecoinfoundation/mobilecoin) was developed by MobileCoin Inc. and published by the [MobileCoin Foundation](https://mobilecoin.foundation/) with an open-source license (GPLv3).

The MobileCoin Foundation maintains the reference implementation of MobileCoin's (the cryptocurrency's) protocol, along with the 'service-layer' technology known as [Fog](https://github.com/mobilecoinfoundation/fog) that facilitates efficient use of MobileCoin in resource-constrained environments like mobile devices, without loss of privacy.

Changes to those reference implementations proposed by MobileCoin Inc. (for example) must be approved by the MobileCoin Foundation's [Technical Committee](https://github.com/mobilecoinfoundation/technical-committee/blob/master/CHARTER.md).
