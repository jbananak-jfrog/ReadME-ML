---
title: JFrog AI Catalog Architecture
deprecated: false
hidden: false
metadata:
  title: JFrog AI Catalog Architecture
  description: >-
    This diagram shows how JFrog's AI Catalog works together with the various
    components of the JFrog platform.
  legacyUUIDs:
    - UUID-09ac565e-a82a-65ce-6218-d18be441ad49
    - UUID-c8aaae0a-a2b6-fb4f-b789-b9acea06c841
  robots: index
---
## AI Catalog High Level Architecture

This diagram shows how JFrog's AI Catalog works together with the various components of the JFrog platform.

<Image alt="Screenshot_2025-08-27_at_17_44_07.png" border={false} src="https://files.readme.io/706b0ee87658aaef28f0b7a864287393f0fc4075767077bb709cadf83ab51202-uuid-473ad575-fe34-030c-84c5-912146a9361b.png" />

With a focus on governance and compliance, the AI Catalog ensures that you can access all necessary models while maintaining the security of your operations.

**How does the JFrog AI Catalog integrate with other JFrog products?**

* **Security:** JFrog's AI Catalog ensures robust security by leveraging Curation, Package Catalog, and JAS to safeguard all operations and manage compliance efficiently.
* **Artifactory:** JFrog Artifactory provides functionalities like Hugging Face and Docker repositories, enabling seamless management of AI models and containerized applications.
* **JFrog ML:** Integration with JFrog ML unlocks capabilities such as model serving, fine-tuning, and data ingestion, streamlining the deployment, refinement, and feeding of data into AI models respectively.

**How does the JFrog AI Catalog integrate with the AI ecosystem?**

* **AI Providers:** Integration with various AI providers through an AI gateway allows for a broader selection of AI services and tools, enhancing the flexibility and capabilities of the AI Catalog.
* **Model Providers:** Supporting diverse model sourcing options from open-source repositories and internally-developed models ensures adaptability and wide applicability for different use cases.

For detailed usage instructions, refer to the relevant sections in this documentation.
