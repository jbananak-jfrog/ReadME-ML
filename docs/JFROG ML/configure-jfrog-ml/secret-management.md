---
title: Secret Management
deprecated: false
hidden: false
metadata:
  title: Secret Management
  description: >-
    Securely managing sensitive data and credentials in complex data projects is
    crucial.
  legacyUUIDs:
    - UUID-c6a6ce65-bdf5-d04a-0aee-6e9c00997f94
    - UUID-a4668689-7538-26cf-966a-7f7a0ab28ead
  robots: index
---
## Manage Credentials by Creating Secrets

Securely managing sensitive data and credentials in complex data projects is crucial.

Use secrets to avoid revealing confidential information in model deployments and data source connections.

In traditional approaches, using credentials in an ML build involved including them in the Python code or using environment variables. Both options pose significant security risks.

Using the JFrog ML Secret Service, you can easily and securely store your credentials and pass them to your Python code with full confidentiality. For example, secrets are used for securely saving API keys. See also [How to Get API Keys for External Providers](/docs/connect-ai-providers#how-to-get-api-keys-for-external-providers).

<Callout icon="❗️" theme="error">
  **Important**

  _**Secret Naming Conventions**_

  * Secret names may be up to 36 characters and must start with a letter. They may contain letters, numbers, and hyphens (`-`), but **not** underscores (`_`). All letters must be in lowercase. Secret names must be a minimum of 3 characters.
  * Secret names may be up to 36 characters, may contain letters, numbers and dash ("-"), and must start with a letter.
  * Use a logical name that will enable you to remember the value of the secret later.
</Callout>

<Callout icon="⚠️" theme="warning">
  **Warning**

  After the secret is created, only the secret name is displayed, not the value.
</Callout>

## Creating Secrets via UI

**To create secrets via the UI:**

1. In the JFrog Platform, select the **Administration** module.
2. Scroll down the left menu and select **AI/ML Settings** > **Secrets**. The Secrets page displays a list of all your current secrets.
3. Click **Create new secret**.
4. Enter **Secret name** and **Secret value**.
5. Click **Save**. The "Successfully created new secret message" appears, and the secret now appears in the _Secrets_ page.
   ![](https://files.readme.io/a1fe5c0ddac3e880dc578b4f1547eee985148506d9d88864b932a30b7bc528f1-uuid-c362639b-120f-a177-ed14-24006a530530.png)

### The Secrets Page

In the _Secrets_ page, you can:

* Sort ascending/descending by Secret name, Environment, or time of creation.
* Create a new secret.
* Delete a secret.

## Creating Secrets via CLI

Secrets may be created directly using the JFrog ML CLI.

**To create secrets via the JFrog ML CLI:**

Use the following commands:

```
frogml secrets set --name <aws-api-key> --value <the_value_of_the_key>
frogml secrets set --name <aws-api-secret> --value <the_value_of_the_secret>
```

## Model Build Credentials

You may need the credentials during a build process. For example, retrieving a pre-trained model or data that was not stored in JFrog ML Feature Store. To retrieve the credentials, import the `SecretServiceClient` and use it to retrieve the secret:

```
from frogml import FrogMlModel
from frogml.core.clients.secret_service import  SecretServiceClient

class TestModel(FrogMlModel)
    # ...

    def build():
        secret_service = SecretServiceClient()
        aws_api_key = secret_service.get_secret('aws-api-key')
        aws_secret_key = secret_service.get_secret('aws-secret-key')
```

<Callout icon="⚠️" theme="warning">
  **Warning**

  Avoid printing or logging secret values as the model `stdout` is visible on the build logs.
</Callout>

## Feature Store Credentials

JFrog ML <Anchor label="Feature Store" title="Feature Store" href="/docs/feature-store">Feature Store</Anchor> integrates with the Secret Service to enable secure access to data sources. Use your secrets in the data source definition to ensure secure authorized access.

### Connecting to Snowflake

1. Create new secrets with the user name and password:

   ```
   frogml secrets set --name snowflake_user --value <snowflake_user>
   frogml secrets set --name snowflake_password --value <secured_password_1234>
   ```
2. Define a Snowflake data source using the secret names:

   ```
   from frogml.feature_store.data_sources import SnowflakeSource

   # The secret name stored in the Secret Service
   QWAK_SECRET_SNOWFLAKE_USER = '<qwak-secret-snowflake-user>'
   QWAK_SECRET_SNOWFLAKE_PASSWORD = '<qwak-secret-snowflake-password>'

   # Snowflake table details
   DATABASE='<snowflake_db_name>'
   SCHEMA='<snowflake_schema_name>'
   WAREHOUSE='snowflake_data_warehouse_name'
   HOST='<SnowflakeAddress/DNS:port>'

   snowflake_source = SnowflakeSource(
       name='my-snowflake-datasource',
       description='An example snowflake data source',
       date_created_column='DATE_COLUMN',
       username_secret_name=QWAK_SECRET_SNOWFLAKE_USER, 
       password_secret_name=QWAK_SECRET_SNOWFLAKE_PASSWORD,
       host=HOST,
       database=DATABASE,
       schema=SCHEMA,
       warehouse=WAREHOUSE
   )
   ```
