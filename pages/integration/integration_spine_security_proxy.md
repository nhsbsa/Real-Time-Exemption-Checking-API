---
title: Spine Security Proxy (SSP)
keywords: spine, proxy, ssp, security
tags: [integration]
sidebar: overview_sidebar
permalink: integration_spine_security_proxy.html
summary: "Overview of the role of the Spine Security Proxy (SSP) within the Prescription Exemption Checking Service."
---

## Spine Security Proxy (SSP) ##

The SSP is a forward HTTP proxy which will be used as a front end to control and protect access to the Prescription Exemption Checking Service in Phase 1.  It provides a single security point for both authentication and authorisation for dispensing systems. Additional responsibilities include auditing of all requests, throttling of requests and transaction logging for performance purposes.

![Spine Security Proxy](images/integration/Spine Security Proxy Block Diagram.png)

### Proxied Prescription Exemption Checks ###

For Phase 1 implementations the API will be proxied by the Spine Security Proxy. Endpoint location for Spine-connected endpoints is normally carried out on Spine Directory Service. As in later phases the Prescription Exemption Checking Service will be wholly provided by the NHSBSA connecting systems should not look up endpoint details on SDS but retrieve from local configuration.

Systems must connect and authenticate with the proxy using TLS mutual authentication (MA) using spine system certificates.

{% include important.html content="All N3 connected systems are expected to synchronise their local system clocks to the N3 Network Time Protocol (NTP) reference source." %}

An HTTP request to the proxy server will be suffixed with the API call, hence:

```http
POST https://proxy.int.spine2.ncrs.nhs.uk/https://testapps.nhsbsa.nhs.uk/Prescription-Exemption/1.0.0/
```

A number of Spine-specific HTTP headers also need to be populated with the intended Spine interactionID and system ASIDs.

| Header               | Value |
|----------------------|-------|
| `Ssp-TraceID`        | Consumer's TraceID (i.e. GUID/UUID) |
| `Ssp-From`           | Dispensing System's ASID |
| `Ssp-To`             | NHSBSA's ASID |
| `Ssp-InteractionID`  | Prescription Exemption Checking Service InteractionID |

Please refer to the [Spine Security Proxy Implementation Guide](integration_spine_security_proxy_implementation_guide.html) for full technical details.

Interaction ID and SDS details are available at [Spine Security Proxy Implementation Guide](integration_spine_security_proxy_implementation_guide.html)

Furthermore, the consumer's client certificate is validated to ensure it includes valid certificate `CN` and `DN` details and that the certificate has not been added to the certificate revocation list (CRL).