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
    * NodePort: exposes a set of pods to outside world (development only).
    * LoadBalancer (Legacy): Allows network traffic into a cluster (give access to a specific set of pods and also sets up cloud-provider loadbalancer).
    * Ingress: exposes a set of services to the outside world.
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

## Kubectl Commands
```bash
# Applying a config file
kubectl apply -f <file.yaml> | <folder> <-R (if config files are defined in subfolders)>
# Inspecting a pod configuration
kubectl describe <object_type> <object_name:opt>
# Listing all resource of object type
kubectl get <object_type>
# Delete all resource for this object.
kubectl delete -f <config-file>
# Updating the image of a container in a deployment.
kubectl set image <object_type>/<object_name>  <container_name> = <new_image>
# Creating a secret
kubectl create secret generic <secret_name> --from-literal <key=value>
# Executing into a container
kubectl exec -it <container> sh
```


## Minkube Commands

```bash
# start minikube
minikube start --driver=hyperkit --no-vtx-check \
  --disable-driver-mounts \
  --addons=ingress, metrics-server \
  --cpus=2 --memory=4096 \
  --v=1
# List all available addons
minikube addons list
# Enable metrics-server addon
minikube addons enable metrics-server
# Enable Ingress addon
minikube addons enable ingress
# Get the url of a defined service
minikube service <service-name> --url

# Configuring docker on host to attach to the minikube docker-daemon
eval $(minikube -p minikube docker-env)
```

## Possible Issues
### `ingress`, and `ingress-dns` addons are currently only supported on Linux with docker driver which is the default, we can solve this by using hyperkit driver. This can be installed by following the below steps.
```bash
# start minikube with hyperkit driver - default is docker-driver
brew install hyperkit
# Download the latest release of the Hyperkit driver
curl -LO https://storage.googleapis.com/minikube/releases/latest/docker-machine-driver-hyperkit
# Make it executable
chmod +x docker-machine-driver-hyperkit
# Move it to a location in your PATH
sudo mv docker-machine-driver-hyperkit /usr/local/bin/
# Set correct permissions (required for networking and VM control)
sudo chown root:wheel /usr/local/bin/docker-machine-driver-hyperkit
sudo chmod u+s /usr/local/bin/docker-machine-driver-hyperkit
# Confirm it's installed
which docker-machine-driver-hyperkit
```


## Google Kubernetes Engine Setup
**Steps:**
* Enable Kubernetes Engine <a href='https://console.cloud.google.com/marketplace/product/google/container.googleapis.com?q=search&referrer=search&inv=1&invt=Ab3KvA&project=concise-quarter-466411-d9&pli=1'>API</a>
* Create Kubernetes <a href='https://console.cloud.google.com/kubernetes/list/overview?inv=1&invt=Ab3KvA&project=concise-quarter-466411-d9'>cluster</a>
* Authenticate the google cloud shell using the below commands:
    ```bash
    # set project
    gcloud config set project <project-id|concise-quarter-466411-d9>
    # set compute zone
    gcloud config set compute/zone <zone|europe-west4-a>
    # get cluster credentials
    gcloud container clusters get-credentials <kubernetes cluster name|multi-cluster>
    # Run Kubectl commands to test access to the cluster
    kubectl create secret generic pgpassword --from-literal <key=value>
    # install helm
    curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 && chmod 700 get_helm.sh && ./get_helm.sh
    # Deploy ingress controller
    helm upgrade --install ingress-nginx ingress-nginx \
    --repo https://kubernetes.github.io/ingress-nginx \
    --namespace ingress-nginx --create-namespace
    ```
