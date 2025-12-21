---
title: Use the Model Dashboards
deprecated: false
hidden: false
metadata:
  title: Use the Model Dashboards
  description: >-
    The model dashboards enable you to monitor and interpret a model's
    performance.
  legacyUUIDs:
    - UUID-55302b8d-08af-416c-72e1-68141963c71d
    - UUID-26cb2236-2b87-1873-404b-298a5e8430c6
  robots: index
---
The model dashboards enable you to monitor and interpret a model's performance.

There are two dashboards:

* Deployed models
* Allowed models, not deployed

#### External API Models

This section describes the model dashboard for models that have been allowed, but that are not deployed.

<Image alt="modeldashboard.png" border={false} src="https://files.readme.io/d617d3968c9e7b6abb6eb1e36d0cee8c25fb9cce1bc822f7d7a8cbcada4b9f93-uuid-77f9b35c-94e6-6893-516a-e4b0f63f54cf.png" />

Note that the dashboard only shows data when there is traffic.

<Table align={["left","left","left","left"]}>
  <thead>
    <tr>
      <th>

      </th>

      <th>
        Metric
      </th>

      <th>
        Description
      </th>

      <th>
        Example/Usefulness
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        **1**
      </td>

      <td>
        **Average Throughput:**
      </td>

      <td>
        The average number of requests processed by the model per unit of time.

        Measurement: Requests per minute (rpm) or tokens per second (depends on configuration).
      </td>

      <td>
        Indicates how many queries the model can handle on average.

        Helps understand the model’s performance capabilities.
      </td>
    </tr>

    <tr>
      <td>
        **2**
      </td>

      <td>
        **Average Time to First Token (TTFT):**
      </td>

      <td>
        The average time (in seconds) for the model to return the first token of the response.

        Measurement: Seconds
      </td>

      <td>
        Reflects perceived responsiveness.

        Critical for user experience, showing the wait time before seeing output.
      </td>
    </tr>

    <tr>
      <td>
        **3**
      </td>

      <td>
        **Throughput Over Time:**
      </td>

      <td>
        A time-series chart showing changes in throughput over a selected period.
      </td>

      <td>
        Useful for identifying performance trends, traffic spikes, or bottlenecks.

        Helps with performance monitoring and improving model efficiency.
      </td>
    </tr>

    <tr>
      <td>
        **4**
      </td>

      <td>
        **Time To First Token Over Time:**
      </td>

      <td>
        A time-series chart showing changes in TTFT over a selected period.
      </td>

      <td>
        Useful for spotting and troubleshooting latency variations due to load, network issues, or model performance.

        Helps understand model response times.
      </td>
    </tr>

    <tr>
      <td>
        **5**
      </td>

      <td>
        **Error Percentage:**
      </td>

      <td>
        The overall percentage of requests that resulted in errors, out of all requests.

        Measurement: %
      </td>

      <td>
        Indicates reliability.

        Example: If 2 out of 100 requests fail, the error percentage is 2%.
      </td>
    </tr>

    <tr>
      <td>
        **6**
      </td>

      <td>
        **Error Rate Over Time:**
      </td>

      <td>
        A time-series chart showing the percentage of failed requests over time.
      </td>

      <td>
        Useful for detecting when errors occur and correlating them with system conditions.

        Critical for understanding error trends and maintaining system health.
      </td>
    </tr>

    <tr>
      <td>
        **7**
      </td>

      <td>
        **Time Period Displayed:**
      </td>

      <td>
        Indicates the time window used for all the metrics shown on the dashboard. Options include the last hour, last 24 hours, last 7 days, etc.
      </td>

      <td>
        Helps in setting the context for the data being reviewed.
      </td>
    </tr>
  </tbody>
</Table>

#### Deployed Model Packages

This section describes the model dashboard for models that are already deployed.

<Image alt="deployed models dashboard" border={false} src="https://files.readme.io/c194d1977b77cbc482ab8f97d94263f13d54502c42a1296da3622df30069ed8c-uuid-08cba23a-9fb9-9140-d949-e1f4cbf25271.png" />

<Table align={["left","left","left","left"]}>
  <thead>
    <tr>
      <th>

      </th>

      <th>
        Metric
      </th>

      <th>
        Description
      </th>

      <th>
        Example/Usefulness
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        **1**
      </td>

      <td>
        **Latency Metrics** (P50 E2E Latency)
      </td>

      <td>
        Measures the median (P50) end-to-end latency of requests, i.e., the time it takes for a request to be processed and a response returned.
      </td>

      <td>
        Indicates how quickly the model responds under typical conditions. Lower latency = better responsiveness.
      </td>
    </tr>

    <tr>
      <td>
        **2**
      </td>

      <td>
        **Request Throughput** (Request Success Rate)
      </td>

      <td>
        The number of successful requests handled by the model per second.
      </td>

      <td>
        Shows how many requests the model can process concurrently and whether it is keeping up with demand.
      </td>
    </tr>

    <tr>
      <td>
        **3**
      </td>

      <td>
        **Token Throughput** (Prompt/Output Tokens/sec)
      </td>

      <td>
        The rate at which tokens are processed or generated by the model, measured in tokens per second:

        * Prompt tokens/sec → how quickly the model ingests the input.
        * Output tokens/sec → how quickly the model generates the response.
      </td>

      <td>
        Useful for evaluating efficiency of inference and ensuring the model meets performance requirements for larger inputs/outputs.
      </td>
    </tr>

    <tr>
      <td>
        **4**
      </td>

      <td>
        **Request Queue** (Waiting Requests)
      </td>

      <td>
        The number of incoming requests waiting in the queue because the model is at capacity.
      </td>

      <td>
        If the queue grows, it indicates the model cannot keep up with traffic. This could point to resource limits or scaling needs.
      </td>
    </tr>

    <tr>
      <td>
        **5**
      </td>

      <td>
        **Token Latency**
      </td>

      <td>
        Measures the average time to process or generate a single token.
      </td>

      <td>
        Provides a fine-grained view of efficiency beyond end-to-end request latency.
      </td>
    </tr>
  </tbody>
</Table>
