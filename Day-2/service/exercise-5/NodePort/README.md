# exercise-5: Node Port

You will create a Node Port service and access it.

## Create a Deployment

Here is the deployment file:
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-deployment-50000
spec:
  selector:
    matchLabels:
      app: metrics
      department: engineering
  replicas: 3
  template:
    metadata:
      labels:
        app: metrics
        department: engineering
    spec:
      containers:
      - name: hello
        image: "gcr.io/google-samples/hello-app:2.0"
        env:
        - name: "PORT"
          value: "50000"
```

```sh 
kubectl apply -f my-deployment-50000.yaml
```

## Create a Node Port service

Complete the given `service.yaml` file to expose the pods created from the deployment above.

```
apiVersion: v1
kind: Service
metadata:
  name: my-np-service
spec:
  type: NodePort
  ...
```

```sh 
kubectl apply -f service.yaml
```

## Test the service connectivity

Determine the public IP of any worker node:
```
kubectl get nodes --output wide
```

Access the service
```
[NODE_IP_ADDRESS]:[NODE_PORT]
# NODE_PORT is the random port given by kube
```

Is everything OK? Ensure the service has entries in its `endpoints`:
```sh
kubectl describe svc my-np-service
```

Are the pods running?
If yes, debug the situation. Hint: command `kubectl describe pod NAME_OF_YOUR_POD` could help.

Once the deployment fixed, try again to access the service.

## Clean all resources

```sh
kubectl  delete -f .
```
