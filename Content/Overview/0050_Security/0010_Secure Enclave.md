---
title: Secure Enclaves
---
Secure Enclaves are a region of memory which provide encrypted code execution. There are many different implementations
of secure enclaves, with varying tradeoffs and security properties. In the cloud infrastructure context, secure enclaves
change the paradigm of remote computing by providing an extension of your [trusted computing base](https://en.wikipedia.org/wiki/Trusted_computing_base)
to include the remote machine.

MobileCoin utilizes Intel's Software Guard eXtensions (SGX) for Confidentiality and Integrity. To understand how SGX 
improves the integrity of MobileCoin's Federated Byzantine Agreement Consensus Protocol, as well as how SGX improves 
the confidentiality of user data on-chain, please see our Tech Talk: [Why we use SGX for MobileCoin](https://www.youtube.com/watch?v=Hwf_Q31woLo).
