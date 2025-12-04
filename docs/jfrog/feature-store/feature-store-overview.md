---
title: Feature Store Overview
deprecated: false
hidden: false
metadata:
  title: Feature Store Overview
  description: JFrog ML's Feature Store is a centralized service that facilitates the discoverability, reuse and accuracy of features.
  robots: index
  legacyUUIDs:
    - UUID-9074faef-2442-3b9b-43fa-3303aeb3b6b7
    - UUID-a3608a20-7235-77b9-9b1c-42948cb5e9f1
---
JFrog ML's Feature Store is a centralized service that facilitates the discoverability, reuse and accuracy of features.

It provides a centralized method for developing features using batch or streaming data, and for serving those features instantly or retrieving them as training data. It also allows the discovery and reuse of available features, instead of recreating identical or similar ones.

The Feature Store serves the following main purposes:

* **Source of truth**: A single and discoverable source of truth for features to be used by machine learning models.
* **Feature collaboration**: A mechanism that enables data scientists and machine learning engineers to share features between projects.
* **Ensures Consistency (Prevents Training/serving skew)**: Systematically ensures features generated for training (offline) and inference (online) are identical.

### Feature Store Concepts

The JFrog ML <Anchor label="Feature Store" href="https://jfrog.com/blog/what-is-a-feature-store-in-ml-and-do-i-need-one/" target="_blank">Feature Store</Anchor> follows three main concepts:



<Table>
  <tbody>
    <tr>
      <td>
        **Entity Keys**
      </td>
      <td>
        The specific identifier (for example: `user_id`, `transaction_id`, `merchant_id`) for which feature values are calculated and retrieved.
      </td>
    </tr>
    <tr>
      <td>
        **Data Sources**
      </td>
      <td>
        The external systems (for example, databases, event streams) from which raw data is ingested to create features.
      </td>
    </tr>
    <tr>
      <td>
        **Feature Sets**
      </td>
      <td>
        The operational unit of the Feature Store. This computational definition (schema and logic) takes raw data as input and outputs a logical group of related features.
        
        Feature Set Types:
        
        * **Batch Feature Sets:** Features defined from static or historical batch data sources (for example, Snowflake, BigQuery).
        * **Streaming Features Sets:** Features defined from streaming data sources (for example, Kafka) for continuous, near real-time updates.
        * **Real Time Feature Sets:** Features based on data provided directly at the time of an inference request and are not pre-computed.
        
        
<Callout icon="📘" theme="info">
        **Note**
        
        JFrog ML Cloud (SaaS) supports [Batch Feature Sets](/docs/batch-feature-set "Batch Feature Set") only. To use real-time and streaming features, please opt for JFrog ML hybrid deployments.
</Callout>

      </td>
    </tr>
  </tbody>
</Table>



### Feature Consumption

With the JFrog ML Feature Store you can **define** features once, **calculate** them once, and **reuse** them at any time.

JFrog ML's Feature Store systematically ensures that there are no discrepancies between data generated for training and serving. Both the offline and the online stores are populated from the same singular feature extraction process.

* **Inference**: Serve the most up-to-date feature values for a given Key from one centralized location.
* **Training**: Keep a log of all features, and then retrieve them for training, at any point in time.
![feature-store-materialization.png](../image/uuid-a4086297-addb-f7cd-8da2-7eba66c0b28d.png)
