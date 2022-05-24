# Introduction to Monitoring Docker containers

## Docker Logs

First start a container in detatched mode that prints date every second. We are using `busybox` image here and calling the container `date-printer`

```bash

docker run --name date-printer -d busybox sh -c "while true; do $(echo date); sleep 1; done"

```
Now `follow` its logs. `ctrl + c` to exit. 

```bash
docker logs -f date-printer
```

Retrieve logs only `since` last 3 seconds
```bash
docker logs date-printer --since=3s
```

See: https://docs.docker.com/engine/reference/commandline/logs/

# Docker Exec - Run commands inside a running container!

Launch a redis container and ensure it's running.

```bash
    docker run -d redis
    docker ps 
    # note down the container name
```
Now `set` some data using redis using its cli. First connect to its container shell.

```bash
    docker exec -it <redis_container_name> sh
```
And Once you're inside the container (when the prompt goes `#`), type

```bash
    redis-cli
```
The prompt should now look like `127.0.0.1:6379` (redis runs on 6379 by default)

```bash
    set name "Jane"
    exit
    exit
    # Exit from `redis-cli` and then from the `container shell`
```

Now from the terminal you can directly retrieve that data. Below output should print `Jane`

```bash
    docker exec -it <redis_container_name> redis-cli get name
```




