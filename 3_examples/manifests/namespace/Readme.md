## Deploying with Kustomize

### Just with Kubectl
```bash
kubectl apply -k .
```

### Build and deploy
```bash
kustomize build . | kubectl apply -f -
```

### Just build and see manifest output

```bash
kustomize build .
```

### Delete
```bash
kubectl delete -k .
```