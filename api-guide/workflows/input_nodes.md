# Input Nodes

The outputs from one model can be used as the inputs in another model. This allows you to link together the models in a graph. Linking models helps you build sophisticated AI solutions, that can zero-in on a specific use case.

## Supported input and output types

To view your available models, just open your app and click Model Mode icon on the left hand side of the screen. From here just click the Create a Custom Model button in the top righthand corner of the screen.

Different models accept different types of inputs and return different types of outputs. They are named after the fields in the Data object of our API. This object is uses in inputs, annotations, models and workflows. Some examples include:

#### Inputs

* Concepts
* Embeddings
* Image
* Image or video
* Regions

#### Outputs

* Concepts
* Clusters
* Regions

## The building blocks

You can create workflows out of any Clarifai Models or custom models that you have created for your app. The inputs and outputs supported by your custom models will depend on the inputs and outputs supported by the Clarifai Models, or model templates that you have used to build them.

### Sample workflow with multiple connected nodes

The The following is an example of how to build a workflow with multiple connected nodes. Note that model IDs and model version IDs from the public `clarifai/main` application are fixed, so they are already hard-coded in the code examples below. It is possible to use other public model or model version IDs.

{% tabs %}
{% tab title="gRPC Java" %}
```java
import com.clarifai.grpc.api.*;
import com.clarifai.grpc.api.status.*;

// Insert here the initialization code as outlined on this page:
// https://docs.clarifai.com/api-guide/api-overview/api-clients#client-installation-instructions

MultiWorkflowResponse postWorkflowsResponse = stub.postWorkflows(
  PostWorkflowsRequest.newBuilder()
      .setUserAppId(UserAppIDSet.newBuilder().setAppId("{YOUR_APP_ID}"))
      .addWorkflows(
          Workflow.newBuilder()
              .setId("auto-annotation-workflow-id")
              .addNodes(
                  WorkflowNode.newBuilder()
                      .setId("general-embed")
                      .setModel(
                          Model.newBuilder()
                              .setId("bbb5f41425b8468d9b7a554ff10f8581")
                              .setModelVersion(
                                  ModelVersion.newBuilder()
                                      .setId("bb186755eda04f9cbb6fe32e816be104")
                              )
                      )
              )
              .addNodes(
                  WorkflowNode.newBuilder()
                      .setId("general-concept")
                      .setModel(
                          Model.newBuilder()
                              .setId("aaa03c23b3724a16a56b629203edc62c")
                              .setModelVersion(
                                  ModelVersion.newBuilder()
                                      .setId("aa7f35c01e0642fda5cf400f543e7c40")
                              )
                      )
              )
              .addNodes(
                  WorkflowNode.newBuilder()
                      .setId("general-cluster")
                      .setModel(
                          Model.newBuilder()
                              .setId("cccbe437d6e54e2bb911c6aa292fb072")
                              .setModelVersion(
                                  ModelVersion.newBuilder()
                                      .setId("cc2074cff6dc4c02b6f4e1b8606dcb54")
                              )
                      )
              )
              .addNodes(
                  WorkflowNode.newBuilder()
                      .setId("mapper")
                      .setModel(
                          Model.newBuilder()
                              .setId("synonym-model-id")
                              .setModelVersion(
                                  ModelVersion.newBuilder()
                                      .setId("{YOUR_SYNONYM_MODEL_VERSION_ID}")
                              )
                      )
                      .addNodeInputs(NodeInput.newBuilder().setNodeId("general-concept"))
              )
              .addNodes(
                  WorkflowNode.newBuilder()
                      .setId("greater-than")
                      .setModel(
                          Model.newBuilder()
                              .setId("greater-than-model-id")
                              .setModelVersion(
                                  ModelVersion.newBuilder()
                                      .setId("{YOUR_GREATER_THAN_MODEL_VERSION_ID}")
                              )
                      )
                      .addNodeInputs(NodeInput.newBuilder().setNodeId("mapper"))
              )
              .addNodes(
                  WorkflowNode.newBuilder()
                      .setId("less-than")
                      .setModel(
                          Model.newBuilder()
                              .setId("less-than-model-id")
                              .setModelVersion(
                                  ModelVersion.newBuilder()
                                      .setId("{YOUR_LESS_THAN_MODEL_VERSION_ID}")
                              )
                      )
                      .addNodeInputs(NodeInput.newBuilder().setNodeId("mapper"))
              )
      )
      .build()
);

if (postWorkflowsResponse.getStatus().getCode() != StatusCode.SUCCESS) {
    throw new RuntimeException("Post workflows failed, status: " + postWorkflowsResponse.getStatus());
}
```
{% endtab %}

{% tab title="gRPC NodeJS" %}
```javascript
// Insert here the initialization code as outlined on this page:
// https://docs.clarifai.com/api-guide/api-overview/api-clients#client-installation-instructions

stub.PostWorkflows(
    {
        user_app_id: {
            app_id: "e83440590d104cee97ef84af1856837d"
        },
        workflows: [
            {
                id: "auto-annotation-workflow-id",
                nodes: [
                    {
                        id: "general-embed",
                        model: {
                            id: "bbb5f41425b8468d9b7a554ff10f8581",
                            model_version: {
                                id: "bb186755eda04f9cbb6fe32e816be104"
                            }
                        }
                    },
                    {
                        id: "general-concept",
                        model: {
                            id: "aaa03c23b3724a16a56b629203edc62c",
                            model_version: {
                                id: "aa7f35c01e0642fda5cf400f543e7c40"
                            }
                        }
                    },
                    {
                        id: "general-cluster",
                        model: {
                            id: "cccbe437d6e54e2bb911c6aa292fb072",
                            model_version: {
                                id: "cc2074cff6dc4c02b6f4e1b8606dcb54"
                            }
                        }
                    },
                    {
                        id: "mapper",
                        model: {
                            id: "synonym-model-id",
                            model_version: {
                                id: "{YOUR_SYNONYM_MODEL_VERSION_ID}"
                            }
                        },
                        node_inputs: [
                            {node_id: "general-concept"}
                        ]
                    },
                    {
                        id: "greater-than",
                        model: {
                            id: "greater-than-model-id",
                            model_version: {
                                id: "{YOUR_GREATER_THAN_MODEL_VERSION_ID}"
                            }
                        },
                        node_inputs: [
                            {node_id: "mapper"}
                        ]
                    },
                    {
                        id: "less-than",
                        model: {
                            id: "less-than-model-id",
                            model_version: {
                                id: "{YOUR_LESS_THAN_MODEL_VERSION_ID}"
                            }
                        },
                        node_inputs: [
                            {node_id: "mapper"}
                        ]
                    },
                ]
            }
        ]
    },
    metadata,
    (err, response) => {
        if (err) {
            throw new Error(err);
        }

        if (response.status.code !== 10000) {
            console.log(response.status);
            throw new Error("Post workflows failed, status: " + response.status.description);
        }
    }
);
```
{% endtab %}

{% tab title="gRPC Python" %}
```python
# Insert here the initialization code as outlined on this page:
# https://docs.clarifai.com/api-guide/api-overview/api-clients#client-installation-instructions

post_workflows_response = stub.PostWorkflows(
    service_pb2.PostWorkflowsRequest(
        user_app_id=resources_pb2.UserAppIDSet(
            app_id="cdd79189eb6f44049b6c5b58f14a87e6"
        ),
        workflows=[
            resources_pb2.Workflow(
                id="auto-annotation-workflow-id",
                nodes=[
                    resources_pb2.WorkflowNode(
                        id="general-embed",
                        model=resources_pb2.Model(
                            id="bbb5f41425b8468d9b7a554ff10f8581",
                            model_version=resources_pb2.ModelVersion(
                                id="bb186755eda04f9cbb6fe32e816be104"
                            )
                        )
                    ),
                    resources_pb2.WorkflowNode(
                        id="general-concept",
                        model=resources_pb2.Model(
                            id="aaa03c23b3724a16a56b629203edc62c",
                            model_version=resources_pb2.ModelVersion(
                                id="aa7f35c01e0642fda5cf400f543e7c40"
                            )
                        )
                    ),
                    resources_pb2.WorkflowNode(
                        id="general-cluster",
                        model=resources_pb2.Model(
                            id="cccbe437d6e54e2bb911c6aa292fb072",
                            model_version=resources_pb2.ModelVersion(
                                id="cc2074cff6dc4c02b6f4e1b8606dcb54"
                            )
                        ),
                    ),
                    resources_pb2.WorkflowNode(
                        id="mapper",
                        model=resources_pb2.Model(
                            id="synonym-model-id",
                            model_version=resources_pb2.ModelVersion(
                                id="{YOUR_SYNONYM_MODEL_VERSION_ID}"
                            )
                        ),
                        node_inputs=[
                            resources_pb2.NodeInput(node_id="general-concept")
                        ]
                    ),
                    resources_pb2.WorkflowNode(
                        id="greater-than",
                        model=resources_pb2.Model(
                            id="greater-than-model-id",
                            model_version=resources_pb2.ModelVersion(
                                id="{YOUR_GREATER_THAN_MODEL_VERSION_ID}"
                            )
                        ),
                        node_inputs=[
                            resources_pb2.NodeInput(node_id="mapper")
                        ]
                    ),
                    resources_pb2.WorkflowNode(
                        id="less-than",
                        model=resources_pb2.Model(
                            id="less-than-model-id",
                            model_version=resources_pb2.ModelVersion(
                                id="{YOUR_LESS_THAN_MODEL_VERSION_ID}"
                            )
                        ),
                        node_inputs=[
                            resources_pb2.NodeInput(node_id="mapper")
                        ]
                    ),
                ]
            )
        ]
    ),
    metadata=metadata
)

if post_workflows_response.status.code != status_code_pb2.SUCCESS:
    raise Exception("Post workflows failed, status: " + post_workflows_response.status.description)
```
{% endtab %}

{% tab title="cURL" %}
```text
curl -X POST 'https://api.clarifai.com/v2/users/me/apps/{{app}}/workflows' \
    -H 'Authorization: Key {{PAT}}' \
    -H 'Content-Type: application/json' \
    --data-raw '{
        "workflows": [
            {
                "id": "auto-annotation-workflow-id",
                "nodes": [
                    {
                        "id": "general-embed",
                        "model": {
                            "id": "{YOUR_GENERAL_EMBED_MODEL_ID}",
                            "model_version": {
                                "id": "{YOUR_GENERAL_EMBED_MODEL_VERSION_ID}"
                            }
                        }
                    },
                    {
                        "id": "general-concept",
                        "model": {
                            "id": "{YOUR_GENERAL_CONCEPT_MODEL_ID}",
                            "model_version": {
                                "id": "{YOUR_GENERAL_CONCEPT_MODEL_VERSION_ID}"
                            }
                        }
                    },
                    {
                        "id": "general-cluster",
                        "model": {
                            "id": "{YOUR_GENERAL_CLUSTER_MODEL_ID}",
                            "model_version": {
                                "id": "{YOUR_GENERAL_CLUSTER_MODEL_VERSION_ID}"
                            }
                        }
                    },
                    {
                        "id": "mapper",
                        "model": {
                            "id": "synonym-model-id",
                            "model_version": {
                                "id": "{YOUR_MAPPER_MODEL_VERSION_ID}"
                            }
                        },
                        "node_inputs": [
                            {
                                "node_id": "general-concept"
                            }
                        ]
                    },
                    {
                        "id": "greater-than",
                        "model": {
                            "id": "greater-than-model-id",
                            "model_version": {
                                "id": "{YOUR_GREATER_THAN_MODEL_VERSION_ID}"
                            }
                        },
                        "node_inputs": [
                            {
                                "node_id": "mapper"
                            }
                        ]
                    },
                    {
                        "id": "less-than",
                        "model": {
                            "id": "less-than-model-id",
                            "model_version": {
                                "id": "{YOUR_LESS_THAN_MODEL_VERSION_ID}"
                            }
                        },
                        "node_inputs": [
                            {
                                "node_id": "mapper"
                            }
                        ]
                    },
                ]
            }
        ]
    }'
```
{% endtab %}
{% endtabs %}

