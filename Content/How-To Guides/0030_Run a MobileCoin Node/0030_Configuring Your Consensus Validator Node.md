---
title: Configuring Your Consensus Validator Node
description: 
---
## Downloading the Consensus Server Software (Joe)

The MobileCoin Consensus Service facilitates transactions between MobileCoin users. The software needed to participate in the MobileCoin Consensus network can either be:

-   Downloaded and run using a pre-built Docker container from the Docker Hub (preferred method).

-   Built from source code.

![](https://lh3.googleusercontent.com/JYktqIKjI4tee6EqgsPWXan8bQqfRLCOFJ3YpWT9asb4S4mbyBsND5i93AVQbKSXSKXimPT0tiFYgO0YmgsjCkfGu50uDS9wwbYeATwhT76GAVoJl-tLVZeIr4Ci0hZ2do9bgbcV)  Note: We prefer that you run the container over downloading the software on your own because we have developed ...

### Downloading The Pre-Built Container from Docker Hub

To download the container from Docker Hub for running the consensus server, use the Docker Pull Command:

`$ docker pull mobilecoin/node_hw:<version_tag>`

### Building from Source

Most of the information required to build from source can be found at this location:

<https://github.com/mobilecoinofficial/mobilecoin/blob/master/consensus/service/BUILD.md>

![](https://lh3.googleusercontent.com/JYktqIKjI4tee6EqgsPWXan8bQqfRLCOFJ3YpWT9asb4S4mbyBsND5i93AVQbKSXSKXimPT0tiFYgO0YmgsjCkfGu50uDS9wwbYeATwhT76GAVoJl-tLVZeIr4Ci0hZ2do9bgbcV)  Note: To build the consensus server on your own, you will need the production enclave.

## Configuring your Node

\[Need to add intro.\]

### Accepting Connections from Trusted and Untrusted Peers

Your node will listen for messages from both trusted and untrusted peers. You must configure your node to listen via the --peer-listen-uri parameter to the consensus service.

An example peer-listen-uri is the following:

`mcp://0.0.0.0:8443/?tls-chain=/certs/your-tls.crt&tls-key=/certs/your-tls.key`

The components of the URI and their functions are:

| URI Component | Value | Function |
| ------------- | ----- | -------- |
| Scheme | mcp or mcp-insecure | Determines whether to use tls in the connection to the peer. Most often, these will be mcp.<br />If you configure to terminate TLS in the consensus service by using the mcp scheme, you will also need to provide the tls-chain and tls-key query parameters. |
| Address and Port | Address of node and peer listening port | Address and listening port of the consensus peer. |
| tls-chain query parameter | Path to local certificate chain | Used to terminate TLS in the consensus-service. |
| tls-key query parameter | Path to local certificate keyfile | Used to terminate TLS in the consensus-service. |

### Configuring your Node to Connect with your Trusted Peers

One of the largest responsibilities of a consensus node operator in the MobileCoin network is selecting your peers and defining your quorum set according to which institutions and individuals you trust. The validity and efficacy of the consensus algorithm depend on the trust relationships encapsulated in the networking and peer relationships of the consensus nodes.

#### Network Configuration Overview

The consensus and broadcast networking configuration is encapsulated in the network.toml file for your node, passed via `--network` to the consensus service. An example toml file for a 3-node network is below:

```
broadcast_peers = [

"mcp://peer1.test.mobilecoin.com:443/?consensus-msg-key=MCowBQYDK2VwAyEA-21ShHmvuuynH7EcIgkdH2dWxCojgnWYbHxLrRseQ1s=",

"mcp://peer2.test.mobilecoin.com:443/?consensus-msg-key=MCowBQYDK2VwAyEA0MaP19zCG3C87t98UOemqip3R9hmmaPmcSFAaehPQzQ=",

"mcp://peer3.test.mobilecoin.com:443/?consensus-msg-key=MCowBQYDK2VwAyEAk-iUVhhmmXn23VCJP0xqqtJabA9oQaJwdrHwHnfeJco=",

]

quorum_set = { threshold = 2, members = [

   { type = "Node", args = "peer1.test.mobilecoin.com:443" },

   { type = "Node", args = "peer2.test.mobilecoin.com:443" },

   { type = "Node", args = "peer3.test.mobilecoin.com:443" },

] }

tx_source_urls = [

   "https://s3-us-west-1.amazonaws.com/mobilecoin.chain/node1.test.mobilecoin.com/",

   "https://s3-us-west-1.amazonaws.com/mobilecoin.chain/node2.test.mobilecoin.com/",

   "https://s3-us-west-1.amazonaws.com/mobilecoin.chain/node3.test.mobilecoin.com/",

]
```

#### Broadcasting to Trusted and Untrusted Peers

The broadcast_peers define the nodes to which your node will broadcast messages.

MobileCoin uses Universal Resource Identifiers (URIs) to specify peers. These include the address of the peer, the peer's public key, and can optionally include connection information, such as the certificate authority bundle, and tls-hostname.

The URIs indicate the following:

| URI Component | Value | Function |
| ------------- | ----- | -------- |
| Scheme | mcp or mcp-insecure | Determines whether to use tls in the connection to the peer. Most often, these will be mcp. |
| Address and Port | Address of node and peer listening port | DNS address and listening port of the consensus peer. |
| consensus-msg-key query parameter | The public message signer key of the peer. | This is verified against every message from that peer to ensure it was signed by the peer you expect. |

#### Defining a Consensus Quorum Set 

The `quorum_set` section of the network.toml defines which nodes you trust. The quorum set is used to determine whether a sufficient set of nodes in the network agree on a value for you to also cast a vote in agreement. This is the process of solving the Byzantine Agreement Problem.

The quorum_set is a recursive structure which includes the following for each set:

| Quorum Set Component | Value | Function |
| -------------------- | ----- | -------- |
| Threshold | Integer | Defines the number of members of the set which must cast a vote for your node to be convinced and also vote in agreement. |
| Members | A list of peers or sets. | A member of a quorum set can be a peer or another quorum set. |
| Member Type | Node or InnerSet | Defines which type of quorum set member. |
| Member Args | Args to construct either a Node or InnerSet member of the quorum set. | If Node, then the arg is the address and port of the node.<br />If InnerSet, then the args are threshold and members. |

Example quorum sets:

Two-of-three simple majority:

```
quorum_set = { threshold = 2, members = [

   { type = "Node", args = "peer1.test.mobilecoin.com:443" },

   { type = "Node", args = "peer2.test.mobilecoin.com:443" },

   { type = "Node", args = "peer3.test.mobilecoin.com:443" },

] }
```

Two-of-three, where one member is an InnerSet quorum set:

```
quorum_set = { threshold = 2, members = [

   { type = "Node", args = "peer1.test.mobilecoin.com:443" },

   { type = "Node", args = "peer2.test.mobilecoin.com:443" },

   { type = "InnerSet", args = { 

        threshold: 2,

        members = [

            { type = "Node", args = "peer3.test.mobilecoin.com:443" },

            { type = "Node", args = "peer4.test.mobilecoin.com:443" },

            { type = "Node", args = "peer5.test.mobilecoin.com:443" },

        ]}

    }

] }
```

##### Quorum Intersection (Sara)

#### Obtaining Ledger Contents from S3 Archive for Catchup

The `tx_src_urls` section of the network.toml specifies the locations to search for obtaining ledger contents from your trusted peers' public archive backups. These are provided as a list of S3 bucket URLs, and catchup involves:

1.  Get quorum's current agreed block height and check against current local block height.

2.  If behind, round-robin through the `tx_src_urls` to pull blocks from S3.

### Signing Consensus Messages

Each consensus validator node signs the consensus messages that it emits, using the Ed25519 `msg-signer-key` parameter passed to consensus.

To obtain this key, use the following code:

```
openssl genpkey -algorithm ed25519 -out msg-signer.key
```

The base64 string in the pem file may be copied directly into the msg-signer-key parameter value, for example:

```
--msg-signer-key MC4CAQAwBQYDK2VwBCIEIJVq95XBwx7XpKnDElrxlL/dwNer0EqKc2igPojJSxHV
```

Next, provide the corresponding public key to peers, who wish you include you in their broadcast_peers specification in the network.toml. This public key must be URL-safe base64 encoded. You can perform this transformation with the following code:

```
openssl pkey -in msg-signer-key.pem -pubout | sed 's/+/-/g; s/\//_/g'
```

Finally, publish the following broadcast_peer public URI:

```
mcp://my_node.com:443/?consensus-msg-key=MCowBQYDK2VwAyEAJ4PLBX2wiSBk8OHS-Qe3EfnpuNiHpH_BpN1wJG2tE2U=
```

![](https://lh3.googleusercontent.com/JYktqIKjI4tee6EqgsPWXan8bQqfRLCOFJ3YpWT9asb4S4mbyBsND5i93AVQbKSXSKXimPT0tiFYgO0YmgsjCkfGu50uDS9wwbYeATwhT76GAVoJl-tLVZeIr4Ci0hZ2do9bgbcV)  Note:  The msg-signer-key is private and should be safeguarded with best practices because anyone with this key could impersonate you and send consensus messages as though they were emitted from your node.

### Distributing Ledger Contents to S3 Archive

The main purpose of a consensus validator node is to agree on whether a transaction is valid, and write that transaction to the ledger. We encourage node operators to offer the ledger publicly via an S3 Archive for three reasons:

-   Allows clients to sync the ledger from multiple nodes that they trust

-   Allows independent auditing of transactions and block signatures

-   Allows peers to obtain blocks they missed while they are in catchup

The `ledger-distribution` process runs in the container, and is configured by the AWS environment variables provided to the container.

The parameters to ledger-distrubution are set by default in the container, and include:

| Parameter | Value | Function |
| --------- | ----- | -------- |
| --ledger-path | Path to local ledger | New blocks added to the local ledger will be synced to S3. |
| --start-from | "next" "last" or "zero" | Indicates whether to start syncing from the next block appended to the ledger, the last written block, or from the origin block. Default is "next," to not overwrite blocks in S3 on restart. |
| --dest | S3 path to ledger bucket | Set by the AWS_PATH environment variable. Note:  AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY must also both be set, containing credentials with write permissions to the bucket. |

### Obtaining the Origin Block

The origin block ships in the container, located at /var/lib/mobilecoin/origin_data/.

You can also obtain the origin block published with each release via S3, for example:

```
aws s3 cp s3://mobilecoin.testnet/${BOOTSTRAP_DATE}/${RELEASE_REVISION}/origin-block-data.mdb
```

### Updating the Quorum Set and Network Configuration

You may wish to update your network.toml for the following reasons:

-   A peer is no longer trusted

-   A peer is no longer available or has changed their details

-   A new trusted peer is available

To update your network configuration, you must edit the network.toml config file, and restart your node.