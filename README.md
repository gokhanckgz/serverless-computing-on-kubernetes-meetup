## SERVERLESS COMPUTING ON KUBERNETES MEETUP

 - Presentation path -> Presentation/Serverless Computing on Kubernetes - Meetup.pptx

## Demo

##### Prerequisites

- Virtualbox or KVM driver
- Minikube
- Helm 

#### Minikube Start

- sudo systemctl start docker
- minikube start

#### Helm Installation

- ```kubectl -n kube-system create sa tiller && kubectl create clusterrolebinding tiller --clusterrole cluster-admin --serviceaccount=kube-system:tiller```

- ```helm init --skip-refresh --upgrade --service-account tiller```

- If you get error "helm init --skip-refresh --upgrade --service-account tiller" , you can use tiller.yaml under yamls folder (for compability, apiVersion changed to apps/v1 ve spec>selector>matchLabels> added)

#### Openfaas Installation

-   ```curl https://raw.githubusercontent.com/openfaas/faas-netes/master/namespaces.yml > namespaces.yaml``` 

- ``` kubectl apply -f namespaces.yaml ```

- ``` kubectl -n openfaas create secret generic basic-auth --from-literal=basic-auth-user=admin --from-literal=basic-auth-password=openfaas ```

- ``` helm repo update && helm upgrade openfaas --install openfaas/openfaas --namespace openfaas --set basic_auth=true --set functionNamespace=openfaas-fn --set faasIdler.dryRun=false ```

- If you want change inactivity_duration on faas-idler you can edit label via : ``` kubectl edit deploy faas-idler -n openfaas ```

#### Openfaas UI access

- ``` kubectl get svc -n openfaas gateway-external -o wide ```

- ``` kubectl get nodes -o wide ```   -> get ip address of node and acces via "(nodeIp):31112"

#### Openfaas-cli Installation

- ``` curl -sSL https://cli.openfaas.com | sudo sh ```

- ``` export OPENFAAS_URL="$(nodeIp):31112" ```

- ``` faas-cli login -u admin -p openfaas ```


#### Deploy Sample Application from Store

- ``` faas-cli store deploy figlet --label "com.openfaas.scale.zero=true" ```


#### Access Prometheus

- ``` kubectl port-forward deploy/prometheus -n openfaas 8080:9090 ```

- Access via ```localhost:8080```

#### Custom Functions Samples

- Custom functions samples under yamls/custom-fn folder (content copied from https://github.com/openfaas/faas)
