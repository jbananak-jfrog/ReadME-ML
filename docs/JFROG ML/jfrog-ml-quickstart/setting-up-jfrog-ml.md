---
title: Set Up JFrog ML
deprecated: false
hidden: false
metadata:
  title: Setting Up JFrog ML
  description: >-
    This page describes how to set up JFrog ML, including installing and
    configuring FrogML CLI and SDK.
  legacyUUIDs:
    - UUID-6e11a5e2-fe80-213a-886f-c0f20f329857
    - UUID-d3527769-ec6e-1fbc-dd2d-1c400b5eee17
  robots: index
---
This page describes how to set up JFrog ML, including installing and configuring FrogML CLI and SDK.

<Callout icon="📘" theme="info">
  **Note for Self-Managed**

  For self-managed, follow the instructions in [Activate AI ML](/docs/setting-up-jfrog-ml).
</Callout>

## Setting Up JFrog ML in SAAS

**To set up JFrog ML in SAAS:**

<Callout icon="❗️" theme="error">
  **Important**

  Only a Platform **Admin** can perform the initial setup.
</Callout>

1. Click on the **AI/ML** link in the left sidebar of your JFrog platform. This will open the JFrog ML setup form.

   <Image alt="JFrog ML Setup Form" border={false} src="https://files.readme.io/e4d6fb33474a29a2b9dcfa89a760b4d5a7de4c4e51940e12c0363ff9ff3308ff-uuid-7f7888c7-668d-78cb-a11f-cfb4eee8e2eb.png" />
2. During setup, you'll need to select:

   1. A JFrog project where JFrog ML will store generated artifacts, such as Docker images and files.
   2. A cloud provider (currently AWS & GCP; Azure support).
   3. A cloud region for deployment.
3. Click **Get Started**. JFrog ML is enabled in your JFrog account.

## Install FrogML CLI

After your Admin has setup the JFrog ML on your account (SAAS or self-managed), you are ready to start using the JFrog ML solution. Within JFrog ML, the FrogML SDK & CLI are your gateway to building and deploying models and features on JFrog ML, seamlessly integrating with your existing JFrog platform.

<Callout icon="❗️" theme="error">
  **Important**

  `frogml-sdk` supports Python versions 3.9 to 3.11. Make sure you install it using a compatible Python version.
</Callout>

**To install frogml-cli:**

1. Optional: Create a new virtual environment using your preferred Python framework, such as **virtualenv**, **poetry** or **conda**.
2. Install the FrogML CLI in your virtual environment with one of the following commands:

   Using uv (recommended method, as this enables the CLI to be accessed from any environment)

   ```
   uv tool install frogml-cli
   ```

   Using pip / conda:

   ```
   pip install frogml-cli
   ```

   Using poetry:

   ```
   poetry add frogml-cli
   ```

<Callout icon="📘" theme="info">
  **Note**

  Installing the FrogML CLI also installs the FrogML SDK.
</Callout>

## Configuring FrogML SDK and CLI

After installing the frogml-cli, you need to configure it. You have two main options, with several sub-options:

* Configure [Via CLI](/docs/setting-up-jfrog-ml#to-configure-via-cli)
* Configure [Via environment variable](/docs/setting-up-jfrog-ml#to-configure-via-environment-variable)

### To configure via CLI:

**Sub-option 1: Define access token:**

1. With this option, you must first [generate an access token](/administration/docs/access-tokens) in the Jfrog platform.

   <Image alt="configuring-qwak-sdk-jfrogml.png" border={false} src="https://files.readme.io/994c0d7f55ccbd32b229d01d1b5707fc6f8ab0ddd785e8d0d5afe33f5275170b-uuid-e2092192-8a39-bb2e-7f5e-2ce203e74599.png" />
2. Open the terminal and type in the following command, replacing the variables:

   ```
   frogml config add --url={server_url} --access-token={access_token_from_artifactory} --server-id={optional_server_id}
   ```

   For example:

   ```
   frogml config add --url=https://mydemo.jfrog.io --access-token=token123 --server-id=testing-server
   ```

**Sub-option 2: Only if you already have the JF_CLI installed and configured, use your username and password:**

For this option, you need to make sure you have enabled token generation API as follows:

1. In the Administration module, select **Security** > **General**.
2. Scroll down and click **Enable token generation via api**. under **Basic Authentication**.
3. Click **Save**.

Then, type in the following command, replacing the variables:

```
frogml config add --url={server_url} --username={user_name} --password{password} --server-id={optional_server_id}frogml config add --url={server_url}
```

When configured correctly, you'll receive the following message:

```
Logged in successfully to: <PLATFORM_BASE_URL>
```

<Callout icon="📘" theme="info">
  **Note**

  There is an additional option, called "interactive". If you have previously configured the frogml-cli, it enables you to load the configuration. Alternatively, it enables your to connect to a new server using the access token or username and password, but in an interactive 'guided' manner.

  <Image alt="interactive_config_via_cli.gif" border={false} src="https://files.readme.io/ade0f02f2c03299c158f5f76e4c1d5f58297e21528a09fa6bd951f03c6f8b4ec-uuid-a6c3024a-c6bc-e93d-d6e0-f6bb2b6ac03f.gif" />
</Callout>

### To configure via environment variable:

JFrog ML also allows you to configure your session authentication key via environment variables. Note that this is only relevant for the individual Terminal session or until the next computer restart.

To set the environment variable, use the following command:

```
export JF_URL=<PLATFORM_BASE_URL>
export JF_ACCESS_TOKEN=<ACCESS_TOKEN>
```

Here you need to define the `platform_base_URL` and the `access_token`.

The same behavior occurs when you're running Python code that calls the FrogML SDK; it will read the environment variable for authentication.

## Troubleshooting

### Unrecognized `frogml` Command

| Problem  | Local system fails to recognize the `frogml` command.          |
| :------- | :------------------------------------------------------------- |
| Solution | Ensure it is added to your system's PATH environment variable. |

### Read/Write Permission Issues

<Table align={["left","left"]}>
  <thead>
    <tr>
      <th>
        Problem
      </th>

      <th>
        Issues related to read/write permissions with the following error message:

        ```
        Could not read user configuration from <home_path>/.frogml/config.json. Please make sure one has been set using frogml config command.
        ```
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        Solution
      </td>

      <td>
        Manually create the .frogml directory in your home path and rerun the configure command:

        ```
        mkdir -p <home_path>/.frogml
        ```
      </td>
    </tr>
  </tbody>
</Table>

## Additional Troubleshooting

_For any other issues with installing and configuring your FrogML SDK/CLI please refer to_  <Anchor label="Installation Issues" title="Installation Issues" href="/docs/installation-issues">Installation Issues</Anchor>  _or_  <Anchor label="Performance Issues" title="Performance Issues" href="/docs/performance-issues">Performance Issues</Anchor>  _pages._