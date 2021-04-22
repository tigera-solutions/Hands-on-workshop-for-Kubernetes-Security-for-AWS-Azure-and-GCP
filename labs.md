## 1. Getting a workshop environment (pre-workshop)
#### 1. Setup minikube (example for Mac)
```
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-darwin-amd64
sudo install minikube-darwin-amd64 /usr/local/bin/minikube

mkdir tigeraworkshop
cd tigeraworkshop
minikube start --network-plugin=cni --cni=calico --memory=4096 --driver=hyperkit
```

#### 2. Verify minikube and calico is working
```
kubectl get no
kubectl get pods -l k8s-app=calico-node -n kube-system
```
## 2. Connecting your cluster to Calico Cloud (Lab1)
#### 1. Connect your cluster
```
 curl https://installer.calicocloud.io/xxxxxx_yyyyyyy-saay-management_install.sh | bash
```
Wait until installing the Calico Cloud components is finishing. You can see the progress in your terminal window. It can take up to a few minutes.

## 3. Accessing your Calico Cloud environment and setting up a demo application (Lab2)
#### 1. Adjust timing (faster logs -- cool for demo, not for production)
```
kubectl patch felixconfiguration.p default -p '{"spec":{"flowLogsFlushInterval":"10s"}}'
kubectl patch felixconfiguration.p default -p '{"spec":{"dnsLogsFlushInterval":"10s"}}'
kubectl patch felixconfiguration.p default -p '{"spec":{"flowLogsFileAggregationKindForAllowed":1}}'
```
#### 2. Login into your Calico Cloud
Connect to Calico Cloud using the Service Connect URL that has been assigned.
Use the service account token to login.

#### 3. Setup a demo application
```
kubectl apply -f https://installer.calicocloud.io/storefront-demo.yaml
```
#### 4. Clone the repository
```
git clone https://github.com/tigera-solutions/Hands-on-workshop-for-Kubernetes-Security-for-AWS-Azure-and-GCP.git
cd Hands-on-workshop-for-Kubernetes-Security-for-AWS-Azure-and-GCP
```
## 4. Dashboard quickstart (Lab3)
Explore the calicocloud user interface. (https://docs.calicocloud.io/get-started/tour). 

#### 1. Service graph
![alt text](https://docs.calicocloud.io/images/service-graph4.png)

#### 2. Policy Dashboard
![alt text](https://docs.calicocloud.io/images/policy-filters.png)

#### 3. Flow visualizer
![alt text](https://docs.calicocloud.io/images/flow-viz.png)

## 5. Security polices 

#### 1. Policy recommendation (LAB4)
Create a policy by using the Policy Recommendation option in CalicoCloud. From the Policy Dashboard, select the Policy Recommendation icon. Select the namespace `storefront` and pod `frontend-...` and click recommend. To verify the impact of the policy, click `stage`. Use the Service Graph or Flow visualizer to see the impact of the rule. When you are confident that this rule is correct, we can `enforce` the rule. 

#### 2. Apply a tiered network policy (LAB5)
```
kubectl apply -f ./networkpolicy/
```
#### 3. Deploy a rogue pod (LAB6)
```
kubectl apply -f https://installer.calicocloud.io/rogue-demo.yaml
```
#### 4. Verify the impact of the pod

#### 5. Create a quarantine rule (LAB7)
* insert a new tier called `security` before the `storefront` tier
* create a new policy called `quarantine`
* set the scope to `global`
* Set the Apply to `quarantine=true` (you need to create the key and value)
* create  an ingress rule `Action Log, Match All Endpoints`
* create  an ingress rule `Action Deny, Match All Endpoints`
* create  an egress rule `Action Log, Match All Endpoints`
* create  an egress rule `Action Deny, Match All Endpoints`
* `stage` this rule and verify that it is not blocking any valid traffic (there should be no endpoints that are affected by this rule)
* `enforce` the rule

#### 6. Apply the quarantine label
```
kubectl label po attacker-app-5f8d5574bf-tqnjf quarantine=true
```
## DNS policies (LAB8)
#### Create a DNS policy to limit access to *.tigera.io
*  insert a new rule after `zone-kubernetes-dns` called `api-access`
*  Set the scope to Namespace `storefront`
*  Set Apply to `fw-zone=internet`
*  uncheck the ingress checkbox
*  check the egress checkbox
*  Create an Egress rule 
      * Allow is Protocol is `TCP`
      * port `80` and `443` 
      * egress domain `*.tigera.io`
* Stage / Enforce the rule

#### Create a pod to test the DNS policy
```
kubectl run -it --rm --image xxradar/hackon -n storefront -l fw-zone=internet -- bash
```
Verify that the rule is applied to the pod. Inside the pod
```
dig www.tigera.io
...
dig docs.projectcalico.org
```
Verify the DNS egress rule inside the pod
```
curl https://www.tigera.io
...
curl https://docs.projectcalico.org
```
## Interesting dashboard (LAB9)
Go to the Kibana shortcut and explore the different dashboards.
#### 1.Kibana logs and dashboards
![alt text](https://docs.calicocloud.io/images/kibana-logs.png)


 
