---
title: JFrog ML Quickstart
deprecated: false
hidden: false
metadata:
  title: Get Started with JFrog ML
  description: Deploy your first model on JFrog ML in minutes!
  legacyUUIDs:
    - UUID-6bfdcfc1-3a3e-05d6-a792-59fa6b2ec132
    - UUID-3c15078f-7592-e203-9f96-b217d8498dff
  robots: index
---
Deploy your first model on JFrog ML in minutes!

<Callout icon="📘" theme="info">
  **Installation**

  Follow the [Set Up JFrog ML](doc:setting-up-jfrog-ml) guide prior to completing this guide.
</Callout>

## Building Your First Model

It's time to build your first ML model on JFrog ML.🚀

A model build is a trained, serialized and tested model instance, bundled with necessary dependencies that can be later deployed to production.

JFrog ML builds models on a scalable infrastructure that allows you to customize resources, whether using a pre-trained model or running live training of any size.

## Creating a Model

To begin, you need to create a new model and a new project on JFrog ML. Projects enable you to group and organize your models in a single location.

Creating models and projects can be done either through the user interface or by using the FrogML CLI.

This example tutorial, shows how to create a model using the FrogML CLI.

1. Create the `Credit Risk` model and associate it with an existing project (for example, the `Credit Risk Modeling` project):

   ```text
   frogml models create "Credit Risk" --project-key={Artifactory-project-key}
   ```
2. Generate a local example model with all the required files:

   ```
   frogml models init --example credit_risk .
   ```
3. Remotely build the model on JFrog ML by pointing the `FrogML` client to your local model directory and specifying the remote model ID. JFrog ML generates model IDs automatically by converting the model name to lowercase and removing any spaces:

   ```
   frogml models build ./credit_risk --model-id credit_risk --name "credit_risk_build_v1"
   ```

   Which will show the following in the terminal:

   ```
   ✅ Fetching model code (0:00:00.22)
   ✅ Registering frogml build - 100% (0:00:05.77)

   Build ID 2cac1883-47eb-44dd-9806-bdd9887dcc16 triggered remotely

   ########### Follow build logs in the CLI
   frogml models builds logs -b 2cac1883-47eb-44dd-9806-bdd9887dcc16 --follow

   ########### Follow build logs in the platform
   https://mydemo.jfrog.io/ui/ml/models/credit_risk/build/2cac1883-47eb-44dd-9806-bdd9887dcc16
   ```

## Viewing Build Logs

To monitor the progress of your build on JFrog ML, view logs via the CLI or UI.

Logs provide real-time updates on the process, including any errors or warnings or debug information.

**CLI:** Run `frogml models builds logs -b <build-id> --follow` and replace `build-id` with your build id.

**App:** To view build logs via the UI, follow the links provided in the terminal when building a model.

```
frogml models build ./credit_risk --model-id “credit_risk” --name "credit_risk_build_v1"
✅ Fetching model code (0:00:00.30)
✅ Registering frogml build - 100% (0:00:06.01)

Build ID ed761a55-72ff-4136-9484-f6a9d667e000 triggered remotely

########### Follow build logs in the CLI
frogml models builds logs -b ed761a55-72ff-4136-9484-f6a9d667e000 --follow

########### Follow build logs in the platform
https://mydemo.jfrog.io/ui/ml/models/credit_risk/build/ed761a55-72ff-4136-9484-f6a9d667e000
```

<Image alt="viewing-buildlogs-jfrogml.png" border={false} src="https://files.readme.io/427361d08f02bc1a14daabe284ff7a18a176c45f2c7993e551d23a7f92dc90b3-uuid-28802b14-750c-2e1d-0750-b808329427ce.png" />

_An example of the build logs page on FrogML_

> Using the `--deploy` flag will build and automatically deploy your model.
>
> ```
> frogml models build ./credit_risk --model-id credit_risk --name "credit_risk_build_v1" --deploy
> ```
>
> Which will show the following in the terminal:
>
> ```
> # ✅ Fetching Model Code (0:00:00.19)
> # ✅ Registering frogml Build - 100% (0:00:06.17)
> # ✅ Deploying - Waiting for build to finish (0:03:14.53)
> # 
> # Build ID a08faef3-dbb8-483d-8017-94b35f259c9c finished successfully and deployed
> # 
> ########### To view the model using Frogml platform
> # https://mydemo.jfrog.io/ui/ml/models/b731b293-405a-491e-a17d-8c63c3d03017/credit_risk
> ```

## Deploying Your Model

After a successful build, our model can be deployed as a real-time inference endpoint on JFrog ML, ready to handle predictions. Copy the build ID from the previous build step and replace it with `YOUR_BUILD_ID`

```
frogml models deploy realtime --model-id credit_risk --build-id {YOUR_BUILD_ID}
```

After running the deployment command, you can expect to see the following output:

```
╒═══════════════╤══════════════════════════════════════╕
│ Environment   │ jfrog_demo                            │
├───────────────┼──────────────────────────────────────┤
│ Model ID      │ credit_risk                          │
├───────────────┼──────────────────────────────────────┤
│ Build ID      │ f42af8a7-2942-459f-b768-a981b7098cb7 │
├───────────────┼──────────────────────────────────────┤
│ Deployment ID │ e2d9a66f-26be-4201-b1ea-f1ccf312d5d4 │
╘═══════════════╧══════════════════════════════════════╛

Deployment initiated successfully, Use --sync to wait for deployment to be ready.
```

## Testing Your Model

After a successful model deployment, you can test your live inference endpoint.

The Frogml Python SDK includes a real-time client module which you have to separately install:

```
pip install frogml-inference
```

You can use it to run inference and predictions using your deployed real-time model:

```
from frogml_inference.realtime_client import RealTimeClient

FROGML_MODEL_ID = 'credit_risk'

if __name__ == '__main__':
  feature_vector = [
    {
      "Purpose": "male",
      "Age_cat": "male",
      "Housing": "male",
      "Age": 3,
      "UserId": "male",
      "Job": 2,
      "Saving accounts": "male",
      "Checking account": "male",
      "Duration": 4,
      "Sex": "male",
      "Credit amount": 54.2
    }]
  
  client = RealTimeClient(model_id=FROGML_MODEL_ID)
  response = client.predict(feature_vector)
  print(response)
```

Once you begin making predictions using the deployed model, you will be able to view relevant metrics in the Health dashboard on the **Model Overview** tab.

<Image alt="Health Dashboard" border={false} src="https://files.readme.io/aaa54580ce30be2ba6be3b4f4ac244414be52c2781ad23fb5709fe2795045f3b-uuid-c775e2a0-8928-8d1f-f2e5-290fcb894819.png" />

## Querying Model Predictions

Querying model predictions is an essential step in the machine learning development process. With JFrog ML, it's easy to query your model's predictions and view relevant metrics.

1. Open your model page on the JFrog Application.
2. Select the **Analytics** tab.
3. Click **Run**, and you'll see a table containing a row for every prediction made against the model.
   ![](https://files.readme.io/a07558344f54383960de540117aecb833433ef430e53a38734a9fdd2e8b103aa-uuid-905ee08a-6227-73eb-c357-e15313332776.png)

_The Analytics tab on the model page_
