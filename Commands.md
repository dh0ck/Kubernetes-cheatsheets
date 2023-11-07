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

# Commands to modify existing objects
```bash
# Replace an existing configuration of a replica set after modifying a definition file
kubectl replace -f replicaset-definition.yml

# Change the number of replicas in a replica set by updating the defintion file
kubectl scale --replicas=6 -f replicaset-definition.yml

# Change the number of replicas in a replica set without updating the definition file
kubectl scale --replicas=6 replicaset <nombre de replicaset>

# Edit in real time the configuration of a replicaset to apply changes without restart
kubectl edit replicaset <name>
kubectl edit rs <name>

# Rollout of a deployment to the previous version (if anything went wrong during a deploy)
# With the "history" command all deployments can be seen. Doing this changes the number. 
# For example, if there were 3 versions, if we revert to version 2, version 2 disappears from the list
# and version 4 appears. It will be the last version chronologically, but its equal to version 2
kubectl rollout undo deployment/myapp-deployment

# Change the image used in a deployment to a different version
kubectl set image <deployment name> <container name> nginx=nginx:1.18-perl --record

# Change the default namespacee (what comes next makes the former dev namespace to become the default one,
# and we don't need to write --namespace=dev to refer to it in kubectl commands). This doesn't mean that
now dev is called default. Default still is another namespace, but to access it, we need  --namespace=default
kubectl config set-context $(kubectl config current-context) --namespace=dev

# Apply a taint to a node (taint-effect says what happens to nodes that don't support that taint
# (NoSchedule | PreferNoSchedule | NoExecute))
kubectl taint nodes node-name key=value:taint-effect

# Example of taint
kubectl taint nodes <node name> <key>=<value>:<effect>
kubectl taint nodes node1 app=blue:NoSchedule

# Add toleration to pod
kubectl taint nodes node1 app=blue: NoSchedule

# Add label to a node, so that a pod with that pod selector can run only on that node
kubectl label nodes <node name> <label key>=<label value>

# If we edit a definition file and it doesn't allow to updated the running object, instead of deleting it 
# and create it again, we can do:
kubectl replace --force -f <file yaml>

# Return to the previous version of a deployment
kubectl rollout undo <deployment>
```
