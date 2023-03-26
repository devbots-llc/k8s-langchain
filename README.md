# A [LangChain Agent](https://github.com/hwchase17/langchain) for interacting with Kubernetes Clusters using Large Language Models

## Capabilities
  - Connect to a remote GKE Cluster
  - Supports most of core, apps, batch, networking, and rbac API groups
    - Specifically: `configmap, namespace, persistentvolume, persistentvolumeclaim, pod ,secret, serviceaccount, service, node, daemonset, deployment, replicaset, statefulset, job, cronjob, ingress, clusterrole, clusterrolebinding, role, rolebinding`
  - List namespaces
  - List object names by type in a namespace
  - Get a resource by name, type, and namespace (if applicable)
  
## Required Env Vars
| Name | Description |
|----------------------------|-----------------------------|
| OPENAI_API_KEY | API Key for OpenAI API |
| OPENAI_ORG_ID | Organization ID for OpenAI API |
| GOOGLE_PROJECT_ID | Project ID for GCP |
| GOOGLE_REGION | Region for GCP |
| GOOGLE_CLUSTER_ID | Cluster Name for GCP |

This assumes that you are running locally and are able to authenticate by running:
```bash
gcloud auth application-default login
```

## Example Output
```python
k8s_agent.run("get the gitlab runner deployment in the gitlab-runner namespace")
```

```bash
> Entering new AgentExecutor chain...
Action: k8s_get_object_names
Action Input: "gitlab-runner,deployment"
Observation: gitlab-runner
Thought:I should determine the operation I need to perform.
Action: k8s_determine_operations
Action Input: "deployment"
Observation: create,read,list,update,delete
Thought:I need to perform a get operation on the deployment.
Action: k8s_get_resource
Action Input: "gitlab-runner,deployment,gitlab-runner"
Observation: api_version: apps/v1
kind: Deployment
metadata:
  annotations:
    deployment.kubernetes.io/revision: '3'
    meta.helm.sh/release-name: gitlab-runner
    meta.helm.sh/release-namespace: gitlab-runner
  creation_timestamp: 2023-03-06 02:04:43+00:00
  generation: 4
  labels:
    app: gitlab-runner
    app.kubernetes.io/managed-by: Helm
    chart: gitlab-runner-0.50.1
    heritage: Helm
    release: gitlab-runner
  name: gitlab-runner
  namespace: gitlab-runner
spec:
  progress_deadline_seconds: 600
  revision_history_limit: 10
...
```

