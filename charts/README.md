# Installation

Databunker charts can be installed only with [Helm 3](https://helm.sh/docs/).

Before installing containers, you need to add Databunker ```helm``` repository.

Run the following command:
```
helm repo add databunker https://databunker.org/charts/
```

Update all the repositories to ensure ```helm``` is aware of the latest versions.
```
helm repo update
```

# Start Databunker service together with MySQL container

You can start Databunker with auto-generated self-signed SSL certificate using the following command:
```
helm install databunker databunker/databunker \
  --set mariadb.primary.persistence.enabled=false \
  --set certificates.customCAs\[0\].secret="databunker"
```

## Usefull commands

```
export POD_NAME=$(kubectl get pods --namespace default -l "app.kubernetes.io/name=databunker,app.kubernetes.io/instance=databunker" -o jsonpath="{.items[0].metadata.name}")
export CONTAINER_PORT=$(kubectl get pod --namespace default $POD_NAME -o jsonpath="{.spec.containers[0].ports[0].containerPort}")
echo "Visit http://127.0.0.1:8080 to use your application"
kubectl --namespace default port-forward $POD_NAME 8080:$CONTAINER_PORT
```


# Starting Databunker DEMO

## Start in AWS
Use the following command:
```
helm install demo databunker/databunker-demo --set service.type=LoadBalancer
```

It takes a few seconds for the load balancer starts working.

You can use the following command to get URL of the load balancer:

```
kubectl get svc | grep demo
```

You can open this url in browser. By default, it will use port 3000.


## Start using NodePort
You can run the following command:
```
helm install demo databunker/databunker-demo --set service.type=NodePort
```

### Default port for NodePort

The default port is **30300**.

## Running Databunker demo on the local machine

You can open `http://localhost:30300/` in your browser.

## Removing **databunker-demo** deployment

Use the following command:
```
helm uninstall demo
```

## Chart Parameters

| Name                            | Description                                                | Value                |
| ------------------------------- | ---------------------------------------------------------- | -------------------- |
| `service.type`                  | Databunker Service Type                                    | `ClusterIP`          |
| `service.nodePort`              | Databunker API and UI port                                 | `"30300"`            |
