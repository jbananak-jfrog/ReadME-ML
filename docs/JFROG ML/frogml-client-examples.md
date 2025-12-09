---
title: FrogML Client Examples
deprecated: false
hidden: false
metadata:
  title: FrogML Client Examples
  description: Use FrogML to programmatically access many of the JFrog ML functions and operations.
  robots: index
  legacyUUIDs:
    - UUID-b95666d0-3525-57f7-3303-9c917c7a52c6
    - UUID-4f76733a-210a-f0a6-b031-ef6d33908663
---
Use `FrogML` to programmatically access many of the JFrog ML functions and operations.

The FrogML client is a wrapper and a single entry point for different JFrog ML clients such as the `BatchClient`, `DeploymentClient`, and more.

##### JFrog ML Configuration File

If the frogml-cli is already configured, the method fetches the token from the pre-configured conf file.

```
from frogml import FrogMLClient

client = FrogMLClient()
token = client.get_token()
```

### Builds

#### get\_latest\_build

Returns the latest build by its model ID.

Optionally gets a build\_status, by default filters on 'SUCCESSFUL'.

##### Args:

* **model\_id** (*str*) - The model ID.
* **build\_status** (*str*) - Build statuses to filter on. Valid values are 'SUCCESSFUL', 'IN\_PROGRESS', 'FAILED'.

##### Returns:

* `str` - The build ID of the latest build according to the build status. None if no builds match the filter.

```
from frogml import FrogMLClient

client = FrogMLClient()
client.get_latest_build(model_id="model_id")
```

#### get\_builds\_by\_tags

Returns a list of builds by a given model ID, filtered by a list of build tags.

Note that the method has several options of filtering builds by tags.

##### Args:

* **model\_id** (*str*) - The model ID.
* **tags** (*List[str]*) - List of tags to filter by.
* **match\_any** (*boolean, optional, default=True*) - Setting this to True will return builds with at least one tag from the given filter list of tags. Setting it to False will only return builds where all the tags in the provided list are present.
* **include\_extra\_tags** (*boolean, optional, default=True*) - Setting it to True will return builds that match exactly or more tags from the provided list of tags. Setting it to False will return exactly the list of tags included.

##### Returns:

* `List[Build]` - Returns a list of builds that contain the requested tags.

```
from frogml import FrogMLClient

client = FrogMLClient()

# 1. Return builds with either "tag_1" or "tag_2" or both "tag_1" and "tag_2".
#    The builds may have additional tags not listed in the filter.
client.get_builds_by_tags(model_id="model_id", tags=["tag_1", "tag_2"])

# 2. Return builds with both ["tag1", "tag2"] and optional additional tags.
client.get_builds_by_tags(model_id="model_id", tags=['tag1', 'tag2'], include_extra_tags=True, match_any=False)

# 3. Return builds strictly with "tag1" and "tag2" and no additional tags.
client.get_builds_by_tags(model_id="model_id", tags=['tag1', 'tag2'], include_extra_tags=False, match_any=False)

# 4. Return builds with either "tag1" or "tag2" or both, without extra tags.
client.get_builds_by_tags(model_id="model_id", tags=['tag1', 'tag2'], include_extra_tags=False, match_any=True)
```

#### list\_builds

List builds by its model ID and explicit filters.

##### Args

* **model\_id** (*str*) - The model ID
* **tags** (*List[str]*) - List of tags to filter by
* **filters** (*List[str]*) - List of metric and parameter filters

##### Returns

* `List[Build]` - List of builds that contains the requested filters.

```
from frogml import FrogMLClient

client = FrogMLClient()
builds_list = client.list_builds(model_id='your-model-id')
```

#### list\_file\_tags

List file tags by its model ID.

##### Args:

* **model\_id** (*str*) - The model ID
* **build\_id** (*str, optional, default=""*) - The build ID - If not specified, returns all model file tags
* **filter** (*FileTagFilter, optional, default=None*) - Filter the returning list

  * If not specified, returns all model file tags
  * When provided:

    * **value** (*str*) - Filter value
    * **type** (*enum*) - Filter type

      * `FILE_TAG_FILTER_TYPE_CONTAINS`
      * `FILE_TAG_FILTER_TYPE_PREFIX`

##### Returns:

* `List[FileTag]` - List of file tags with their specifications.

```
from frogml import FrogMLClient
from frogml.core.clients.file_versioning.file_tag_filter import FileTagFilter

client = FrogMLClient()

output = client.list_file_tags(
        model_id="model",
        build_id="build_id",
        filter=FileTagFilter(
                value="value",
                type=FileTagFilter.type.FILE_TAG_FILTER_TYPE_PREFIX
        )
)
```

#### list\_data\_tags

List data tags by its model ID.

##### Args:

* **model\_id** (*str*) - The model ID
* **build\_id** (*str, optional, default=""*) - The build ID - If not specified, returns all model data tags
* **filter** (*DataTagFilter, optional, default=None*) - Filter the returning list

  * If not specified, returns all model file tags
  * When provided:

    * **value** (*str*) - Filter value
    * **type** (*enum*) - Filter type

      * `DATA_TAG_FILTER_TYPE_CONTAINS`
      * `DATA_TAG_FILTER_TYPE_PREFIX`

##### Returns:

* `List[DataTag]` - List of data tags with their specifications.

```
from frogml import FrogMLClient
from frogml.core.clients.data_versioning.data_tag_filter import DataTagFilter

client = FrogMLClient()

output = client.list_data_tags(
        model_id="your-model-id",
        build_id="build_id",
        filter=DataTagFilter(
                value="value",
                type=DataTagFilter.type.DATA_TAG_FILTER_TYPE_PREFIX
        )
)
```

### Tags

#### set\_tag

Assign a tag to an existing build.

##### Args:

* **build\_id** (*str*) - The build ID
* **tag** (*str*) - The tag to assign

##### Returns:

* `List[Build]` - List of builds that contains the requested filters.

```
from frogml import FrogMLClient

client = FrogMLClient()

client.set_tag(build_id="build_id", tag="tag")
```

#### set\_tags

Assign a list of tags to an existing build.

##### Args:

* **build\_id** (*str*) - The build ID
* **tags** (*List[str]*) - List of tags to assign

##### Returns:

* `List[Build]` - List of builds that contains the requested filters.

```
from frogml import FrogMLClient

client = FrogMLClient()

client.set_tags(build_id="build_id", tags=["tag_1", "tag_2"])
```

### Projects

#### create\_project

Create a new project.

##### Args:

* **project\_name** (*str*) - The requested name
* **project\_description** (*str*) - The requested description

##### Returns:

* `str` - The project ID of the newly created project.

#### get\_project

Get model by its project ID.

##### Args:

* **project\_id** (*str*) - The project ID

##### Returns:

* `Optional[Project]` - Project by ID.

#### list\_projects

List projects.

##### Returns:

* `List[Project]` - List of projects.

```
from frogml import FrogMLClient 

client = FrogMLClient() 

client.list_projects()
```

#### delete\_project

Delete project by its project ID.

##### Args:

* **project\_id** (*str*) - The project ID

### Models

#### create\_model

Create a new model.

##### Args:

* **project\_id** (*str*) - The project ID to associate the model
* **model\_name** (*str*) - The requested name
* **model\_description** (*str*) - The requested description

##### Returns:

* `str` - The model ID of the newly created project.

```
from frogml import FrogMlClient

client = FrogMlClient()

client.create_model(project_id="project_id", model_name="model_name", model_description="model_description")
```

#### get\_model

Get model by its model ID.

##### Args:

* **model\_id** (*str*) - The model ID

##### Returns:

* `Optional[Model]` - Model by ID.

```
from frogml import FrogMLClient

client = FrogMLClient()

client.get_model(model_id="model_id")
```

#### get\_model\_metadata

Get model metadata by its model ID.

##### Args:

* **model\_id** (*str*) - The model ID

##### Returns:

* `Optional[ModelMetadata]` - Model metadata by ID.

```
from frogml import FrogMLClient

client = FrogMLClient()

client.get_model_metadata(model_id="model_id")
```

#### delete\_model

Delete model by its project & model ID's.

##### Args:

* **project\_id** (*str*) - The project ID
* **model\_id** (*str*) - The model ID

```
from frogml.sdk.frogml_client.client import FrogMLClient

client = FrogMLClient()

client.delete_model(project_id="project_id", model_id="model_name")
```

#### list\_models

Retrieves all models that belong to the project with the given ID.

##### Args:

* **project\_id** (*str*) - the project ID

```
from frogml import FrogMLClient

client = FrogMLClient()

client.list_models(project_id="project_id")
```

#### list\_model\_metadata

Retrieves the metadata of all models that belong to the project with a given ID

##### Args:

* **project\_id** (*str*) - the project ID

```
from frogml import FrogMLClient

client = FrogMLClient()

client.list_model_metadata(project_id="project_id")
```

### Deployments

#### get\_deployed\_build\_id\_per\_environment

Get deployed build ID per environment by its model ID.

##### Args:

* **model\_id** (*str*) - The model ID

##### Returns:

* `Dict[str, str]` - Map environment to deployed build ID. None if the model is not deployed.

```
from frogml import FrogMLClient

client = FrogMLClient()

client.get_deployed_build_id_per_environment(model_id="your_model_id")
```

### Batch Executions

#### list\_executions

List batch executions by model ID

##### Args:

* **model\_id** (*str*) - The model ID
* **build\_id** (*str, optional, default=""*) - The build ID - If not specified, returns all batch executions

##### Returns:

* `List[Execution]` - List of executions with their specifications.

```
from frogml import FrogMLClient

client = FrogMLClient()

client.list_executions(model_id="your_model_id", build_id="build_id")
```

#### list\_execution\_tasks

List batch executions tasks by its job ID

##### Args:

* **execution\_id** (*str*) - The execution ID

##### Returns:

* `List[Task]` - List of execution tasks with their specifications.

```
from frogml import FrogMLClient

client = FrogMLClient()

client.list_execution_tasks(execution_id="execution_id")
```
