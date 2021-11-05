# This Section is in Development

This is a reminder that this "Run Fog" section is still being edited and requires another pass.

# About this Manual

MobileCoin Fog is a scalable service infrastructure that enables a smartphone to manage a privacy-preserving cryptocurrency with locally-stored cryptographic keys. Unique from the public ledger, the Fog Service returns all or some portion of user transactions obliviously to enable operators to look up transactions. The MobileCoin Fog Service is composed of three services and a ledger sync process:

-   Ingest: Processes each transaction in the public ledger according to registered users.

-   Report: Serves the Ingest's pubkey and SGX report to construct transactions (Txos).

-   View: Obliviously provides Txos belonging to registered users.

-   Ledger: Serves the contents of the public ledger obliviously.

![](https://lh3.googleusercontent.com/AaHG9Iez8m4ODL5ZK6DE3zo5AH8cP3PIqpYFauqsnVGev1oiMi1IsQ1bRbc7YdjfHmwbxhmEgkUaXg8FCJsGOrDZ53F1RDRPU4o25P_GACHvs3ilOMRD--Igmx5kx1INYwKt8lNL)
Figure 1: The Fog System Overview.

This manual contains a comprehensive overview of one part of the MobileCoin Fog Server, the Fog Ingest Service, and instructions on how to set up and manage a remote Fog Ingest node on the MobileCoin Network.

For more information about the MobileCoin Network, check out the [MobileCoin Ecosystem](https://docs.google.com/document/d/1vbnqfwW6sED7t61YKDIAItK4eWM3lSIDXoT18Nl9KfY/edit?usp=sharing) - a brief document about the MobileCoin open-source software network.

## Who Should Read this Manual

This manual should be read and referenced by operators, who have been entrusted with setting up and managing the MobileCoin Fog services in the MobileCoin network. 

## How to Use this Manual

This manual provides operators with quick help for setting up and operating the MobileCoin Fog Servers. This document is separated into three sections:

1.  Overview of the Fog Servers

2.  Step-by-step instructions for common tasks

3.  Lists of errors and alerts the Fog Servers may generate with solutions.

Special symbols are used throughout the manual to make you aware of important information and valuable tips.

## Special Symbols Used in this Manual

The following symbols represent important information and valuable tips associated with the MobileCoin Fog Ingest Server and are displayed throughout the manual. Every noted symbol has significant meaning that you should heed:

![](https://lh5.googleusercontent.com/B88UcyxpWYktoK4neFxCbGHrAW3yg9ciF9dd-IsHL2a79iIADuTd96ikGe8DcTQ9bN87KIwi1xb8HSTjlKmyc2KhU7fEfB_uSdw0WDJmgmrN4kydIXAc8mEM2QV6_ikxTeNPctSM)  Note: Important information concerning operating the Fog Ingest Server.

![](https://lh5.googleusercontent.com/jnmJKdfktFYNYmiMMSyT9xByfEgefebqp_x4qJTxKh4KscODPU_vs_WbN7F2L21M3wMQbvPOg1IrzXv-IRqmC102YpgdPE_6w8alUEdEps00kPoGT5TtuClMEJAYBDivyaLy67By) Warning: Failure to follow directions may result in preventing your Fog Ingest Server from operating successfully in the MobileCoin Network.