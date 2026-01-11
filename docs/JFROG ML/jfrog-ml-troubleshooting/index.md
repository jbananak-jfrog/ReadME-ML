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

[Performance Issues](/docs/performance-issues)

[Installation Issues](/docs/installation-issues)

<Anchor label="Network and Connectivity Issues" title="Network and Connectivity Issues" href="/docs/network-and-connectivity-issues">Network and Connectivity Issues</Anchor>

<br />

<Callout icon="📘" theme="info">
  Make sure you are always working with the latest version of Frog ML SDK, or run the command:

  ```
  pip install --upgrade frogml frogml-cli
  ```
</Callout>

## Performance Issues

**I see an increase in 429 errors**

A high error rate due to`429 error`(too many requests) is caused by a shortage of resources. This issue can be solved by either scaling vertically or horizontally.

**Vertical scaling**

To scale up, try adding more compute power to your current machines.

* Add more resources (CPU / RAM).
* If CPU / memory aren't fully utilized, increase the number of concurrent workers.

**Horizontal scaling**

To scale out, try adding additional nodes or machines to your infrastructure to cope with the new demand.

* Manually add more pods (increase from 4 to a higher number).
* Use Autoscaling based on CPU / Memory / Latency.

## Installation Issues

**I can't install FrogML SDK**

* Python SDK deployment on M1 - make sure you are not running with rosetta.

**I'm getting grpc errors (have 'x86_64', need 'arm64')**

When using`conda`and running on Mac M1 CPU, simply run the following command:

`conda install -c conda-forge grpcio`

If the issue is

_dependency_injector/providers.cpython-39-darwin.so' (mach-o file, but is an incompatible architecture (have 'x86_64', need 'arm64'))_

Do the following:

`pip uninstall dependency\_injector`

`ARCHFLAGS="-arch arm64" pip install dependency\_injector --compile --no-cache-dir`

## Network and Connectivity Issues

<br />
