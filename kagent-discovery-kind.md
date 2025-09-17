### kagent lab for use with kind

Create a new kind cluster
```
kind create cluster -n kagent-lab
```

Install metrics into the cluster
```
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml \
  && kubectl patch deployment metrics-server -n kube-system --type='json' -p='[{"op":"add","path":"/spec/template/spec/containers/0/args/-","value":"--kubelet-insecure-tls"}]'
```

Install kagent CLI:
```
export KAGENT_VERSION=0.6.12
curl -s https://raw.githubusercontent.com/kagent-dev/kagent/refs/heads/main/scripts/get-kagent | bash -s -- --version $KAGENT_VERSION
```

Install with the kagent CLI:
```
kagent install
```

Wait for all the pods to enter Running state
```
kubectl get pod -n kagent 
```

Use Port Forward to access the UI
```
kubectl -n kagent port-forward service/kagent-ui 8080:80 >/dev/null 2>&1 &
```

Access the Kagent UI with your Browser through [http://localhost:8080]{http://localhost:8080}

isit the kagent dashboard by clicking on the tab labeled "kagent Dashboard".
If presented with a wizard, click the link _Skip Wizard_ to access the home page.

Explore the user interface.
The main page displays a list of pre-built agents with different capabilities, with access to different sets of tools.
For example there is a Kubernetes agent, a Helm agent, an Istio agent, an observability agent, etc..

Agents interact with Models (LLMs) and Tools.
Through the user interface you can inspect and manage agents, models, and tools.

Under the _View_ menu, select and explore each of _Models_, then _Tools_, and _Tool Servers_ in turn, and otherwise develop some familiarity with the user interface.

Create an agent
===============

To better understand agents, let us create a custom agent that can answer questions about what is running on the cluster.

- Click _+ Create_, and select _New Agent_.
- Enter the following details:
  - Name: `my-kubernetes-agent`
  - Namespace: `kagent`
  - Agent type: select `Declarative`
  - Description: `My first Kubernetes agent`
  - Agent instructions: keep the default instructions.
  - Model: select `gpt-4.1-mini`

Next, select the tools we wish the agent to have access to:

- Click _+ Add Tools & Agents_, and add these tools:
  - helm_list_releases
  - helm_get_release
  - k8s_get_resources
  - k8s_get_available_api_resources

- Save your tools selection and finally, click _Create Agent_

You will be taken back to the agents page, and the new agent `my-kubernetes-agent` will be listed.
Wait a few seconds for the agent to become ready, refresh the page if necessary.

Interact with the agent
=======================

Once the agent is ready, click on it to interact with it.

Try some example prompts, such as:

- _Can you show me the namespaces that exist in the cluster?_
- _What Helm releases are running in my cluster?_
- _Tell me more about the helm release `kagent-crds`_

Note how the user interface presents "arguments" and "results" that represent the tool interactions that take place.
