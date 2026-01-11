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

<br />

## Network and Connectivity Issues

<br />
