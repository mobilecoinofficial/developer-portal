---
title: "SGX Attestation Crate"
description: "This crate exists to support remote attestation via Intel SGX, albeit on a lower level than the Intel SGX messaging. This enables application selection of the enclave's key algorithms and negotiation, rather than Intel's selection of the NIST P256 curve."
use_file: "mobilecoinfoundation/mobilecoin/attest/core/README.md"
---
This crate exists to support remote attestation via Intel SGX, albeit on a lower level than the Intel SGX messaging. This enables application selection of the enclave's key algorithms and negotiation, rather than Intel's selection of the NIST P256 curve. In particular, it provides:

 * A rust-like API for interacting with the SGX C library types and functions exposed by Baidu's rust-sgx-sdk.
 * Support for and linkage to the SGX SDK simulation libraries via a cargo feature.
 * An API for parsing and validating IAS verification reports in a `no_std` environment.
 * Common serialization for SGX data structures, as X86_64 C structs (to aid validation of structures on non-x86_64 platforms)
 * Common API for dealing with SGX structures, regardless of environment.
 
 Please see [this document](https://github.com/mobilecoinfoundation/mobilecoin/blob/master/attest/core/README.md) for more details.
