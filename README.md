# AzureAKSIngress
Deploying applications in MS Azure AKS using Ingress <br/><br/>
* Clone the repository and navigate to the folder lab-05 <br/>
* Open powershell in administrator mode and login with below command. If you have multiple subscriptions then select the correct subcription by second command <br/>
  $ az login --use-device <br/>
  $ az account set --subscription <<-subsctption name/id->> (optional if you have multiple subcriptions) <br/>
* Create resource group "aksdemo". <br/>
  $ az group create --name aksdemo --location southeastasia <br/>
* Create an AKS cluster with 2 nodes (if you want to attach ACR then add --attach-acr <acrName>) <br/>
  $ az aks create --name aksdemo -g aksdemo --node-count 2 --generate-ssh-keys <br/>
* Switch context. (To configure kubectl to connect to your Kubernetes cluster, use the az aks get-credentials command). <br/>
  $ az aks get-credentials -n aksdemo -g aksdemo <br/>
* Refer below commands for verification of contexts.<br/>
  $ kubectl config view <br/>
  $ kubectl config current-context (output should be aksdemo). <br/>
  $ kubectl config get contexts <br/>
* Check nodes and pods in your AKS cluster. <br/>
  $ kubectl get nodes -o wide <br/>
  $ kubectl get pods <br/>
* Add a NGINX ingress controller without customizing the defaults. <br/>
  $ helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx <br/>
  $ helm repo update <br/>
  $ helm install nginx-ingress ingress-nginx/ingress-nginx --namespace ingress-basic --set controller.replicaCount=2 --set controller.nodeSelector."beta\.kubernetes\.io/os"=linux --set defaultBackend.nodeSelector."beta\.kubernetes\.io/os"=linux
* Verify NGINX controller installation <br/>
  $ kubectl get pods -n ingress-basic -l app.kubernetes.io/name=ingress-nginx --watch <br/>
* Inspect the ingress controller Service <br/>
  $ kubectl get svc nginx-ingress-ingress-nginx-controller -n ingress-basic <br/><br/>
  
  #### Deploy the application (s)
* Deploy the cats application (pod and ClusterIP service) <br/>
  $ kubectl apply -f cats.yaml <br/>
* Deploy the "Dogs" application (pod and ClusterIP service) <br/>
  $ kubectl apply -f dogs.yaml <br/>
* Deploy the "Birds" application (pod and ClusterIP service) <br/> 
  $ kubectl apply -f birds.yaml <br/>
* Create the ingress resource <br/>
  $ kubectl apply -f ingress.yaml <br/>
* List existing pods & services <br/>
  $ kubectl get pods <br/>
  $ kubectl get svc <br/>
* List existing ingress <br/>
  $ kubectl get ingress <br/>
  <br/>
#### Access the application <br/>
* Get the ingress controller External IP <br/>
  $ kubectl get svc ingress-nginx-controller -n ingress-basic <br/>
* Browse to the cats, dogs and birds service <br/>
  $ http://<<-ingress-controller-external-ip->>/cats <br/>
  $ http://<<-ingress-controller-external-ip->>/dogs <br/>
  $ http://<<-ingress-controller-external-ip->>/birds <br/>
* Clean up resources <br/>
  $ kubectl get all <br/>
  $ kubectl delete ingress --all <br/>
  $ kubectl delete all --all -n ingress-basic <br/>
  $ kubectl delete namespace ingress-basic <br/>
* List existing resources <br/>
  $ kubectl get all <br/>
* Clean your Azure enviornment. Other than "aksdemo" there will be an autogenerated resource group created which also needs to be deleted <br/>
  $ az group delete --name aksdemo --yes -y <br/>
  $ az group delete --name <<-resource group name->> --yes -y <br/>
  

  
  
  

