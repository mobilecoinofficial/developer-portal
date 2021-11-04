---
title: Common Errors, Alerts, and Glossary
---
## Common Errors

| Common Error | Error Message | Solution |
| ------------ | ------------- | -------- |
| Attestation | "ConsensusEnclave::peer_accept failed: Attested AKE error: Handshake error: The IAS report could not be verified: Error verifying the quote contents: The quote could not be verified: The quote's report could not be verified" | "Verifying your MRENCLAVE value, and software version" |
| Attestation | UTC CRIT thread main on /usr/bin/consensus-service panicked! panicked at 'Failed starting consensus service :-(: ReportCache(RaClient(Reqwest(reqwest::Error { kind: Status(401), url: "<https://api.trustedservices.intel.com/sgx/attestation/v4/sigrl/00000bc6>" })))', consensus/service/src/bin/main.rs: | Verify your IAS credentials, both the IAS_API_KEY and the IAS_SPID, as well as the IAS_MODE=PROD. |
| Enclave | \ | \ |
| Firmware out of date, BIOS out of date | \[need to dig up this error message, we have a real-world example somewhere\] | \ |
| MRENCLAVE | \[Need to test with an incorrect enclave version to get this error message\] | \  |
| MRSIGNER | \[Note: when describing the signer: the signer is the measurement that links the enclave back to the foundation (who signed it)\] | \ |
| Quorum Sets | Out-of-date error (halting or forking) | \ |
| Failure to retrieve block from S3 | \ | Wait a little longer (this is a self-recovering error, but can be unsettling) |
| SSL certificate errors | SSL_VERIFY_FAILED | One of the peers you are connecting to has an expired or misconfigured SSL certificate |
| \ | \ | \ |

## Alerts

| Common Alert | Alert Message | Solution |
| ------------ | ------------- | -------- |
|\ |\ |\ |
|\ |\ |\ |

# Glossary

| Term | Definition |
| ---- | ---------- |
| **Consensus Server** | The MobileCoin Consensus Service facilitates transactions between MobileCoin users. The MobileCoin Consensus Protocol solves the Byzantine Agreement Problem by requiring each user to specify a set of peers that they trust, called a quorum.  |
| **Consensus Validator Node** | Runs Intel's Software Guard eXtensions (SGX), which provides defense-in-depth improvements to privacy and trust. |
| **IAS Account** | An account after you have obtained your Intel Attestation Service credentials. |
| **MobileCoin LedgerDB** | Storage location for the LedgerDB containing downloaded blocks. |
| **MobileCoin Tokens (MOB)** | MobileCoin tokens (MOB, pronounced moh-bee) are a new cryptocurrency that you can send over the internet. |
| **Mobilecoind** | A payment system that lets you send money over the internet using a new currency called "mobilecoins" or MOB. |
| **mobilecoindDB** | Storage location for the mobilecoindDB containing indexed transactions for registered accounts. |
| **MRENCLAVE** | The measurement of the enclave. A stronger measurement of the key that includes all of the contents of the enclave, as well as MRSIGNER. |
| **MRSIGNER** | A measurement of the key, which was used to sign the enclave, but does not contain any contents of the enclave itself. |
| **Quorum Set: Trusted and Untrusted Peers** | To run consensus, you must develop a quorum set that provides the basis of trust required to solve the Byzantine Agreement Problem. You must also link your identity to your node for both Intel's attestation verification, as well as to sign messages in consensus. Determine who you trust for your quorum. By figuring out who to include in your quorum set, you specify peers that you trust. MobileCoin uses Universal Resource Identifiers (URIs) to specify peers. These include the address of the peer, the peer's public key, and can optionally include connection information, such as the certificate authority bundle, and tls-hostname. |
| **Secure Enclaves** | The MobileCoin Network implements secure enclaves using Intel's Software Guard eXtensions (SGX) to process new transactions according to the MobileCoin Consensus Protocol for integrity and confidentiality. Recent advances in trusted computing make it possible to run software on a remote host without exposing sensitive data to that server's operator, even when she has complete control of the remote computer's operating system (i.e. root access). |
| **Universal Resource Identifiers (URIs)** | Used by MobileCoin to specify peers. The information includes the address of the peer, the peer's public key, and can optionally include connection information, such as the certificate authority bundle, an the tls-hostname. |