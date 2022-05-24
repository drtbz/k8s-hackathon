### Run a Webserver

Run an nginx webserver as below.

```bash
    docker run -d -p 9000:80 nginx 
    # -p stands for publish (publish a container's port to the host)

# You will be able to see the webpage on localhost:9000 if you are running docker locally or in a linux vm.

```

> `-d` flag indicates that the container will run in detatched mode, so it's not waiting for input via `stdin`

```bash
# See running containers
    docker ps    
```

Now, run another instance of nginx container. Just use a different port.

```bash
    docker run -d -p 9001:80 nginx
    docker ps # notice the new container from same image. Much like new objects from a class.
```

#### View the contents of the webpage from Terminal

You can view the webpage from browser or terminal using http client tools such as `curl` or `httpie` 

```bash
curl localhost:9000

# or via httpie (for coloured output and more helpful info)
http localhost:9001
```
    
#### Stop the nginx containers.

```bash           
    # run `docker ps` to list the running containers. 

    docker stop <container-id> 
            or
    docker stop <container_name_1> <container_name_2> # Note: it's container name not image name.
```

> Bonus: Look up for the difference between `stop`, `kill` and `rm` commands

#### See all containers including the `created` and `exited` ones

```bash
# `docker ps` only shows containers that are of status `Running`. To see all containers, use `-a` flag

    docker ps -a
```

#### You can always start a stopped container by issuing `start` command

Find the container, you want to start by looking up on `docker ps -a`. Just because the container has stopped or exited, it doesn't mean it's dead.

```bash
    docker start <container_id or name>
```

----

## Cleanup

### For removing a stopped container

```bash
docker rm <container_id/name>
```
### For removing images
```bash 
docker rmi image <image_id_1> <image_id_2>
```

---

### Optional: Clean up


>Important: As with any system, use clean up commands with caution. 

> **Do not run** below commands if you have other images and containers in your workstation that you do not wish to delete!

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
Give these links a quick read:
* https://docs.docker.com/engine/reference/commandline/system_prune/
* https://docs.docker.com/config/pruning/
* https://docs.docker.com/engine/reference/commandline/rmi/

#### To check if your environment is cleaned up,

```bash
docker ps -a
docker images -a
```