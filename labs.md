## 1. Getting a workshop environment
### 1. Setup minikube
```
mkdir tigeraworkshop
cd tigeraworkshop
minikube start --network-plugin=cni --cni=calico --memory=4096
```

### 1. Verify calico is working
```
kubectl get no
kubectl get pods -l k8s-app=calico-node -n kube-system
```

### 1. Connect your cluster
```
 curl https://installer.calicocloud.io/xxxxxx_yyyyyyy-saay-management_install.sh | bash
```
### 1. Adjust timing (faster logs -- cool for demo, not for production)
```
kubectl patch felixconfiguration.p default -p '{"spec":{"flowLogsFlushInterval":"10s"}}'
kubectl patch felixconfiguration.p default -p '{"spec":{"flowLogsFileAggregationKindForAllowed":1}}'
```
### 1. Setup a demo application
```
kubectl apply -f https://installer.calicocloud.io/storefront-demo.yaml
```
## 2. Dashboard quickstart

### 1. Service graph

### 2. Policy recommendation 

