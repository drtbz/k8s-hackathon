

helm install -f ./strings-app/values.yaml strings-app ./strings-app --namespace=dev --create-namespace --dry-run

helm status strings-app -n dev

helm ls --all-namespaces

helm ls strings-app -n dev

helm upgrade -f ./strings-app/values.yaml --set frontend.replicas=1 ./strings-app --namespace=dev --create-namespace --dry-run

helm upgrade -f ./strings-app/values.yaml --set frontend.replicas=1 strings-app ./strings-app --namespace=dev --create-namespace

# Values.yaml = Default. Overrides per env if needed
helm upgrade -f ./strings-app/values.yaml ./dev.yaml ./strings-app --namespace=dev --create-namespace --dry-run

More Commands: https://helm.sh/docs/helm/helm_upgrade/