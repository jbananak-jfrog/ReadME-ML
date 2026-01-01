---
title: Runtime Metrics
deprecated: false
hidden: false
metadata:
  title: Runtime Metrics
  description: >-
    Runtime metrics are track anything about your models over time, from latency
    to error rates to prediction success rates.
  legacyUUIDs:
    - UUID-96c54f6f-11e9-c03a-653a-468867f033d4
    - UUID-3f918a48-ce37-b7da-1c3e-2e0dccb3f0ac
  robots: index
---
Runtime metrics  track your models' metrics over time, from latency to error rates to prediction success rates.

The JFrog ML UI displays the model metrics in the Health in the in the **Overview** tab.

<Image alt="Health Metrics Dashboard" border={false} src="https://files.readme.io/078cfb2faa4a96e79f56e0b3538d74b9f7d3323f64e6323a2419f420b69bc60c-uuid-c61977ae-e3d2-e7ae-65c7-74ac4100383e.png" />

| Metric                   | Description                                                                                                                        |
| ------------------------ | ---------------------------------------------------------------------------------------------------------------------------------- |
| Median response          | The median time elapsed, in milliseconds, of model inference requests in the last 5 minutes.                                       |
| Error percentage         | The percentage of 5XX error type of total request in the last 5 minutes.                                                           |
| Predication success rate | The average success inference requests per minute.                                                                                 |
| Throughput               | The total amount of requests, in 1-minute long windows.                                                                            |
| Latency                  | The time elapsed, in milliseconds, of model inference requests. If one or more of these operations fail, this is the time to fail. |
| Error rate               | The percentage of 5XX errors of total request in 1 aggregation of 1-minute metrics to Datadog.                                     |

### Exporting Metrics to Your Grafana

JFrog ML enables connection to the hosted Prometheus and sending of metrics to Grafana.

If you wish to export the metrics to your Grafana, please contact [mlteam@jfrog.com](mailto:mlteam@jfrog.com).

<br />
