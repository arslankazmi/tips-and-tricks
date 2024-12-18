# Useful Docker Commands

So I've been building some custom docker containers for reliable deployment of some machine learning projects, and I'd like to write down some of the more useful commands and command-line arguments I've learned.
Docker commands can get quite long and unwieldy pretty quickly, so instead I'm going to introduce each command and command variant separately.

This might be a little long winded, so feel free to skip to the end where I'll recap just the commands in a nice looking table.

## Building a docker container

The basic command to build a container simply builds a container based on the contents of a Dockerfile in the current directory:

```
docker build .
```

If you want to give the resultant container a cool and memorable name, you can tag specific builds with ```-t```, eg:

```
docker build -t hoopy-frood
```

## Running a docker container

The basic run uses a ```docker run <image-name/tag>``` command.

All this does is run a container in the current terminal window. If you close the terminal, typically the container also exits and stops. This is why it's common to see a command run at the end of adocker file to keep it running indefinitely, like: ```tail -f /dev/null```.

In my case I had to run a server application, which already runs indefinitely untill closed. However, I also had to map container ports in order to allow the server application to be accessed from outside the container.

To map a port on the host machine to a container/application port:

```
docker run -p <host-port>:<container-port> hoopy-frood
```

### Port mapping

So If I had an container with an app running on port 1337, and I wanted to expose it using port 8008 on my host machine, I would do:

```
docker run -p 8008:1337 hoopy-frood
```

### Making the container network equal to the host's

```
docker run --network host hoopy-frood
```

It's best to combine this with the -p command so you're app can be accessed using the host's ip address.

```
docker run --network host -p 8008:1337 hoopy-frood
```

### Mounting local directories/files into the container

Another cool trick is to be able to mount a local directory or file as if it was part of the container's internal directory/file structure. In my case this as useful for binding an external .env file as if it was the container app;s internal .env config file. 

```
docker run -v /path/to/local/.env:/app/.env hoopy-frood
```

Now if I want to wuickly mess with some settings in the .env file, all I have to do afterwards is restart the container and it runs with the latest configuration changes.

```
docker restart <container-id>
```

### A word about container ids

With some commands you have to use a container id to copy in files, start or stsop or restart a runnning container. These ids are long and cumbersome, and nobody likes to type in or copy-paste jummbled strings. If you use the docker ps command to take a look at your runing containers, you'll see they also have a "name". You can use these snames to refer to specific containers instead.

```
docker restart amazing-camel
```

>Docker will generate a name if one is not provided using the ```--name``` argument when running the container for the first time.

## Restart Policies

Sometimes you might want a container too keep running even after a host machine crash or restart. You can use the ```--restart``` parameter to accomplish this.

If you want the container to restart regardless of circumstance, use ```--restart always```

```
docker run --restart always hoopy-frood
```

If you want the container to restart only if it was not stopped manually, use ```--restart unless-stopped```

```
docker run --restart unless-stopped hoopy-frood.
```


## Summary Table

Let's summarize all this knowledge into a table shall we?

| Root Command   | Parameter | Parameter Value           | Argument       | Description                                                           | Example                                                      |
|----------------|-----------|---------------------------|----------------|-----------------------------------------------------------------------|--------------------------------------------------------------|
| docker build . | -t        | image-tag                 | tag name       | builds a docker image with a specific tag name                        | ```docker build -t hoopy-frood```                            |
| docker run     | -p        | host-port:internal-port   | container name | Maps a port on the host to a port inside the container (port binding) | ```docker run -p 8008:1337 hoopy-frood```                    |
| docker run     | --network | host                      | host           | Makes the container use the host machine's network entirely           | ```docker run --network host hoopy-frood```                  |
| docker run     | -v        | /local/path:internal/path | container name | Binds the host's local path to container's internal path              | ```docker run -v /path/to/local/.env:/app/.env hoopy-frood```|
| docker run     | --restart | always, unless-stopped*   | container name | Sets the restart policy of said container                             | ```docker run --restart unless-stopped hoopy-frood.```       |

> Examples are of the form ```<root-command> <parameter> <parameter-value> <argument>```

## Addendum

* other options include:

| Restart Policy     | Description                                                                                                        | Behavior                                                                                                                                         | Example                                       |
|--------------------|--------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------|
| `no`               | No restart policy (default). The container will not restart automatically if it stops or crashes.                   | The container stops and remains in the stopped state after it exits, regardless of the exit code.                                                 | `docker run --restart no my-container`        |
| `on-failure`       | Restart the container if it exits with a non-zero exit code (indicating an error).                                  | Restarts the container on failure. Does not restart on successful exit (exit code 0).                                                             | `docker run --restart on-failure my-app`      |
| `on-failure:N`     | Restart the container up to `N` times if it exits with a non-zero exit code.                                        | Restarts the container on failure a maximum of `N` times. After that, it remains in the stopped state.                                             | `docker run --restart on-failure:5 my-app`    |
| `always`           | Always restart the container regardless of the exit code (including manual `docker stop`).                          | Restarts the container automatically when it stops for any reason. It will even restart after the container is manually stopped or on system boot. | `docker run --restart always my-app`          |
| `unless-stopped`   | Always restart the container unless it was manually stopped.                                                        | Behaves like `always`, but **will not** restart after a manual stop or if the Docker daemon was stopped and the container was stopped beforehand.  | `docker run --restart unless-stopped my-app`  |

>Table generated using ChatGPT

I just typically use unless-stopped.

- **`docker run --name`**: Names the **container**.
- **`docker build -t`**: Names the **image**.

