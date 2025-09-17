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
