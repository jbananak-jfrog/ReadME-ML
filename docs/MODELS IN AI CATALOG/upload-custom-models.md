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

1. From the JFrog platform menu, select **AI/ML** > **Registry**.

2. Click the **+** adjacent to the project on the left for which you want to upload and allow the custom model.

   <Image align="left" border={true} src="https://files.readme.io/59b21bb5a7cadec577a9018b016dbc97fff99275d772faca893b031bb66b0739-uploadcustommodels.png" className="border" />

3. Click **Upload custom model**.

   <Image align="left" border={true} src="https://files.readme.io/e524fcb438af404cf71ba74a0c179860ed1177a1fae373d09b3e268b2a335721-upload_model_needs_replacing.png" className="border" />

4. If you have not Installed it yet, install the `frogml-cli` (use Python versions **3.10 to 3.13**).
   ```shell Shell
   pip install frogml-cli
   ```
   <br />

5. Create the model either by entering the details here, or using the CLI.

   <Table align={["left","left"]}>
     <thead>
       <tr>
         <th style={{ textAlign: "left" }}>
           Enter details in  
           _Upload Model_ pane:
         </th>

         <th style={{ textAlign: "left" }}>
           Using CLI:
         </th>
       </tr>
     </thead>

     <tbody>
       <tr>
         <td style={{ textAlign: "left" }}>
           a. Enter:

           1. Model Name
           2. Project

           b. Click **Create Model**.
         </td>

         <td style={{ textAlign: "left" }}>
           ```shell
           frogml models create "<MODEL_NAME>" --project "<PROJECT_NAME>"
           ```
         </td>
       </tr>
     </tbody>
   </Table>

   <br />

6. Select a template or an example as the base for your model.

   | Start from Template                      | Start from Example                           |
   | :--------------------------------------- | :------------------------------------------- |
   | `frogml models init --example general .` | `frogml models init --example credit_risk .` |

   <br />

7. Build and deploy - trigger your first build in JFrog ML.
   ```shell
   frogml models build ./<MODEL_FOLDER> --model-id "<MODEL_NAME>" --deploy
   ```
   <br />

<br />
