# Docker Registry

> **Important**: We'll be using github container registry (ghcr) for the purpose of labs and to faciliate collaboration. You can also look at Azure Container Registry (`acr`) as required. See: [Azure container registry](https://azure.microsoft.com/en-gb/services/container-registry/)

## Create a Docker hub account and connect to it.

Make sure you're using a personal / temporary github account.

1. Follow the instructions on official github docs below on how to connect yourself to `ghcr`

* **[Authenticating to ghcr](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-container-registry#authenticating-to-the-container-registry)**

    > Recommended: Use expiry dates on PATs (personal access tokens) and revoke access once you're done with the hackathon

## Tag and Push your image to registry.

1. Before you can push your image to your registry, You need to tag it. 

    Tag your `hello-web` image as below. Replace the <image-id> and <github-username> as needed.

    ```bash
        docker images hello-web 
        
        # Copy the image-id
        docker tag <image-id> <github-username>/hello-web:1.0

        docker push <github-username>/hello-web:1.0
    ```

2. Go to `hub.docker.com` to ensure your image is pushed correctly. We will come to this image later during kubernetes labs.

3. You can always test out your image by running it as follows. 

```bash
    docker run -d -p 9010:80 <github-username>/hello-web:1.0
    docker ps
    docker stop <hello-web_container_id>
```
---

### Optional: Clean up

>Important: As with any system, use clean up commands with caution. 

> **Do not run** below commands if you have other images and containers in your workstation that you do not wish to delete! Instead, just remove the images and containers for above `strings-app`

> Do not force delete images that are still referenced by containers. 

#### To clean up dangling resources
```bash
docker system prune
```

#### To clean up **all** resources including images,containers,volumes, etc. 
```
docker system prune --all

# `y' when prompted
```
