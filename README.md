# `dotnet-monitor` Demo

## How to run this sample?

Replace `dotnet-hello-world` and `dotnet-monitor` with your desired container names

```bash
# Build a docker image for your application
docker build -t dotnet-hello-world ./src/

# Build the sidecar image for dotnet-monitor
docker build -t dotnet-monitor ./sidecar/

# Push your images
# If pushing to ACR, you may need to login
# az acr login -n <my-registry.acr.io>
docker push dotnet-hello-world
docker push dotnet-monitor

# Ensure your cluster has access to pull images from your registry

kubectl apply -f kubernetes-manifests/hello.deployment.yaml
kubectl apply -f kubernetes-manifests/hello.sevice.yaml
# Optional: If you have an ingress controller
kubectl apply -f kubernetes-manifests/hello.ingress.yaml

# Clean up
kubectl delete -f kubernetes-manifests/*.yml
```

## How to run this sample with Skaffold?

Alternatively you can use [Skaffold](https://skaffold.dev/) to run this application

```bash
# Replace my-registry.azurecr.io
skaffold config set default-repo my-registry.azurecr.io

skaffold run

# Clean up
skaffold delete
```

## How do I access the API exposed by `dotnet-monitor`?

```bash
$ kubectl get pod -l app=dotnet-hello-world
NAME                                 READY   STATUS    RESTARTS   AGE
dotnet-hello-world-dc6f67566-t2dzd   2/2     Running   0          37m

$ kubectl port-forward pods/dotnet-hello-world-dc6f67566-t2dzd 7000:52323
Forwarding from 127.0.0.1:7000 -> 52323
Forwarding from [::1]:7000 -> 52323
```

Once you've established a port forward, you can download diagnostic artifacts by hitting these endpoints:

1. Process dump: `http://localhost:7000/dump`
2. CPU profile: `http://localhost:7000/cpuprofile`

