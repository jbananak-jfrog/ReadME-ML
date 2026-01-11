---
title: JFrog ML Troubleshooting
deprecated: false
hidden: false
metadata:
  title: JFrog ML Troubleshooting
  description: 'This section reviews the following topics:'
  legacyUUIDs:
    - UUID-6937fc23-a764-1e99-dc9a-4e103faac5f4
    - UUID-838f4093-5a4e-51e4-4a15-59c48a4997cf
  robots: index
---
This section reviews the following topics:

[Performance Issues](/docs/jfrog-ml-troubleshooting#performance-issues)

[Installation Issues](/docs/jfrog-ml-troubleshooting#installation-issues)

[Network and Connectivity Issues](/docs/jfrog-ml-troubleshooting#network-and-connectivity-issues)

<Callout icon="📘" theme="info">
  Make sure you are always working with the latest version of Frog ML SDK, or run the command:

  ```
  pip install --upgrade frogml frogml-cli
  ```
</Callout>

## Performance Issues

##### Symptom: Increase in 429 errors

A high error rate due to`429 error`(too many requests) is caused by a shortage of resources.

##### Solution

This issue can be solved by either scaling vertically or horizontally.

**Vertical scaling**

To scale up, try adding more compute power to your current machines.

* Add more resources (CPU / RAM).
* If CPU / memory aren't fully utilized, increase the number of concurrent workers.

**Horizontal scaling**

To scale out, try adding additional nodes or machines to your infrastructure to cope with the new demand.

* Manually add more pods (increase from 4 to a higher number).
* Use Autoscaling based on CPU / Memory / Latency.

## Installation Issues

##### Symptom: I can't install FrogML SDK

Python SDK deployment on M1

##### Solution

Make sure you are not running with rosetta.

##### Symptom: Getting grpc errors (have 'x86_64', need 'arm64')

##### Solution

When using`conda`and running on Mac M1 CPU, simply run the following command:

`conda install -c conda-forge grpcio`

If the issue is:

_dependency_injector/providers.cpython-39-darwin.so' (mach-o file, but is an incompatible architecture (have 'x86_64', need 'arm64'))_

Do the following:

`pip uninstall dependency\_injector`

`ARCHFLAGS="-arch arm64" pip install dependency\_injector --compile --no-cache-dir`

## Network and Connectivity Issues

<br />

### Accessing JFrog ML over VPN/Proxy

If you're using a VPN or a proxy, you may encounter issues when running JFrog ML commands. This section provides guidance on how to resolve common errors related to SSL certificate verification when behind a VPN or proxy.

#### Certificate Validation Errors

##### Symptom: 

**If your VPN/Proxy encrypts traffic with additional certificates, they should be added to the CA certificate file. Typical certificate issues are appearing when <Anchor label="configuring" title="Setting Up JFrog ML" href="/docs/setting-up-jfrog-ml">configuring</Anchor> your JFrog ML CLI:**

```
Caused by SSLError(SSLCertVerificationError(1, 
'[SSL: CERTIFICATE_VERIFY_FAILED] certificate verify failed: unable to get local issuer certificate (_ssl.c:1129)')
```

##### Solution

**1. Add the CA Certificate to `certifi'`s Bundle**

JFrog ML SDK relies on Python's `certifi` library for server certificate validation. To identify the location of your certificate validation file, run the following Python snippet:

```
import certifi
print(certifi.where())
```

This will output the path to your certificate validation file.

Next, append your VPN's or Proxy's CA certificate to this `cacert.pem` file. Open the file in a text editor with administrative privileges and add the certificate at the end.

**2. Verifying the new Certificate CA with OpenSSL**

After adding your custom certificate, you can validate the connection using OpenSSL with the following command:

```
openssl s_client -connect dev-qwak.us.auth0.com:443 -CAfile /path/to/your/cacert.pem
```

Check the output for a line that says `Verify return code: 0 (ok)`. This indicates that the certificate has been successfully verified. If the verification fails, you will see a different return code along with a description of the failure.

#### Connecting to Cloud Resources Behind Private Networks

##### Symptoms

* You receive timeout errors when JFrog ML tries to access your cloud resources.
* Your cloud resource logs show unauthorized or blocked access attempts from the JFrog ML IP addresses.
* Data transfers or API calls between JFrog ML and your cloud resources are failing without a clear error message.

##### Solution

You may get these symptons if your cloud resources are behind a private network or VPC, you may need to whitelist specific IP addresses to allow the JFrog ML platform to access them. For example when connecting to a new BigQuery or S3 based <Anchor label="Data Source" title="Data Sources" href="/docs/data-sources">Data Source</Anchor> you might get a timeout error.

To ensure seamless connectivity, please add the following range of JFrog ML IP addresses to your network's whitelist:

```
23.21.54.216  
44.212.137.42
```

<Callout icon="📘" theme="info">
  **Note**

  _**Need assistance?**_

  For more information and questions, feel free to reach out to JFrog ML support using the in-platform chat.
</Callout>
