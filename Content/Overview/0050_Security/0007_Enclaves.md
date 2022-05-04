---
title: Secure Enclaves (SGX)
---
At least some of the code running on the nodes that participate in consensus protocol must be able to view the complete transactions. However, the operators of these nodes don't need to see this data themselves. MobileCoin confines the sensitive transaction data and all of the code that operates on it in a "secure enclave", bringing the latest advancements in confidential computing to cryptocurrency. Secure Enclaves are a region of memory which provide encrypted code execution. 

A secure enclave provides strong guarantees about the confidentiality and integrity of the software actions being performed inside. Using a secure enclave, we can conceal the complete transactions even from the operators of the nodes that participate in the MobileCoin consensus protocol.

MobileCoin implements secure enclaves using Intel's Software Guard eXtensions (SGX) technology. To understand how SGX improves the confidentiality of user data on-chain, please see our Tech Talk: [Why we use SGX for MobileCoin](https://www.youtube.com/watch?v=Hwf_Q31woLo).
