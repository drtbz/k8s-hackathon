# Containers and Kubernetes Hackathon 

### Please make sure you're comfortable with `docker` and `kubernetes` fundamentals before starting this. If not, it's highly recommended that you complete at least some of the hands-on, prescriptive labs in [1_fundamentals](../1_fundamentals)

### Guidelines

* You're free to use the language, web framework of your choice. 
* Focus more on containerization and kubernetes aspects rather than the actual design and implementation of the app itself. You just need two components at the very least, such as the one demonstrated in webinars. Something like below:
    * A front-end webapp 
    * A backend api
    * Bonus: third component for caching or storage such as `Redis`.
* Containerize the app components and use `ghcr` for image registry. Do not put any sensitive content into the app.
* Recommended: Explore and use `docker-compose` for bringing up multiple containers together (locally) without `kuberentes`.
  * Avoid hard-coding backend service api url. Use env variables when possible. (can be done as part of Dockerfile too)
* Build essential kubernetes manifests for all your app components and deploy them to your cluster.
* Use ConfigMap and inject backend service location / url as an environment variable (may need some refactoring to Dockerfile if required)
* You can use the below to repos as a reference, but please make sure you're building the app, dockerfiles, kubernetes manifests from the scratch
    * https://github.com/suren-m/cndev (for containerization)
    * https://github.com/suren-m/cndev-config/tree/main/kustomization/base (for manifests. Note: you don't need to use kustomize. Just plain manifests to start with and bring in helm later on if time permits)

### Bonus Scenarios

* Use Helm to configure your app for multiple environments. (maybe to `dev1` and `dev2` namespaces, each taking different values for `replicas`)
* Use `Github Actions` CI builds and pushing images to `ghcr`
* Incorporate `image scanning` into your CI pipeline
* Apply `resource limits` on containers
* Apply `resource quotas` and `limit ranges` on namespaces
* Explore a `Service Mesh` or `dapr` as you see fit. Refer to recordings of webinars delivered on these topics.
* Explore `Bridge to Kubernetes` or `Attach vs code to pod` feature for debugging your app running on your own namespace.
* Explore `GitOps` using a tool such as `ArgoCd`. Refer to recordings of webinars delivered on GitOps.
* For DevOps / Cloud Engineers - Use `Terraform` for `iac` for your `aks` cluster 



