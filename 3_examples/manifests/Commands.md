### Pods
```bash
kubectl get pods --field-selector=status.phase=Running
kubectl get pod -l app=hello-web -o jsonpath="{.items[0].metadata.name}"
# all pods
kubectl get pod -l app=hello-web -o jsonpath="{.items[*].metadata.name}"
```

### Events
```bash
kubectl get events --sort-by=.metadata.creationTimestamp
```

### LimitRanges and ResourceQuotas
```bash
kubectl get limits -n <target-ns>
kubectl get quota -n <target-ns>
# or
kubectl get limits --all-namespaces -o json
kubectl get quota --all-namespaces -o yaml
```

### Deployments
```bash
kubectl scale deploy/<name> --replicas=5
# set version / tag of image
kubectl set image deploy/<name> nginx=nginx:1.23.1
```

### Rollouts
```bash
kubectl rollout status <deploy/statefulset/daemonset name>
# Creates new replicaset too for deployment
kubectl rollout restart <deploy/statefulset/daemonset name>
kubectl rollout history <deploy/statefulset/daemonset name>
kubectl rollout undo <deploy/statefulset/daemonset name> --to-revision=3
```

### Labels and Selectors
```bash
kubectl get po --selector app=hello-web 
kubectl get po -l app=hello-web 
kubectl logs --selector app=hello-web --follow
kubectl get nodes --show-labels
# label columns
k get nodes -L topology.kubernetes.io/zone -L agentpool
# equality-based filter
kubectl get pods -l environment=production,tier=frontend
# set based AND
kubectl get pods -l 'environment in (production),tier in (frontend)'
# set based OR
kubectl get pods -l 'environment in (production, qa)'
kubectl get pods -l 'environment,environment notin (frontend)'
```

### Logs
```bash
# all containers, all pods in a deployment, move to next one if one pod errors
kubectl logs -l app=<name> --all-containers --ignore-errors --follow
###