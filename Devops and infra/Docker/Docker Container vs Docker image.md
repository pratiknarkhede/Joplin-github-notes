### Docker Image

A Docker image is a lightweight, standalone, and executable software package that includes everything needed to run a piece of software,

including the code, runtime, libraries, environment variables, and config files. Images are immutable, meaning once they're created, they don't change. Instead, any changes result in the creation of a new image. Images serve as the blueprint for containers.

**Key Points About Docker Images:**

- **Immutable:** Once an image is created, it does not change.
- **Reusable:** The same image can be used to start multiple containers.
- **Version Controlled:** Images can be versioned, allowing for easy rollback and updates.
- **Registry Storage:** Images are typically stored in a registry (e.g., Docker Hub) for easy sharing and distribution.

&nbsp;

&nbsp;

### Docker Container

A container is a runtime instance of a Docker image. When you run an image, Docker creates a container from that image. Containers encapsulate the application and its environment. Unlike images, containers are mutable, meaning their state can change as they execute commands and applications.

**Key Points About Docker Containers:**

- **Isolated:** Containers are isolated from each other and the host system, though they share the same kernel.
- **Ephemeral:** Containers can be easily created, started, stopped, moved, and deleted.
- **Mutable:** The state of a container can change over time as it runs.
- **Layered:** Changes made to a running container are written to a writable layer atop the immutable image layers, allowing for temporary changes during the container's lifecycle.

&nbsp;

Imagine you have a simple web application written in Java using the SpringBoot framework.  
<br/><br/>

**Docker Image Creation:**

1.  You create a `Dockerfile` that specifies the base image (e.g., openjdk:11 ), along with the steps to install dependencies, copy your application code into the image, and define how the application should be executed.
2.  You build the Docker image using the command `docker build -t my-springboot-app .`. This process creates an image named `my-springboot-app` which includes the Java runtime, your spring application, and its dependencies.
3.  This image can be pushed to a Docker registry like Docker Hub, making it available for others to use.

**Running a Container:**

1.  To run your application, you start a container from the `my-springboot-app` image using the command `docker run -d -p 5000:5000 my-springboot-app`.
2.  Docker creates a container from the image. This container is a running instance of your springboot application, accessible on port 5000.
3.  If you decide to make changes to your application, you would rebuild the image and restart the container to see those changes.

In this example, the **Docker image** is a static blueprint for your springboot application, including the environment and code. The **container** is a live, running instance of this image. You can start, stop, move, or delete the container without affecting the underlying image, and you can create many containers from the same image.

&nbsp;

&nbsp;

&nbsp;

&nbsp;