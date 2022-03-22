---
title: Resource Revolution
---

<img src="https://mobilecoinwp.wpengine.com/wp-content/uploads/2021/08/smoke-header.png" width="400">

MobileCoin has a significantly smaller impact on our planet and uses far less energy compared to other cryptocurrencies. Here is why:

-   The driver of energy usage in major cryptocurrencies is the consensus mechanism known as "Proof of Work." On a computer, "Work" equates to CPU cycles, which cost energy, and "[Proof of Work](https://bitcoin.org/bitcoin.pdf)" incentivizes miners (people who volunteer their computers to spin through enormous numbers of those cycles), to verify transactions.
-   Estimates put Bitcoin power usage at 128,000,000,000 kWh per year*, which is similar to the entire energy consumption of the country of Argentina**. In comparison, you could power all of MobileCoin with the same energy to power a few households.
-   MobileCoin uses a network consensus model known as Federated Byzantine Agreement (FBA). Unlike mining (which uses Proof of Work), the energy to consense on MobileCoin transactions is nearly zero.

Alexander Rose, Executive Director of [The Long Now](http://longnow.org/), reflects on sustainability:

> "The inherent problem with previous cryptographic systems that require computers to burn energy doing math means that greed will always be at odds with energy usage, and therefore at odds with earth's climate. We want to make sure there are ways of doing secure and private financial transactions without this level of Proof of Work."

With MobileCoin's blockchain design, we've pioneered a far more sustainable route to building the future of digital payments.

For more details and data on MobileCoin's energy usage, please read the technical explanation below.

. . .

Technical Explanation
---------------------

![](https://miro.medium.com/max/1400/1*pqhWTTuAEdfG5EIgDyBsmQ.png)

From University of Cambridge, Centre for Alternative Finance, https://cbeci.org/

### Background: Consensus Mechanisms

At its core, a cryptocurrency is a collection of transaction events. These are messages that say things like, "I will send 5 MOB to Carol." Users submit these messages to the decentralized network, and if nodes in that network think the messages are legitimate and valid, then the messages will be added to the collection of historical events.

However, decentralized networks don't have centralized arbiters of ultimate truth. Each node (i.e. network participant) is inclined to accept valid messages as they appear. If two legitimate messages are submitted to different nodes at the same time, but those messages contradict each other, then those nodes will be in conflict if they immediately accept the messages they see. For instance, if I only have 5 MOB and write the two messages "I will send 5 MOB to Bob" and "I will send 5 MOB to Carol," there is no way two nodes that obtain the messages can be in agreement.

This is a coordination problem. Nodes in different parts of the network will hear about messages (transactions) at different times. They need to coordinate with each other to prevent accidental disagreements.

Note that cryptocurrencies decide the "order of events" by recording transactions in "blockchains." A blockchain is a series of "blocks," where each block contains a collection of transactions. Blocks are appended to the chain one after the other.

### High Energy Usage in Proof of Work

In the [original proposal](https://bitcoin.org/bitcoin.pdf), Satoshi Nakamoto outlined a network coordination mechanism known as Proof of Work.

When nodes want to make new blocks, they must guess-and-check a difficult computation problem (they compute a hash function over and over until the output meets a difficult criteria). If blocks are produced rapidly, then the "difficulty" of the computation will rise (it will fall if blocks are produced too slowly). This means if a large amount of computational power is used, then the "cumulative difficulty" over successive blocks will also be large (it is easier to solve the computation problem with more computational power).

The blockchain with the highest cumulative difficulty is always assumed to be the "official" chain. Since the network is decentralized, it is still possible for nodes to temporarily create blocks that contradict each other. However, a node will always discard (or "orphan") blocks if it sees a chain with higher cumulative difficulty. Over time, the network is inclined to stay intact as all participants track the mathematically "official" chain.

Why would nodes bother wasting energy on Proof of Work? Simply, they are awarded for creating blocks. So-called "block rewards" are newly minted coins granted to block creators (transaction fees are also added to these rewards).

### Measuring Energy Consumption of Cryptocurrencies

#### Proof of Work

In Proof of Work systems, the energy cost of the network is easily estimated, because at equilibrium the marginal cost of adding/removing hash power is equal to the marginal gain/loss of revenue.

In other words, the amount of energy expended over a time-span in a Proof of Work system is approximately equal to the amount of energy (electricity) that can be purchased by block rewards over that time-span. In other systems, it's not so straightforward.

#### Proof of Stake

In Proof of Stake, a participant puts some amount of their own coins into an escrow wallet while they validate transactions and construct blocks. In each "unit of time" (e.g. 1 second), each stake-holder has a probability of making a new block proportional to the fraction of coins they own relative to the total number of coins that exist. Nodes are incentivized to construct blocks honestly, otherwise their staked coins will become worthless if falsification is discovered (similar to the concept of 'wasting energy' working on useless blocks in the Proof of Work model).

Proof of Stake requires far less energy than Proof of Work, because block construction only relies on the energy required to validate transactions and run the basic consensus mechanism.

Therefore, the energy usage for Proof of Stake networks is lower-bounded by the energy for the participants to sit idle. This [excellent article](https://medium.com/tqtezos/proof-of-work-vs-proof-of-stake-the-ecological-footprint-c58029faee44) on Tezos discusses energy use in their Proof of Stake network.

#### Federated Byzantine Agreement (FBA)

The Federated Byzantine Agreement model is categorically different from Proof of Work and Proof of Stake, where one node somewhere in the network creates each block. Instead, nodes using FBA collaborate to reach agreement on statements before those statements are considered "final" by any node. The [Stellar Consensus Protocol](https://www.stellar.org/papers/stellar-consensus-protocol?locale=en) (SCP) is a sophisticated protocol that implements FBA.

In SCP, many nodes collaborate to decide the contents of each block before the block is created. Doing so enables a faster, more efficient network, because it is not necessary to spend time resolving conflicts between different parts of the network (or waiting for those conflicts to be resolved).

Similar to Proof of Stake, the power consumption of an SCP network is lower-bounded by the energy of the participants to sit idle.

### MobileCoin Power Consumption: Back of the Envelope

In MobileCoin, the FBA participants are located in remote cloud services on Intel SGX-capable machines. If we dive into Microsoft Azure, a cloud provider used by operators on the MobileCoin network, we find they offer Intel SGX in their Confidential Compute platform through the [DC-series](https://docs.microsoft.com/en-us/azure/virtual-machines/dcv2-series).

In [How Can I Calculate CO2eq emissions for my Azure VM?](https://devblogs.microsoft.com/sustainable-software/how-can-i-calculate-co2eq-emissions-for-my-azure-vm/), we get an estimate of around 4.302 kWh for a 24 hour period on a standard machine. If we double this value, to give some breathing room, since we don't have data on a DC-series machine, then we have about 10 kWh per day, which gives us about 3,600 kWh per year, per server.

The current size of the network is around 10 nodes, so we can estimate around 36,000 kWh per year for the network, [which is on the order of a home](https://www.eia.gov/tools/faqs/faq.php?id=97&t=3), with estimates at 10,649 kWh per year per home. Even with many more nodes, the energy consumption of the MobileCoin network will still be on the order of a neighborhood as opposed to a country.

This estimate of the network size does not include "watcher nodes." Watcher nodes listen to validator nodes and record the blocks created by the consensus network (which is composed of validator nodes). Any user of MobileCoin can easily set up and run their own watcher node, so it isn't possible to estimate how many watchers are running now, or will be running in the future. However, it is reasonable to assume that in the long run there will be between 10x and 1000x as many watcher nodes as validator nodes. Watcher nodes should not require more power consumption than validator nodes.

In the long run, the MobileCoin network can be expected to use less energy than that of a small town with e.g. ~5,000 residents, even if MobileCoin becomes a global phenomenon.

. . .

By [Sara Drakeley](https://medium.com/@saradrakeley), [koe](https://medium.com/@koe000), and [Emily Huh](https://medium.com/@emilyhuh)

**University of Cambridge, Centre for Alternative Finance*: <https://cbeci.org/>

** Argentina energy consumption: <https://www.worlddata.info/america/argentina/energy-consumption.php>
