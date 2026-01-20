---
title: Model Version Management
deprecated: false
hidden: false
metadata:
  title: Model Version Management
  description: Summary
  legacyUUIDs:
    - UUID-ba36aa0f-b07b-ae12-32c2-3734d41a7cac
    - UUID-8ab1812c-5320-a8cb-dbe1-80fcae8118c4
  robots: index
---
## Summary

Model Versions are trained experiments saved with their code, dependencies, and optional data like metrics or parameters. Version Management ensures these experiments are reproducible, traceable, and securely stored in JFrog Artifactory as a single source of truth.

With the FrogML SDK, you can easily log, package, and upload models as trackable versions. Validated models can be promoted to production builds, creating immutable Docker images through an automated workflow. You can also include custom prediction logic to ensure deployed models handle specific inference requirements.

The _Versions_ tab in the Custom Models page shows a list of available versions of the model you selected from the AI Catalog. This list might include versions that have been built, unbuilt, or updated.

<Image alt="catboost_model_mor_1.png" border={false} src="https://files.readme.io/85273a3a00c871ce486b2cf6f05754fb9d53a92df6307fb2723a1f5d5a841f5d-uuid-c313ed70-f97c-5376-c818-15fa4886459a.png" />

* **Unbuilt Versions:** New versions exist but have not yet been built (processed). These model versions have the "**Build Version**" button in the Build column.
* **Built Versions:** Versions of a model that have already undergone the necessary processing and compilation that makes them ready for deployment and integration into various applications. These model versions have the "**Rebuild Version**" button in the Build column.

<Cards columns={4}>
  <Card title="Train Your Model →" href="https://readme.com" target="_blank">
    Train your machine learning model locally using your preferred frameworks and datasets.
  </Card>

  <Card title="Log Your Model →">
    Using the FrogML SDK's log\_model() function, specify your model and the target Artifactory repository. You can include metadata, such as dependencies and training metrics; properties, parameters, and metrics are optional. For custom inference logic, dependencies, predict\_file, and code\_dir are mandatory.
  </Card>

  <Card title="Secure Packaging →">
    The SDK automatically handles the work of packaging your model and all its associated files.
  </Card>

  <Card title="Upload to Artifactory →">
    The packaged model is then uploaded to your designated ML repository in Artifactory, where it becomes a new, trackable Model Version. This gives you a single, centralized location for all your experiments and models.
  </Card>
</Cards>

<Callout icon="📘" theme="info">
  **Note**

  To learn more about FrogML and machine learning repositories in Artifactory, see [Machine Learning Repositories](/artifactory/docs/machine-learning-repositories)
</Callout>

### Log your model

**Simple model version**

```python
import frogml
from catboost import CatBoostClassifier

# Assumes you have a trained CatBoost model catboost_model = CatBoostClassifier()
frogml.catboost.log_model(
    model=catboost_model,
    repository="nlp",
    model_name="sentiment_analysis",
    version="v2_dataset_exp"
)
```

<br />

**Model with additional metadata and properties**

```python
import frogml

# Assumes you have a trained model

loaded_model = get_my_model()
frogml.catboost.log_model(
    model=loaded_model,
    repository="<JFML_MODEL_REPO>",
    model_name="<JFML_MODEL_NAME>",
    version="<Experiment Name>",
    properties={"trained_by": "data_science_team"},
    metrics={"accuracy": 0.92, "f1_score": 0.88}
)
```

## Custom Prediction Logic Integration

This feature meets the demand for custom prediction logic in real-world deployment scenarios, accommodating common requirements like data validation, transformation, and other runtime needs. It enables the inclusion of user-defined prediction functions within a model version, ensuring the deployed model performs exactly as intended.

### How to add custom prediction code:

1. In your python code, use the example code snippet below to add a custom prediction function.
2. If you want to define the name of the new build version, add the line: `version="<new version name>"` in the code.

**Logging a Hugging Face model without a custom prediction function**:

```python
import frogml
from transformers import DistilBertTokenizer, DistilBertForSequenceClassification

model_name = "distilbert/distilbert-base-uncased-finetuned-sst-2-english"
tokenizer = DistilBertTokenizer.from_pretrained(model_name)
model = DistilBertForSequenceClassification.from_pretrained(model_name)

frogml.huggingface.log_model(
    repository="nlp-models",
    model_name="sentiment_analysis",
    model=model,
    tokenizer=tokenizer,
)
```

**Logging a Hugging Face model with a custom prediction function:**

```python
import frogml
from transformers import DistilBertTokenizer, DistilBertForSequenceClassification

model_name = "distilbert/distilbert-base-uncased-finetuned-sst-2-english"
tokenizer = DistilBertTokenizer.from_pretrained(model_name)
model = DistilBertForSequenceClassification.from_pretrained(model_name)

frogml.huggingface.log_model(
    repository="nlp-models",
    model_name="sentiment_analysis",
    model=model,
    tokenizer=tokenizer,
    code_dir="./relative_path/to/code/dir/",
    dependencies="./relative_path/to/code/dir/requirements.txt",
    predict_file="/path/to/code/dir/predict.py" # This file must be in code_dir
)
```

<Callout icon="❗️" theme="error">
  **Important**

  **`code_dir` Path and Model Pickling**

  The `code_dir` parameter specifies the local directory whose contents will be packaged with your model. During the remote build, these contents are copied directly into a `main` directory, which serves as the root of the Python import path for the build.

  This is critical for serialized models (such as Scikit-learn) that are "pickled". The unpickling process on the server must be able to find the exact module path that was used when the model was saved. If your model class is defined inside a package, you must log the parent directory.

  **Example Scenario:**

  <Table align={["left","left"]}>
    <thead>
      <tr>
        <th>

        </th>

        <th>
          Description
        </th>
      </tr>
    </thead>

    <tbody>
      <tr>
        <td>
          **Local Structure:**
        </td>

        <td>
          ```
          .
          └── serving_code/
              ├── __init__.py
              └── predict.py  (contains YourModelClass)
          ```
        </td>
      </tr>

      <tr>
        <td>
          **Serving Code:**
        </td>

        <td>
          `code_dir="serving_code"`
        </td>
      </tr>

      <tr>
        <td>
          **Remote Result:**
        </td>

        <td>
          The contents of `serving_code` (for example, `predict.py`) are copied to `main`. The remote structure becomes `main/predict.py`.
        </td>
      </tr>

      <tr>
        <td>
          **The Error:**
        </td>

        <td>
          The pickled model looks for its class at `serving_code.predict.YourModelClass`, but the `serving_code` module no longer exists. This results in the `No module named 'serving_code'` error.
        </td>
      </tr>
    </tbody>
  </Table>

  **Recommended Solution:**  Log the parent directory that contains your package.

  |                    | Description                                                                                                 |
  | :----------------- | :---------------------------------------------------------------------------------------------------------- |
  | **Serving Code:**  | `code_dir="."` (assuming that your script is in the parent folder).                                         |
  | **Remote Result:** | The `serving_code` folder is correctly copied. The remote structure becomes `main/serving_code/predict.py`. |
  | **The Solution:**  | The unpickler can now successfully import `serving_code.predict` and load your model.                       |
</Callout>

## Promotion of Model Version to Production-Ready Build

The FrogML platform enables efficient movement of successful experiments from development to production. It allows conversion of a Model Version—an experiment with code and metadata—into a Build, which serves as a self-contained, traceable record of all artifacts and dependencies required to create a production-ready Docker image for deployment.. This automated process is seamless, and one model version can generate multiple builds, thus facilitating deployment of the same model with varied dependencies or configurations.

<Cards columns={3}>
  <Card title="Initiate Promotion →">
    Initiate the process by clicking a button in the JFrog UI, selecting the desired model version, and providing any required details, such as a name for the new build
  </Card>

  <Card title="Automated Build Process →">
    Behind the scenes, the system retrieves all files associated with the model version, including code and dependencies, and packages them into a secure, temporary workspace.
  </Card>

  <Card title="Build Registration →">
    The system registers the new build in JFrog Artifactory, creating a fully self-contained, production-ready Docker image.
  </Card>
</Cards>

### How to create a build from the model versions tab:

1. Navigate to **AI Catalog**  > **Models**.

   <Image alt="figma_modelversions.png" border={false} src="https://files.readme.io/83365bda9fe77c0043f33c9e865dbe2d963f291e4123862bde023aa15a149ea7-uuid-28020f7c-e313-368c-111a-fac0da8eb475.png" />
2. Select the custom model for which you want to create a build, and select the **Versions** tab.
3. Create a build according to the following guidelines:

   * To create a build of an unbuilt version: Click the **Build version** button adjacent to the required version.
   * To create a new build of a built version: Click the **Rebuild version** button adjacent to the required version. The **Rebuild Version** option offers the functionality to recompile or update a built model to incorporate new changes or improvements. For example, if you want to add different tags, or use different resources.
