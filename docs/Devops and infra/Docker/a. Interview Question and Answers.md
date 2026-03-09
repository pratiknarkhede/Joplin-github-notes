## What is the extension of a Dockerfile?

A Dockerfile doesn't actually have an extension. It's simply named `Dockerfile` with no extension, and Docker recognizes it by this exact name. When you create a Dockerfile, you simply name the file "Dockerfile" (with a capital 'D').

However, you might sometimes see Dockerfiles with names like `Dockerfile.dev` or `Dockerfile.prod` for different environments, but these are custom naming conventions. When using these variant names, you'll need to specify the filename explicitly when building the image with the `-f` flag:

&nbsp;

`docker build -f Dockerfile.dev -t myapp:dev .`

&nbsp;

* * *

&nbsp;

## Difference between docker-compose.yml and Dockerfile

These two files serve completely different but complementary purposes in the Docker ecosystem:

**Dockerfile:**

- Defines how to build a single Docker image
- Contains instructions for creating layers in the image
- Specifies the base image, environment, files to include, commands to run during build
- Focuses on configuring a single container/service
- Used with `docker build` command

**docker-compose.yml:**

- Defines and configures multiple related services/containers
- Specifies how containers should interact with each other
- Configures networks, volumes, environment variables, and dependencies between containers
- Allows you to bring up an entire application stack with a single command
- Used with `docker-compose up` command
- Can reference images built from Dockerfiles or pull from registries

Think of it this way: a Dockerfile is the blueprint for building a single component, while docker-compose.yml is the orchestration plan for how multiple components work together.

&nbsp;

* * *

## What is written inside a Dockerfile?

A Dockerfile contains a series of instructions that Docker follows to build an image. Here's what you typically find inside:

1.  **FROM** - Specifies the base image to start from (e.g., `FROM openjdk:11-jdk`)
2.  **LABEL** - Adds metadata to the image (e.g., `LABEL maintainer="example@example.com"`)
3.  **ENV** - Sets environment variables (e.g., `ENV APP_HOME=/usr/app`)
4.  **WORKDIR** - Sets the working directory for following instructions (e.g., `WORKDIR /usr/app`)
5.  **COPY/ADD** - Copies files from your host into the image
    - `COPY` is simpler, just copying local files
    - `ADD` has additional features like extracting tar archives and downloading from URLs
6.  **RUN** - Executes commands in the container during build time (e.g., `RUN mvn package`)
7.  **EXPOSE** - Documents which ports the container listens on (e.g., `EXPOSE 8080`)
8.  **VOLUME** - Creates mount points for external volumes (e.g., `VOLUME /data`)
9.  **CMD** - Default command to run when container starts (e.g., `CMD ["java", "-jar", "app.jar"]`)
10. **ENTRYPOINT** - Configures the container to run as an executable (e.g., `ENTRYPOINT ["java"]`)

Here's an example of a Dockerfile for a Java application:

&nbsp;

```DockerFile
FROM openjdk:11-jdk

WORKDIR /app

COPY pom.xml .
COPY src ./src

RUN mvn package -DskipTests

EXPOSE 8080

CMD ["java", "-jar", "target/myapp.jar"]
```

&nbsp;

Each instruction creates a new layer in the image, so it's best practice to combine related commands to reduce the number of layers.

&nbsp;

* * *

## How do you expose a port in Docker?

There are two parts to exposing ports in Docker:

1.  **Dockerfile EXPOSE instruction:**
    - The `EXPOSE` instruction in a Dockerfile (e.g., `EXPOSE 8080`) documents which ports the container listens on.
    - This is primarily documentation and doesn't actually publish the port to the host.
    - Example: `EXPOSE 8080` indicates the container listens on port 8080.
2.  **Port mapping when running the container:**
    - To actually make the port accessible from outside the container, you need to map it when starting the container.
    - Use the `-p` or `--publish` flag with `docker run` to map container ports to host ports.
    - The format is `-p host_port:container_port`
    - Example: `docker run -p 80:8080 myapp` maps port 8080 in the container to port 80 on the host.

In docker-compose.yml, you would specify ports like this:

```yaml
services:
  myapp:
    image: myapp
    ports:
      - "80:8080"
```

&nbsp;

* * *

&nbsp;

### 1\. What is Docker and what problem does it solve?

Docker is a platform that enables developers to package applications and their dependencies into standardized units called containers that can run consistently across different environments.

It solves several key problems:

- **Consistency**: "Works on my machine" problem is eliminated as containers run the same regardless of the host environment.
- **Isolation**: Applications and their dependencies are isolated from each other.
- **Efficiency**: Containers share the host OS kernel and are more lightweight than virtual machines.
- **Scalability**: Containers can be easily replicated for scaling applications.
- **DevOps Integration**: Docker facilitates CI/CD pipelines by providing consistent environments from development to production.

* * *

### 2\. What is the difference between a Docker image and a Docker container?

**Docker Image:**

- A read-only template containing a set of instructions for creating a container
- Includes the application code, runtime, libraries, dependencies, and configuration
- Can be stored in repositories like Docker Hub or private registries
- Never changes once built (immutable)
- Comparable to a class in object-oriented programming

**Docker Container:**

- A runnable instance of an image
- Adds a writable layer on top of the image layers
- Has its own filesystem, networking, process space
- Can be started, stopped, moved, and deleted
- Multiple containers can run from the same image
- Comparable to an object (instance of a class) in programming

* * *

&nbsp;

### 3\. Explain Docker layering and how it works

Docker images are built using a layered filesystem called Union File System:

1.  **Layer structure**: ==Each instruction in a Dockerfile creates a new layer.==
2.  **Read-only layers**: Each layer is read-only except the top container layer.
3.  **Layer caching:** ==During builds, Docker reuses unchanged layers from cache, making builds faster.==
4.  **Copy-on-write**: When a file in a lower layer needs modification, it's copied to the top writable layer.
5.  **Layer sharing**: Multiple containers can share the same underlying image layers, saving disk space.

For example, in this Dockerfile:

```Dockerfile
FROM ubuntu:20.04  
RUN apt-get update  
COPY app.jar /app/  
CMD ["java", "-jar", "/app/app.jar"]
```

&nbsp;

Each line creates a separate layer. If you change only the `app.jar` file and rebuild, Docker will reuse the cached Ubuntu and apt-get layers, only rebuilding from the COPY instruction onwards.

&nbsp;

* * *

### 4\. What are Docker volumes and why are they used?

Docker volumes are a ==mechanism for persisting data generated by and used by Docker containers.== They are completely managed by Docker and are isolated from the core functionality of the host machine.

Key reasons to use volumes:

1.  **Data persistence**: Data survives container removal/recreation
2.  **Sharing data**: Between containers or between host and containers
3.  **Performance**: Better I/O performance than using the container's writable layer
4.  **Database storage**: Ideal for database data files
5.  **Configuration sharing**: Share configuration files across containers
6.  **Backups**: Easy to back up or migrate data

&nbsp;

* * *

&nbsp;

### **What is an Entrypoint in Docker?**

&nbsp;

**<span style="color: #fafafc;">the</span> **Entrypoint** <span style="color: #fafafc;">in Docker is like the "main command" or "starting point" that gets executed when a container starts. It defines what the container will do by default when it runs.</span>**

&nbsp;

Think of it as the "job" the container is designed to perform. For example:

- If your container is meant to run a web server, the entrypoint might be the command to start the web server.
- If your container is meant to run a script, the entrypoint could be the command to execute that script.

&nbsp;

**Difference Between `ENTRYPOINT` and `CMD`** :

Both `ENTRYPOINT` and `CMD` define what happens when a container starts, but they behave slightly differently:

- - - **`ENTRYPOINT`** : Defines the main command that cannot be overridden (unless explicitly modified).
        - **`CMD`** : Provides default arguments to the entrypoint, which can be overridden when running the container.

&nbsp;

```bash
ENTRYPOINT ["command", "param1", "param2"]
```

this executes specified command on container startup with specified arguments

&nbsp;

&nbsp;