---
title: Inference Distribution (Analytics)
excerpt: >-
  Evaluate model stability and detect drift by comparing production data against
  training baselines.
deprecated: false
hidden: false
metadata:
  title: Inference Distribution (Analytics)
  description: >-
    This section outlines the process of comparing the distribution of inference
    data against the baseline established by training data. Understanding these
    distributions is essential for assessing model performance and identifying
    potential drifts or biases that may impact prediction accuracy.
  legacyUUIDs:
    - UUID-0c6cbe4c-7d9f-1f95-fe0b-22948b28817a
    - UUID-8a9bfb66-4435-0ada-2e50-a7532c5442ba
  robots: index
---
Use the analytics in this section to validate that your deployed model is operating on data consistent with what it learned during training. By analyzing these distributions, you can proactively identify **data drift**, spot emerging **biases**, and assess whether shifts in input data are negatively impacting **prediction accuracy**.

**Inference Data**: Data used by the model to make predictions post-deployment, combined with the outputs for each of the processed predictions.

**Training Data Baseline**: The original dataset upon which the model was trained, serving as a benchmark for expected data distributions.

This guide helps you navigate the following distinct analysis workflows:

* [Requirements](/docs/inference-distribution-analytics#requirements)Requirements: Prerequisites for setting up distribution monitoring
* [Inference Data vs. Training Baselines](/docs/inference-distribution-analytics#inference-data-vs-training-baselines)Inference Data vs Training Baselines: Direct comparison to spot deviations from the original benchmark.
* [Inference-only Data](/docs/inference-distribution-analytics#inference-only-data) Inference-only Data: Analyzing production trends when baseline data is unavailable
* [Available Statistics](/docs/inference-distribution-analytics#available-statistics)Available Statistics: The specific metrics used to quantify distribution changes.

### Requirements

JFrog ML automatically logs inference data to the JFrog ML Analytics Lake, which is later used for monitoring and data distribution. However, there are a few hard requirements for enabling the data Distribution dashboards for your model.

1. The model should have a `ModelSchema` defined
2. The data in the ModelSchema should be **numerical** or **categorical**

Distribution is being charted both on inference-only data, as well as comparing inference with the training baseline data. For the latter is **mandatory to log your training dataset** as a Pandas Dataframe to the JFrog ML Registry during your Build process.

### Inference Data vs. Training Baselines

In order to compare **inference** data with **training** data please select the relevant build from `Build ID` in the UI and the relevant Dataframe that was previously logged and tagged, under the`Data tag` dropdown.

<Image alt="UI for selecting build and data tag for comparison." border={false} src="https://files.readme.io/830e833fbc6f40e146111ee266f3c79a3272dae24f0fabd507cfa72eca7c2960-uuid-e4526ebd-2416-2638-3df5-4fde40c0b7c5.png" />

**Example Dashboard:**

<Image alt="real-time-churn-model.png" border={false} src="https://files.readme.io/309e3cbb9fc60efc0143aa48cd4a2952e9eaec6ec54cbb3155c0ed054a59cd4a-uuid-a2247c18-8207-3f93-4d7f-8673676976f7.png" />

### Inference-Only Data

**Example Dashboard:**

<Image alt="inference-only-data.png" border={false} src="https://files.readme.io/40d6f6054d42dfee83ebca181da432858e7ddb04bc3a6cdbe3f5da37169c32b3-uuid-870a011b-ff84-e58c-9a8d-99f9117e1947.png" />

### Available Statistics

#### Average (Mean)

The mean is the average of all numbers in a dataset. It's calculated by summing all values and dividing by the number of values. The mean provides a central location for the data. The mean is used to find the central tendency of the data. It's sensitive to outliers, meaning that extreme values can significantly affect it.

#### Standard Deviation (Std. Deviation)

Standard deviation measures the amount of variation or dispersion of a set of values. A low standard deviation indicates that the values tend to be close to the mean, whereas a high standard deviation indicates that the values are spread out over a wider range. It's used to quantify the amount of variability or spread in a set of data values. It helps in understanding how much the data deviates from the average.

#### Minimum (Min)

The minimum is the smallest number in a dataset. It helps in identifying the lower boundary of the data distribution, enabling the understanding of data range and detecting outliers.

#### 25th Percentile (P25)

The 25th percentile (also known as the first quartile) is the value below which 25% of the observations in a dataset fall. It marks the lower quartile of the data. It's used to understand the distribution of data by dividing the dataset into four equal parts. The 25th percentile gives insights into the lower end of the data distribution, excluding the lowest outliers.

#### 50th Percentile (Median, P50)

The 50th percentile, or median, is the middle value of a dataset when it is ordered in ascending or descending order. If there is an even number of observations, the median is the average of the two middle numbers. The median is a measure of central tendency that is less sensitive to outliers compared to the mean. It effectively represents the middle of a dataset.

#### 75th Percentile (P75)

The 75th percentile (also known as the third quartile) is the value below which 75% of the observations in a dataset fall. It marks the upper quartile of the data. Similar to the 25th percentile but for the upper end of the data distribution, it helps in understanding the distribution above the median. It is useful for identifying the range within which the bulk of the data points lie, excluding the highest outliers.

#### Maximum (Max)

The maximum is the largest number in a dataset. It provides the upper boundary of the data distribution, allowing for the assessment of the data range and the detection of outliers.

<Callout icon="📘" theme="info">
  **Note**

  Please note that the inference distribution charts do not incorporate data from `FeatureStoreInput`. To view this data, please refer to the Feature Set page where your model retrieves its inference data.
</Callout>
