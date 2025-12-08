---
title: Runtime Metrics
deprecated: false
hidden: false
metadata:
  title: Runtime Metrics
  description: Runtime metrics are track anything about your models over time, from latency to error rates to prediction success rates.
  robots: index
  legacyUUIDs:
    - UUID-96c54f6f-11e9-c03a-653a-468867f033d4
    - UUID-3f918a48-ce37-b7da-1c3e-2e0dccb3f0ac
---
Runtime metrics are track anything about your models over time, from latency to error rates to prediction success rates.

The JFrog ML UI displays the model metrics in the Health in the in the **Overview** tab.

![Health Metrics Dashboard](https://files.readme.io/078cfb2faa4a96e79f56e0b3538d74b9f7d3323f64e6323a2419f420b69bc60c-uuid-c61977ae-e3d2-e7ae-65c7-74ac4100383e.png)

<Table>
  <thead>
    <tr>
      <th>
        Metric
      </th>
      <th>
        Description
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>
        Median response
      </td>
      <td>
        The median time elapsed, in milliseconds, of model inference requests in the last 5 minutes.
      </td>
    </tr>
    <tr>
      <td>
        Error percentage
      </td>
      <td>
        The percentage of 5XX error type of total request in the last 5 minutes.
      </td>
    </tr>
    <tr>
      <td>
        Predication success rate
      </td>
      <td>
        The average success inference requests per minute.
      </td>
    </tr>
    <tr>
      <td>
        Throughput
      </td>
      <td>
        The total amount of requests, in 1-minute long windows.
      </td>
    </tr>
    <tr>
      <td>
        Latency
      </td>
      <td>
        The time elapsed, in milliseconds, of model inference requests. If one or more of these operations fail, this is the time to fail.
      </td>
    </tr>
    <tr>
      <td>
        Error rate
      </td>
      <td>
        The percentage of 5XX errors of total request in 1 aggregation of 1-minute metrics to Datadog.
      </td>
    </tr>
  </tbody>
</Table>



### Exporting Metrics to Your Grafana

JFrog ML allows to connect to the hosted Prometheus and send metrics to Grafana.

If you wish to export the metrics to your Grafana, please contact [mlteam@jfrog.com](mailto:mlteam@jfrog.com).
