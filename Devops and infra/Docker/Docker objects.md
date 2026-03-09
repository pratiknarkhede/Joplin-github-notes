When you use Docker, you are creating and using images, containers, networks, volumes, plugins, and other objects. This section is a brief overview of some of those objects.

#### Images

&nbsp;

An image is a read-only template with instructions for creating a Docker container. Often, an image is based on another image, with some additional customization. For example, you may build an image which is based on the `ubuntu` image, but installs the Apache web server and your application, as well as the configuration details needed to make your application run.

&nbsp;

To build your own image, you create a Dockerfile with a simple syntax for defining the steps needed to create the image and run it.

Each instruction in a Dockerfile creates a layer in the image.

When you change the Dockerfile and rebuild the image, only those layers which have changed are rebuilt.

This is part of what makes images so lightweight, small, and fast, when compared to other virtualization technologies.

&nbsp;

&nbsp;

&nbsp;

#### Containers

A container is a runnable instance of an image. You can create, start, stop, move, or delete a container using the Docker API or CLI.

You can connect a container to one or more networks, attach storage to it, or even create a new image based on its current state.

By default, a container is relatively well isolated from other containers and its host machine

&nbsp;

### How They Work Together

1.  **Image Creation**: A user writes a `Dockerfile` defining how to build a Docker image. The user then uses the Docker client to send a build command to the Docker daemon, which builds the image.
2.  **Image Storage**: Once built, images can be pushed to a Docker registry by the daemon, allowing them to be shared and deployed elsewhere.
3.  **Container Deployment**: To run a container, the user instructs the Docker client to start a container from an image. The client sends this command to the daemon, which pulls the image from the specified registry (if it's not already available locally) and starts the container.

This architecture separates concerns, allowing the daemon to focus on managing Docker objects while the client acts as the user interface. Additionally, by allowing communication over a network, remote management of containers and images becomes possible, enabling more complex deployments and operations.

&nbsp;

##### Example Docker run command

&nbsp;

The following command runs an ubuntu container, attaches interactively to your local command-line session, and runs `/bin/bash`.

*`docker run -i -t ubuntu /bin/bash`*

&nbsp;

&nbsp;

When you run this command, the following happens

&nbsp;

1.  If you don't have the `ubuntu` image locally, Docker pulls it from your configured registry, as though you had run `docker pull ubuntu` manually.
    
2.  Docker creates a new container, as though you had run a `docker container create` command manually.
    
3.  Docker allocates a read-write filesystem to the container, as its final layer. This allows a running container to create or modify files and directories in its local filesystem.
    
4.  Docker creates a network interface to connect the container to the default network, since you didn't specify any networking options. This includes assigning an IP address to the container. By default, containers can connect to external networks using the host machine's network connection.
    
5.  Docker starts the container and executes `/bin/bash`. Because the container is running interactively and attached to your terminal (due to the `-i` and `-t` flags), you can provide input using your keyboard while Docker logs the output to your terminal.
    
6.  When you run `exit` to terminate the `/bin/bash` command, the container stops but isn't removed. You can start it again or remove it.
    

&nbsp;

&nbsp;

Docker uses a technology called `namespaces` to provide the isolated workspace called the container. When you run a container, Docker creates a set of namespaces for that container.

These namespaces provide a layer of isolation. Each aspect of a container runs in a separate namespace and its access is limited to that namespace.

&nbsp;

&nbsp;

&nbsp;

&nbsp;