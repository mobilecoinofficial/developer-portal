---
title: Secure Enclaves
---
At least some of the code running on the nodes that participate in MCP must be able to view the complete transactions. However, the operators of these nodes don't need to see this data themselves. The final layer of the MobileCoin system confines the sensitive transaction data and all of the code that operates on it in a "secure enclave", bringing the latest advancements in confidential computing to cryptocurrency.

Secure Enclaves are a region of memory which provide encrypted code execution. There are many different implementations of secure enclaves, with varying tradeoffs and security properties. In the cloud infrastructure context, secure enclaves change the paradigm of remote computing by providing an extension of your [trusted computing base](https://en.wikipedia.org/wiki/Trusted_computing_base) to include the remote machine.

A secure enclave provides strong guarantees about the confidentiality and integrity of the software actions being performed inside. Using a secure enclave, we can conceal the complete transactions even from the operators of the nodes that participate in MCP.

MobileCoin utilizes Intel's Software Guard eXtensions (SGX) for secure enclaves. To understand how SGX 
improves the integrity of MobileCoin's Federated Byzantine Agreement Consensus Protocol, as well as how SGX improves 
the confidentiality of user data on-chain, please see our Tech Talk: [Why we use SGX for MobileCoin](https://www.youtube.com/watch?v=Hwf_Q31woLo).
