# Glossary

| Term | Definition |
| ----------- | ----------- |
| **Consensus Server** | The MobileCoin Consensus Service facilitates transactions between MobileCoin users. The MobileCoin Consensus Protocol solves the Byzantine Agreement Problem by requiring each user to specify a set of peers that they trust, called a quorum.  |
| **Consensus Validator Node** | Runs Intel's Software Guard eXtensions (SGX), which provides defense-in-depth improvements to privacy and trust. |
| **Fog Service** | Composed of three services and a ledger sync process: Ledger, Ingest, View, and Report. |
| **IAS Account** | An account after you have obtained your Intel Attestation Service credentials. |
| **Ingest Server** | Part of the Fog Service, the Ingest Server processes each transaction in the ledger according to registered users. |
| **Ledger Server** | Part of the Fog Service, the Ledger server serves the contents of the ledger obliviously. |
| **MobileCoin LedgerDB** | Storage location for the LedgerDB containing downloaded blocks. |
| **MobileCoin Tokens (MOB)** | MobileCoin tokens (MOB, pronounced moh-bee) are a new cryptocurrency that you can send over the internet. |
| **Mobilecoind** | A payment system that lets you send money over the internet using a new currency called "mobilecoins" or MOB. |
| **mobilecoindDB** | Storage location for the mobilecoindDB containing indexed transactions for registered accounts. |
| **MRENCLAVE** | The measurement of the enclave. A stronger measurement of the key that includes all of the contents of the enclave, as well as MRSIGNER. |
| **MRSIGNER** | A measurement of the key, which was used to sign the enclave, but does not contain any contents of the enclave itself. |
| **Quorum Set: Trusted and Untrusted Peers**  | To run consensus, you must develop a quorum set that provides the basis of trust required to solve the Byzantine Agreement Problem. You must also link your identity to your node for both Intel's attestation verification, as well as to sign messages in consensus. Determine who you trust for your quorum. By figuring out who to include in your quorum set, you specify peers that you trust. MobileCoin uses Universal Resource Identifiers (URIs) to specify peers. These include the address of the peer, the peer's public key, and can optionally include connection information, such as the certificate authority bundle, and tls-hostname. |
| **Report Server** | Part of the Fog Service, the Report Server serves the Ingest's pubkey and SGX report to construct transactions. |
| **Secure Enclaves** | The MobileCoin Network implements secure enclaves using Intel's Software Guard eXtensions (SGX) to process new transactions according to the MobileCoin Consensus Protocol for integrity and confidentiality. Recent advances in trusted computing make it possible to run software on a remote host without exposing sensitive data to that server's operator, even when she has complete control of the remote computer's operating system (i.e. root access). |
| **Universal Resource Identifiers (URIs)** | Used by MobileCoin to specify peers. The information includes the address of the peer, the peer's public key, and can optionally include connection information, such as the certificate authority bundle, and the tls-hostname. |
| **View Server** | Part of the Fog Service, the View Server obliviously provides TxOs belonging to registered users. |

# Frequently Asked Questions

This section includes some of the most frequently asked questions (FAQs) by node operators:

### Certs

Question: Is it possible in general to generate self-signed certs and put their own cert in front of it?

Answer:

-   It is possible to use self-signed certs behind a load-balancer, as long as each consensus node is uniquely addressable by a URI without a path, e.g. mcp://somenode:12345

-   Platform providers can use a wildcard cert at the load balancer. Currently, MobileCoin nodes operate this way. All nodes connect via TLS to the frontend NGINX ingress, but the nginx ingress then connects back to the nodes in the clear. The cert needs to be pinned internally, so that nginx knows that this cert belongs to this specific host. 

-   You will need to share your public keys with every partner and update them when they change.

Question: Can platform providers use a letsencrypt cert that they are already using?

     Answer: You could use a wildcard cert that you already have, but you need a new cert (even if self-signed) for every consensus participant.

### Intel Attestation Service

Question: Do customers need their own Intel Attestation Service (IAS) Service Provider ID (SPID) key and IAS API Key?

Answer: No. All customers of a provider can use the same IAS SPID and IAS API key.

### Nodes

Question: Can platform providers launch nodes in different ways?

Answer:  [Need answer]

### S3

Question: Do customers control whether their node pushes blocks to S3?

Answer: Either way - platform providers can provide an option to provision bucket for ledger distribution (per-org, per customer)

### TLS Certifications

Question: Do all nodes need to have unique TLS certs / can platform providers manage bastion hosts and have insecure connections between nodes internally?

Answer: All nodes do need to have TLS certs

Question: Is there something specific that has to be in the TLS certs?

     Answer: All by DNS and not by IP

Question: If we renew a cert do we have to restart the node?

     Answer: Yes, because there is a TLS listener inside the nodes