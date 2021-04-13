## 1. Getting a workshop environment (pre-workshop)
### 1. Setup minikube
```
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-darwin-amd64
sudo install minikube-darwin-amd64 /usr/local/bin/minikube

mkdir tigeraworkshop
cd tigeraworkshop
minikube start --network-plugin=cni --cni=calico --memory=4096
```

### 2. Verify calico is working
```
kubectl get no
kubectl get pods -l k8s-app=calico-node -n kube-system
```
## 2. Connecting your cluster to CalicoCloud
### 1. Connect your cluster
```
 curl https://installer.calicocloud.io/xxxxxx_yyyyyyy-saay-management_install.sh | bash
```
### 2. Adjust timing (faster logs -- cool for demo, not for production)
```
kubectl patch felixconfiguration.p default -p '{"spec":{"flowLogsFlushInterval":"10s"}}'
kubectl patch felixconfiguration.p default -p '{"spec":{"flowLogsFileAggregationKindForAllowed":1}}'
```
### 3. Setup a demo application
```
kubectl apply -f https://installer.calicocloud.io/storefront-demo.yaml
```
### 4. Clone the repository
```
git clone https://github.com/tigera-solutions/Hands-on-workshop-for-Kubernetes-Security-for-AWS-Azure-and-GCP.git
cd Hands-on-workshop-for-Kubernetes-Security-for-AWS-Azure-and-GCP
```
## 3. Dashboard quickstart

### 1. Service graph

### 2. Policy recommendation 

### 3. Apply a tiered network policy
```
kubectl apply -f ./networkpolicy/
```
### 4. Deploy a rogue pod
```
kubectl apply -f https://installer.calicocloud.io/rogue-demo.yaml
```
### 5. Verify the impact of the pod

### 6. Create a quarantine rule

### 7. Apply the qurantine label
```
kubectl label po attacker-app-5f8d5574bf-tqnjf quarantine=true
```

