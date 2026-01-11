---
title: Network and Connectivity Issues
deprecated: false
hidden: false
metadata:
  title: Network and Connectivity Issues
  description: >-
    If you're using a VPN or a proxy, you may encounter issues when running
    JFrog ML commands. This section provides guidance on how to resolve common
    errors related to SSL certificate verification when behind a VPN or proxy.
  legacyUUIDs:
    - UUID-4159744a-ed3a-82c2-cafb-46ffda3a0701
    - UUID-0a8d18ec-415a-fd20-73ac-9b0110f72848
  robots: index
---
## Accessing JFrog ML over VPN/Proxy

If you're using a VPN or a proxy, you may encounter issues when running JFrog ML commands. This section provides guidance on how to resolve common errors related to SSL certificate verification when behind a VPN or proxy.

### Certificate Validation Errors

#### Symptom

If your VPN/Proxy encrypts traffic from with additional certificates, they should be added to the CA certificate file. Typical certificate issues are appearing when <Anchor label="configuring" title="Setting Up JFrog ML" href="/docs/setting-up-jfrog-ml">configuring</Anchor> your JFrog ML CLI:

```
Caused by SSLError(SSLCertVerificationError(1, 
'[SSL: CERTIFICATE_VERIFY_FAILED] certificate verify failed: unable to get local issuer certificate (_ssl.c:1129)')
```

#### Solution

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

### Connecting to Cloud Resources Behind Private Networks

When your cloud resources are behind a private network or VPC, you may need to whitelist specific IP addresses to allow the JFrog ML platform to access them. For example when connecting to a new BigQuery or S3 based <Anchor label="Data Source" title="Data Sources" href="/docs/data-sources">Data Source</Anchor> you might get a timeout error.

#### Symptoms

You might be facing this issue if:

* You receive timeout errors when JFrog ML tries to access your cloud resources.
* Your cloud resource logs show unauthorized or blocked access attempts from the JFrog ML IP addresses.
* Data transfers or API calls between JFrog ML and your cloud resources are failing without a clear error message.

#### Solution

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
