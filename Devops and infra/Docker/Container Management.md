### `docker create`

- **Purpose**: The `docker create` command creates a Docker container but does not start it. It sets up the container according to your specifications but leaves it in a stopped state. This command is useful when you want to prepare a container configuration ahead of time but delay its execution until a later moment.
    
- **Use Case**: Use `docker create` when you need to set up containers in advance, configuring them for future use without immediately running them. It allows for more control over the start time of the container, facilitating scenarios where the container's execution needs to be triggered by specific events or conditions.
    

docker create --name my_nginx -p 8080:80 nginx

&nbsp;

&nbsp;

### `docker run`

- **Purpose**: The `docker run` command is a combination of `docker create` and `docker start`. It creates the container and immediately starts it. It's the most commonly used command for running containers because it combines creation and execution into a single step, making it convenient for most use cases.
    
- **Use Case**: Use `docker run` for the majority of situations where you want to create and start a container immediately. It's particularly useful for development, testing, and one-off tasks in containers. `docker run` is ideal for interactive sessions or when you want to see the output of a container right away.
    

docker run --name my_nginx -p 8080:80 -d nginx

&nbsp;

&nbsp;

**Immediate Execution**: `docker run` creates and starts the container immediately, while `docker create` only creates the container without starting it.

**Control over Start**: `docker create` provides an opportunity to delay the start of a container until you're ready, allowing for more nuanced control over the container lifecycle.

&nbsp;

&nbsp;

&nbsp;

&nbsp;