## 1. Getting a workshop environment
### 1. Setup minikube
```
mkdir tigeraworkshop
cd tigeraworkshop
minikube start --network-plugin=cni --cni=calico --memory=4096
```

### 2. Verify calico is working
```
kubectl get no
kubectl get pods -l k8s-app=calico-node -n kube-system
```

### 2. Connect your cluster
```
 curl https://installer.calicocloud.io/xxxxxx_yyyyyyy-saay-management_install.sh | bash
```
### 3. Adjust timing (faster logs -- cool for demo, not for production)
```
kubectl patch felixconfiguration.p default -p '{"spec":{"flowLogsFlushInterval":"10s"}}'
kubectl patch felixconfiguration.p default -p '{"spec":{"flowLogsFileAggregationKindForAllowed":1}}'
```
### 4. Setup a demo application
```
kubectl apply -f https://installer.calicocloud.io/storefront-demo.yaml
```
## 2. Dashboard quickstart

### 1. Service graph

### 2. Policy recommendation 

