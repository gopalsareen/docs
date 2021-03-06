# Create, Get, Update, Delete

### Create Model

To create a model, you need to specify the model's name and other required fields \(which depend on the model\). Specifying the ID is optional.

Below, we create a classifier model with one initial concept. You can always add and remove concepts later.

{% tabs %}
{% tab title="gRPC Java" %}
```java
import com.clarifai.grpc.api.*;
import com.clarifai.grpc.api.status.*;

// Insert here the initialization code as outlined on this page:
// https://docs.clarifai.com/api-guide/api-overview/api-clients#client-installation-instructions

SingleModelResponse postModelsResponse = stub.postModels(
    PostModelsRequest.newBuilder().addModels(
        Model.newBuilder()
            .setId("petsID")
            .setOutputInfo(
                OutputInfo.newBuilder().setData(
                    Data.newBuilder().addConcepts(Concept.newBuilder().setId("boscoe"))
                )
            )
    ).build()
);

if (postModelsResponse.getStatus().getCode() != StatusCode.SUCCESS) {
    throw new RuntimeException("Post models failed, status: " + postModelsResponse.getStatus());
```
{% endtab %}

{% tab title="gRPC NodeJS" %}
```javascript
// Insert here the initialization code as outlined on this page:
// https://docs.clarifai.com/api-guide/api-overview/api-clients#client-installation-instructions

stub.PostModels(
    {
        models: [
            {
                id: "petsID",
                output_info: {
                    data: {concepts: [{id: "boscoe"}]},
                }
            }
        ]
    },
    metadata,
    (err, response) => {
        if (err) {
            throw new Error(err);
        }

        if (response.status.code !== 10000) {
            throw new Error("Post models failed, status: " + response.status.description);
        }
    }
);
```
{% endtab %}

{% tab title="gRPC Python" %}
```python
# Insert here the initialization code as outlined on this page:
# https://docs.clarifai.com/api-guide/api-overview/api-clients#client-installation-instructions

post_models_response = stub.PostModels(
    service_pb2.PostModelsRequest(
        models=[
            resources_pb2.Model(
                id="petsID",
                output_info=resources_pb2.OutputInfo(
                    data=resources_pb2.Data(
                        concepts=[resources_pb2.Concept(id="boscoe", value=1)]
                    ),
                )
            )
        ]
    ),
    metadata=metadata
)

if post_models_response.status.code != status_code_pb2.SUCCESS:
    raise Exception("Post models failed, status: " + post_models_response.status.description)
```
{% endtab %}

{% tab title="js" %}
```javascript
app.models.create(
  "petsID",
  [
    { "id": "boscoe" }
  ]
).then(
  function(response) {
    // do something with response
  },
  function(err) {
    // there was an error
  }
);
```
{% endtab %}

{% tab title="python" %}
```python
from clarifai.rest import ClarifaiApp
app = ClarifaiApp(api_key='YOUR_API_KEY')

model = app.models.create('petsID', concepts=['boscoe'])
```
{% endtab %}

{% tab title="java" %}
```java
client.createModel("petsID")
    .withOutputInfo(ConceptOutputInfo.forConcepts(
        Concept.forID("boscoe")
    ))
    .executeSync();
```
{% endtab %}

{% tab title="csharp" %}
```csharp
using System.Collections.Generic;
using System.Threading.Tasks;
using Clarifai.API;
using Clarifai.DTOs.Predictions;

namespace YourNamespace
{
    public class YourClassName
    {
        public static async Task Main()
        {
            var client = new ClarifaiClient("YOUR_API_KEY");

            await client.CreateModel(
                    "petsID",
                    concepts: new List<Concept> {new Concept("boscoe")})
                .ExecuteAsync();
        }
    }
}
```
{% endtab %}

{% tab title="objective-c" %}
```text
[_app createModel:@[@"cat", @"dog"] name:@"petsModel" modelID:@"petsID" conceptsMutuallyExclusive:NO closedEnvironment:NO completion:^(ClarifaiModel *model, NSError *error) {
    NSLog(@"model: %@", model);
}];
```
{% endtab %}

{% tab title="php" %}
```php
use Clarifai\API\ClarifaiClient;
use Clarifai\DTOs\Predictions\Concept;

$client = new ClarifaiClient('YOUR_API_KEY');

$response = $client->createModel('MODEL_ID')
    ->withConcepts([new Concept('CONCEPT1')])
    ->executeSync();

if ($response->isSuccessful()) {
    echo "Response is successful.\n";
} else {
    echo "Response is not successful. Reason: \n";
    echo $response->status()->description() . "\n";
    echo $response->status()->errorDetails() . "\n";
    echo "Status code: " . $response->status()->statusCode();
}
```
{% endtab %}

{% tab title="cURL" %}
```text
curl -X POST \
  -H "Authorization: Key YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '
  {
    "model": {
      "id": "petsID",
      "output_info": {
        "data": {
          "concepts": [
            {
              "id": "boscoe",
              "value": 1
            }
          ]
        }
      }
    }
  }'\
  https://api.clarifai.com/v2/models
```
{% endtab %}
{% endtabs %}

### Add Concepts To A Model

You can add concepts to a model at any point. As you add concepts to inputs, you may want to add them to your model.

{% tabs %}
{% tab title="gRPC Java" %}
```java
import com.clarifai.grpc.api.*;
import com.clarifai.grpc.api.status.*;

...

MultiModelResponse patchModelsResponse = stub.patchModels(
    PatchModelsRequest.newBuilder()
        .setAction("merge")  // Supported actions: overwrite, merge, remove
        .addModels(
            Model.newBuilder()
                .setId("petsID")
                .setOutputInfo(
                    OutputInfo.newBuilder().setData(
                        Data.newBuilder().addConcepts(Concept.newBuilder().setId("charlie"))
                    )
                )
        )
        .build()
);

if (patchModelsResponse.getStatus().getCode() != StatusCode.SUCCESS) {
    throw new RuntimeException("Patch models failed, status: " + patchModelsResponse.getStatus());
}
```
{% endtab %}

{% tab title="gRPC NodeJS" %}
```javascript
// Insert here the initialization code as outlined on this page:
// https://docs.clarifai.com/api-guide/api-overview/api-clients#client-installation-instructions

stub.PatchModels(
    {
        action: "merge",  // Supported actions: overwrite, merge, remove
        models: [
            {
                id: "petsID",
                output_info: {data: {concepts: [{id: "charlie"}]}}
            }
        ]
    },
    metadata,
    (err, response) => {
        if (err) {
            throw new Error(err);
        }

        if (response.status.code !== 10000) {
            throw new Error("Patch models failed, status: " + response.status.description);
        }
    }
);
```
{% endtab %}

{% tab title="gRPC Python" %}
```python
# Insert here the initialization code as outlined on this page:
# https://docs.clarifai.com/api-guide/api-overview/api-clients#client-installation-instructions

patch_models_response = stub.PatchModels(
    service_pb2.PatchModelsRequest(
        action="merge",  # Supported actions: overwrite, merge, remove
        models=[
            resources_pb2.Model(
                id="petsID",
                output_info=resources_pb2.OutputInfo(
                    data=resources_pb2.Data(
                        concepts=[resources_pb2.Concept(id="charlie")]
                    ),
                )
            )
        ]
    ),
    metadata=metadata
)

if patch_models_response.status.code != status_code_pb2.SUCCESS:
    raise Exception("Patch models failed, status: " + patch_models_response.status.description)
```
{% endtab %}

{% tab title="js" %}
```javascript
app.models.initModel({model_id}).then(function(model) {
  updateModel,
  function(err) {
    // there was an error
  }
});

function updateModel(model) {
  model.mergeConcepts({"id": "charlie"}).then(
    function(response) {
      // do something with response
    },
    function(err) {
      // there was an error
    }
  );
}
```
{% endtab %}

{% tab title="python" %}
```python
from clarifai.rest import ClarifaiApp
app = ClarifaiApp(api_key='YOUR_API_KEY')

model = app.models.get('{model_id}')
model.add_concepts(['charlie'])
```
{% endtab %}

{% tab title="java" %}
```java
client.modifyModel("{{model_id}}")
    .withConcepts(Action.MERGE, Concept.forID("dogs"))
    .executeSync();

// Or, if you have a ConceptModel object, you can do it in an OO fashion
final ConceptModel model = client.getModelByID("{model_id}").executeSync().get().asConceptModel();
model.modify()
    .withConcepts(Action.MERGE, Concept.forID("dogs"))
    .executeSync();
```
{% endtab %}

{% tab title="csharp" %}
```csharp
using System.Collections.Generic;
using System.Threading.Tasks;
using Clarifai.API;
using Clarifai.API.Requests.Models;
using Clarifai.DTOs.Models;
using Clarifai.DTOs.Predictions;

namespace YourNamespace
{
    public class YourClassName
    {
        public static async Task Main()
        {
            var client = new ClarifaiClient("YOUR_API_KEY");

            await client.ModifyModel(
                    "petsID",
                    ModifyAction.Merge,
                    concepts: new List<Concept> {new Concept("dogs")})
                .ExecuteAsync();

            // Or, if you have a ConceptModel object, you can do it in an OO fashion
            ConceptModel model = (ConceptModel) (
                await client.GetModel<Concept>("petsID")
                    .ExecuteAsync()).Get();
            await model.Modify(
                    ModifyAction.Merge,
                    concepts: new List<Concept> {new Concept("dogs")})
                .ExecuteAsync();
        }
    }
}
```
{% endtab %}

{% tab title="objective-c" %}
```text
ClarifaiConcept *concept = [[ClarifaiConcept alloc] initWithConceptName:@"dress"];
[app addConcepts:@[concept] toModelWithID:@"{model_id}" completion:^(ClarifaiModel *model, NSError *error) {
    NSLog(@"model: %@", model);
}];
```
{% endtab %}

{% tab title="php" %}
```php
use Clarifai\API\ClarifaiClient;
use Clarifai\DTOs\Inputs\ModifyAction;
use Clarifai\DTOs\Predictions\Concept;

$client = new ClarifaiClient('YOUR_API_KEY');

$response = $client->modifyModel('MODEL_ID')
    ->withModifyAction(ModifyAction::merge())
    ->withConcepts([new Concept('CONCEPT')])
    ->executeSync();

if ($response->isSuccessful()) {
    echo "Response is successful.\n";
} else {
    echo "Response is not successful. Reason: \n";
    echo $response->status()->description() . "\n";
    echo $response->status()->errorDetails() . "\n";
    echo "Status code: " . $response->status()->statusCode();
}
```
{% endtab %}

{% tab title="cURL" %}
```text
curl -X PATCH \
  -H "Authorization: Key YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '
  {
    "models": [
      {
        "id": "petsID",
        "output_info": {
          "data": {
            "concepts": [
              {
                "id": "charlie"
              }
            ]
          }
        }
      }
    ],
    "action": "merge"
  }'\
  https://api.clarifai.com/v2/models/
```
{% endtab %}
{% endtabs %}

### Remove Concepts From A Model

Conversely, if you'd like to remove concepts from a model, you can also do that.

{% tabs %}
{% tab title="gRPC Java" %}
```java
import com.clarifai.grpc.api.*;
import com.clarifai.grpc.api.status.*;

// Insert here the initialization code as outlined on this page:
// https://docs.clarifai.com/api-guide/api-overview/api-clients#client-installation-instructions

MultiModelResponse patchModelsResponse = stub.patchModels(
    PatchModelsRequest.newBuilder()
        .setAction("remove")  // Supported actions: overwrite, merge, remove
        .addModels(
            Model.newBuilder()
                .setId("petsID")
                .setOutputInfo(
                    OutputInfo.newBuilder().setData(
                        Data.newBuilder().addConcepts(Concept.newBuilder().setId("charlie"))
                    )
                )
        )
        .build()
);

if (patchModelsResponse.getStatus().getCode() != StatusCode.SUCCESS) {
    throw new RuntimeException("Patch models failed, status: " + patchModelsResponse.getStatus());
}
```
{% endtab %}

{% tab title="gRPC NodeJS" %}
```javascript
// Insert here the initialization code as outlined on this page:
// https://docs.clarifai.com/api-guide/api-overview/api-clients#client-installation-instructions

stub.PatchModels(
    {
        action: "remove",  // Supported actions: overwrite, merge, remove
        models: [
            {
                id: "petsID",
                output_info: {data: {concepts: [{id: "charlie"}]}}
            }
        ]
    },
    metadata,
    (err, response) => {
        if (err) {
            throw new Error(err);
        }

        if (response.status.code !== 10000) {
            throw new Error("Patch models failed, status: " + response.status.description);
        }
    }
);
```
{% endtab %}

{% tab title="gRPC Python" %}
```python
# Insert here the initialization code as outlined on this page:
# https://docs.clarifai.com/api-guide/api-overview/api-clients#client-installation-instructions

patch_models_response = stub.PatchModels(
    service_pb2.PatchModelsRequest(
        action="remove",  # Supported actions: overwrite, merge, remove
        models=[
            resources_pb2.Model(
                id="petsID",
                output_info=resources_pb2.OutputInfo(
                    data=resources_pb2.Data(
                        concepts=[resources_pb2.Concept(id="charlie")]
                    ),
                )
            )
        ]
    ),
    metadata=metadata
)

if patch_models_response.status.code != status_code_pb2.SUCCESS:
    raise Exception("Patch models failed, status: " + patch_models_response.status.description)
```
{% endtab %}

{% tab title="js" %}
```javascript
app.models.initModel({model_id}).then(function(model) {
  updateModel,
  function(err) {
    // there was an error
  }
});

function updateModel(model) {
  model.deleteConcepts({"id": "charlie"}).then(
    function(response) {
      // do something with response
    },
    function(err) {
      // there was an error
    }
  );
}
```
{% endtab %}

{% tab title="python" %}
```python
from clarifai.rest import ClarifaiApp
app = ClarifaiApp(api_key='YOUR_API_KEY')

model = app.models.get('{model_id}')
model.delete_concepts(['charlie'])
```
{% endtab %}

{% tab title="java" %}
```java
client.modifyModel("{{model_id}}")
    .withConcepts(Action.REMOVE, Concept.forID("dogs"))
    .executeSync();

// Or, if you have a ConceptModel object, you can do it in an OO fashion
final ConceptModel model = client.getModelByID("{{model_id}}").executeSync().get().asConceptModel();
model.modify()
    .withConcepts(Action.REMOVE, Concept.forID("dogs"))
    .executeSync();
```
{% endtab %}

{% tab title="csharp" %}
```csharp
using System.Collections.Generic;
using System.Threading.Tasks;
using Clarifai.API;
using Clarifai.API.Requests.Models;
using Clarifai.DTOs.Models;
using Clarifai.DTOs.Predictions;

namespace YourNamespace
{
    public class YourClassName
    {
        public static async Task Main()
        {
            var client = new ClarifaiClient("YOUR_API_KEY");

            await client.ModifyModel(
                    "petsID",
                    ModifyAction.Remove,
                    concepts: new List<Concept> {new Concept("dogs")})
                .ExecuteAsync();

// Or, if you have a ConceptModel object, you can do it in an OO fashion
            ConceptModel model = (ConceptModel) (
                    await client.GetModel<Concept>("petsID")
                        .ExecuteAsync())
                .Get();
            await model.Modify(
                    ModifyAction.Remove,
                    concepts: new List<Concept> {new Concept("dogs")})
                .ExecuteAsync();
        }
    }
}
```
{% endtab %}

{% tab title="objective-c" %}
```text
ClarifaiConcept *concept = [[ClarifaiConcept alloc] initWithConceptName:@"dress"];
[app deleteConcepts:@[concept] fromModelWithID:@"{model_id}" completion:^(ClarifaiModel *model, NSError *error) {
    NSLog(@"model: %@", model);
}];
```
{% endtab %}

{% tab title="php" %}
```php
use Clarifai\API\ClarifaiClient;
use Clarifai\DTOs\Inputs\ModifyAction;
use Clarifai\DTOs\Predictions\Concept;

$client = new ClarifaiClient('YOUR_API_KEY');

$response = $client->modifyModel('MODEL_ID')
    ->withModifyAction(ModifyAction::remove())
    ->withConcepts([new Concept('CONCEPT')])
    ->executeSync();

if ($response->isSuccessful()) {
    echo "Response is successful.\n";
} else {
    echo "Response is not successful. Reason: \n";
    echo $response->status()->description() . "\n";
    echo $response->status()->errorDetails() . "\n";
    echo "Status code: " . $response->status()->statusCode();
}
```
{% endtab %}

{% tab title="cURL" %}
```text
curl -X PATCH \
  -H "Authorization: Key YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '
  {
    "models": [
      {
        "id": "petsID",
        "output_info": {
          "data": {
            "concepts": [
              {
                "id": "charlie"
              }
            ]
          }
        }
      }
    ],
    "action": "remove"
  }'\
  https://api.clarifai.com/v2/models/
```
{% endtab %}
{% endtabs %}

### Update Model Name and Configuration

Here we will change the model name to 'newname' and the model's configuration to have concepts\_mutually\_exclusive=true and closed\_environment=true.

{% tabs %}
{% tab title="gRPC Java" %}
```java
import com.clarifai.grpc.api.*;
import com.clarifai.grpc.api.status.*;

// Insert here the initialization code as outlined on this page:
// https://docs.clarifai.com/api-guide/api-overview/api-clients#client-installation-instructions

MultiModelResponse patchModelsResponse = stub.patchModels(
    PatchModelsRequest.newBuilder()
        .setAction("overwrite")
        .addModels(
            Model.newBuilder()
                .setId("petsID")
                .setName("newname")
                .setOutputInfo(
                    OutputInfo.newBuilder()
                        .setData(
                            Data.newBuilder()
                                .addConcepts(Concept.newBuilder().setId("birds"))
                                .addConcepts(Concept.newBuilder().setId("hurd"))
                        )
                        .setOutputConfig(
                            OutputConfig.newBuilder()
                                .setConceptsMutuallyExclusive(true)
                                .setClosedEnvironment(true)
                        )
                )
    ).build()
);

if (patchModelsResponse.getStatus().getCode() != StatusCode.SUCCESS) {
    throw new RuntimeException("Patch models failed, status: " + patchModelsResponse.getStatus());
}
```
{% endtab %}

{% tab title="gRPC NodeJS" %}
```javascript
// Insert here the initialization code as outlined on this page:
// https://docs.clarifai.com/api-guide/api-overview/api-clients#client-installation-instructions

stub.PatchModels(
    {
        action: "overwrite",
        models: [
            {
                id: "petsID",
                name: "newname",
                output_info: {
                    data: {concepts: [{id: "birds"}, {id: "hurd"}]},
                    output_config: {concepts_mutually_exclusive: true, closed_environment: true}
                }
            }
        ]
    },
    metadata,
    (err, response) => {
        if (err) {
            throw new Error(err);
        }

        if (response.status.code !== 10000) {
            throw new Error("Patch models failed, status: " + response.status.description);
        }
    }
);
```
{% endtab %}

{% tab title="gRPC Python" %}
```python
# Insert here the initialization code as outlined on this page:
# https://docs.clarifai.com/api-guide/api-overview/api-clients#client-installation-instructions

patch_models_response = stub.PatchModels(
    service_pb2.PatchModelsRequest(
        action="overwrite",
        models=[
            resources_pb2.Model(
                id="petsID",
                name="newname",
                output_info=resources_pb2.OutputInfo(
                    data=resources_pb2.Data(
                        concepts=[
                            resources_pb2.Concept(id="birds"),
                            resources_pb2.Concept(id="hurd")
                        ]
                    ),
                    output_config=resources_pb2.OutputConfig(
                        concepts_mutually_exclusive=True,
                        closed_environment=True,
                    )
                )
            )
        ]
    ),
    metadata=metadata
)

if patch_models_response.status.code != status_code_pb2.SUCCESS:
    raise Exception("Patch models failed, status: " + patch_models_response.status.description)
```
{% endtab %}

{% tab title="js" %}
```javascript
app.models.initModel({model_id}).then(
  updateModel,
  function(err) {
    // there was an error
  }
);

function updateModel(model) {
  model.update({
    name: 'newname',
    conceptsMutuallyExclusive: true,
    closedEnvironment: true,
    concepts: ['birds', 'hurd']
  }).then(
}
```
{% endtab %}

{% tab title="python" %}
```python
from clarifai.rest import ClarifaiApp
app = ClarifaiApp(api_key='YOUR_API_KEY')

model = app.models.get('{model_id}')

# only update the name
model.update(model_name="newname")

# update the model attributes
model.update(concepts_mutually_exclusive=True, closed_environment=True)

# update more together
model.update(model_name="newname",
             concepts_mutually_exclusive=True, closed_environment=True)

# update attributes together with concepts
model.update(model_name="newname",
             concepts_mutually_exclusive=True,
             concepts=["birds", "hurd"])
```
{% endtab %}

{% tab title="java" %}
```java
client.modifyModel("{{model_id}}")
    .withName("newname")
    .withConceptsMutuallyExclusive(true)
    .withClosedEnvironment(true)
    .executeSync();
```
{% endtab %}

{% tab title="csharp" %}
```csharp
using System.Threading.Tasks;
using Clarifai.API;

namespace YourNamespace
{
    public class YourClassName
    {
        public static async Task Main()
        {
            var client = new ClarifaiClient("YOUR_API_KEY");

            await client.ModifyModel(
                    "someModel",
                    name: "{newName}",
                    areConceptsMutuallyExclusive: true,
                    isEnvironmentClosed: false)
                .ExecuteAsync();
        }
    }
}
```
{% endtab %}

{% tab title="objective-c" %}
```text
[_app updateModel:@"{model_id}" name:@"newName" conceptsMutuallyExclusive:NO closedEnvironment:NO completion:^(ClarifaiModel *model, NSError *error) {
    NSLog(@"model: %@", model);
}];
```
{% endtab %}

{% tab title="php" %}
```php
use Clarifai\API\ClarifaiClient;

$client = new ClarifaiClient('YOUR_API_KEY');

$response = $client->modifyModel('MODEL_ID')
    ->withName('NEW_MODEL_NAME')
    ->withAreConceptsMutuallyExclusive(false)
    ->withIsEnvironmentClosed(false)
    ->executeSync();

if ($response->isSuccessful()) {
    echo "Response is successful.\n";
} else {
    echo "Response is not successful. Reason: \n";
    echo $response->status()->description() . "\n";
    echo $response->status()->errorDetails() . "\n";
    echo "Status code: " . $response->status()->statusCode();
}
```
{% endtab %}

{% tab title="cURL" %}
```text
curl -X PATCH \
  -H "Authorization: Key YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '
  {
    "models": [
      {
        "id": "petsID",
        "name": "newname",
        "output_info": {
          "data": {"concepts": [{"id": "birds"}, {"id": "hurd"}]},
          "output_config": {
            "concepts_mutually_exclusive": true,
            "closed_environment": true
          }
        }
      }
    ],
    "action": "overwrite"
  }'\
  https://api.clarifai.com/v2/models/
```
{% endtab %}
{% endtabs %}

## Get

### List Model Types

Learn about available model types and their hyperparameters. This endpoint lists all the possible models that are creatable \(when creatable=true\), or in general in the platform \(the others ones have creatable=false\).

{% tabs %}
{% tab title="gRPC Java" %}
```java
import com.clarifai.grpc.api.*;
import com.clarifai.grpc.api.status.*;

// Insert here the initialization code as outlined on this page:
// https://docs.clarifai.com/api-guide/api-overview/api-clients#client-installation-instructions

MultiModelTypeResponse listModelTypesResponse = stub.listModelTypes(ListModelTypesRequest.newBuilder().build());

for (ModelType modelType : listModelTypesResponse.getModelTypesList()) {
    System.out.println(modelType);
}
```
{% endtab %}

{% tab title="gRPC NodeJS" %}
```javascript
// Insert here the initialization code as outlined on this page:
// https://docs.clarifai.com/api-guide/api-overview/api-clients#client-installation-instructions

stub.ListModelTypes(
    {
        page: 1,
        per_page: 500
    },
    metadata,
    (err, response) => {
        if (err) {
            throw new Error(err);
        }

        if (response.status.code !== 10000) {
            throw new Error("Received status: " + response.status.description + "\n" + response.status.details);
        }

        for (const model_type of response.model_types) {
            console.log(model_type)
        }
    }
);
```
{% endtab %}

{% tab title="gRPC Python" %}
```python
# Insert here the initialization code as outlined on this page:
# https://docs.clarifai.com/api-guide/api-overview/api-clients#client-installation-instructions

response = stub.ListModelTypes(service_pb2.ListModelTypesRequest(), metadata=metadata)

for model_type in response.model_types:
  print(model_type)
```
{% endtab %}

{% tab title="cURL" %}
```text
curl -X GET 'https://api.clarifai.com/v2/models/types?per_page=20&page=1' \
    -H 'Authorization: Key YOUR_API_KEY'
```
{% endtab %}
{% endtabs %}

### Get Models

To get a list of all models including models you've created as well as [Clarifai models](https://github.com/Clarifai/docs/tree/1c1d25cdd43190c38a2edb313297c0d566b3a0e3/api-guide/model/api-guide/model/public-models.md):

{% tabs %}
{% tab title="gRPC Java" %}
```java
import com.clarifai.grpc.api.*;
import com.clarifai.grpc.api.status.*;
import java.util.List;

// Insert here the initialization code as outlined on this page:
// https://docs.clarifai.com/api-guide/api-overview/api-clients#client-installation-instructions

MultiModelResponse listModelsResponse = stub.listModels(
    ListModelsRequest.newBuilder().build()
);

if (listModelsResponse.getStatus().getCode() != StatusCode.SUCCESS) {
    throw new RuntimeException("List models failed, status: " + listModelsResponse.getStatus());
}

List<Model> models = listModelsResponse.getModelsList();
for (Model model : models) {
    System.out.println(model);
}
```
{% endtab %}

{% tab title="gRPC NodeJS" %}
```javascript
// Insert here the initialization code as outlined on this page:
// https://docs.clarifai.com/api-guide/api-overview/api-clients#client-installation-instructions

stub.ListModels(
    {},
    metadata,
    (err, response) => {
        if (err) {
            throw new Error(err);
        }

        if (response.status.code !== 10000) {
            throw new Error("List models failed, status: " + response.status.description);
        }

        for (const model of response.models) {
            console.log(JSON.stringify(model, null, 2));
        }
    }
);
```
{% endtab %}

{% tab title="gRPC Python" %}
```python
# Insert here the initialization code as outlined on this page:
# https://docs.clarifai.com/api-guide/api-overview/api-clients#client-installation-instructions

list_models_response = stub.ListModels(
    service_pb2.ListModelsRequest(),
    metadata=metadata
)

if list_models_response.status.code != status_code_pb2.SUCCESS:
    raise Exception("List models failed, status: " + list_models_response.status.description)

for model in list_models_response.models:
    print(model)
```
{% endtab %}

{% tab title="js" %}
```javascript
app.models.list().then(
  function(response) {
    // do something with response
  },
  function(err) {
    // there was an error
  }
);
```
{% endtab %}

{% tab title="python" %}
```python
from clarifai.rest import ClarifaiApp
app = ClarifaiApp(api_key='YOUR_API_KEY')

# this is a generator
app.models.get_all()
```
{% endtab %}

{% tab title="java" %}
```java
client.getModels().getPage(1).executeSync();
```
{% endtab %}

{% tab title="csharp" %}
```csharp
using System.Threading.Tasks;
using Clarifai.API;

namespace YourNamespace
{
    public class YourClassName
    {
        public static async Task Main()
        {
            var client = new ClarifaiClient("YOUR_API_KEY");

            await client.GetModels()
                .Page(1)
                .ExecuteAsync();
        }
    }
}
```
{% endtab %}

{% tab title="objective-c" %}
```text
[_app getModels:1 resultsPerPage:30 completion:^(NSArray<ClarifaiModel *> *models, NSError *error) {
    NSLog(@"models: %@", models);
}];
```
{% endtab %}

{% tab title="php" %}
```php
use Clarifai\API\ClarifaiClient;
use Clarifai\DTOs\Models\Model;

$client = new ClarifaiClient('YOUR_API_KEY');

$response = $client->getModels()
    ->executeSync();

if ($response->isSuccessful()) {
    $models = $response->get();

    foreach ($models as $model) {
        echo $model->modelID() . ' ' . $model->type() . "\n";
    }
} else {
    echo "Response is not successful. Reason: \n";
    echo $response->status()->description() . "\n";
    echo $response->status()->errorDetails() . "\n";
    echo "Status code: " . $response->status()->statusCode();
}
```
{% endtab %}

{% tab title="cURL" %}
```text
curl -X GET \
  -H "Authorization: Key YOUR_API_KEY" \
  https://api.clarifai.com/v2/models
```
{% endtab %}
{% endtabs %}

### Get Model By Id

All models have unique Ids. You can get a specific model by its id:

{% tabs %}
{% tab title="gRPC Java" %}
```java
import com.clarifai.grpc.api.*;
import com.clarifai.grpc.api.status.*;

// Insert here the initialization code as outlined on this page:
// https://docs.clarifai.com/api-guide/api-overview/api-clients#client-installation-instructions

SingleModelResponse getModelResponse = stub.getModel(
    GetModelRequest.newBuilder()
        .setModelId("petsID")
        .build()
);

if (getModelResponse.getStatus().getCode() != StatusCode.SUCCESS) {
    throw new RuntimeException("Get model failed, status: " + getModelResponse.getStatus());
}

Model model = getModelResponse.getModel();
System.out.println(model);
```
{% endtab %}

{% tab title="gRPC NodeJS" %}
```javascript
// Insert here the initialization code as outlined on this page:
// https://docs.clarifai.com/api-guide/api-overview/api-clients#client-installation-instructions

stub.GetModel(
    {model_id: "petsID"},
    metadata,
    (err, response) => {
        if (err) {
            throw new Error(err);
        }

        if (response.status.code !== 10000) {
            throw new Error("List models failed, status: " + response.status.description);
        }

        const model = response.model;
        console.log(JSON.stringify(model, null, 2));
    }
);
```
{% endtab %}

{% tab title="gRPC Python" %}
```python
# Insert here the initialization code as outlined on this page:
# https://docs.clarifai.com/api-guide/api-overview/api-clients#client-installation-instructions

get_model_response = stub.GetModel(
    service_pb2.GetModelRequest(model_id="petsID"),
    metadata=metadata
)

if get_model_response.status.code != status_code_pb2.SUCCESS:
    raise Exception("Get model failed, status: " + get_model_response.status.description)

model = get_model_response.model
print(model)
```
{% endtab %}

{% tab title="js" %}
```javascript
app.models.get({model_id}).then(
  function(response) {
    // do something with response
  },
  function(err) {
    // there was an error
  }
);
```
{% endtab %}

{% tab title="python" %}
```python
from clarifai.rest import ClarifaiApp
app = ClarifaiApp(api_key='YOUR_API_KEY')

# get model by id
model = app.models.get(model_id')

# get model by name
model = app.models.get('my_model1')
```
{% endtab %}

{% tab title="java" %}
```java
client.getModelByID("{model_id}").executeSync();
```
{% endtab %}

{% tab title="csharp" %}
```csharp
using System.Threading.Tasks;
using Clarifai.API;
using Clarifai.DTOs.Predictions;

namespace YourNamespace
{
    public class YourClassName
    {
        public static async Task Main()
        {
            var client = new ClarifaiClient("YOUR_API_KEY");

            // Change Concept to whatever the model's type is.
            await client.GetModel<Concept>("{model_id}")
                .ExecuteAsync();
        }
    }
}
```
{% endtab %}

{% tab title="objective-c" %}
```text
[_app getModel:@"model_id" completion:^(ClarifaiModel *model, NSError *error) {
    NSLog(@"model: %@", model);
}];
```
{% endtab %}

{% tab title="php" %}
```php
use Clarifai\API\ClarifaiClient;
use Clarifai\DTOs\Models\Model;
use Clarifai\DTOs\Models\ModelType;

$client = new ClarifaiClient('YOUR_API_KEY');

$response = $client->getModel(ModelType::concept(), 'MODEL_ID')
    ->executeSync();

if ($response->isSuccessful()) {
    echo "Response is successful.\n";

    $model = $response->get();

    echo $model->modelID() . ' ' . $model->type() . "\n";
} else {
    echo "Response is not successful. Reason: \n";
    echo $response->status()->description() . "\n";
    echo $response->status()->errorDetails() . "\n";
    echo "Status code: " . $response->status()->statusCode();
}
```
{% endtab %}

{% tab title="cURL" %}
```text
curl -X GET \
  -H "Authorization: Key YOUR_API_KEY" \
  https://api.clarifai.com/v2/models/petsID
```
{% endtab %}
{% endtabs %}

### Get Model Output Info By Id

The output info of a model lists what concepts it contains.

{% tabs %}
{% tab title="gRPC Java" %}
```java
import com.clarifai.grpc.api.*;
import com.clarifai.grpc.api.status.*;

// Insert here the initialization code as outlined on this page:
// https://docs.clarifai.com/api-guide/api-overview/api-clients#client-installation-instructions

SingleModelResponse getModelOutputInfoResponse = stub.getModelOutputInfo(
    GetModelRequest.newBuilder()
        .setModelId("petsID")
        .build()
);

if (getModelOutputInfoResponse.getStatus().getCode() != StatusCode.SUCCESS) {
    throw new RuntimeException("Get model output info failed, status: " + getModelOutputInfoResponse.getStatus());
}

Model model = getModelOutputInfoResponse.getModel();
System.out.println(model);
```
{% endtab %}

{% tab title="gRPC NodeJS" %}
```javascript
// Insert here the initialization code as outlined on this page:
// https://docs.clarifai.com/api-guide/api-overview/api-clients#client-installation-instructions

stub.GetModelOutputInfo(
    {model_id: "petsID"},
    metadata,
    (err, response) => {
        if (err) {
            throw new Error(err);
        }

        if (response.status.code !== 10000) {
            throw new Error("List models failed, status: " + response.status.description);
        }

        const model = response.model;
        console.log(JSON.stringify(model, null, 2));
    }
);
```
{% endtab %}

{% tab title="gRPC Python" %}
```python
# Insert here the initialization code as outlined on this page:
# https://docs.clarifai.com/api-guide/api-overview/api-clients#client-installation-instructions

get_model_response = stub.GetModelOutputInfo(
    service_pb2.GetModelRequest(model_id="petsID"),
    metadata=metadata
)

if get_model_response.status.code != status_code_pb2.SUCCESS:
    raise Exception("Get model failed, status: " + get_model_response.status.description)

model = get_model_response.model
print(model)
```
{% endtab %}

{% tab title="js" %}
```javascript
app.models.initModel({model_id}).then(
  getModelOutputInfo,
  handleError
);

function getModelOutputInfo(model) {
  model.getOutputInfo().then(
    function(response) {
      // do something with response
    },
    function(err) {
      // there was an error
    }
  );
}
```
{% endtab %}

{% tab title="python" %}
```python
from clarifai.rest import ClarifaiApp
app = ClarifaiApp(api_key='YOUR_API_KEY')

model = app.models.get('my_model1')
model.get_info(verbose=True)
```
{% endtab %}

{% tab title="java" %}
```java
client.getModelByID("{model_id}").executeSync();
```
{% endtab %}

{% tab title="csharp" %}
```csharp
using System.Threading.Tasks;
using Clarifai.API;
using Clarifai.DTOs.Models;
using Clarifai.DTOs.Predictions;

namespace YourNamespace
{
    public class YourClassName
    {
        public static async Task Main()
        {
            var client = new ClarifaiClient("YOUR_API_KEY");

            var r = await client.GetModel<Concept>("{model-id}")
                .ExecuteAsync();
            var model = (ConceptModel) r.Get();
            var outputInfo = model.OutputInfo;
        }
    }
}
```
{% endtab %}

{% tab title="objective-c" %}
```text
[_app getModelByID:@"{model_id}" completion:^(ClarifaiModel *model, NSError *error) {
    NSLog(@"model: %@", model);
}];
```
{% endtab %}

{% tab title="php" %}
```php
use Clarifai\API\ClarifaiClient;
use Clarifai\DTOs\Models\Model;
use Clarifai\DTOs\Models\ModelType;

$client = new ClarifaiClient('YOUR_API_KEY');

$response = $client->getModel(ModelType::concept(), 'MODEL_ID')
    ->executeSync();

if ($response->isSuccessful()) {
    echo "Response is successful.\n";

    /** @var Model $model */
    $model = $response->get();

    $modelOutputInfo = $model->outputInfo();

    echo $modelOutputInfo->typeExt() . "\n";
    echo $modelOutputInfo->message() . "\n";
} else {
    echo "Response is not successful. Reason: \n";
    echo $response->status()->description() . "\n";
    echo $response->status()->errorDetails() . "\n";
    echo "Status code: " . $response->status()->statusCode();
}
```
{% endtab %}

{% tab title="cURL" %}
```text
curl -X GET \
  -H "Authorization: Key YOUR_API_KEY" \
  https://api.clarifai.com/v2/models/petsID/output_info
```
{% endtab %}
{% endtabs %}

### List Model Versions

Every time you train a model, it creates a new version. You can list all the versions created.

{% tabs %}
{% tab title="gRPC Java" %}
```java
import com.clarifai.grpc.api.*;
import com.clarifai.grpc.api.status.*;
import java.util.List;

// Insert here the initialization code as outlined on this page:
// https://docs.clarifai.com/api-guide/api-overview/api-clients#client-installation-instructions

MultiModelVersionResponse listModelVersionsResponse = stub.listModelVersions(
    ListModelVersionsRequest.newBuilder()
        .setModelId("petsID")
        .build()
);

if (listModelVersionsResponse.getStatus().getCode() != StatusCode.SUCCESS) {
    throw new RuntimeException("List model versions failed, status: " + listModelVersionsResponse.getStatus());
}

List<ModelVersion> modelVersions = listModelVersionsResponse.getModelVersionsList();
for (ModelVersion modelVersion : modelVersions) {
    System.out.println(modelVersion);
}
```
{% endtab %}

{% tab title="gRPC NodeJS" %}
```javascript
// Insert here the initialization code as outlined on this page:
// https://docs.clarifai.com/api-guide/api-overview/api-clients#client-installation-instructions

stub.ListModelVersions(
    {model_id: "petsID"},
    metadata,
    (err, response) => {
        if (err) {
            throw new Error(err);
        }

        if (response.status.code !== 10000) {
            throw new Error("List model versions failed, status: " + response.status.description);
        }

        for (const model_version of response.model_versions) {
            console.log(JSON.stringify(model_version, null, 2));
        }
    }
);
```
{% endtab %}

{% tab title="gRPC Python" %}
```python
# Insert here the initialization code as outlined on this page:
# https://docs.clarifai.com/api-guide/api-overview/api-clients#client-installation-instructions

list_model_versions_response = stub.ListModelVersions(
    service_pb2.ListModelVersionsRequest(
        model_id="petsID"
    ),
    metadata=metadata
)

if list_model_versions_response.status.code != status_code_pb2.SUCCESS:
    raise Exception("List model versions failed, status: " + list_model_versions_response.status.description)

for model_version in list_model_versions_response.model_versions:
    print(model_version)
```
{% endtab %}

{% tab title="js" %}
```javascript
app.models.initModel('{id}').then(
  function(model) {
    model.getVersions().then(
      function(response) {
        // do something with response
      },
      function(err) {
        // there was an error
      }
    );
  },
  function(err) {
    // there was an error
  }
);
```
{% endtab %}

{% tab title="python" %}
```python
from clarifai.rest import ClarifaiApp
app = ClarifaiApp(api_key='YOUR_API_KEY')

model = app.models.get('{id}')
model.list_versions()
```
{% endtab %}

{% tab title="java" %}
```java
client.getModelVersions("{model_id}").getPage(1).executeSync();
```
{% endtab %}

{% tab title="csharp" %}
```csharp
using System.Threading.Tasks;
using Clarifai.API;

namespace YourNamespace
{
    public class YourClassName
    {
        public static async Task Main()
        {
            var client = new ClarifaiClient("YOUR_API_KEY");

            await client.GetModelVersions("{model_id}")
                .ExecuteAsync();
        }
    }
}
```
{% endtab %}

{% tab title="objective-c" %}
```text
[app listVersionsForModel:@"{model_id}" page:1 resultsPerPage:30 completion:^(NSArray<ClarifaiModelVersion *> *versions, NSError *error) {
    NSLog(@"versions: %@", versions);
}];
```
{% endtab %}

{% tab title="php" %}
```php
use Clarifai\API\ClarifaiClient;
use Clarifai\DTOs\Models\ModelVersion;

$client = new ClarifaiClient('YOUR_API_KEY');

$response = $client->getModelVersions('MODEL_ID')
    ->executeSync();

if ($response->isSuccessful()) {
    echo "Response is successful.\n";

    /** @var ModelVersion[] $modelVersions */
    $modelVersions = $response->get();
    foreach ($modelVersions as $modelVersion) {
        echo $modelVersion->id() . ' ' . $modelVersion->modelTrainingStatus() . "\n";
    }

} else {
    echo "Response is not successful. Reason: \n";
    echo $response->status()->description() . "\n";
    echo $response->status()->errorDetails() . "\n";
    echo "Status code: " . $response->status()->statusCode();
}
```
{% endtab %}

{% tab title="cURL" %}
```text
curl -X GET \
  -H "Authorization: Key YOUR_API_KEY" \
  https://api.clarifai.com/v2/models/petsID/versions
```
{% endtab %}
{% endtabs %}

### Get Model Version By Id

To get a specific model version, you must provide the model\_id as well as the version\_id. You can inspect the model version status to determine if your model is trained or still training.

{% tabs %}
{% tab title="gRPC Java" %}
```java
import com.clarifai.grpc.api.*;
import com.clarifai.grpc.api.status.*;

// Insert here the initialization code as outlined on this page:
// https://docs.clarifai.com/api-guide/api-overview/api-clients#client-installation-instructions


SingleModelVersionResponse getModelVersionResponse = stub.getModelVersion(
    GetModelVersionRequest.newBuilder()
        .setModelId("petsID")
        .setVersionId("{YOUR_MODEL_VERSION_ID}")
        .build()
);

if (getModelVersionResponse.getStatus().getCode() != StatusCode.SUCCESS) {
    throw new RuntimeException("Get model version failed, status: " + getModelVersionResponse.getStatus());
}

ModelVersion modelVersion = getModelVersionResponse.getModelVersion();
System.out.println(modelVersion);
```
{% endtab %}

{% tab title="gRPC NodeJS" %}
```javascript
// Insert here the initialization code as outlined on this page:
// https://docs.clarifai.com/api-guide/api-overview/api-clients#client-installation-instructions

stub.GetModelVersion(
    {model_id: "petsID", version_id: "{YOUR_MODEL_VERSION_ID}"},
    metadata,
    (err, response) => {
        if (err) {
            throw new Error(err);
        }

        if (response.status.code !== 10000) {
            throw new Error("Get model version failed, status: " + response.status.description);
        }

        const model_version = response.model_version;
        console.log(JSON.stringify(model_version, null, 2));
    }
);
```
{% endtab %}

{% tab title="gRPC Python" %}
```python
# Insert here the initialization code as outlined on this page:
# https://docs.clarifai.com/api-guide/api-overview/api-clients#client-installation-instructions

get_model_version_response = stub.GetModelVersion(
    service_pb2.GetModelVersionRequest(
        model_id="petsID",
        version_id="{YOUR_MODEL_VERSION_ID}"
    ),
    metadata=metadata
)

if get_model_version_response.status.code != status_code_pb2.SUCCESS:
    raise Exception("Get model version failed, status: " + get_model_version_response.status.description)

model_version = get_model_version_response.model_version
print(model_version)
```
{% endtab %}

{% tab title="js" %}
```javascript
app.models.initModel('{id}').then(
  function(model) {
    model.getVersion('{version_id}').then(
      function(response) {
        // do something with response
      },
      function(err) {
        // there was an error
      }
    );
  },
  function(err) {
    // there was an error
  }
);
```
{% endtab %}

{% tab title="python" %}
```python
from clarifai.rest import ClarifaiApp
app = ClarifaiApp(api_key='YOUR_API_KEY')

model = app.models.get('{id}')
model.get_version('{version_id}')
```
{% endtab %}

{% tab title="java" %}
```java
client.getModelVersionByID("{model_id}", "{version_id}").executeSync();

// Or in a more object-oriented manner:
client.getModelByID("{model_id}")
    .executeSync().get() // Returns Model object
    .getVersionByID("{version_id}").executeSync();
```
{% endtab %}

{% tab title="csharp" %}
```csharp
using System.Threading.Tasks;
using Clarifai.API;

namespace YourNamespace
{
    public class YourClassName
    {
        public static async Task Main()
        {
            var client = new ClarifaiClient("YOUR_API_KEY");

            await client.GetModelVersion("{model_id}", "{version_id}")
                .ExecuteAsync();
        }
    }
}
```
{% endtab %}

{% tab title="objective-c" %}
```text
[app getVersionForModel:@"{model_id}" versionID:{version_id} completion:^(ClarifaiModelVersion *version, NSError *error) {
    NSLog(@"version: %@", version);
}];
```
{% endtab %}

{% tab title="php" %}
```php
use Clarifai\API\ClarifaiClient;
use Clarifai\DTOs\Models\ModelVersion;

$client = new ClarifaiClient('YOUR_API_KEY');

$response = $client->getModelVersion('MODEL_ID', 'MODEL_VERSION_ID')
    ->executeSync();

if ($response->isSuccessful()) {
    echo "Response is successful.\n";

    /** @var ModelVersion $modelVersion */
    $modelVersion = $response->get();
    echo $modelVersion->id() . ' ' . $modelVersion->modelTrainingStatus() . "\n";

} else {
    echo "Response is not successful. Reason: \n";
    echo $response->status()->description() . "\n";
    echo $response->status()->errorDetails() . "\n";
    echo "Status code: " . $response->status()->statusCode();
}
```
{% endtab %}

{% tab title="cURL" %}
```text
curl -X GET \
  -H "Authorization: Key YOUR_API_KEY" \
  https://api.clarifai.com/v2/models/petsID/versions/{YOUR_MODEL_VERSION_ID}
```
{% endtab %}
{% endtabs %}

### Get Model Training Inputs

You can list all the inputs that were used to train the model.

{% tabs %}
{% tab title="gRPC Java" %}
```java
import com.clarifai.grpc.api.*;
import com.clarifai.grpc.api.status.*;

// Insert here the initialization code as outlined on this page:
// https://docs.clarifai.com/api-guide/api-overview/api-clients#client-installation-instructions

MultiInputResponse listModelInputsResponse = stub.listModelInputs(
    ListModelInputsRequest.newBuilder()
        .setModelId("petsID")
        .build()
);

if (listModelInputsResponse.getStatus().getCode() != StatusCode.SUCCESS) {
    throw new RuntimeException("List model inputs failed, status: " + listModelInputsResponse.getStatus());
}

List<Input> inputs = listModelInputsResponse.getInputsList();
for (Input input : inputs) {
    System.out.println(input);
}
```
{% endtab %}

{% tab title="gRPC NodeJS" %}
```javascript
// Insert here the initialization code as outlined on this page:
// https://docs.clarifai.com/api-guide/api-overview/api-clients#client-installation-instructions

stub.ListModelInputs(
    {model_id: "petsID"},
    metadata,
    (err, response) => {
        if (err) {
            throw new Error(err);
        }

        if (response.status.code !== 10000) {
            throw new Error("List model inputs failed, status: " + response.status.description);
        }

        for (const input of response.inputs) {
            console.log(JSON.stringify(input, null, 2));
        }
    }
);
```
{% endtab %}

{% tab title="gRPC Python" %}
```python
# Insert here the initialization code as outlined on this page:
# https://docs.clarifai.com/api-guide/api-overview/api-clients#client-installation-instructions

list_model_inputs_response = stub.ListModelInputs(
    service_pb2.ListModelInputsRequest(model_id="petsID"),
    metadata=metadata
)

if list_model_inputs_response.status.code != status_code_pb2.SUCCESS:
    raise Exception("List model inputs failed, status: " + list_model_inputs_response.status.description)

for input_object in list_model_inputs_response.inputs:
    print(input_object)
```
{% endtab %}

{% tab title="js" %}
```javascript
app.models.initModel('{id}').then(
  function(model) {
    model.getInputs().then(
      function(response) {
        // do something with response
      },
      function(err) {
        // there was an error
      }
    );
  },
  function(err) {
    // there was an error
  }
);
```
{% endtab %}

{% tab title="python" %}
```python
from clarifai.rest import ClarifaiApp
app = ClarifaiApp(api_key='YOUR_API_KEY')

model = app.models.get('{id}')
model.get_inputs()
```
{% endtab %}

{% tab title="java" %}
```java
client.getModelInputs("{model_id}").getPage(1).executeSync();
```
{% endtab %}

{% tab title="csharp" %}
```csharp
using System.Threading.Tasks;
using Clarifai.API;

namespace YourNamespace
{
    public class YourClassName
    {
        public static async Task Main()
        {
            var client = new ClarifaiClient("YOUR_API_KEY");

            await client.GetModelInputs("{model_id}")
                .Page(1)
                .ExecuteAsync();
        }
    }
}
```
{% endtab %}

{% tab title="objective-c" %}
```text
[app listTrainingInputsForModel:@"{model_id}" page:1 resultsPerPage:30 completion:^(NSArray<ClarifaiInput *> *inputs, NSError *error) {
    NSLog(@"inputs: %@", inputs);
}];
```
{% endtab %}

{% tab title="php" %}
```php
// Coming soon
```
{% endtab %}

{% tab title="cURL" %}
```text
curl -X GET \
  -H "Authorization: Key YOUR_API_KEY" \
  https://api.clarifai.com/v2/models/petsID/inputs
```
{% endtab %}
{% endtabs %}

### Get Model Training Inputs By Version

You can also list all the inputs that were used to train a specific model version.

{% tabs %}
{% tab title="gRPC Java" %}
```java
import com.clarifai.grpc.api.*;
import com.clarifai.grpc.api.status.*;
import java.util.List;

// Insert here the initialization code as outlined on this page:
// https://docs.clarifai.com/api-guide/api-overview/api-clients#client-installation-instructions

MultiInputResponse listModelInputsResponse = stub.listModelInputs(
    ListModelInputsRequest.newBuilder()
        .setModelId("petsID")
        .setVersionId("{YOUR_MODEL_VERSION_ID}")
        .build()
);

if (listModelInputsResponse.getStatus().getCode() != StatusCode.SUCCESS) {
    throw new RuntimeException("List model inputs failed, status: " + listModelInputsResponse.getStatus());
}

List<Input> inputs = listModelInputsResponse.getInputsList();
for (Input input : inputs) {
    System.out.println(input);
}
```
{% endtab %}

{% tab title="gRPC NodeJS" %}
```javascript
// Insert here the initialization code as outlined on this page:
// https://docs.clarifai.com/api-guide/api-overview/api-clients#client-installation-instructions

stub.ListModelInputs(
    {
        model_id: "petsID",
        version_id: "{YOUR_MODEL_VERSION_ID}"
    },
    metadata,
    (err, response) => {
        if (err) {
            throw new Error(err);
        }

        if (response.status.code !== 10000) {
            throw new Error("List model inputs failed, status: " + response.status.description);
        }

        for (const input of response.inputs) {
            console.log(JSON.stringify(input, null, 2));
        }
    }
);
```
{% endtab %}

{% tab title="gRPC Python" %}
```python
# Insert here the initialization code as outlined on this page:
# https://docs.clarifai.com/api-guide/api-overview/api-clients#client-installation-instructions

list_model_inputs_response = stub.ListModelInputs(
    service_pb2.ListModelInputsRequest(
        model_id="petsID",
        version_id="{YOUR_MODEL_VERSION_ID}"
    ),
    metadata=metadata
)

if list_model_inputs_response.status.code != status_code_pb2.SUCCESS:
    raise Exception("List model inputs failed, status: " + list_model_inputs_response.status.description)

for input_object in list_model_inputs_response.inputs:
    print(input_object)
```
{% endtab %}

{% tab title="js" %}
```javascript
app.models.initModel({id: '{model_id}', version: '{version_id}'}).then(
  function(model) {
    model.getInputs().then(
      function(response) {
        // do something with response
      },
      function(err) {
        // there was an error
      }
    );
  },
  function(err) {
    // there was an error
  }
);
```
{% endtab %}

{% tab title="python" %}
```python
from clarifai.rest import ClarifaiApp
app = ClarifaiApp(api_key='YOUR_API_KEY')

model = app.models.get('{id}')
model.get_inputs('{version_id}')
```
{% endtab %}

{% tab title="java" %}
```java
client.getModelInputs("{model_id}")
    .fromSpecificModelVersion("{version_id}")
    .getPage(1)
    .executeSync();
```
{% endtab %}

{% tab title="csharp" %}
```csharp
using System.Threading.Tasks;
using Clarifai.API;

namespace YourNamespace
{
    public class YourClassName
    {
        public static async Task Main()
        {
            var client = new ClarifaiClient("YOUR_API_KEY");

            await client.GetModelInputs("{model_id}", "{version_id}")
                .Page(1)
                .ExecuteAsync();
        }
    }
}
```
{% endtab %}

{% tab title="objective-c" %}
```text
[_app listTrainingInputsForModel:@"{model_id}" page:1 resultsPerPage:30 completion:^(NSArray<ClarifaiInput *> *inputs, NSError *error) {
    NSLog(@"inputs: %@", inputs);
}];
```
{% endtab %}

{% tab title="php" %}
```php
// Coming soon
```
{% endtab %}

{% tab title="cURL" %}
```text
curl -X GET \
  -H "Authorization: Key YOUR_API_KEY" \
  https://api.clarifai.com/v2/models/petsID/versions/{YOUR_MODEL_VERSION_ID}/inputs
```
{% endtab %}
{% endtabs %}

### Delete A Model

You can delete a model using the model\_id.

{% tabs %}
{% tab title="gRPC Java" %}
```java
import com.clarifai.grpc.api.*;
import com.clarifai.grpc.api.status.*;

// Insert here the initialization code as outlined on this page:
// https://docs.clarifai.com/api-guide/api-overview/api-clients#client-installation-instructions

BaseResponse deleteModelResponse = stub.deleteModel(
    DeleteModelRequest.newBuilder()
        .setModelId("petsID")
        .build()
);

if (deleteModelResponse.getStatus().getCode() != StatusCode.SUCCESS) {
    throw new RuntimeException("Delete model failed, status: " + deleteModelResponse.getStatus());
}
```
{% endtab %}

{% tab title="gRPC NodeJS" %}
```javascript
// Insert here the initialization code as outlined on this page:
// https://docs.clarifai.com/api-guide/api-overview/api-clients#client-installation-instructions

stub.DeleteModel(
    {model_id: "petsID"},
    metadata,
    (err, response) => {
        if (err) {
            throw new Error(err);
        }

        if (response.status.code !== 10000) {
            throw new Error("Delete model failed, status: " + response.status.description);
        }
    }
);
```
{% endtab %}

{% tab title="gRPC Python" %}
```python
# Insert here the initialization code as outlined on this page:
# https://docs.clarifai.com/api-guide/api-overview/api-clients#client-installation-instructions

delete_model_response = stub.DeleteModel(
    service_pb2.DeleteModelRequest(model_id="petsID"),
    metadata=metadata
)

if delete_model_response.status.code != status_code_pb2.SUCCESS:
    raise Exception("Delete model failed, status: " + delete_model_response.status.description)
```
{% endtab %}

{% tab title="js" %}
```javascript
app.models.delete('{id}').then(
  function(response) {
    // do something with response
  },
  function(err) {
    // there was an error
  }
);
```
{% endtab %}

{% tab title="python" %}
```python
from clarifai.rest import ClarifaiApp
app = ClarifaiApp(api_key='YOUR_API_KEY')

app.models.delete('{id}')
```
{% endtab %}

{% tab title="java" %}
```java
client.deleteModel("{model_id}").executeSync();
```
{% endtab %}

{% tab title="csharp" %}
```csharp
using System.Threading.Tasks;
using Clarifai.API;

namespace YourNamespace
{
    public class YourClassName
    {
        public static async Task Main()
        {
            var client = new ClarifaiClient("YOUR_API_KEY");

            await client.DeleteModel("{model_id}")
                .ExecuteAsync();
        }
    }
}
```
{% endtab %}

{% tab title="objective-c" %}
```text
[app deleteModel:@"{model_id}" completion:^(NSError *error) {
    NSLog(@"model is deleted");
}];
```
{% endtab %}

{% tab title="php" %}
```php
use Clarifai\API\ClarifaiClient;

$client = new ClarifaiClient('YOUR_API_KEY');

$response = $client->deleteModel('MODEL_ID')
    ->executeSync();

if ($response->isSuccessful()) {
    echo "Response is successful.\n";
} else {
    echo "Response is not successful. Reason: \n";
    echo $response->status()->description() . "\n";
    echo $response->status()->errorDetails() . "\n";
    echo "Status code: " . $response->status()->statusCode();
}
```
{% endtab %}

{% tab title="cURL" %}
```text
curl -X DELETE \
  -H "Authorization: Key YOUR_API_KEY" \
  https://api.clarifai.com/v2/models/petsID
```
{% endtab %}
{% endtabs %}

### Delete A Model Version

You can also delete a specific version of a model with the model\_id and version\_id.

{% tabs %}
{% tab title="gRPC Java" %}
```java
import com.clarifai.grpc.api.*;
import com.clarifai.grpc.api.status.*;

// Insert here the initialization code as outlined on this page:
// https://docs.clarifai.com/api-guide/api-overview/api-clients#client-installation-instructions

BaseResponse deleteModelVersionResponse = stub.deleteModelVersion(
    DeleteModelVersionRequest.newBuilder()
        .setModelId("petsID")
        .setVersionId("{YOUR_MODEL_VERSION_ID}")
        .build()
);

if (deleteModelVersionResponse.getStatus().getCode() != StatusCode.SUCCESS) {
    throw new RuntimeException("Delete model version failed, status: " + deleteModelVersionResponse.getStatus());
}
```
{% endtab %}

{% tab title="gRPC NodeJS" %}
```javascript
// Insert here the initialization code as outlined on this page:
// https://docs.clarifai.com/api-guide/api-overview/api-clients#client-installation-instructions

stub.DeleteModelVersion(
    {
        model_id: "petsID",
        version_id: "{YOUR_MODEL_VERSION_ID}"
    },
    metadata,
    (err, response) => {
        if (err) {
            throw new Error(err);
        }

        if (response.status.code !== 10000) {
            throw new Error("Delete model version failed, status: " + response.status.description);
        }
    }
);
```
{% endtab %}

{% tab title="gRPC Python" %}
```python
# Insert here the initialization code as outlined on this page:
# https://docs.clarifai.com/api-guide/api-overview/api-clients#client-installation-instructions

delete_model_version_response = stub.DeleteModelVersion(
    service_pb2.DeleteModelVersionRequest(
        model_id="petsID",
        version_id="{YOUR_MODEL_VERSION_ID}"
    ),
    metadata=metadata
)

if delete_model_version_response.status.code != status_code_pb2.SUCCESS:
    raise Exception("Delete model version failed, status: " + delete_model_version_response.status.description)
```
{% endtab %}

{% tab title="js" %}
```javascript
app.models.delete('{model_id}', '{version_id}').then(
  function(response) {
    // do something with response
  },
  function(err) {
    // there was an error
  }
);
```
{% endtab %}

{% tab title="python" %}
```python
from clarifai.rest import ClarifaiApp
app = ClarifaiApp(api_key='YOUR_API_KEY')

app.models.delete('{id}', '{version_id}')

# or

model = app.models.get('{id}')
model.delete_version('{version_id}')
```
{% endtab %}

{% tab title="java" %}
```java
client.deleteModelVersion("{model_id}", "{version_id}").executeSync();

// Or in a more object-oriented manner:
client.getModelByID("{model_id}")
    .executeSync().get() // Returns Model object
    .deleteVersion("{version_id}")
    .executeSync();
```
{% endtab %}

{% tab title="csharp" %}
```csharp
using System.Threading.Tasks;
using Clarifai.API;
using Clarifai.DTOs.Predictions;

namespace YourNamespace
{
    public class YourClassName
    {
        public static async Task Main()
        {
            var client = new ClarifaiClient("YOUR_API_KEY");

            await client.DeleteModelVersion("{model_id}", "{version_id}")
                .ExecuteAsync();

            // Or in a more object-oriented manner:
            var model = (await client.GetModel<Concept>("{model_id}")
                .ExecuteAsync()).Get();  // Returns a Model<Concept> object.
            await model.DeleteModelVersion("{version_id}").ExecuteAsync();
        }
    }
}
```
{% endtab %}

{% tab title="objective-c" %}
```text
[app deleteVersionForModel:{model_id} versionID:{version_id} completion:^(NSError *error) {
    NSLog(@"model version deleted");
}];
```
{% endtab %}

{% tab title="php" %}
```php
use Clarifai\API\ClarifaiClient;

$client = new ClarifaiClient('YOUR_API_KEY');

$response = $client->deleteModelVersion('MODEL_ID', 'MODEL_VERSION_ID')
    ->executeSync();

if ($response->isSuccessful()) {
    echo "Response is successful.\n";
} else {
    echo "Response is not successful. Reason: \n";
    echo $response->status()->description() . "\n";
    echo $response->status()->errorDetails() . "\n";
    echo "Status code: " . $response->status()->statusCode();
}
```
{% endtab %}

{% tab title="cURL" %}
```text
curl -X DELETE \
  -H "Authorization: Key YOUR_API_KEY" \
  https://api.clarifai.com/v2/models/petsID/versions/{YOUR_MODEL_VERSION_ID}
```
{% endtab %}
{% endtabs %}

### Delete All Models

If you would like to delete all models associated with an application, you can also do that. Please proceed with caution as these cannot be recovered.

{% tabs %}
{% tab title="gRPC Java" %}
```java
import com.clarifai.grpc.api.*;
import com.clarifai.grpc.api.status.*;

// Insert here the initialization code as outlined on this page:
// https://docs.clarifai.com/api-guide/api-overview/api-clients#client-installation-instructions

BaseResponse deleteModelsResponse = stub.deleteModels(
    DeleteModelsRequest.newBuilder()
        .setDeleteAll(true)
        .build()
);

if (deleteModelsResponse.getStatus().getCode() != StatusCode.SUCCESS) {
    throw new RuntimeException("Delete models failed, status: " + deleteModelsResponse.getStatus());
}
```
{% endtab %}

{% tab title="gRPC NodeJS" %}
```javascript
// Insert here the initialization code as outlined on this page:
// https://docs.clarifai.com/api-guide/api-overview/api-clients#client-installation-instructions

stub.DeleteModels(
    {delete_all: true},
    metadata,
    (err, response) => {
        if (err) {
            throw new Error(err);
        }

        if (response.status.code !== 10000) {
            throw new Error("Delete models failed, status: " + response.status.description);
        }
    }
);
```
{% endtab %}

{% tab title="gRPC Python" %}
```python
# Insert here the initialization code as outlined on this page:
# https://docs.clarifai.com/api-guide/api-overview/api-clients#client-installation-instructions

delete_models_response = stub.DeleteModels(
    service_pb2.DeleteModelsRequest(delete_all=True),
    metadata=metadata
)

if delete_models_response.status.code != status_code_pb2.SUCCESS:
    raise Exception("Delete models failed, status: " + delete_models_response.status.description)
```
{% endtab %}

{% tab title="js" %}
```javascript
app.models.delete().then(
  function(response) {
    // do something with response
  },
  function(err) {
    // there was an error
  }
);
```
{% endtab %}

{% tab title="python" %}
```python
from clarifai.rest import ClarifaiApp
app = ClarifaiApp(api_key='YOUR_API_KEY')

app.models.delete_all()
```
{% endtab %}

{% tab title="java" %}
```java
client.deleteAllModels().executeSync();
```
{% endtab %}

{% tab title="csharp" %}
```csharp
using System.Threading.Tasks;
using Clarifai.API;

namespace YourNamespace
{
    public class YourClassName
    {
        public static async Task Main()
        {
            var client = new ClarifaiClient("YOUR_API_KEY");

            await client.DeleteAllModels()
                .ExecuteAsync();
        }
    }
}
```
{% endtab %}

{% tab title="objective-c" %}
```text
[_app deleteAllModels:^(NSError *error) {
    NSLog(@"delete all models");
}];
```
{% endtab %}

{% tab title="php" %}
```php
use Clarifai\API\ClarifaiClient;
use ClarifaiIntTests\BaseIntTest;

$client = new ClarifaiClient('YOUR_API_KEY');

$response = $client->deleteAllModels()
    ->executeSync();

if ($response->isSuccessful()) {
    echo "Response is successful.\n";
} else {
    echo "Response is not successful. Reason: \n";
    echo $response->status()->description() . "\n";
    echo $response->status()->errorDetails() . "\n";
    echo "Status code: " . $response->status()->statusCode();
}
```
{% endtab %}

{% tab title="cURL" %}
```text
curl -X DELETE \
  -H "Authorization: Key YOUR_API_KEY" \
  https://api.clarifai.com/v2/models/
```
{% endtab %}
{% endtabs %}

### Train A Model

When you train a model, you are telling the system to look at all the images with concepts you've provided and learn from them. This train operation is asynchronous. It may take a few seconds for your model to be fully trained and ready.

_Note: you can repeat this operation as often as you like. By adding more images with concepts and training, you can get the model to predict exactly how you want it to._

{% tabs %}
{% tab title="gRPC Java" %}
```java
import com.clarifai.grpc.api.*;
import com.clarifai.grpc.api.status.*;

// Insert here the initialization code as outlined on this page:
// https://docs.clarifai.com/api-guide/api-overview/api-clients#client-installation-instructions

SingleModelResponse postModelVersionsResponse = stub.postModelVersions(
    PostModelVersionsRequest.newBuilder()
        .setModelId("petsID")
        .build()
);

if (postModelVersionsResponse.getStatus().getCode() != StatusCode.SUCCESS) {
  throw new RuntimeException("Post model versions failed, status: " + postModelVersionsResponse.getStatus());
}

String modelVersionId = postModelVersionsResponse.getModel().getModelVersion().getId();
System.out.println("New model version ID: " + modelVersionId);
```
{% endtab %}

{% tab title="gRPC NodeJS" %}
```javascript
// Insert here the initialization code as outlined on this page:
// https://docs.clarifai.com/api-guide/api-overview/api-clients#client-installation-instructions

stub.PostModelVersions(
    {model_id: "petsID"},
    metadata,
    (err, response) => {
        if (err) {
            throw new Error(err);
        }

        if (response.status.code !== 10000) {
            throw new Error("Post model versions failed, status: " + response.status.description);
        }
    }
);
```
{% endtab %}

{% tab title="gRPC Python" %}
```python
# Insert here the initialization code as outlined on this page:
# https://docs.clarifai.com/api-guide/api-overview/api-clients#client-installation-instructions

post_model_versions = stub.PostModelVersions(
    service_pb2.PostModelVersionsRequest(
        model_id="petsID"
    ),
    metadata=metadata
)

if post_model_versions.status.code != status_code_pb2.SUCCESS:
    raise Exception("Post model versions failed, status: " + post_model_versions.status.description)
```
{% endtab %}

{% tab title="js" %}
```javascript
app.models.train("{model_id}").then(
  function(response) {
    // do something with response
  },
  function(err) {
    // there was an error
  }
);

// or if you have an instance of a model

model.train().then(
  function(response) {
    // do something with response
  },
  function(err) {
    // there was an error
  }
);
```
{% endtab %}

{% tab title="python" %}
```python
from clarifai.rest import ClarifaiApp
app = ClarifaiApp(api_key='YOUR_API_KEY')

model = app.models.get('{model_id}')
model.train()
```
{% endtab %}

{% tab title="java" %}
```java
client.trainModel("{model_id}").executeSync();
```
{% endtab %}

{% tab title="csharp" %}
```csharp
using System.Threading.Tasks;
using Clarifai.API;
using Clarifai.DTOs.Predictions;

namespace YourNamespace
{
    public class YourClassName
    {
        public static async Task Main()
        {
            var client = new ClarifaiClient("YOUR_API_KEY");

            await client.TrainModel<Concept>("{model_id}")
                .ExecuteAsync();
        }
    }
}
```
{% endtab %}

{% tab title="objective-c" %}
```text
ClarifaiImage *image = [[ClarifaiImage alloc] initWithURL:@"https://samples.clarifai.com/puppy.jpeg"]
[app getModel:@"{id}" completion:^(ClarifaiModel *model, NSError *error) {
    [model train:^(ClarifaiModel *model, NSError *error) {
        NSLog(@"model: %@", model);
    }];
}];
```
{% endtab %}

{% tab title="php" %}
```php
use Clarifai\API\ClarifaiClient;
use Clarifai\DTOs\Models\ModelType;

$client = new ClarifaiClient('YOUR_API_KEY');

$response = $client->trainModel(ModelType::concept(), 'MODEL_ID')
    ->executeSync();

if ($response->isSuccessful()) {
    echo "Response is successful.\n";
} else {
    echo "Response is not successful. Reason: \n";
    echo $response->status()->description() . "\n";
    echo $response->status()->errorDetails() . "\n";
    echo "Status code: " . $response->status()->statusCode();
}
```
{% endtab %}

{% tab title="cURL" %}
```text
curl -X POST \
  -H "Authorization: Key YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  https://api.clarifai.com/v2/models/petsID/versions
```
{% endtab %}
{% endtabs %}

## Predict With A Model

Once you have trained a model you are ready to use your new model to get predictions. The predictions returned will only contain the concepts that you told it to see.

{% tabs %}
{% tab title="gRPC Java" %}
```java
import com.clarifai.grpc.api.*;
import com.clarifai.grpc.api.status.*;

// Insert here the initialization code as outlined on this page:
// https://docs.clarifai.com/api-guide/api-overview/api-clients#client-installation-instructions

MultiOutputResponse postModelOutputsResponse = stub.postModelOutputs(
    PostModelOutputsRequest.newBuilder()
        .setModelId("petsID")
        .setVersionId("{YOUR_MODEL_VERSION_ID}")  // Optional. Defaults to the latest version.
        .addInputs(
            Input.newBuilder().setData(
                Data.newBuilder().setImage(
                    Image.newBuilder().setUrl("https://samples.clarifai.com/metro-north.jpg")
                )
            )
        )
        .build()
);

if (postModelOutputsResponse.getStatus().getCode() != StatusCode.SUCCESS) {
  throw new RuntimeException("Post model outputs failed, status: " + postModelOutputsResponse.getStatus());
}

// Since we have one input, one output will exist here.
Output output = postModelOutputsResponse.getOutputs(0);

System.out.println("Predicted concepts:");
for (Concept concept : output.getData().getConceptsList()) {
    System.out.printf("%s %.2f%n", concept.getName(), concept.getValue());
}
```
{% endtab %}

{% tab title="gRPC NodeJS" %}
```javascript
// Insert here the initialization code as outlined on this page:
// https://docs.clarifai.com/api-guide/api-overview/api-clients#client-installation-instructions

stub.PostModelOutputs(
    {
        model_id: "petsID",
        version_id: "{YOUR_MODEL_VERSION_ID}",  // This is optional. Defaults to the latest model version.
        inputs: [
            {data: {image: {url: "https://samples.clarifai.com/metro-north.jpg"}}}
        ]
    },
    metadata,
    (err, response) => {
        if (err) {
            throw new Error(err);
        }

        if (response.status.code !== 10000) {
            throw new Error("Post model outputs failed, status: " + response.status.description);
        }

        // Since we have one input, one output will exist here.
        const output = response.outputs[0];

        console.log("Predicted concepts:");
        for (const concept of output.data.concepts) {
            console.log(concept.name + " " + concept.value);
        }
    }
);
```
{% endtab %}

{% tab title="gRPC Python" %}
```python
# Insert here the initialization code as outlined on this page:
# https://docs.clarifai.com/api-guide/api-overview/api-clients#client-installation-instructions

post_model_outputs_response = stub.PostModelOutputs(
    service_pb2.PostModelOutputsRequest(
        model_id="petsID",
        version_id="{YOUR_MODEL_VERSION_ID}",  # This is optional. Defaults to the latest model version.
        inputs=[
            resources_pb2.Input(
                data=resources_pb2.Data(
                    image=resources_pb2.Image(
                        url="https://samples.clarifai.com/metro-north.jpg"
                    )
                )
            )
        ]
    ),
    metadata=metadata
)
if post_model_outputs_response.status.code != status_code_pb2.SUCCESS:
    raise Exception("Post model outputs failed, status: " + post_model_outputs_response.status.description)

# Since we have one input, one output will exist here.
output = post_model_outputs_response.outputs[0]

print("Predicted concepts:")
for concept in output.data.concepts:
    print("%s %.2f" % (concept.name, concept.value))
```
{% endtab %}

{% tab title="js" %}
```javascript
app.models.predict("{model_id}", ["https://samples.clarifai.com/puppy.jpeg"]).then(
  function(response) {
    // do something with response
  },
  function(err) {
    // there was an error
  }
);

// or if you have an instance of a model

model.predict("https://samples.clarifai.com/puppy.jpeg").then(
  function(response) {
    // do something with response
  },
  function(err) {
    // there was an error
  }
);
```
{% endtab %}

{% tab title="python" %}
```python
from clarifai.rest import ClarifaiApp
app = ClarifaiApp(api_key='YOUR_API_KEY')

model = app.models.get('YOUR_MODEL_ID')

response = model.predict_by_url('https://samples.clarifai.com/puppy.jpeg')
```
{% endtab %}

{% tab title="java" %}
```java
client.predict("{model_id}")
    .withInputs(
        ClarifaiInput.forImage("https://samples.clarifai.com/puppy.jpeg")
    )
    .executeSync();
```
{% endtab %}

{% tab title="csharp" %}
```csharp
using System.Threading.Tasks;
using Clarifai.API;
using Clarifai.DTOs.Inputs;
using Clarifai.DTOs.Predictions;

namespace YourNamespace
{
    public class YourClassName
    {
        public static async Task Main()
        {
            var client = new ClarifaiClient("YOUR_API_KEY");

            await client.Predict<Concept>(
                    "{model_id}",
                    new ClarifaiURLImage("https://samples.clarifai.com/puppy.jpeg"))
                .ExecuteAsync();
        }
    }
}
```
{% endtab %}

{% tab title="objective-c" %}
```text
ClarifaiImage *image = [[ClarifaiImage alloc] initWithURL:@"https://samples.clarifai.com/puppy.jpeg"]
[app getModel:@"{model_id}" completion:^(ClarifaiModel *model, NSError *error) {
    [model predictOnImages:@[image]
                completion:^(NSArray<ClarifaiSearchResult *> *outputs, NSError *error) {
                    NSLog(@"outputs: %@", outputs);
                }];
}];
```
{% endtab %}

{% tab title="php" %}
```php
use Clarifai\API\ClarifaiClient;
use Clarifai\DTOs\Inputs\ClarifaiURLImage;
use Clarifai\DTOs\Models\ModelType;
use Clarifai\DTOs\Outputs\ClarifaiOutput;
use Clarifai\DTOs\Predictions\Concept;

$client = new ClarifaiClient('YOUR_API_KEY');

$response = $client->predict(ModelType::concept(), 'MODEL_ID',
        new ClarifaiURLImage('https://samples.clarifai.com/puppy.jpeg'))
    ->executeSync();

if ($response->isSuccessful()) {
    echo "Response is successful.\n";

    /** @var ClarifaiOutput $output */
    $output = $response->get();

    echo "Predicted concepts:\n";
    /** @var Concept $concept */
    foreach ($output->data() as $concept) {
        echo $concept->name() . ': ' . $concept->value() . "\n";
    }
} else {
    echo "Response is not successful. Reason: \n";
    echo $response->status()->description() . "\n";
    echo $response->status()->errorDetails() . "\n";
    echo "Status code: " . $response->status()->statusCode();
}
```
{% endtab %}

{% tab title="cURL" %}
```text
curl -X POST \
  -H "Authorization: Key YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '
  {
    "inputs": [
      {
        "data": {
          "image": {
            "url": "https://samples.clarifai.com/puppy.jpeg"
          }
        }
      }
    ]
  }'\
  https://api.clarifai.com/v2/models/petsID/outputs

# Model version defaults to latest. If you want to specify the model version, use this URL:
# https://api.clarifai.com/v2/models/petsID/versions/{YOUR_MODEL_VERSION_ID}/outputs
```
{% endtab %}
{% endtabs %}

### Search Models By Name And Type

You can search all your models by name and type of model.

{% tabs %}
{% tab title="gRPC Java" %}
```java
import com.clarifai.grpc.api.*;
import com.clarifai.grpc.api.status.*;
import java.util.List;

// Insert here the initialization code as outlined on this page:
// https://docs.clarifai.com/api-guide/api-overview/api-clients#client-installation-instructions

MultiModelResponse postModelsSearchesResponse = stub.postModelsSearches(
    PostModelsSearchesRequest.newBuilder()
        .setModelQuery(
            ModelQuery.newBuilder()
                .setName("gen*")
                .setType("concept")
        )
        .build()
);

if (postModelsSearchesResponse.getStatus().getCode() != StatusCode.SUCCESS) {
    throw new RuntimeException("Post models searches failed, status: " + postModelsSearchesResponse.getStatus());
}

List<Model> models = postModelsSearchesResponse.getModelsList();
for (Model model : models) {
    System.out.println(model);
}
```
{% endtab %}

{% tab title="gRPC NodeJS" %}
```javascript
// Insert here the initialization code as outlined on this page:
// https://docs.clarifai.com/api-guide/api-overview/api-clients#client-installation-instructions

stub.PostModelsSearches(
    {
        model_query: {
            name: "gen*",
            type: "concept"
        }
    },
    metadata,
    (err, response) => {
        if (err) {
            throw new Error(err);
        }

        if (response.status.code !== 10000) {
            throw new Error("Post model searches failed, status: " + response.status.description);
        }

        const models = response.models;
        for (const model of models) {
            console.log(JSON.stringify(model, null, 2));
        }
    }
);
```
{% endtab %}

{% tab title="gRPC Python" %}
```python
# Insert here the initialization code as outlined on this page:
# https://docs.clarifai.com/api-guide/api-overview/api-clients#client-installation-instructions

post_models_searches_response = stub.PostModelsSearches(
    service_pb2.PostModelsSearchesRequest(
        model_query=resources_pb2.ModelQuery(
            name="gen*",
            type="concept"
        )
    ),
    metadata=metadata
)

if post_models_searches_response.status.code != status_code_pb2.SUCCESS:
    raise Exception("Post models searches failed, status: " + post_models_searches_response.status.description)

for model in post_models_searches_response.models:
    print(model)
```
{% endtab %}

{% tab title="js" %}
```javascript
app.models.search('general-v1.3', 'concept').then(
  function(response) {
    // do something with response
  },
  function(err) {
    // there was an error
  }
);
```
{% endtab %}

{% tab title="python" %}
```python
from clarifai.rest import ClarifaiApp
app = ClarifaiApp(api_key='YOUR_API_KEY')

# search model name
app.models.search('general-v1.3')

# search model name and type
app.models.search(model_name='general-v1.3', model_type='concept')
```
{% endtab %}

{% tab title="java" %}
```java
client.findModel()
    .withModelType(ModelType.CONCEPT)
    .withName("general-v1.3")
    .getPage(1)
    .executeSync();
```
{% endtab %}

{% tab title="csharp" %}
```csharp
using System.Threading.Tasks;
using Clarifai.API;
using Clarifai.DTOs.Models;

namespace YourNamespace
{
    public class YourClassName
    {
        public static async Task Main()
        {
            var client = new ClarifaiClient("YOUR_API_KEY");

            await client.SearchModels(
                    modelType: ModelType.Concept,
                    name: "general-v1.3")
                .ExecuteAsync();
        }
    }
}
```
{% endtab %}

{% tab title="objective-c" %}
```text
[app searchForModelByName:@"general-v1.3" modelType:ClarifaiModelTypeConcept completion:^(NSArray<ClarifaiModel *> *models, NSError *error) {
    NSLog(@"models: %@", models);
}];
```
{% endtab %}

{% tab title="php" %}
```php
use Clarifai\API\ClarifaiClient;
use Clarifai\DTOs\Models\Model;
use Clarifai\DTOs\Models\ModelType;

$client = new ClarifaiClient('YOUR_API_KEY');

$response = $client->searchModels('general*', ModelType::concept())
    ->executeSync();

if ($response->isSuccessful()) {
    echo "Response is successful.\n";

    /** @var Model[] $models */
    $models = $response->get();

    foreach ($models as $model) {
        echo $model->name() . ' ' . $model->type() . "\n";
    }
} else {
    echo "Response is not successful. Reason: \n";
    echo $response->status()->description() . "\n";
    echo $response->status()->errorDetails() . "\n";
    echo "Status code: " . $response->status()->statusCode();
}
```
{% endtab %}

{% tab title="cURL" %}
```text
curl -X POST \
  -H "Authorization: Key YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '
  {
    "model_query": {
      "name": "gen*",
      "type": "concept"
    }
  }'\
  https://api.clarifai.com/v2/models/searches
```
{% endtab %}
{% endtabs %}

