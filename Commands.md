# Arguments equivalence (short vs long form)
```bash
--all-namespaces = -A
--labels = -l
--namespace = -n

configmaps = cm
namespaces = ns
deployments = deploy
replicaset = rs
serviceaccount = sa
service = svc
persistentVolume = pv
persistentVolumeClaim = pvc
```

# Commands for creating K8 objects
```bash
# Create a pod with the Docker Nginx image. <name> can be whatever
# "run" is only to run individual pods from the CLI, and create takes a config file as input
kubectl run <name> --image=nginx

# Like before, exposing a port
kubectl run <name> --image=nginx --port=8080

# The yaml definition file that would be created can be exported without creating the object. Useful for generating an initial config file and modifying it later
kubectl run <name> --image=nginx --dry-run -o yaml > <output_file>

# The yaml definition file of an object that is already running can also be exported
kubectl get <tipo> <name> -o yaml > <output_file>

# Create a pod with a label
kubectl run redis --image=nginx:alpine --labels='tier=db'
kubectl run redis --image=nginx:alpine -l 'tier=db'

# Help of a specific command
kubectl run --help

# Create a resource from a definition file
kubectl create -f definition.yml

# Like before, but logging the causes of changes. These changes can be seen later with "kubectl history". Also in "kubectl describe" these logs can be seen in the Annotations section
kubectl create -f deployment.yaml --record

# Apply changes in a definition file to a running object. Not everything can be updated like this
kubectl apply -f pod-definition.yml

# Create a pod in namespace x
kubectl create -f pod.yaml --namespace=x

# Inline creation of a pod, specifying a Docker image and number of replicas
kubectl create deploy webapp --image=kodekloud/webapp-color --replicas=3

# Create a namespace
kubectl create namespace <name>
kubectl create ns <name>

# Crear a secret in CLI 
kubectl create secret generic app-secret --from-literal=DB_Host=mysql --from-literal=DB_User=root --from-literal=DB_Password=passwd

# Create a secret 
kubectl create secret generic <secret-name> --from-file=<path_to_file>

# Create a service account
kubectl create serviceaccount <name>

# Crear a token for a service account
kubectl create token <service account name>

# Inline creation of a job 
k create job --image=<image name> <job name>

# Inline creation of an ingress
k create ingress <ingress name> -n <namespace> --rule="/endpoint=<service>:<port>"
k create ingress <ingress name> -n <namespace> --rule="/pay=pay-service:8082"

# Create a service to "expose" a resource
k expose <resource kind> <resource name> -n <namespace>
k expose deploy ingress-controller -n ingress-space --name ingress --port=80 --target-port=80 --type NodePort
```
