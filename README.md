# Kubernetes

Kubernetes docs (RU)
https://kubernetes.io/ru/docs/concepts/overview/what-is-kubernetes/

Minikube - training single Kubernetes node
https://minikube.sigs.k8s.io/docs/

Katacoda lessons
https://www.katacoda.com/

Free play wih Kubernetes
https://training.play-with-kubernetes.com/
https://labs.play-with-k8s.com/

RedHat Kubernetes lessons
https://developers.redhat.com/topics/kubernetes

Kubespray
https://github.com/kubernetes-sigs/kubespray

Helm - container registry
https://helm.sh/

```bash

# minikube

minikube version
minikube start
# start with custom name
minikube start -p my-cluster-name
# start master node with parameters
minikube start --cpus=4 --memory=8gb --disk-size=30gb
minikube start --cpus=4 --memory=8000mb --disk-size=30000mb
# ssh to master node as user docker, pass tcuser (root without pass)
minikube ssh

minikube stop
minikube delete
minikube delete --all

# kubectl

# version client + server
kubectl version
# version client
kubectl version --client
# cluster info
kubectl cluster-info
# cluster state
kubectl get componentstatuses
# all cluster servers
kubectl get nodes
kubectl get namespaces

# pods

# get pods from default namespace
kubectl get pods
# get pods from all namespaces
kubectl get pods -A
# get pods from specific namespace
kubectl get pods --namespace=default
# get full pod info
kubectl describe pods <pod-name> [--namespace=default]
kubectl describe pods hello-minikube

# run pod
kubectl run <pod-name> --image=<image> --port=8080
kubectl run hello-minikube --image=k8s.gcr.io/echoserver:1.4 --port=8080

# execute command in pod
kubectl exec <pod-name> -- <command>
kubectl exec hello-minikube -- date
# execute command inside pod
kubectl exec -it hello-minikube -- sh

# see/follow pod logs
kubectl logs <pod-name> [--follow --namespace=default]
kubectl logs hello-minikube
kubectl logs --follow --namespace=default hello-minikube

# expose port from pod
kubectl port-forward <pod-name> <host-port:pod-port>
kubectl port-forward hello-minikube 3000:8080

# delete pod
kubectl delete pods <pod-name>
kubectl delete pods hello-minikube

# deployments

# get deployments from default namespace
kubectl get deployments
kubectl get deploy
# get deployments from all namespaces
kubectl get deployments -A
# get deployments from specific namespace
kubectl get deployments --namespace=default
# get full deployment info
kubectl describe deployments <deployment-name>
kubectl describe deployments hello-minikube

# get replica-set info
kubectl get rs
# get horizontal pod autoscaler info
kubectl get hpa
# get rollout info - status / history
kubectl rollout status deployment/<deployment-name>
kubectl rollout status deployment/hello-minikube
kubectl rollout history deployment/<deployment-name>
kubectl rollout history deployment/hello-minikube

# create deployment - deploy apps
kubectl create deployment <deployment-name> --image=host/name:latest
kubectl create deployment hello-minikube --image=k8s.gcr.io/echoserver:1.4

# defined scale deployment
kubectl scale deployment <deployment-name> --replicas 2
kubectl scale deployment hello-minikube --replicas 2
# autoscale deployment
kubectl autoscale deployment <deployment-name> --min=2 --max=4 --cpu-percent=80
kubectl autoscale deployment hello-minikube --min=2 --max=4 --cpu-percent=80

# update deployment with new image
kubectl set image deployment/<deployment-name> <container-name>=<image> --record
kubectl set image deployment/hello-minikube echoserver=k8s.gcr.io/echoserver:1.4 --record
# update deployment with current version of image (useful for 'latest' - pull image again)
kubectl rollout restart deployment/<deployment-name>
kubectl rollout restart deployment/hello-minikube

# rollback to prev version (from history)
kubectl rollout undo deployment/<deployment-name>
kubectl rollout undo deployment/hello-minikube
# rollback to custom version (from history)
kubectl rollout undo deployment/<deployment-name> --to-revision=<history-revision-id>
kubectl rollout undo deployment/hello-minikube --to-revision=4

# services
# ClusterIP - internal, inside k8s cluster only
# NodePort - external, port for all worker nodes
# ExternalName - external, DNS CNAME Record
# LoadBalancer - in cloud clusters (AWS, Azure, GCP)

# get services from default namespace
kubectl get services
kubectl get svc
# get services from all namespaces
kubectl get services -A
# get services from specific namespace
kubectl get services --namespace=default
# get full service info
kubectl describe services <service-name>
kubectl describe services hello-minikube

# create service - expose
kubectl expose deployment <deployment-name> --type=<service-type> --port=<port>
kubectl expose deployment hello-minikube --type=ClusterIP --port=8080
kubectl expose deployment hello-minikube --type=NodePort --port=8080
kubectl expose deployment hello-minikube --type=LoadBalancer --port=8080

# delete service
kubectl delete services <service-name>
kubectl delete services hello-minikube

# files - *.yaml / *.yml

# run pod / deployment from file
kubectl apply -f <file-name>
kubectl apply -f  hello-minikube.yml

# delete pod / deployment / service from file
kubectl delete -f <file-name>
kubectl delete -f  hello-minikube.yml

# nodes

# get nodes
kubectl get nodes
# get full node info
kubectl describe nodes <node-name>
kubectl describe nodes minikube

```
