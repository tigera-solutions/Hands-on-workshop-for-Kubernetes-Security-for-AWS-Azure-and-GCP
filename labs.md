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

## 2. Connect your cluster
```
 curl https://installer.calicocloud.io/79bbbkkkg_radarhackcom-saay-management_install.sh | bash
```
## 3. Setup a demo application
```
kubectl apply -f https://installer.calicocloud.io/storefront-demo.yaml
```