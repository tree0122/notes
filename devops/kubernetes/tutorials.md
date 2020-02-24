### 学习过程

#### 2020-02-24

####1. [Create a Kubernetes Cluster](https://kubernetes.io/docs/tutorials/kubernetes-basics/create-cluster/cluster-intro/)
> **minikube version**: check minikube is installed  
> **minikube start**: start a cluster  
> **kubectl version**: check kubectl is installed   
> **kubectl cluster-info**: view cluster detail  
> **kubectl get nodes**: show all nodes in this cluster

####2. [Deploy an App](https://kubernetes.io/docs/tutorials/kubernetes-basics/deploy-app/deploy-intro/)
> **kubectl version**: check kubectl is installed   
> **kubectl create deployment <name> <--image=${image}>**: create a deployment   
> **kubectl get deployments**: show deployments in this cluster   
> **kubectl proxy**: enables direct access to the API from  terminals 

####3. [Exploring Your App](https://kubernetes.io/docs/tutorials/kubernetes-basics/deploy-app/deploy-intro/)
> **kubectl get pods**: look for existing pods  
> **kubectl describe pods**: view container and image inside the pod  
> **kubectl logs $POD_NAME**: retrieve logs  
> **kubectl exec $POD_NAME <cmdInContainer>**: execute commands directly on the container  

####4. [Expose App publicly](https://kubernetes.io/docs/tutorials/kubernetes-basics/expose/expose-intro/)
> **kubectl get services**: list the current Services  
> **kubectl expose deployment --type="NodePort" --port 8080**: create a new service and expose it to external traffic  
> **kubectl describe <services/kubernetes-bootcamp>**: find service detail  
> **kubectl get pods -l <run=kubernetes-bootcamp>(label)**: find pod detail with label  
> **kubectl get services -l <run=kubernetes-bootcamp>(label)**: find service detail with label  
> **kubectl label pod $POD_NAME app=v1**: add label to pod   
> **kubectl delete service -l run=kubernetes-bootcamp**: delete service with label   

####5. [Scale up App](https://kubernetes.io/docs/tutorials/kubernetes-basics/scale/scale-intro/)
> **kubectl get deployments**: list the current deployments  
> **kubectl get rs**: see the ReplicaSet created by the Deployment  
> **kubectl scale deployments/kubernetes-bootcamp --replicas=4**: scale the Deployment to 4 replicas  
> **kubectl get pods -o wide**: find pod detail  

####6. [Update App](https://kubernetes.io/docs/tutorials/kubernetes-basics/update/update-intro/)
> **kubectl describe pods**: view the current image version of the app  
> **kubectl set image deployments/kubernetes-bootcamp kubernetes-bootcamp=jocatalin/kubernetes-bootcamp:v2**: update the image of the application to version 2  
> **kubectl rollout status deployments/kubernetes-bootcamp**: The update can be confirmed  
> **kubectl rollout undo deployments/kubernetes-bootcamp**: roll back to our previously working version  

