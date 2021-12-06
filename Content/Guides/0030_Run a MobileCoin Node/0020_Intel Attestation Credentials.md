---
title: Getting Intel Attestation Service Credentials
description: 
---
The Intel Attestation Service (IAS) credentials allow node operators to register with Intel as Licensed Enclave operators. This credential links the operator identity with the node's attestation evidence provided to other nodes.

**Step 1:** Apply for and obtain an Intel License Agreement to run MobileCoin. Please see: [Partner Intel License Agreement](https://docs.google.com/document/d/1ATv98iLMDlghbC0q8GmbpL6iSlcquy4sVTWAg4nxS6U/edit?usp=sharing). 

?> Approval can take up to two weeks.

**Step 2:** Once your request is approved and the license issued (confirmed via email), you can create your account at the [Intel Trusted Portal](https://api.portal.trustedservices.intel.com/) using the email associated with your Partner Intel License Agreement.

**Step 3:** Once you log in at the Trusted Portal, this landing page displays. Click on the Intel SGX Attestation Service link to create an EPID subscription.

![](https://lh4.googleusercontent.com/U6-KVCBt9pjC927kKm-cXopvqM3YGtzrCg3z-FOtb3-OWYROU5CqlewzuusCNdfaN55viRp-rvd7NMsUCWzrP6fJh0uE_6plXX_yoTB13ug4LgBobharuP2MpuOuWanog0dAAB1G)

**Step 4:** After you select the Intel Attestation Service, click on the **Subscribe Linkable** blue button (under the Production Access section.)

![](https://lh5.googleusercontent.com/cnKa_YeUeAP3SSy4MFcAo1VOSwgxzII4-HGUqVKSENQFV3XNuQQAuFrs2XSlfaVduzJeeP9PGncGnZVLeysOGjtfBxkfB0oCKlMLQJtcLD7k4k6DITVd6bxS3Sm7X_KG3QAiSsDB)

**Step 5:** {#step-5} Manage your Subscriptions: Click on your username and select **Manage Subscriptions** to see existing subscriptions. The values of the environment variables are available in this section, as mentioned in [Step 6](#step-6), which are required for the consensus service on start-up.

![](https://lh4.googleusercontent.com/eCAoPtnI8peRm4tSCyJUubr6HKVFGgdlPyxK0mJC2Z8XXc0hcOygCjc9AaH-AefnWcvbAQXaosc0CLmqh4p6lmqQS0ZxPv0959Nl0bpD4RjAbGq-mpD01aWs5uJNFHKuaDvfjlUn)

?> On this page, you also can navigate to the Analytics Reports, as shown here, of your attestation requests by clicking on the Analytics reports button.

![](https://lh4.googleusercontent.com/21ruNRyWDjVaT9FdW21ZRrYP8j6pfkTrnWdqlDiUnMd-8jYYicoE1G27RP72zp07vX0OfMdfcBFqO9D3OPs9rtGbtV7dw_shO7Zm5ElNI1UKhm_3QtAeFj7tvQUY-xSm7hrcGG9c)

**Step 6:** {#step-6} Running the Consensus Server with attestation credentials: Provide the following environment variables when running the consensus service:

-   IAS_SPID

-   IAS_API_KEY

The value of both of these values can be found on the [Manage Subscriptions page](#step-5), under your PROD subscription. You can use either the primary or secondary IAS_API_KEY interchangeably.

You also will need to provide the following environment variables in order to get a successful attestation result:

-   SGX_MODE=HW

-   IAS_MODE=PROD

**Step 7:**  Verifying attestation results: On start-up, your consensus validator node will attest to the Intel Attestation Service (IAS). 

The following example log output contains measurement values:

```
2020-09-23 14:08:31.155881673 UTC INFO Measurements: MrEnclave: 49f3e9e5fbb268ea00c78557fb1bd4efa133555a45de2ea30d3fee04443c79af MrSigner: bf7fa957a6a94acb588851bc8767e0ca57706c79f4fc2aa6bcb993012c3c386c, mc.enclave_type: mc_consensus_enclave::ConsensusServiceSgxEnclave, mc.local_node_id: peer1.prod.mobilecoin.com:443, mc.app: consensus-service, mc.local_node_id: peer1.prod.mobilecoin.com:443, mc.module: mc_sgx_report_cache_untrusted, mc.src: sgx/report-cache/untrusted/src/lib.rs:186
```

If your attestation fails, the consensus service will crash. 

?> You can see the output of the attestation, if your log level is set to debug, via setting the environment variable RUST_LOG=debug.

The attestation output looks like the following for a SW_HARDENING_NEEDED response:

```
2020-09-23 14:08:31.155812272 UTC DEBG Quote verified by remote attestation service VerificationReport { sig: VerificationSignature([...]), chain: [[...]], http_body: "{\"nonce\":\"8c048a88269c9b65afead6485cf637ea\",\"id\":\"241816748536716026694834647818919791828\",\"timestamp\":\"2020-09-23T14:08:31.099517\",\"version\":4,\"epidPseudonym\":\"gKH0dexEpYfuyaGgaKKWmH4VJ8r0L3af1W//p6ya+WaN9BAlSW1Gj3NOWvrQIEAyLCof3fwS9pkLnZrYk3CXQMSVKQF9q6j4TSTYdC8OicjpaV9nYrAdYWJ9rf3vxtshavmGUP58xTtknFQOxncAsjzn2maqbm4xhqCrMkzs0fY=\",\"advisoryURL\":\"https://security-center.intel.com\",\"advisoryIDs\":[\"INTEL-SA-00334\"],\"isvEnclaveQuoteStatus\":\"SW_HARDENING_NEEDED\",\"isvEnclaveQuoteBody\":\"AgABAMYLAAALAAoAAAAAAL3lg34HPOkq5u/y0l94xuMAAAAAAAAAAAAAAAAAAAAADw8DBf+ABgAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAABQAAAAAAAAAHAAAAAAAAAEnz6eX7smjqAMeFV/sb1O+hM1VaRd4uow0/7gREPHmvAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAC/f6lXpqlKy1iIUbyHZ+DKV3BsefT8Kqa8uZMBLDw4bAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAEAAQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAABoVXGssD3gYgQ3Gv3zt+rbG9e6ZxZPexmk4vFhAuK0EAYbh4AJHgKTfIWJYeXcJek9l7rISY1aKIMRzJMqixbn\"}" }..., mc.enclave_type: mc_consensus_enclave::ConsensusServiceSgxEnclave, mc.local_node_id: peer1.prod.mobilecoin.com:443, mc.app: consensus-service, mc.local_node_id: peer1.prod.mobilecoin.com:443, mc.module: mc_sgx_report_cache_untrusted, mc.src: sgx/report-cache/untrusted/src/lib.rs:176
```

#### Quote Status Responses

The isvEnclaveQuoteStatus indicates the Intel Attestation Service's assessment of the quote.

| isvEnclaveQuoteStatus | Meaning |
| --------------------- | ------- |
| OK | The enclave's configuration is trusted, as associated with the IAS credentials which requested the verification. |
| SW_HARDENING_NEEDED | The current enclave is subject to the service advisories provided in the response in the advisoryIDs. The client and peers determine which advisoryIDs they are comfortable with, and proceed to make an attested connection. |
| GROUP_OUT_OF_DATE | The enclave's configuration has expired, likely due to an advisory. The client and peers should not trust a GROUP_OUT_OF_DATE enclave in general. However, depending on the severity of a security advisory, the advisory may still be addressed by MobileCoin enclave developers for a period of time. See the [Addressing Group-out-of-Date Attestation Status](/how-to-guides/run-a-mobilecoin-node/running-your-node) section for a full discussion on the operations protocol in an out-of-date scenario. |

### Validating the Attestation

Upon making an attested connection, either via a client-node attested connection, or a node-node attested connection, the cached attestation evidence is validated.

The following example shows a sequence diagram for client-node attestation:

![](https://lh5.googleusercontent.com/Dl6IipZaMXyMR42cDREDPau8uQq1FNJO3fu27C6e_PmDmhIUYnjp0qlQX9v-gF9oDhIa_75J7lusq4BVawFP9Ng_7kKkZQveFnYp7Mn3DWRlV5wpUzuqZ4DX4-l4uNy3E0Qse1Ec)

Figure 1: An example of a sequence diagram for client-node attestation.

### Live Intel Service Advisories

| Advisory | Description | Mitigation |
| -------- | ----------- | ---------- |
| INTEL-SA-00334 | [Load Value Injection Advisory](https://www.intel.com/content/www/us/en/security-center/advisory/intel-sa-00334.html) | CPUs which are not vulnerable to LVI will need to be deployed (these do not exist as of Sep 2020). |

## SGX Provisioning

### Provisioning with Azure

Azure offers SGX machines through their [Confidential Computing](https://azure.microsoft.com/en-us/solutions/confidential-compute/) platform. Intel is in the process of adding SGX-capable machines in multiple regions and availability zones.

?> You will need to request SGX quota (per core) in specific regions. It can take a few days for quota requests to be fulfilled.

Specifications for provisioning with Azure:

| Specification | Preferred | Notes |
| ------------- | --------- | ----- |
| Machine Type | DC4s, DC8s | The number refers to the number of cores available. DC2s are also available, but are too underpowered for MobileCoin workloads. |
| Persistent Storage | Premium SSDs | These are not available in all regions or with all core types. Standard SSD is also acceptable, but may introduce iops bottlenecks. |
| Regions | West EU,<br />UK South | MobileCoin consensus is slightly latency sensitive, so provisioning in regions somewhat geo-located with existing consensus peers is beneficial to the whole network. |