# Getting started with Kubernetes

## Types of kubernetes objects
- **Statefulset**:
- **Replicacontroller**:
- **Secret**: Types of secrets
    * generic 
    * docker-registry   
    * tls: tls keys
- **Deployment**: maintains a set of identical pods, ensuring they have the correct config and the right number of replicas
- **Pod**: run one or more closely related containers
- **Service**: setup networking in a Kubernetes cluster
    * ClusterIP: exposes a set of pod to other objects in the same cluster.
    * NodePort: exposing a set of pods to outside world (development only).
    * LoadBalancer:
    * Ingress:
- **Volume**: An object that allows a container to store consistent data at the pod level. Data survives container restart but not pod restart.
    * **PV (persistent volume)**: Data is not tied to a pod and it survives a pod restart .
    * **PVC (persistent volume claim)**: advertised volumes available in the cluster

## Types of PersistentVolumeClaim
* **Statically provisioned PV**: these are created ahead of time.
* **Dynamically provisioned PV**: these are created just in time when needed

## PersistentVolumeClaim access modes:
* **ReadWriteOnce**: Can be used by a single node
* **ReadOnlyMany**: Multiple nodes can read from this
* **ReadWriteMany**: Can be read and written to by many nodes
--

## Kubernetes
## Kubectl Commands
```bash
# Applying a config file
kubectl apply -f <file.yaml> | <folder> <-R (if using nested folder structure)>
# Inspecting a pod configuration
kubectl describe <object_type> <object_name:opt>
# Listing all resource of object type
kubectl get <object_type>
# Delete all resource for this object.
kubectl delete -f <config-file>
# Updating the image of a container in a deployment.
kubectl set image <object_type>/<object_name>  <container_name> = <new_image>
# Creating a secre
kubectl create secret generic <secret_name> --from-literal(from command) <key>=<value>
# Executing into a container
kubectl exec -it <container> sh

# Configuring docker on host to attach to the minikube docker-daemon
eval $(minikube -p minikube docker-env)
```
