---
title: Upload Custom Models
deprecated: false
hidden: false
metadata:
  title: Upload Custom Models
  description: To use your own custom models, you just need to upload the models to JFrog ML. After uploading, custom models are automatically allowed for use.
  robots: index
  legacyUUIDs:
    - UUID-14d208d2-0549-5ded-41c7-9a5f05aab81b
    - UUID-231d62b3-20dc-eb84-7c57-5c8163630dbf
---
To use your own custom models, you just need to upload the models to JFrog ML. After uploading, custom models are automatically allowed for use.

**To upload a custom model**:

1. From the JFrog platform menu, select **AI/ML** > **Models**.

   ![uploadmodelbutton.png](https://files.readme.io/42e68b1d0722a75399c91e2a8136b5f6928e9fd6626eb53c664b4abc5dca4d1a-uuid-3e6f1070-9d8e-0a2c-ecf1-a22575b106d9.png)
2. Click **Upload Model**.

   ![uploadmodel.png](https://files.readme.io/70b10e7495204032da4d0dd4d1a8da2eb2af50608222df32768abf8d7887fa79-uuid-7d904203-e479-3cac-1ab5-db9604ecc1fa.png)
3. If you have not Installed it yet, install the frogml CLI (use Python versions. 3.9 to 3.11).

   ```
   pip install frogml-cli
   ```
4. Create the model either by entering the details here, or using the CLI.

   

<Table>
  <thead>
    <tr>
      <th>
        Enter details in *Upload Model* pane:
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
        
        Click **Create Model**./
      </td>
      <td>
        ```
        frogml models create "<MODEL_NAME>" --project "<PROJECT_NAME>"
        ```
      </td>
    </tr>
  </tbody>
</Table>


5. Select a template or an example as the base for your model.

   

<Table>
  <thead>
    <tr>
      <th>
        Start from Template
      </th>
      <th>
        Start from Example
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>
        frogml models init --example general .
      </td>
      <td>
        frogml models init --example credit\_risk .
      </td>
    </tr>
  </tbody>
</Table>


6. Build and deploy - trigger your first build in JFrog ML.

   ```
   frogml models build ./<MODEL_FOLDER> --model-id "<MODEL_NAME>" --deploy
   ```
