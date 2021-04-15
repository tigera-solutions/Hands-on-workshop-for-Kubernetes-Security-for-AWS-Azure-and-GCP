## 1. Getting a workshop environment (pre-workshop)
#### 1. Setup minikube (example for Mac)
```
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-darwin-amd64
sudo install minikube-darwin-amd64 /usr/local/bin/minikube

mkdir tigeraworkshop
cd tigeraworkshop
minikube start --network-plugin=cni --cni=calico --memory=4096
```

#### 2. Verify minikube and calico is working
```
kubectl get no
kubectl get pods -l k8s-app=calico-node -n kube-system
```
## 2. Connecting your cluster to CalicoCloud
#### 1. Connect your cluster
```
 curl https://installer.calicocloud.io/xxxxxx_yyyyyyy-saay-management_install.sh | bash
```
#### 2. Adjust timing (faster logs -- cool for demo, not for production)
```
kubectl patch felixconfiguration.p default -p '{"spec":{"flowLogsFlushInterval":"10s"}}'
kubectl patch felixconfiguration.p default -p '{"spec":{"flowLogsFileAggregationKindForAllowed":1}}'
```
#### 3. Setup a demo application
```
kubectl apply -f https://installer.calicocloud.io/storefront-demo.yaml
```
#### 4. Clone the repository
```
git clone https://github.com/tigera-solutions/Hands-on-workshop-for-Kubernetes-Security-for-AWS-Azure-and-GCP.git
cd Hands-on-workshop-for-Kubernetes-Security-for-AWS-Azure-and-GCP
```
## 3. Dashboard quickstart

#### 1. Service graph

## 4. Security polices 

#### 1. Policy recommendation 

#### 2. Apply a tiered network policy
```
kubectl apply -f ./networkpolicy/
```
#### 3. Deploy a rogue pod
```
kubectl apply -f https://installer.calicocloud.io/rogue-demo.yaml
```
#### 4. Verify the impact of the pod

#### 5. Create a quarantine rule
* insert a new tier called `security` before the `storefront` tier
* create a new policy called `quarantine`
* set the scope to `global`
* Set the Apply to `qurantine=true` (you need to create the key and value)
* create  an ingress rule `Action Log, Match All Endpoints`
* create  an ingress rule `Action Deny, Match All Endpoints`
* create  an egress rule `Action Log, Match All Endpoints`
* create  an egress rule `Action Deny, Match All Endpoints`

#### 6. Apply the quarantine label
```
kubectl label po attacker-app-5f8d5574bf-tqnjf quarantine=true
```
## DNS policies
#### Create a DNS policy to limit access to *.tigera.io
*  insert a new rule after `zone-kubernetes-dns` called `api-access`
*  Set the scope to Namespace `storefront`
*  Set Apply to `fw-zone=internet`
*  Create an Egress rule 
      Allow is Protocol is `TCP`
      port `80` and `443` 
      egress domain t `*.tigera.io`
    
