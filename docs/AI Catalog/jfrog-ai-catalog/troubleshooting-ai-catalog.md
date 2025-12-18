---
title: Troubleshooting AI Catalog
deprecated: false
hidden: false
metadata:
  title: Troubleshooting AI Catalog
  description: Problem
  robots: index
  legacyUUIDs:
    - UUID-5d11ac49-bd22-a739-bf90-5bfb3e677080
    - UUID-1c107c30-8da0-362d-dcad-02fc0ecb84af
---
### Deploy Model Validate Fails



<Table>
  <tbody>
    <tr>
      <td>
        Problem
      </td>
      <td>
        Local system fails to recognize the `frogml` command.
      </td>
    </tr>
    <tr>
      <td>
        Solution:
      </td>
      <td>
        Ensure it's added to your system's PATH environment variable
      </td>
    </tr>
  </tbody>
</Table>



<Table>
  <tbody>
    <tr>
      <td>
        Possible Cause 2:
      </td>
      <td>
        Deployed model not supported
      </td>
    </tr>
    <tr>
      <td>
        Verify:
      </td>
      <td>
        Model supported
      </td>
    </tr>
  </tbody>
</Table>



### Execute Inference Fails - 404



<Table>
  <tbody>
    <tr>
      <td>
        Possible Cause:
      </td>
      <td>
        Execute performed after blocking
      </td>
    </tr>
    <tr>
      <td>
        Verify:
      </td>
      <td>
        Model not blocked
      </td>
    </tr>
  </tbody>
</Table>



### Execute Inference Fails - 401



<Table>
  <tbody>
    <tr>
      <td>
        Possible Cause 1:
      </td>
      <td>
        Bad token
      </td>
    </tr>
    <tr>
      <td>
        Verify:
      </td>
      <td>
        Token value
      </td>
    </tr>
  </tbody>
</Table>



<Table>
  <tbody>
    <tr>
      <td>
        Possible Cause 2:
      </td>
      <td>
        Incorrect Model Name
      </td>
    </tr>
    <tr>
      <td>
        Verify:
      </td>
      <td>
        Model name/known model
      </td>
    </tr>
  </tbody>
</Table>



### Block Model Fails



<Table>
  <tbody>
    <tr>
      <td>
        Possible Cause:
      </td>
      <td>
        Model is currently deployed
      </td>
    </tr>
    <tr>
      <td>
        Verify:
      </td>
      <td>
        Model is undeployed for the project before blocking
      </td>
    </tr>
  </tbody>
</Table>



### Delete Connection Fails



<Table>
  <tbody>
    <tr>
      <td>
        Possible Cause:
      </td>
      <td>
        Model is allowed/connected
      </td>
    </tr>
    <tr>
      <td>
        Verify:
      </td>
      <td>
        Try blocking first and then deleting the connection
      </td>
    </tr>
  </tbody>
</Table>



### Error Allowing Model Usage

Receive error:

![curationerrorcropped.png](https://files.readme.io/0f430a7574c850cf2efbe4f78a8727c163e4a0b7dd291e176fed4fd1468160aa-uuid-e57cc6f5-42c2-5b4c-7cad-30da24978702.png)

<Table>
  <tbody>
    <tr>
      <td>
        Possible Cause:
      </td>
      <td>
        Repository isn't yet enabled when allowing model usage for an open source model
      </td>
    </tr>
    <tr>
      <td>
        Verify:
      </td>
      <td>
        Curation is turned on, and package type and repository are enabled, according to the instructions in [Set Up Curation Settings for Model Packages](/docs/set-up-curation-settings-for-model-packages "Set Up Curation Settings for Model Packages")
      </td>
    </tr>
  </tbody>
</Table>
