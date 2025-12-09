---
title: Performance Issues
deprecated: false
hidden: false
metadata:
  title: Performance Issues
  description: I see an increase in 429 errors
  robots: index
  legacyUUIDs:
    - UUID-a9c70a8e-e0e3-e4ee-e5e9-51950360778b
    - UUID-4e3fbf00-9657-d829-4ba0-f0c94aeb87d8
---
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
