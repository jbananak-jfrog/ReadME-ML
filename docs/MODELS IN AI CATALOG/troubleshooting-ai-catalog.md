---
title: Troubleshooting
excerpt: Troubleshooting your AI Catalog
deprecated: false
hidden: false
metadata:
  title: Troubleshooting AI Catalog
  description: Problem
  legacyUUIDs:
    - UUID-5d11ac49-bd22-a739-bf90-5bfb3e677080
    - UUID-1c107c30-8da0-362d-dcad-02fc0ecb84af
  robots: index
---
### Deploy Model Validate Fails

| Problem   | Local system fails to recognize the _frogml_ command.        |
| :-------- | :----------------------------------------------------------- |
| Solution: | Ensure it's added to your system's PATH environment variable |

| Possible Cause 2: | Deployed model not supported |
| :---------------- | :--------------------------- |
| Verify:           | Model supported              |

### Execute Inference Fails - 404

| Possible Cause: | Execute performed after blocking |
| :-------------- | :------------------------------- |
| Verify:         | Model not blocked                |

### Execute Inference Fails - 401

| Possible Cause 1: | Bad token   |
| :---------------- | :---------- |
| Verify:           | Token value |

| Possible Cause 2: | Incorrect Model Name   |
| :---------------- | :--------------------- |
| Verify:           | Model name/known model |

### Block Model Fails

| Possible Cause: | Model is currently deployed                          |
| :-------------- | :--------------------------------------------------- |
| Verify:         | Model is undeployed for the project before blocking. |

### Delete Connection Fails

| Possible Cause: | Model is allowed/connected                          |
| :-------------- | :-------------------------------------------------- |
| Verify:         | Try blocking first and then deleting the connection |

### Error Allowing Model Usage

Receive error:

<Image alt="curationerrorcropped.png" border={false} src="https://files.readme.io/0f430a7574c850cf2efbe4f78a8727c163e4a0b7dd291e176fed4fd1468160aa-uuid-e57cc6f5-42c2-5b4c-7cad-30da24978702.png" />

| Possible Cause: | Repository isn't yet enabled when allowing model usage for an open source model                                                                                                                                                            |
| :-------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Verify:         | Curation is turned on, and package type and repository are enabled, according to the instructions in [Set Up Curation Settings for Model Packages](/docs/ai-catalog-quick-start#prerequisite-set-up-curation-settings-for-model-packages). |
