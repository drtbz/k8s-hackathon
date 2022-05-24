# Deployments

In this lab, we will create deployments with replica sets and apply everything else we've learnt so far along with it.

> **Important : This is one of the most important labs in our workshop. Please make sure to take your time to understand the content and complete it. The exercises here will also incorporate various other things we have learnt thus far. Feel free to re-visit them and practice again till you're fully comfortable with the concepts covered here**

* For this lab, we will be using the same, simple `hello-web` app from docker registry lab earlier.

---

## Exercise 1 - Create a Deployment manifest file for `hello-web:1.0`

1. Create a manifest file using `--dry-run` and `-o yaml`

    ```bash  
    
    # Make sure to replace <your-username> with your docker hub username 
    # (or surenmcode if you're having problems with your docker hub username)

    kubectl create deploy hello-web --image=<github-username>/hello-web:1.0 -o yaml --dry-run=client > hello-web.yaml    
    ```

2. Open `hello-web.yaml` and change the number of replicas to `3`. Also, Make sure the value for image looks correct.

    > Take a few minutes to understand the deployment manifest. Notice the creation of default label called `app`

  Your `hello-web.yaml` should look like below.
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: hello-web
  name: hello-web
spec:
  replicas: 3 # Number of pods for replica set
  selector:
    matchLabels:
      app: hello-web
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: hello-web
    spec:
      containers:
      - image: <github-username>/hello-web:1.0
        name: hello-web
        resources: {}
status: {}
```

3. Deploy your app.

    ```bash
    kubectl apply -f hello-web.yaml
    ```

4. View the Deployment and Replica Sets of hello-web. Notice the use of commas to query more objects and usage of labels

    ```bash
    kubectl get deploy,rs,po -l app=hello-web
    ```

5. Access your app by `Port-Forwarding` to your deployment

```bash
# Notice how in this case, backend port points to 8080 of app container inside the pod
kubectl port-forward deployment.apps/hello-web 9003:8080

http localhost:9003 # or the port that you forwarded to
```
---

## Exercise 2 -Expose the `hello-web` deployment as a Service

1. Generate a manifest for a `cluster-ip` service that exposes the `hello-web` deployment. 

> Spend a minute to understand what we're doing here.

```bash
kubectl expose deploy hello-web --name hello-web-svc --port=80 --target-port=80 --type=ClusterIP --dry-run=client -o yaml > hello-web-svc.yaml
```

2. Take a look at `hello-web-svc.yaml` and make sure you know what it's doing. It should look like below:

```yaml
apiVersion: v1
kind: Service
metadata:
  creationTimestamp: null
  labels:
    app: hello-web
  name: hello-web-svc
spec:
  ports:
  - port: 80
    protocol: TCP
    targetPort: 80
  selector: # this is how service knows where to send the traffic to. (using selector on labels)
    app: hello-web 
  type: ClusterIP
status:
  loadBalancer: {}
```

3. Apply the Service

    ```bash
    kubectl apply -f hello-web-svc.yaml
    ```

4. Port-forward to the Service to make sure it's working as intended.

```bash
# We're forwarding from port 80 because the service itself runs on port 80. It's the backend app container that runs on 8080.
kubectl port-forward service/hello-web-svc 9004:80

http localhost:9004 # or the port that was forwarded to
```

5. Take a look at all the objects that are related to our `hello-web`. (labels greatly help us here too!)

```bash
k get all -o wide -l app=hello-web
```
---

## Exercise - 3 Deploy a newer version of the app

* Create a 'newer' version of the hello-web app. Maybe, just update the text on `index.html` to  something else.

* Build and push the newer version of the `hello-web:2.0` app to your `ghcr`. See [docker_registry](../1_docker_fundamentals/05_docker_registry.md) lab once again if you're unsure of how to do this.

1. In order to observe how the update gets applied, open a second pane / terminal and set a watcher on replica sets. 

    ```bash
    kubectl get rs -w
    ```       

2. Update the image used by 'hello-web' deployment to use version `2.0`

There are two ways to do this: (choose one you feel comfortable for now)

* Command-line approach
    ```bash
    # set the deployment image    
    kubectl set image deployment hello-web hello-web=<github-username>/hello-web:2.0    

    # Notice how the desired, current and ready states change on replica set watcher.
    ```
* **Or** using the manifest file 

    * Update the `image` field of `hello-web.yaml` to point to `2.0`
    * And then do a `kubectl apply -f hello-web.yaml`

3. Take a look at the replica sets. Notice the new one that's created and the number of pods in Ready state. Also notice `the images` column for version of image that was used.

    ```bash
    k get rs -o wide
    ```

4. Port-forward again to the `hello-web-svc`. This time, it will display `internal server error` or `hello world - version 2` of the app.

```bash
kubectl port-forward service/hello-web-svc 9005:80
# take a look at localhost:9005 
```
---

## Exercise - 4 Rollback to Previous Version

1. View the rollout history for 'mydeploy' and roll back to the previous revision.

```bash
    # view previous rollout revisions and configurations.
    kubectl rollout history deploy hello-web

    # Make sure you've setup the watcher on second pane to observe how rollback happens
    kubectl get rs -w

    # From main pane, rollback to the previous rollout.
    kubectl rollout undo deploy hello-web    
```

## Exercise - 5 Access the service from within the cluster.

1. The service is only visible within the cluster. To hit it by its `dns name`, just create a busybox container and hit the `hello-web-svc` service by its name as below. 

```bash
# delete any busybox pods if you have anything that's already running.
kubectl delete po busybox

# run a busybox pod and launch its shell 
kubectl run busybox --rm --image=busybox -it --restart=Never -- sh    

# Call hello-web-svc using `wget`. Below would print out the output `Hello from CW app - V1.0 (stable)`
wget -q -O - hello-web-svc

# or 

wget -q -O - hello-web-svc.<your-namespace>
# Above is the recommended approach to call a service running on someone else's namespace as well.
# You can provide protocol and port-number as well if needed. (such as, http://hello-web-svc.john:80/)

exit
```
---

