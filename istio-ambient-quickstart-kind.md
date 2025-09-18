This is based on https://ambientmesh.io/docs/quickstart/.

Create a new kind cluster
```
kind create cluster -n istio-quickstart
```

Install metrics into the cluster
```
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml \
  && kubectl patch deployment metrics-server -n kube-system --type='json' -p='[{"op":"add","path":"/spec/template/spec/containers/0/args/-","value":"--kubelet-insecure-tls"}]'
```

Install latest istio (production installs with helm)
```
curl -sSL https://get.ambientmesh.io | bash -
```

Deploy sample application bookinfo
```
kubectl apply -f https://raw.githubusercontent.com/istio/istio/release-1.26/samples/bookinfo/platform/kube/bookinfo.yaml
```

Let's create an ingress gateway using Kubernetes Gateway API
```
kubectl apply -f https://raw.githubusercontent.com/istio/istio/release-1.26/samples/bookinfo/gateway-api/bookinfo-gateway.yaml
```

On kind, we need to switch to a ClusterIP type from LoadBalancer ( works on Cloud ) 
```
kubectl annotate gateway bookinfo-gateway networking.istio.io/service-type=ClusterIP --namespace=default
```

Check the gateway
```
kubectl get gateway
```

Enable access to the gateway with a port-forward
```
kubectl port-forward svc/bookinfo-gateway-istio 8080:80 >/dev/null 2>&1 &
```

In your web browser, open the productpage route: http://localhost:8080/productpage

Refresh the page a couple times. You’ll see the book reviews and ratings change as the Gateway load balances your requests across the different versions of the reviews service.

## Add Bookinfo to the mesh 

Adding applications to an ambient mesh is as simple as labeling the namespace where the application resides. By adding the applications to the mesh, you automatically secure the communication between them via mutual TLS (mTLS). No restart or redeployment of your applications are needed. As traffic is routed through your ambient mesh, TCP telemetry data is automatically collected for you.

```
kubectl label namespace default istio.io/dataplane-mode=ambient
```

## Verifying mTLS with observability tools

[Kiali](https://ambientmesh.io/docs/observability/#dashboards) is an observability console for Istio, and you can use it to verify that your traffic is indeed encrypted.

Installing the monitoring tools ( includes prometheus ) 
```
kubectl apply -f https://get.ambientmesh.io/monitoring.yaml
```

Create a port-forwarding to access kiali
```
kubectl port-forward svc/kiali 20001:20001 -n istio-system >/dev/null 2>&1 &
```

You can now access Kiali on localhost port 20001.

<img width="2429" height="1152" alt="Screenshot from 2025-09-18 13-46-45" src="https://github.com/user-attachments/assets/86f0a027-4a8a-48e2-b591-2f456279c28b" />



