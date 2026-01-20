---
title: Upload Custom Models
deprecated: false
hidden: false
metadata:
  title: Upload Custom Models
  description: >-
    To use your own custom models, you just need to upload the models to JFrog
    ML. After uploading, custom models are automatically allowed for use.
  legacyUUIDs:
    - UUID-14d208d2-0549-5ded-41c7-9a5f05aab81b
    - UUID-231d62b3-20dc-eb84-7c57-5c8163630dbf
  robots: index
---
To use your own custom models, you just need to upload the models to JFrog ML. After uploading, custom models are automatically allowed for use.

**To upload a custom model**:

1. From the JFrog platform menu, select **AI/ML** > **Models**.

   <Image alt="uploadmodelbutton.png" border={false} src="https://files.readme.io/42e68b1d0722a75399c91e2a8136b5f6928e9fd6626eb53c664b4abc5dca4d1a-uuid-3e6f1070-9d8e-0a2c-ecf1-a22575b106d9.png" />

2. Click **Upload Model**.

   <Image align="center" alt="uploadmodel.png" border={false} width="80% " src="https://files.readme.io/70b10e7495204032da4d0dd4d1a8da2eb2af50608222df32768abf8d7887fa79-uuid-7d904203-e479-3cac-1ab5-db9604ecc1fa.png" />

3. If you have not Installed it yet, install the frogml CLI (use Python versions. 3.10 to 3.13)

```shell
pip install frogml-cli
```

4. Create the model either by entering the details here, or using the CLI.

<Table>
  <thead>
    <tr>
      <th>
        Enter details in _Upload Model_ pane:
      </th>

      <th>
        Using CLI:
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        Enter:

        * Model Name
        * Project

        Click **Create Model**.
      </td>

      <td>
        ```shell
        frogml models create "<MODEL_NAME>" --project "<PROJECT_NAME>"
        ```
      </td>
    </tr>
  </tbody>
</Table>

5. Select a template or an example as the base for your model.

| Start from Template                    | Start from Example                         |
| -------------------------------------- | ------------------------------------------ |
| frogml models init --example general . | frogml models init --example credit_risk . |

6. Build and deploy - trigger your first build in JFrog ML.

```shell
frogml models build ./<MODEL_FOLDER> --model-id "<MODEL_NAME>" --deploy
```
