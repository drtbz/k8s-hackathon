# Getting Started with Docker

## Make sure you have Docker-desktop installed with Kubernetes enabled. Speak to your team lead / point-of-contact if you have any doubts on this and docker desktop licensing.

### Create yourself a couple of parent directories for the hackathon. Perhaps, one for the labs on Day1 and another (will be a git repo) for the actual application you'll be building collaboratively.

## Hello World Container

From terminal, run

```bash 
    docker run hello-world
```

> **Note:** `docker run` = `docker create` + `docker start` (+ also `docker attach`)

* Where did the image `hello-world` come from? Is it an official image?

>  Hint: See https://hub.docker.com/_/hello-world 

* Observe the output log, starting from `pulling` the image from public registry.

* Understand the architecture. (We'll look at build shortly)

![Docker Architecture](./assets/docker_architecture.svg)

---

### OS Container - Run an ubuntu container and interact with its shell environment!

```bash
    docker run -it ubuntu bash    

    Or
    # by default, ubuntu:latest will be used. To use a specific version,
    # Tags are usually listed in Image's repository. In this case, official ubuntu image repository on docker hub (https://hub.docker.com/_/ubuntu) 
    docker run -it ubuntu:<preferred_tag> bash
```

Execute some commands inside that docker container

```bash 
    ls
```

> Exit the container shell by typing `exit`

Take your time to understand what `-it` flag does when used with `docker run`

``` 
`i` - keep STDIN open even if not attached
`t` - Allocate a pseudo-tty
```

> An easy mnemonic to remember: `-it` -> interactive terminal

### See locally cached images

```bash
    docker images 
```
The result shows images that are locally cached in your environment. So, the next time you `run` that exact image, it won't be pulled from registry.

### Exploring docker run

* Run the same ubuntu image again but with a little twist

1. Run it without `bash` in the end. Notice how it still works the same way as before? This is because the ubuntu image's `cmd` is set to run `bash` by default. `exit` the container shell.
    ```bash
        docker run -it ubuntu
    ```

> Note: If we just run `docker run ubuntu`, bash will exit automatically as it does not have an stdin to listen to. We'll see later about executing a `process` using `EntryPoint` in docker and how web apps make use of it.

2. Now override the default `cmd` with some other command such as `ls`. Run the below and notice the difference in result.

```bash
    docker run -it ubuntu ls    
```

* Observe the result. What's going on here? We are overriding the default cmd for that image

> See https://docs.docker.com/engine/reference/run/#cmd-default-command-or-options

---


