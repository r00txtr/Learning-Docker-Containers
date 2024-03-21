To complete Exercise 3, you'll need to follow these steps:

### 1. Define a multi-container application using Docker Compose:

Create a `docker-compose.yml` file in your project directory and define your multi-container application. Here's an example with two services, one for a web server and another for a database (MySQL):

```yaml
version: '3.8'

services:
  web:
    image: nginx:latest
    ports:
      - "80:80"
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf
    depends_on:
      - db

  db:
    image: mysql:latest
    environment:
      MYSQL_ROOT_PASSWORD: example
      MYSQL_DATABASE: my_database
```

In this example:
- We define two services: `web` and `db`.
- The `web` service uses the official Nginx image and mounts a custom `nginx.conf` file.
- The `db` service uses the official MySQL image and sets environment variables for the MySQL root password and database name.

### 2. Run the application and verify its functionality:

Navigate to the directory containing your `docker-compose.yml` file and run the following command:

```bash
docker compose up -d
```

This command will start the services defined in your `docker-compose.yml` file and attach their output to the terminal. You should see output indicating that the services are running.

You can then open a web browser and navigate to `http://localhost` to see the Nginx web server running. If you have a web page configured, you should see it rendered in the browser.

### 3. Experiment with scaling services defined in the `docker-compose.yml` file:

To scale services defined in the `docker-compose.yml` file, you can use the `docker-compose scale` command. For example, if you want to scale the `web` service to run three instances:

```bash
docker compose scale web=3
```

This command will create two additional instances of the `web` service, resulting in a total of three instances running.

You can verify the scaling by running:

```bash
docker compose ps
```

This will show you the status of each service and how many containers are running for each service.

Experiment with different scaling configurations to observe how Docker Compose manages the scaling of services in your multi-container application.

That's it! You have defined a multi-container application using Docker Compose, run the application, and experimented with scaling services.
